---
drill_id: DR-SC-P-018-FULL
source_atom: SC-P-018
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Split a Multi-Session Blocker

## Setup

The practitioner receives a dependency graph for a Kailash SDK feature release. The release has 12 work items across 3 phases.

**Phase 1 — Foundation (must complete before Phase 2 starts):**

| Item | Description                                                                                                      | Estimate   | Blocks                       |
| ---- | ---------------------------------------------------------------------------------------------------------------- | ---------- | ---------------------------- |
| P1-A | Add CRUD node generation to DataFlow models                                                                      | 1 session  | P2-D, P2-E                   |
| P1-B | Refactor Nexus transport layer into pluggable registry + transport abstractions + event bus + background service | 4 sessions | P2-F, P2-G, P2-H, P2-I, P2-J |
| P1-C | Update CLI argument parser for new command structure                                                             | 1 session  | P2-K                         |

**Phase 2 — Features (can start when their Phase 1 dependency completes):**

| Item | Description                                 | Depends on                                                        | Estimate   |
| ---- | ------------------------------------------- | ----------------------------------------------------------------- | ---------- |
| P2-D | Build DataFlow migration tool               | P1-A (CRUD nodes)                                                 | 1 session  |
| P2-E | Add DataFlow bulk operations                | P1-A (CRUD nodes)                                                 | 1 session  |
| P2-F | HTTP transport with route-style endpoints   | P1-B (needs: pluggable registry)                                  | 2 sessions |
| P2-G | CLI transport with clap integration         | P1-B (needs: pluggable registry)                                  | 1 session  |
| P2-H | MCP transport with stdio/SSE sub-transports | P1-B (needs: pluggable registry + event bus)                      | 2 sessions |
| P2-I | Plugin hot-reload system                    | P1-B (needs: pluggable registry + background service)             | 2 sessions |
| P2-J | WebSocket transport with pub/sub            | P1-B (needs: pluggable registry + event bus + background service) | 2 sessions |

**Phase 3 — Integration (starts when all Phase 2 items complete):**

| Item | Description                                        | Depends on        | Estimate   |
| ---- | -------------------------------------------------- | ----------------- | ---------- |
| P3-K | End-to-end integration tests across all transports | All Phase 2 items | 2 sessions |

**Current critical path (sequential):**

```
P1-B (4 sessions) → P2-J (2 sessions) → P3-K (2 sessions) = 8 sessions total
```

P1-A and P1-C are not on the critical path (1 session each, their downstream items finish before P2-J).

## Task

1. **Identify the blocker.** Which Phase 1 item is the critical-path blocker? What is its fan-out (how many Phase 2 items does it block)?

2. **Map partial dependencies.** For each of the 5 Phase 2 items blocked by the blocker, list exactly which sub-components of the blocker they actually need. Which items need only a subset? Which items need the full scope?

3. **Propose a split.** Divide the blocker into two independently-completable deliverables:
   - **Fast half:** The minimum sub-components that unblock the most downstream items. State the estimate.
   - **Full half:** The remaining sub-components. State the estimate.
   - Verify: the two halves must be independently completable (no circular dependency between them).

4. **Define gates.** Write a one-sentence acceptance criterion for each half. The gate must be verifiable (not "looks good" — a specific test or property).

5. **Redraw the critical path.** Show the new critical path after the split. Compute the time saved.

## Model Answer

**1. Blocker identification:**

P1-B (Nexus transport refactor) is the critical-path blocker. It is estimated at 4 sessions and blocks 5 Phase 2 items: P2-F, P2-G, P2-H, P2-I, P2-J. Its fan-out is 5.

**2. Partial dependency map:**

| Phase 2 Item             | Needs from P1-B                                     | Full scope needed?                          |
| ------------------------ | --------------------------------------------------- | ------------------------------------------- |
| P2-F (HTTP transport)    | Pluggable registry only                             | NO — subset                                 |
| P2-G (CLI transport)     | Pluggable registry only                             | NO — subset                                 |
| P2-H (MCP transport)     | Pluggable registry + event bus                      | NO — subset                                 |
| P2-I (Plugin hot-reload) | Pluggable registry + background service             | NO — subset, but different subset than P2-H |
| P2-J (WebSocket pub/sub) | Pluggable registry + event bus + background service | YES — needs all sub-components              |

Three items (P2-F, P2-G, P2-H) need only the pluggable registry (plus event bus for P2-H). Only P2-J needs the full scope. P2-I needs the registry plus background service but not the event bus.

**3. Proposed split:**

**Fast half — P1-B1: Pluggable registry + event bus (1 session)**

Extract `HandlerRegistry` (trait definition, handler registration, type-safe lookup) and `EventBus` (broadcast channel, subscribe, subscribe_filtered, publish) as standalone modules. These two components have no dependency on background service or transport abstractions — they are infrastructure primitives that the transports consume.

Estimate: 1 session. The registry and event bus are self-contained abstractions.

**Full half — P1-B2: Transport abstractions + background service (2-3 sessions)**

Extract `Transport` trait, `HTTPTransport`, `BackgroundService` (task spawning, shutdown coordination, health checking). The transport abstractions use the registry but can be built in parallel with the Phase 2 items that only need the registry.

Estimate: 2-3 sessions. Transport abstractions involve I/O patterns and shutdown coordination.

**Independence check:** P1-B1 (registry + event bus) does not depend on P1-B2 (transports + background service). P1-B2 uses the registry but does not need P1-B1 to be complete first — it can import the registry module once P1-B1 is merged. P1-B1 and P1-B2 can be built in parallel after the initial module boundaries are defined.

**4. Gates:**

- **P1-B1 gate:** The `HandlerRegistry` can register, look up, and invoke handlers by name with type-checked parameters, and the `EventBus` can broadcast to filtered subscribers — demonstrated by unit tests with at least 3 handler types and 2 subscriber filters, all passing.
- **P1-B2 gate:** The `Transport` trait is implemented by at least one concrete transport (HTTP), `BackgroundService` can spawn and gracefully shut down tasks — demonstrated by an integration test that starts a background task, sends a shutdown signal, and verifies clean termination within 5 seconds.

**5. Redrawn critical path:**

**Before split:**

```
P1-B (4 sessions) → P2-J (2 sessions) → P3-K (2 sessions) = 8 sessions
```

**After split:**

```
P1-B1 (1 session) → P2-F (2s), P2-G (1s), P2-H (2s) start immediately
P1-B2 (2-3 sessions) runs in PARALLEL with P2-F, P2-G, P2-H
P2-I starts after P1-B2 (needs background service)
P2-J starts after P1-B2 (needs everything)

New critical path:
  P1-B1 (1s) ─┬─ P2-H (2s) ──────────────┐
               │                            │
               └─ P1-B2 (3s) → P2-J (2s) ──┤→ P3-K (2s) = 7 sessions
                                            │
              (P2-I starts after P1-B2)  ───┘
```

The longest path is now: P1-B1 (1) + P1-B2 (3, in parallel after B1) + P2-J (2) + P3-K (2) = 7 sessions. But since P1-B2 starts immediately after P1-B1 while P2-F/G/H also start, the wall-clock critical path is:

- Session 1: P1-B1 completes
- Sessions 2-4: P1-B2 runs in parallel with P2-F, P2-G, P2-H
- Session 4: P1-B2 completes; P2-I and P2-J can start
- Sessions 5-6: P2-J completes (P2-I also completes in parallel)
- Sessions 7-8: P3-K

**Time saved:** The critical path drops from 8 sessions to 7 sessions (1 session saved on the critical path). But the larger gain is parallelism: P2-F, P2-G, and P2-H start 3 sessions earlier than in the original plan, meaning total project duration compresses from 8 sequential sessions to 8 sessions with 3 items running in parallel during sessions 2-4. If parallel execution is available, wall-clock time drops further.

## Scoring

| Criterion                                                                                        | Points |
| ------------------------------------------------------------------------------------------------ | ------ |
| P1-B correctly identified as the blocker with fan-out of 5                                       | 1      |
| Partial dependencies mapped: at least 3 of 5 items correctly traced to sub-components            | 2      |
| Split separates registry+event bus from transports+background service (or equivalent clean seam) | 2      |
| Two halves are independently completable (no circular dependency)                                | 1      |
| Gates are verifiable (specific tests or properties, not "looks good")                            | 2      |
| Critical path redrawn with correct time computation                                              | 2      |
| **Total**                                                                                        | **10** |

## Extensions

- **Variant A (no clean seam):** Modify the setup so that the event bus depends on the background service for its subscription cleanup thread. Now the registry can be extracted alone, but the event bus cannot be separated from the background service. How does this change the split? Which items can still be unblocked early, and which must wait?
- **Variant B (estimate disagreement):** Add a detail: the original plan estimated P1-B at 4 sessions, but a detailed breakdown of the four sub-components (registry: 0.5s, event bus: 0.5s, transports: 2s, background service: 1.5s) totals 4.5 sessions. The 0.5-session disagreement suggests either the plan assumed overlap or the breakdown double-counts shared work. Identify which interpretation is correct and adjust the split estimates accordingly.
