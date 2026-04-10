---
drill_id: DR-SC-P-024-FULL
source_atom: SC-P-024
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Temporary Workaround Accumulation Trigger

## Setup

The practitioner receives a project timeline from an SDK development effort. The project is building a multi-package platform with three layers: a core library, a transport layer, and application modules that compose both.

**Background:** The core library defines an `EventBus` interface as part of its `Transport` abstraction. The `Transport` module handles message routing between components with delivery guarantees, backpressure, and observability. The `EventBus` was designed to be accessed only through `Transport` -- never instantiated directly.

However, the `Transport` module is blocked on a prerequisite refactor (decoupling session state from the router) that is estimated at 3 sessions of work. The team decides to expose a standalone `EventBus` class -- without `Transport` wrapping -- as a temporary bridge so that other workspaces can proceed.

**Timeline:**

**Day 1 (Session 12):**

```
Decision: Expose standalone EventBus as a temporary bridge.
Rationale: Transport refactor (B0) is blocking 4 downstream workspaces.
  Standalone EventBus lets them proceed while B0 is completed.
Explicit intent: "Replace standalone EventBus with Transport-wrapped
  EventBus when B0 completes. Do not build production features on the
  standalone API."
Consumer count: 0 (just created, no consumers yet).
File: src/events/standalone.py -- 85 lines, no tests, no docs.
```

**Day 2 (Session 13):**

```
Workspace B2 (EventBus core) imports standalone EventBus.
Rationale: B2 needs pub/sub for inter-component messaging.
  Transport is not ready. Standalone EventBus does what B2 needs.
Consumer count: 1 (B2).
B2 adds 3 event types that use the standalone API:
  ComponentStarted, ComponentStopped, HealthCheck.
B2 has 8 tests, all against the standalone EventBus interface.
```

**Day 3 (Session 15):**

```
Workspace B7 (DataFlow-Nexus bridge) imports standalone EventBus.
Rationale: B7 needs to emit events when DataFlow writes complete,
  so Nexus API endpoints can push updates to connected clients.
  The bridge subscribes to DataFlow write events and publishes
  Nexus-formatted notifications.
Consumer count: 2 (B2 + B7).
B7 adds 2 event types: WriteComplete, ClientNotify.
B7 has 5 tests, all against the standalone EventBus interface.
B7 also adds a convenience wrapper: EventBridge(bus, transform_fn)
  that the B7 author expects other workspaces to reuse.
```

**Day 5 (Session 18):**

```
Workspace B8 (webhook delivery) imports standalone EventBus.
Rationale: Webhooks need to subscribe to events and deliver them
  to external URLs. Uses B7's EventBridge wrapper.
Consumer count: 3 (B2 + B7 + B8).
B8 adds 4 event types: WebhookQueued, WebhookDelivered,
  WebhookFailed, WebhookRetry.
B8 has 11 tests, all against the standalone EventBus interface.
```

**Day 6 (Session 19):**

```
Workspace A1 (on_source_change reactive triggers) imports standalone EventBus.
Rationale: Source-change detection needs to publish events when
  monitored files change. Uses B7's EventBridge wrapper.
Consumer count: 4 (B2 + B7 + B8 + A1).
A1 adds 2 event types: SourceChanged, SourceDeleted.
A1 has 6 tests against standalone EventBus.
```

**Day 8 (Session 22):**

```
Transport refactor (B0) completes. The Transport-wrapped EventBus
  is now available.
Problem: Replacing standalone EventBus with Transport EventBus requires:
  - Updating 4 workspaces (B2, B7, B8, A1)
  - Migrating 11 event types to Transport-compatible definitions
  - Rewriting 30 tests from standalone to Transport interface
  - Retiring B7's EventBridge wrapper (Transport has its own bridging)
  - Verifying that Transport's delivery guarantees do not break
    consumers that assumed fire-and-forget semantics
Estimated migration cost: 2-3 sessions.
Decision: "Defer migration to a future cleanup sprint."
```

**Day 12 (Session 26):**

```
Rust SDK (kailash-rs) begins implementing the event system.
The Rust team looks at the Python codebase for reference.
They find 4 workspaces using standalone EventBus and assume
  this is the intended architecture.
The Rust implementation builds its event system as standalone
  (no Transport wrapping), replicating the Python workaround
  in a second language.
Consumer count: 4 Python + 1 Rust implementation = structural lock-in.
```

## Task

1. Read the timeline. Identify the specific day and session at which the practitioner should have intervened. Your answer must name a day, not a range ("Day 2-3") or a vague moment ("when it started growing").

2. Explain why that day -- and not any earlier or later day -- is the critical intervention point. Your explanation must address: what changed at that moment that made the workaround's trajectory predictable, and why waiting one more day made the problem materially worse.

3. At your identified intervention point, the practitioner has exactly two valid actions. Name both, explain what each entails concretely in this scenario, and recommend one with a justification. The wrong answer is "add it to the backlog" or "flag it for future cleanup."

4. Trace the full cost chain from the missed intervention through Day 12. For each subsequent day, identify what new cost was added that would not have existed if the intervention had happened at the correct moment. The cost chain must include the Rust replication on Day 12 and explain why that cost is qualitatively different from the Python consumer costs.

## Model Answer

**Step 1 -- The intervention point is Day 3 (Session 15).**

Day 3 is when workspace B7 becomes the second consumer of the standalone EventBus.

**Step 2 -- Why Day 3, not any other day:**

**Why not Day 1:** On Day 1, the standalone EventBus is created with zero consumers. It is a legitimate bridge decision: Transport is blocked, downstream work needs to proceed, and the workaround is explicitly marked as temporary. Creating a bridge with an explicit replacement intent is a reasonable engineering choice. No intervention is needed because the workaround has no dependents.

**Why not Day 2:** On Day 2, B2 becomes the first consumer. This is expected -- the workaround was created precisely so that workspaces like B2 could proceed. One consumer is the intended use case of a temporary bridge. The workaround is serving its purpose. The practitioner should note B2 as the first consumer and monitor for a second, but no intervention is required.

**Why Day 3:** On Day 3, B7 becomes the second consumer. This is the structural signal. The second consumer proves three things: (1) the standalone EventBus is solving a real, recurring need -- not just B2's specific problem, (2) each new consumer adds to the migration cost when Transport eventually replaces it -- the cost is now 2x what it was on Day 2, growing linearly with each consumer, and (3) B7 builds a convenience wrapper (`EventBridge`) on top of the standalone API, which means the workaround is now a platform that other workspaces will build against. The wrapper is a force multiplier: it makes the standalone API easier to consume, which accelerates consumer accumulation.

**Why not Day 5 or later:** By Day 5, B8 is the third consumer (building on B7's wrapper), and the intervention window has narrowed. Each day after Day 3, the migration cost grows and the political cost of "stopping other workspaces to fix infrastructure" grows with it. On Day 8, the migration is estimated at 2-3 sessions -- large enough to defer. On Day 12, the workaround crosses a language boundary, which converts it from "debt in one codebase" to "architectural precedent across the platform." Every day after Day 3 is a day the intervention would have been harder and more expensive.

**Step 3 -- Two valid actions at Day 3:**

**Action A: Fast-complete the proper implementation.** Reprioritize the Transport refactor (B0) to complete before any third consumer arrives. On Day 3, there are 2 consumers (B2 and B7). Migrating 2 consumers costs roughly: update 2 workspaces, migrate 5 event types, rewrite 13 tests, retire B7's EventBridge wrapper before anyone else adopts it. Estimated cost: 1 session, possibly less. This caps the total migration at a known, bounded cost. B8 and A1 (Days 5-6) would build against the Transport-wrapped EventBus from the start, avoiding migration entirely. The B0 refactor work is not wasted -- it was always needed. The only change is priority: B0 moves from "complete eventually" to "complete now, before the third consumer."

**Action B: Promote the workaround to production-grade.** Accept that the standalone EventBus is the architecture. Invest in making it production-grade: add tests (the standalone has none as of Day 1), add documentation, define a stable API contract, add delivery guarantees that consumers can rely on, and update the architectural documentation to reflect that EventBus is standalone, not Transport-wrapped. This eliminates the migration cost entirely but permanently commits to a different architecture than originally intended. The Transport-wrapped EventBus is either abandoned or redesigned to wrap the standalone implementation rather than replace it.

**Recommendation: Action A (fast-complete).** On Day 3, the migration cost is small (2 consumers, 13 tests). The Transport design was chosen for good reasons (delivery guarantees, backpressure, observability) that the standalone lacks. Promoting the standalone means accepting the loss of those properties or rebuilding them into the standalone -- which converges on reimplementing Transport under a different name. Fast-completing B0 on Day 3 is cheaper than either migrating 4+ consumers later or rebuilding Transport-equivalent features into the standalone.

If B0 cannot be fast-completed (the prerequisite refactor is genuinely blocked on an external dependency), then Action B is correct. But the decision must be made explicitly on Day 3, not deferred until the cost forces it.

**Step 4 -- Cost chain from the missed intervention:**

| Day    | Session | New cost added                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Cumulative migration cost                                                     |
| ------ | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Day 3  | 15      | B7 becomes second consumer. Adds 2 event types, 5 tests, and the EventBridge wrapper. The wrapper is a cost multiplier: it makes the standalone API easier to consume, accelerating future adoption.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 2 workspaces, 5 event types, 13 tests, 1 wrapper to retire                    |
| Day 5  | 18      | B8 becomes third consumer. Builds on B7's EventBridge wrapper -- meaning B8 has a transitive dependency on the standalone API through the wrapper. Adds 4 event types, 11 tests.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | 3 workspaces, 9 event types, 24 tests, 1 wrapper with 1 downstream consumer   |
| Day 6  | 19      | A1 becomes fourth consumer. Also uses B7's EventBridge wrapper. Adds 2 event types, 6 tests.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 4 workspaces, 11 event types, 30 tests, 1 wrapper with 2 downstream consumers |
| Day 8  | 22      | Transport refactor completes but migration is estimated at 2-3 sessions. Decision to defer is made. This is the moment the workaround becomes politically permanent: the "temporary" label remains, but no one is authorized to spend 2-3 sessions on migration.                                                                                                                                                                                                                                                                                                                                                                                                                                        | Same as Day 6, plus the political cost of re-opening the decision later       |
| Day 12 | 26      | **Qualitative cost change.** The Rust team replicates the standalone architecture in kailash-rs. This is not another consumer -- it is a cross-language architectural commitment. The cost changes qualitatively because: (a) the Rust implementation will develop its own consumers, creating a parallel accumulation chain, (b) migrating Rust to Transport-wrapped EventBus requires a second migration effort in a different language with different constraints, (c) the standalone architecture is now documented-by-implementation as "how this platform does eventing" -- any future language port (Ruby, Go) will copy the same pattern. The workaround has become the reference architecture. |

**Total cost at Day 12 that would have been avoided by intervening on Day 3:** 3 workspace migrations (B8, A1, and the Rust implementation would never have built against standalone), 8 event types that would have been defined against Transport from the start, 17 tests that would not need rewriting, the EventBridge wrapper would never have been created (Transport has its own bridging), and the Rust team would have implemented Transport-wrapped EventBus as the reference architecture. The Rust replication is the most expensive single cost because it is unbounded: it creates a second accumulation chain in a second language, with its own consumers, its own wrappers, and its own political inertia against migration.

## Scoring

| Criterion                                                                                                                                                                                             | Points |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Day 3 (Session 15) identified as the intervention point -- not Day 2 (too early, first consumer is expected) and not Day 5 or later (too late, preventable instances already accumulated)             | 2      |
| Explanation addresses why the second consumer specifically is the signal: it proves recurring need, starts linear cost growth, and in this case introduces a force-multiplying wrapper                | 2      |
| Both valid actions named concretely: fast-complete Transport with specific migration scope at Day 3 (2 workspaces, 13 tests), OR promote standalone to production-grade with specific investment list | 2      |
| Recommendation includes justification tied to this scenario's specifics (Transport's delivery guarantees, small Day-3 migration scope), not just a generic preference                                 | 1      |
| Cost chain traces each day individually with the specific new cost added, not just a summary ("it got more expensive")                                                                                | 1      |
| Day 12 Rust replication identified as a qualitative cost change (cross-language architectural commitment), not just another consumer in the linear cost model                                         | 2      |
| **Total**                                                                                                                                                                                             | **10** |

## Extensions

1. **Contested intervention.** On Day 3, the B7 workspace owner pushes back: "The Transport refactor is going to take 3 more sessions. My workspace is blocked. The standalone EventBus works fine. Why are you making me wait?" The practitioner must respond with a concrete argument that addresses the owner's legitimate concern (their workspace IS blocked) while explaining why the second-consumer signal changes the calculus. Score on whether the practitioner proposes a specific unblocking action (e.g., fast-complete a minimal Transport subset that covers B7's needs in 1 session, not the full 3-session refactor) rather than just asserting "the rules say intervene at the second consumer."

2. **Promotion decision.** Change the scenario: the Transport refactor is blocked on an external API that will not be available for 6 weeks. Fast-completion is not possible. The practitioner must execute Action B (promote the standalone to production-grade). The deliverable is a concrete promotion plan: what tests to add, what API contract to stabilize, what documentation to write, what the architectural decision record says, and how to handle the eventual arrival of the external API (does Transport become a wrapper around the now-production standalone, or does it replace it with a second migration?). Score on whether the practitioner treats promotion as a deliberate architectural decision with explicit tradeoffs, not as a passive acceptance of the status quo.
