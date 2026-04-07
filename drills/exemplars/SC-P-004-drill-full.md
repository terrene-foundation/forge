---
drill_id: DR-SC-P-004-FULL
source_atom: SC-P-004
difficulty: beginner
time_box_minutes: 15
---

# Label Every File with a Disposition Before Starting Implementation

## Setup

The practitioner receives a target module containing 12 source files and an architecture brief written by a prior session.

**Architecture brief** (contains 3 deliberate inaccuracies):

> "The `nexus-transport` module requires parity with the Python SDK. The Python SDK has 5 transport abstractions (HandlerRegistry, Transport, EventBus, Plugin, BackgroundService). The Rust implementation currently has only 2 of 5 abstractions implemented. Estimated effort: 2 sessions. The EventBus uses a simple channel with no filtering capability. Auth is not implemented. The scheduler does not exist in Rust."

**Source files** (simplified from the real kailash-rs nexus crate, 12 files):

1. `handler.rs` (340 lines) -- HandlerFn trait, ClosureHandler, HandlerDef, HandlerRegistry with type-safe parameter checking
2. `transport/http.rs` (520 lines) -- HTTP transport with full FastAPI-equivalent routing
3. `transport/cli.rs` (280 lines) -- CLI transport with clap argument parsing
4. `transport/mcp.rs` (410 lines) -- MCP transport with stdio, SSE, and HTTP sub-transports
5. `event_bus.rs` (190 lines) -- 256-capacity broadcast with publish, subscribe, and subscribe_filtered
6. `plugin.rs` (350 lines) -- Plugin system with dependency resolution, hot-reload, per-plugin health checking
7. `auth/mod.rs` (280 lines) -- NexusAuthPlugin with basic_auth, saas_app, JWT, RBAC, APIKey, RateLimit
8. `auth/rate_limit.rs` (120 lines) -- Global and per-user rate limiting
9. `middleware/cors.rs` (95 lines) -- CorsConfig with permissive() and strict() factories
10. `session.rs` (160 lines) -- SessionStore trait, InMemorySessionStore with TTL
11. `config.rs` (200 lines) -- NexusConfig, transport configuration
12. `background.rs` (0 lines) -- Empty file, placeholder only

## Task

1. Read each of the 12 source files. For each file, assign a disposition:
   - **SOLID** -- working code that should not be changed
   - **EXTEND** -- working code that needs additional functionality added
   - **ENHANCE** -- working code that needs improvement to existing functionality
   - **REWRITE** -- code that needs to be replaced entirely
   - **IMPLEMENT** -- file does not exist or is empty; must be created from scratch
   - **NEEDS MIGRATION** -- structural change required (e.g., different architecture)
2. Produce a table with columns: File Path, Line Count, Disposition, One-Sentence Rationale.
3. Compare your disposition table against the architecture brief. List every discrepancy between what the brief claims and what the files actually contain.
4. Revise the effort estimate based on your table. State the revised estimate and explain why it differs from the brief's "2 sessions."

## Model Answer

| File | Lines | Disposition | Rationale |
|---|---|---|---|
| `handler.rs` | 340 | SOLID | Type-safe handler system with compile-time checking. More advanced than Python's class extraction. |
| `transport/http.rs` | 520 | SOLID | Full HTTP transport with routing. Already exceeds Python parity. |
| `transport/cli.rs` | 280 | SOLID | CLI transport with clap. Python has no CLI channel -- Rust exceeds parity. |
| `transport/mcp.rs` | 410 | SOLID | MCP with 3 sub-transports and auth support. Production-ready. |
| `event_bus.rs` | 190 | EXTEND | Broadcast works with filtering, but needs convenience methods (on_event, emit) and a DataFlow bridge. |
| `plugin.rs` | 350 | SOLID | Dependency resolution, hot-reload, health checking. Exceeds Python. |
| `auth/mod.rs` | 280 | SOLID | Full auth with JWT+RBAC+APIKey+RateLimit. Brief says "not implemented" -- this is wrong. |
| `auth/rate_limit.rs` | 120 | SOLID | Global and per-user rate limiting. Working. |
| `middleware/cors.rs` | 95 | SOLID | CORS factories. Working. |
| `session.rs` | 160 | SOLID | Session store with TTL. Working. |
| `config.rs` | 200 | EXTEND | Needs route-style endpoint registration configuration. |
| `background.rs` | 0 | IMPLEMENT | Empty placeholder. This is the only file requiring implementation from scratch. |

**Discrepancies with the architecture brief:**

1. Brief says "only 2 of 5 abstractions implemented." Actual: 4 of 5 abstractions are fully implemented and a 5th (BackgroundService) is the only gap. The brief overstates the gap by 60%.
2. Brief says "EventBus uses a simple channel with no filtering." Actual: `event_bus.rs` has `subscribe_filtered`. The brief is wrong.
3. Brief says "Auth is not implemented." Actual: `auth/mod.rs` has a complete NexusAuthPlugin with JWT, RBAC, APIKey, and RateLimit. The brief is wrong.
4. Brief says "scheduler does not exist in Rust." Actual: `kailash-core::Scheduler` with CronExpr parser is reusable from the core crate (not in the module but available).

**Revised effort estimate:** 1 session (down from 2). The primary deliverable shifts from "implement parity" to "document existing superiority plus implement BackgroundService (~360 lines) plus add convenience wiring." This matches the real finding from journal entry `0001-DISCOVERY-nexus-already-exceeds-python-b0`.

## Scoring Criteria

1. **Disposition derived from files, not brief** (pass/fail): The practitioner's SOLID labels must come from reading the source files, not from trusting the brief. If the practitioner labels `auth/mod.rs` as IMPLEMENT because the brief says "auth not implemented," they are failing the drill regardless of other labels.
2. **Discrepancy count** (pass: 3/3 minimum): The practitioner must identify at least 3 of the 4 discrepancies between the brief and the codebase. The auth discrepancy is the most impactful; missing it means the practitioner would plan to build 280 lines of code that already exists.
3. **Effort estimate direction** (pass/fail): The revised estimate must be lower than the brief's 2 sessions. A practitioner who arrives at the same or higher estimate after finding 8 SOLID files has not connected the disposition data to the planning.
4. **IMPLEMENT vs EXTEND accuracy** (pass/fail): Only `background.rs` should be IMPLEMENT. Any file with working code labelled IMPLEMENT is a critical error -- it means the practitioner would rewrite working code from scratch.
5. **One-sentence rationales are evidence-based** (pass: 10/12): Each rationale must reference something observed in the file, not inferred from the filename or the brief.

## Common Mistakes

1. **Reading the brief, trusting it, and starting implementation.** The practitioner reads "2 sessions, 3 capabilities missing" and begins writing auth, EventBus filtering, and scheduler code. Mid-implementation, they discover `auth/mod.rs` already has 280 lines of production auth code. Each discovery forces a context switch, a re-plan, and often a revert. The kaizen-cli-rs entry (0014) shows the corrective: a 21-file audit that took one pass but saved multiple sessions of mid-implementation re-derivation.

2. **Labelling dispositions from filenames.** The practitioner sees `background.rs` and assigns IMPLEMENT (correct by coincidence), then sees `event_bus.rs` and assigns EXTEND (correct), then sees `plugin.rs` and assigns EXTEND because "plugin systems always need work" (wrong -- it is SOLID). Filename-based disposition is a shortcut that produces correct labels ~60% of the time and wrong labels ~40% of the time. The drill requires reading the file.

3. **Omitting the line count.** Line counts anchor the effort estimate. A file with 520 lines labelled SOLID represents 520 lines of code that do NOT need to be written. A practitioner who omits line counts cannot ground their effort revision in concrete data.

## Extensions

1. **Version mismatch detection.** Add dependency files (`Cargo.toml`, `pyproject.toml`) to the module. Plant 2 version mismatches (e.g., polars 0.39 in the brief vs 0.53 in `Cargo.toml`; ndarray 0.15 vs 0.16). The practitioner must extend their disposition table to include dependency auditing, matching the kailash-ml-crate finding (journal 0001) where 3 critical dependency corrections were found.

2. **21-file scale.** Give the practitioner the full 21-file listing from journal entry `0014-DISCOVERY-existing-codebase-architecture` (kaizen-cli-rs) and a time limit of 20 minutes. This tests whether the disposition-labelling discipline scales -- the practitioner must maintain file-by-file reading under time pressure rather than reverting to brief-based shortcuts.

3. **Disposition-to-plan conversion.** After completing the disposition table, the practitioner must produce a todo list where each todo references specific files by disposition. SOLID files generate zero todos. EXTEND files generate "add X to Y" todos. IMPLEMENT files generate "create Z" todos. The drill succeeds when the todo list is shorter than what the original brief would have produced.
