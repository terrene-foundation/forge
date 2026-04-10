---
drill_id: DR-SC-B-013-FULL
source_atom: SC-B-013
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Map Cross-Workspace Dependency Surface Before Parallelizing

## Setup

The practitioner is planning a multi-workspace implementation session for the Kailash Python SDK. The brief calls for three workspaces to run in parallel, with a target of completing all three within a single session. The codebase has **10 key files** that the three workspaces will touch.

**Workspace W1 — "DataFlow Bulk Operations"**
Add bulk insert and bulk update capabilities to the DataFlow engine.

| File touched | Nature of change                                            |
| ------------ | ----------------------------------------------------------- |
| `engine.py`  | Add `bulk_insert()` and `bulk_update()` methods to DataFlow |
| `models.py`  | Add `BulkOperation` model class for batch tracking          |
| `express.py` | Add `.bulk()` chain method to express query builder         |

**Workspace W2 — "DataFlow Event Hooks"**
Add pre/post event hooks to DataFlow operations so that other packages can react to data changes.

| File touched | Nature of change                                                          |
| ------------ | ------------------------------------------------------------------------- |
| `engine.py`  | Add event emission after every insert/update/delete operation             |
| `hooks.py`   | New file — event hook registry and dispatch                               |
| `events.py`  | New file — event type definitions (DataCreated, DataUpdated, DataDeleted) |

**Workspace W3 — "Nexus EventBus Integration"**
Connect DataFlow events to the Nexus EventBus so that API consumers can subscribe to data changes via WebSocket.

| File touched      | Nature of change                                            |
| ----------------- | ----------------------------------------------------------- |
| `express.py`      | Add `.subscribe()` method for real-time query subscriptions |
| `events.py`       | Import and re-export event types for Nexus consumers        |
| `bridge.py`       | New file — DataFlow-to-Nexus event bridge                   |
| `nexus_config.py` | Add EventBus channel configuration for DataFlow events      |

**Critical dependency (not stated in the brief):** W3 needs to import the event types that W2 creates. If W3 runs before W2, the `events.py` file W3 tries to import from does not exist yet. Additionally, W2 adds event emission to `engine.py`, and W3's bridge needs those events to have a specific interface — if W2 changes the interface during implementation, W3's bridge breaks.

**File overlap summary** (the practitioner must derive this):

| File              | W1  | W2  | W3  |
| ----------------- | --- | --- | --- |
| `engine.py`       | Yes | Yes | —   |
| `models.py`       | Yes | —   | —   |
| `express.py`      | Yes | —   | Yes |
| `hooks.py`        | —   | Yes | —   |
| `events.py`       | —   | Yes | Yes |
| `bridge.py`       | —   | —   | Yes |
| `nexus_config.py` | —   | —   | Yes |

## Task

1. Produce a **dependency graph** for the three workspaces. For each pair (W1-W2, W1-W3, W2-W3), identify:
   - Shared files that both workspaces modify
   - Producer-consumer relationships where one workspace creates an abstraction the other needs
   - Whether the pair can run in parallel, must be sequenced, or can run in parallel with a defined merge protocol

2. Identify the **bottleneck files** — files modified by two or more workspaces — and for each one, state whether the conflict is a merge risk (both modify existing code in the same region) or an addition risk (both add new code to the same file in different regions).

3. Produce a concrete **execution plan**: which workspaces run in parallel, which are sequenced, and what coordination checkpoints are needed between them. The plan must minimize wall-clock time while ensuring no merge conflicts or broken imports.

4. The brief says "three workspaces in parallel, one session." Based on your dependency analysis, is this achievable? If not, what is the realistic execution plan?

## Model Answer

**1. Dependency graph:**

**W1-W2 pair:**

- **Shared file:** `engine.py` — both modify it. W1 adds bulk methods; W2 adds event emission to existing methods. The modifications target different regions of the file (W1 adds new methods, W2 modifies existing methods), but both change the class interface.
- **Producer-consumer:** None. W1's bulk operations do not depend on W2's event hooks, and vice versa. However, a design question surfaces: should W1's `bulk_insert()` emit events via W2's hooks? If yes, this becomes a producer-consumer dependency (W2 must define the hook interface before W1 can emit from bulk operations).
- **Verdict:** Can run in parallel with a merge protocol for `engine.py`. The merge is low-risk because the modifications are in different regions. But the design question (bulk ops + events) must be resolved before launch.

**W1-W3 pair:**

- **Shared file:** `express.py` — W1 adds `.bulk()`, W3 adds `.subscribe()`. Both add new chain methods to the same query builder class. Low merge risk (different methods), but both modify the class definition.
- **Producer-consumer:** None.
- **Verdict:** Can run in parallel with a merge protocol for `express.py`. Low risk.

**W2-W3 pair:**

- **Shared file:** `events.py` — W2 creates it with event type definitions; W3 imports from it and re-exports for Nexus consumers. This is not a merge conflict — it is a creation dependency. If both run in parallel, W3 tries to import from a file that does not exist yet.
- **Producer-consumer:** **W2 produces, W3 consumes.** W3's `bridge.py` imports event types from W2's `events.py`. W3's bridge also depends on W2's event emission interface in `engine.py` — the bridge subscribes to events that W2 emits.
- **Verdict:** **Must be sequenced: W2 before W3.** W3 cannot start until W2's `events.py` interface is defined and committed. At minimum, W2 must complete the event type definitions and event emission interface before W3 begins.

**2. Bottleneck files:**

| File         | Workspaces | Conflict type       | Risk level                                                                                        |
| ------------ | ---------- | ------------------- | ------------------------------------------------------------------------------------------------- |
| `engine.py`  | W1, W2     | Addition risk       | Low — W1 adds new methods, W2 modifies existing ones; different regions. Merge is mechanical.     |
| `express.py` | W1, W3     | Addition risk       | Low — both add new chain methods to the same class but in different code regions.                 |
| `events.py`  | W2, W3     | Creation dependency | High — W2 creates the file, W3 imports from it. Not a merge conflict but a sequencing constraint. |

**3. Execution plan:**

```
Phase 1 (parallel):   W1 + W2
  ├── W1: engine.py (bulk methods), models.py, express.py (.bulk())
  └── W2: engine.py (event hooks), hooks.py, events.py

  Merge checkpoint: merge W1 and W2 branches.
    - engine.py: mechanical merge (different regions)
    - Resolve design question: does bulk_insert() emit events?
      If yes: add event emission to W1's bulk methods post-merge.

Phase 2 (sequential): W3
  └── W3: express.py (.subscribe()), events.py (import/re-export),
          bridge.py, nexus_config.py

  W3 starts after W2's events.py interface is merged and stable.
  express.py merge with W1's .bulk() is mechanical (different methods).
```

**Wall-clock time:** Phase 1 runs two workspaces in parallel. Phase 2 runs one workspace sequentially after the merge checkpoint. Total: ~2 workspace durations (not 1, not 3).

**4. Is "three in parallel, one session" achievable?**

No. The brief's claim of full parallelism is incorrect because W3 has a hard dependency on W2's output. The best achievable plan is 2-phase: W1+W2 in parallel (Phase 1), then W3 sequentially (Phase 2), with a merge checkpoint between phases. This reduces wall-clock time from 3 sequential workspaces to ~2, but not to 1. The brief should be corrected before work begins — launching all three in parallel would produce a broken W3 branch that imports from nonexistent modules and must be reworked.

An aggressive alternative: start W3 simultaneously but have it work against a pre-agreed event type interface (a stub `events.py` committed before any workspace launches). This introduces a fourth task — "define the event interface contract" — that must precede all three workspaces. If the interface changes during W2 implementation, W3 must rework. The pre-agreement reduces the risk but does not eliminate it.

## Scoring

| Criterion                                                                                 | Points |
| ----------------------------------------------------------------------------------------- | ------ |
| All three pairwise dependencies correctly identified                                      | 2      |
| W2→W3 producer-consumer relationship identified as a hard sequencing constraint           | 2      |
| Bottleneck files identified with correct conflict types (addition vs creation dependency) | 2      |
| Execution plan has explicit phases with merge checkpoint                                  | 2      |
| Brief's "three in parallel" correctly challenged with concrete justification              | 1      |
| Design question surfaced (should bulk ops emit events?)                                   | 1      |
| **Total**                                                                                 | **10** |

## Extensions

- **Variant A (harder):** Add a fourth workspace W4 that refactors `engine.py` to extract a base class. W4 conflicts with both W1 and W2 on `engine.py` at the structural level (class hierarchy change, not just method addition). Redraw the dependency graph and explain why W4 must run either first (before W1 and W2 modify methods on the old class structure) or last (after W1 and W2 are merged, refactoring the combined result).
- **Variant B (real scenario):** Reference the actual kailash-py evidence: journal entry `0003-CONNECTION-six-features-share-engine-bottleneck` documents six features all converging on `engine.py` (6,400 lines). Apply the dependency mapping to the six-feature scenario and compare your plan against the journal's actual mitigation (separate modules with minimal engine.py hooks).
