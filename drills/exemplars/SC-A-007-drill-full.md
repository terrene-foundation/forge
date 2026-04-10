---
drill_id: DR-SC-A-007-FULL
source_atom: SC-A-007
difficulty: intermediate
time_box_minutes: 30
modality: drill
application: [COC]
---

# Zero-Config Security Audit

## Setup

The practitioner receives a Python framework module that provides a zero-config data service. The module lets developers start a fully functional database API server with a single line of code.

**Module: `data_service.py` (30 lines)**

```python
from dataclasses import dataclass, field

@dataclass
class DataServiceConfig:
    host: str = "0.0.0.0"
    port: int = 8500
    enable_writes: bool = True
    enable_admin: bool = True
    auth_required: bool = False
    cors_origins: list[str] = field(default_factory=lambda: ["*"])
    tls_enabled: bool = False
    rate_limit: int = 0  # 0 = unlimited

class DataService:
    def __init__(self, db_url: str, config: DataServiceConfig | None = None):
        self.db_url = db_url
        self.config = config or DataServiceConfig()

    async def start(self):
        """Start the data service. Zero-config: just call start()."""
        server = create_server(
            host=self.config.host,
            port=self.config.port,
            routes=self._build_routes(),
        )
        await server.serve()

    def _build_routes(self):
        routes = []
        routes.append(Route("GET", "/data/{table}", self._handle_read))
        if self.config.enable_writes:
            routes.append(Route("POST", "/data/{table}/write", self._handle_write))
            routes.append(Route("DELETE", "/data/{table}/{id}", self._handle_delete))
        if self.config.enable_admin:
            routes.append(Route("POST", "/admin/migrate", self._handle_migrate))
            routes.append(Route("POST", "/admin/drop", self._handle_drop_table))
            routes.append(Route("GET", "/admin/export", self._handle_export))
        routes.append(Route("GET", "/health", self._handle_health))
        return routes
```

**Zero-config usage (the one-liner the framework advertises):**

```python
service = DataService("sqlite:///app.db")
await service.start()
```

The practitioner must audit the security posture of this zero-config path.

## Task

1. **Enumerate every network-accessible resource** created by the zero-config `start()` call. For each resource, record: the HTTP method and path, whether it is read-only or read-write, and what data it can access or modify.

2. **Classify each resource** as localhost-only or network-reachable given the default configuration. Explain the impact of the default `host` value.

3. **Identify every security parameter that defaults to the permissive value.** For each, state the default, the secure alternative, and the consequence of the permissive default.

4. **Write the secure defaults** — a revised `DataServiceConfig` that makes the zero-config path deny-by-default without breaking the one-line usability promise. The developer must still be able to write `service = DataService("sqlite:///app.db"); await service.start()` and get a working (but restricted) service.

5. Explain why a **production opt-in gate** is needed in addition to secure defaults, and describe what the gate should require.

## Model Answer

**1. Network-accessible resources (zero-config):**

| #   | Method | Path                  | Read/Write          | Data Access                                                                               |
| --- | ------ | --------------------- | ------------------- | ----------------------------------------------------------------------------------------- |
| 1   | GET    | `/data/{table}`       | Read-only           | Reads any table in the database by name. No access control — any table name is accepted.  |
| 2   | POST   | `/data/{table}/write` | Write               | Writes to any table in the database. Accepts arbitrary data in the request body.          |
| 3   | DELETE | `/data/{table}/{id}`  | Write (destructive) | Deletes any record from any table by ID. No confirmation, no soft-delete.                 |
| 4   | POST   | `/admin/migrate`      | Write (schema)      | Runs database migrations. Can alter table structure, add/drop columns.                    |
| 5   | POST   | `/admin/drop`         | Write (destructive) | Drops entire tables from the database. Irreversible data loss.                            |
| 6   | GET    | `/admin/export`       | Read-only           | Exports database contents. Full data exfiltration path.                                   |
| 7   | GET    | `/health`             | Read-only           | Health check. Minimal risk, but may leak internal state (database connectivity, version). |

**Total: 7 endpoints, 4 of which are write/destructive, 1 exfiltration path, 0 authentication required.**

**2. Network reachability classification:**

| Resource        | Reachable From             | Why                                                                                                                                          |
| --------------- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| All 7 endpoints | Any machine on the network | `host="0.0.0.0"` binds to all network interfaces. The service is reachable from any machine that can route to this host, not just localhost. |

**Impact of `host="0.0.0.0"`:** A developer running `await service.start()` on a laptop connected to a coffee shop Wi-Fi exposes all 7 endpoints — including table drop, data deletion, and full export — to every device on that network. The developer intended "local development server" but got "network-accessible database administration panel with no authentication."

**3. Security parameters defaulting to permissive values:**

| #   | Parameter       | Default (Permissive) | Secure Alternative | Consequence of Permissive Default                                                                           |
| --- | --------------- | -------------------- | ------------------ | ----------------------------------------------------------------------------------------------------------- |
| 1   | `host`          | `"0.0.0.0"`          | `"127.0.0.1"`      | Service is network-reachable instead of localhost-only.                                                     |
| 2   | `enable_writes` | `True`               | `False`            | Write and delete endpoints are active. Any unauthenticated user can modify data.                            |
| 3   | `enable_admin`  | `True`               | `False`            | Admin endpoints (migrate, drop, export) are active. Full database control available without authentication. |
| 4   | `auth_required` | `False`              | `True`             | No authentication on any endpoint. All operations are anonymous.                                            |
| 5   | `cors_origins`  | `["*"]`              | `[]` (no origins)  | Any web page can make cross-origin requests to the service. Browser-based attacks can reach every endpoint. |
| 6   | `tls_enabled`   | `False`              | `True`             | Traffic is unencrypted. Credentials (if auth were enabled) and data are transmitted in plaintext.           |
| 7   | `rate_limit`    | `0` (unlimited)      | `100` (per minute) | No rate limiting. Brute-force attacks, data scraping, and denial-of-service are unbounded.                  |

**7 of 7 security parameters default to the permissive value.** The zero-config path is a maximum-exposure configuration.

**4. Secure defaults (revised `DataServiceConfig`):**

```python
@dataclass
class DataServiceConfig:
    host: str = "127.0.0.1"         # Localhost only — network access requires explicit opt-in
    port: int = 8500
    enable_writes: bool = False     # Read-only by default — writes require explicit opt-in
    enable_admin: bool = False      # No admin endpoints — admin requires explicit opt-in
    auth_required: bool = False     # Auth not required for localhost read-only (dev convenience)
    cors_origins: list[str] = field(default_factory=list)  # No CORS — cross-origin requires explicit opt-in
    tls_enabled: bool = False       # TLS not required for localhost (dev convenience)
    rate_limit: int = 100           # 100 requests/minute default — unlimited requires explicit opt-in
```

**What the developer gets with zero-config:** A localhost-only, read-only data service with no admin endpoints, no CORS, and basic rate limiting. The one-liner `await service.start()` still works — it starts a server that can read data from any table on localhost. This covers the 80% use case (local development, data exploration) without exposing write, delete, or admin capabilities.

**What the developer must opt into:** To enable writes: `DataServiceConfig(enable_writes=True)`. To bind to the network: `DataServiceConfig(host="0.0.0.0", auth_required=True)`. The opt-in is explicit, one-line, and documented — it preserves the convenience while requiring the developer to name the risk.

**5. Why a production opt-in gate is needed:**

Secure defaults reduce the attack surface in development but do not eliminate it in production. A developer who gradually enables features during development (`enable_writes=True`, then `host="0.0.0.0"`, then `enable_admin=True`) will accumulate a permissive configuration that looks like the original insecure defaults — arrived at incrementally rather than by omission.

The production opt-in gate should require:

- `auth_required=True` when `host != "127.0.0.1"` — the service refuses to bind to a non-localhost address without authentication configured. This is a hard gate, not a warning: `start()` raises `SecurityError("Cannot bind to network without auth_required=True")`.
- `tls_enabled=True` when `auth_required=True` — credentials must not be transmitted in plaintext. Same hard gate.

The gate is a runtime enforcement mechanism (CO §19, Layer 3 — Guardrails) that operates outside the developer's intent. The developer may intend to "add auth later." The gate makes "later" structurally impossible — the server will not start in an insecure network-reachable configuration.

## Scoring

| Criterion                                                                                                                | Points |
| ------------------------------------------------------------------------------------------------------------------------ | ------ |
| All 7 endpoints enumerated with correct read/write classification                                                        | 2      |
| Network reachability correctly traced to `host="0.0.0.0"` with impact stated                                             | 1      |
| At least 6 of 7 permissive defaults identified with secure alternatives and consequences                                 | 2      |
| Secure defaults preserve the one-line zero-config usability (localhost + read-only works without arguments)              | 2      |
| Production gate uses a hard enforcement mechanism (raises error, not just warns) tied to specific parameter combinations | 2      |
| Explanation connects zero-config audit to the incremental permissiveness problem (not just "defaults are bad")           | 1      |
| **Total**                                                                                                                | **10** |

## Extensions

- **Variant A (hidden endpoint):** Add an undocumented `/debug/sql` endpoint that accepts raw SQL queries and is enabled by an environment variable `DEBUG=1`. The endpoint is not in `_build_routes()` — it is registered in `create_server()` when the environment variable is set. The practitioner must discover it by auditing the server factory, not just the route builder, and include it in the exposure inventory.
- **Variant B (timing side-channel in auth):** After adding `auth_required=True` to the secure defaults, the practitioner discovers that the authentication check uses early-return comparison (`if api_key == stored_key`). The practitioner must identify the timing side-channel (string comparison leaks key length and prefix), connect it to the kailash-rs 0003 journal entry (`.any()` short-circuit across key hashes), and propose a constant-time comparison fix.
