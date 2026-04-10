---
drill_id: DR-SC-B-009-FULL
source_atom: SC-B-009
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Cross-SDK Upstream Brokerage

## Setup

The practitioner is working in an application repository (`kz-engage/`) that uses the Kailash Python SDK. During development of a Telegram bot feature, they discover a bug: every POST request to the durable workflow endpoint returns the same cached response, regardless of the request body content.

**Bug symptoms:**

- User sends "What is CO?" via Telegram — receives a correct answer about Cognitive Orchestration.
- User sends "What is CARE?" — receives the same CO answer again.
- User sends "Explain EATP" — receives the CO answer a third time.
- All subsequent questions return the first cached response.

**Investigation trace:**
The practitioner has already narrowed the bug to the Nexus `DurableWorkflowServer` middleware. The dedup fingerprint is computed from the HTTP request method + endpoint path, but it does not include the request body. Since all requests are `POST /api/chat`, they all get the same fingerprint, and the dedup layer returns the cached first response for every subsequent request.

**What the practitioner knows:**

- The bug is in the Nexus middleware, not in the application code. The application correctly passes different request bodies.
- The Nexus middleware is part of the Kailash Python SDK (`kailash-nexus` package).
- The same middleware architecture exists in the Kailash Rust SDK (`kailash-rs`), which uses the same dedup fingerprint algorithm.
- A local workaround exists: set `enable_durability=False` in the server configuration, which disables the dedup layer entirely.

**SDK source confirmation (provided as reference):**

- `kailash-nexus/src/server/durable.py`, line 87: `fingerprint = hash(f"{request.method}:{request.path}")` — body is not included.
- `kailash-rs/nexus/src/server/durable.rs`, line 112: `let fingerprint = hash(&format!("{}:{}", method, path));` — same algorithm, same bug.

## Task

1. **Apply the local mitigation.** Write the configuration change that disables durability to unblock the immediate feature work. Add a code comment that marks this as a temporary mitigation, not a permanent fix.

2. **File the upstream issue against the Python SDK.** Draft a GitHub issue for `terrene-foundation/kailash-py` that includes: (a) a title following conventional format, (b) steps to reproduce, (c) expected vs actual behaviour, (d) root cause analysis pointing to the specific file and line, and (e) a proposed fix (include request body hash in the fingerprint).

3. **Check the other SDK.** The practitioner knows the Rust SDK uses the same algorithm. File a second GitHub issue against `esperie/kailash-rs` for the same bug, cross-referencing the Python issue. Explain why filing against both SDKs is necessary even though the practitioner is only using the Python SDK.

4. **Record in the session journal.** Write a journal entry that documents: (a) the bug discovery, (b) the local mitigation applied, (c) both upstream issue numbers, and (d) the condition under which the local mitigation can be removed (both upstream fixes landed and the application has upgraded to the fixed SDK versions).

5. **Verify no prior fix exists.** Before filing, the practitioner should check whether the issue has already been reported or fixed. State: what would you check, and what would you do if you found that the Python issue was already fixed in a newer SDK version but the Rust issue was still open?

## Model Answer

**Step 1 — Local mitigation:**

```python
# In kz-engage/src/bot/server.py:
server = DurableWorkflowServer(
    workflow=chat_workflow,
    enable_durability=False,  # TEMPORARY MITIGATION: Nexus dedup fingerprint
    # does not include request body — all POST /api/chat requests get the same
    # cached response. Upstream issues: kailash-py#175, kailash-rs#110.
    # Remove this flag once both SDKs ship the fix and we upgrade.
)
```

The comment includes: (a) that it is temporary, (b) the root cause in plain language, (c) the upstream issue references, and (d) the removal condition. A future developer who sees `enable_durability=False` without this context would assume it is an intentional design choice and never re-enable it.

**Step 2 — Python SDK issue:**

```markdown
## Title

fix(nexus): DurableWorkflowServer dedup fingerprint should include request body

## Description

The DurableWorkflowServer dedup fingerprint is computed from `method:path` only
(durable.py:87). For endpoints that receive different payloads at the same path
(e.g., POST /api/chat with varying user messages), all requests get the same
fingerprint, and the dedup layer returns the first cached response for every
subsequent request.

## Steps to reproduce

1. Create a DurableWorkflowServer with a POST endpoint
2. Send POST /api/chat with body {"message": "What is CO?"}
3. Receive correct response
4. Send POST /api/chat with body {"message": "What is CARE?"}
5. Receive the same CO response (should receive a CARE response)

## Expected behaviour

Each unique request body produces a unique fingerprint. The dedup layer only
returns cached responses when both the endpoint AND the body match.

## Actual behaviour

All POST requests to the same endpoint share one fingerprint. The first
response is returned for all subsequent requests regardless of body content.

## Root cause

`kailash-nexus/src/server/durable.py`, line 87:
`fingerprint = hash(f"{request.method}:{request.path}")`
Body is not included in the hash.

## Proposed fix

Include a hash of the request body in the fingerprint:
`fingerprint = hash(f"{request.method}:{request.path}:{hash(request.body)}")`
For streaming bodies, hash the first N bytes or use a content-hash header
if available.

## Cross-reference

Same algorithm exists in kailash-rs (nexus/src/server/durable.rs:112).
Filed as esperie/kailash-rs#110.
```

**Step 3 — Rust SDK issue:**

```markdown
## Title

fix(nexus): Dedup fingerprint does not include request body (same as kailash-py#175)

## Description

The Nexus durable server dedup fingerprint in kailash-rs uses the same
`method:path` algorithm as kailash-py (see terrene-foundation/kailash-py#175).
All POST requests to the same endpoint share one fingerprint regardless of body.

Root cause: nexus/src/server/durable.rs, line 112:
`let fingerprint = hash(&format!("{}:{}", method, path));`

## Why file separately

The Python and Rust SDKs have independent release cycles and maintainers.
A fix in kailash-py does not propagate to kailash-rs automatically. Both
must be fixed independently to prevent the same bug from persisting in
Rust-based consumers. Cross-SDK parity requires explicit filing against
each affected repository.

## Cross-reference

Python counterpart: terrene-foundation/kailash-py#175
```

Filing against both SDKs is necessary because: (a) the SDKs have independent codebases and release cycles — a fix in one does not propagate to the other, (b) Rust-based consumers will hit the same bug independently and file duplicate issues if the original is not tracked, (c) cross-SDK parity (SC-B-016) requires that shared abstractions behave consistently, and a fix in only one SDK creates a parity divergence.

**Step 4 — Session journal entry:**

```markdown
# DISCOVERY: Nexus dedup fingerprint bug — cached responses for all POST requests

## What happened

Herald's Telegram bot returned the same answer for every question. Root cause:
Nexus DurableWorkflowServer dedup fingerprint uses method:path only, ignoring
request body. All POST /api/chat requests get identical fingerprints.

## Local mitigation

Set enable_durability=False in server.py. This disables the dedup layer entirely,
which means durable mode is not active. Acceptable for the current feature but
must be re-enabled once the SDK fix ships.

## Upstream issues filed

- kailash-py#175: terrene-foundation/kailash-py — dedup fingerprint should
  include request body
- kailash-rs#110: esperie/kailash-rs — same algorithm, same bug

## Removal condition

Remove enable_durability=False when BOTH conditions are met:

1. kailash-py ships a release containing the fix for #175
2. kz-engage upgrades to that release
   If only the Python fix ships, the mitigation can be removed. The Rust issue
   is tracked separately for Rust consumers.
```

**Step 5 — Prior fix check:**

Before filing, check:

1. Search `terrene-foundation/kailash-py` issues and PRs for "dedup", "fingerprint", "durable", "cached response".
2. Check the CHANGELOG or release notes for the current and next SDK version.
3. Search `esperie/kailash-rs` issues for the same terms.
4. If the Python issue was already fixed in a newer SDK version: do not file the Python issue — instead, upgrade kz-engage to the fixed version and remove the local mitigation. Still file the Rust issue if it is not fixed there. Update the journal entry to note: "Python fix already shipped in vX.Y.Z; upgraded; Rust issue still open as kailash-rs#110."

## Scoring

| Criterion                                                                                                          | Points |
| ------------------------------------------------------------------------------------------------------------------ | ------ |
| Local mitigation includes temporary marker, root cause, upstream issue references, and removal condition           | 2      |
| Python SDK issue has reproducible steps, root cause with file:line, and proposed fix                               | 2      |
| Rust SDK issue filed separately with cross-reference and explicit justification for dual filing                    | 2      |
| Journal entry documents all four required elements (discovery, mitigation, upstream references, removal condition) | 2      |
| Prior-fix check describes a concrete verification procedure and handles the "already fixed in one SDK" case        | 2      |
| **Total**                                                                                                          | **10** |

## Extensions

- **Variant A (cross-SDK triage cascade):** After filing both issues, the practitioner discovers that three other cross-SDK issues (kailash-py#145, #146, #147) were filed 2 months ago for similar parity bugs. All three have been fixed in the Python SDK but not in Rust. The practitioner must now: (a) check whether the Rust SDK still has each bug by reading the current Rust source, (b) for any that are still open, file or update Rust issues, and (c) propose a cross-SDK parity audit process so that future Python fixes are systematically checked against Rust. This links to SC-B-016 (bidirectional cross-language parity).
- **Variant B (workaround pressure):** The practitioner's immediate manager says "just disable durability permanently — it is not a critical feature for this bot." The practitioner must explain why the local workaround violates CO §17 (Single Source of Truth) and PACT §10 (Monotonic Tightening): disabling durability loosens the system's constraint without signalling the loosening to the SDK. The correct response is: the local mitigation is acceptable as a time-bounded tourniquet, but the upstream fix is the deliverable. The feature flag stays marked as temporary regardless of the manager's preference, because the authority for the fix belongs to the SDK, not to the consumer.
