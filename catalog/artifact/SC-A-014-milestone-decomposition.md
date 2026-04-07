---
atom_id: SC-A-014
name: Decompose milestones with hard-blocking approval gates
craft_layer: artifact
destination: both
craft_areas: [intervene-how, institutional-memory]
applications: [COC, COR, COE]
spec_lineage:
  - CO §26 — Layer 4 — Instructions
  - CO §27 — Structured Workflow
  - CO §28 — Approval Gate
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash/journal/0016-DECISION-pact-sprint-s12-todo-structure.md
    polarity: POSITIVE
    quote: "Organized the 4 PACT spec-conformance issues (#199-#202) into 14 todos across 5 milestones, ordered by dependency graph: [...] M1, M2, M4 can execute in parallel. M3 depends on M2 only because the interim envelope computation for vacant roles requires those roles to exist in the compiled org (TODO-05)."
  - path: loom/kailash-rs/workspaces/nexus-parity/journal/0002-DECISION-granular-todo-decomposition.md
    polarity: POSITIVE
    quote: "Replaced TSG-602 with 10 granular todos (NP-001 through NP-010) organized into 6 milestones: [...] NP-009 (verification) -- depends on all above\nNP-010 (bindings) -- depends on NP-001 + NP-009"
  - path: loom/kz-engage/workspaces/herald/journal/0007-DECISION-milestone-ordering.md
    polarity: POSITIVE
    quote: "8 milestones (M0-M7) with 35 todos, ordered by dependency: [...]\n- M4 (integration) is the convergence point — wires everything together\n- M6 and M7 run in parallel after integration works"
practice_modality: drill
status: verified
---

## The move

When decomposing a multi-session feature into milestones, every milestone must (a) name a single approval gate, (b) name the evidence required to pass that gate, and (c) prevent phase-skipping by making the gate hard-blocking on the next milestone's start. The dependency graph between milestones is explicit: which milestones can run in parallel, which must wait, and why. Milestones without gates are wishlists. Gates without evidence requirements are theatre. Dependencies without explicit justification collapse under parallel execution pressure.

## When it fires

When a feature spans more than one implementation session, or when the todo list exceeds 8-10 items. The trigger is complexity that exceeds single-pass execution: either the work has internal dependencies that force ordering, or the work has independent streams that should run in parallel but need a convergence point. A secondary trigger is a single high-level todo that covers an entire workspace — the 0002 entry's "TSG-602" is the canonical example of a todo that needs decomposition.

## What it serves

CO §26 (Layer 4 — Instructions) requires "structured workflows with approval gates between phases, evidence requirements for completion claims, and mandatory delegation rules." CO §27 (Structured Workflow) requires "at least two phases and at least one approval gate" and that the workflow "SHOULD prevent phase-skipping." CO §28 (Approval Gate) defines a gate as "a structured point where human judgment is exercised before the workflow proceeds." Milestone decomposition is the move that instantiates all three requirements at the feature level: the milestones are the phases, the gates are the approval points, and the dependency graph is the phase-skipping prevention mechanism.

## Evidence walkthrough

- **kailash-py 0016** (POSITIVE) — Four PACT (spec) conformance issues were decomposed into 14 todos across 5 milestones. The dependency graph was explicit: M1 (write-time tightening), M2 (compilation + bridges), and M4 (EATP record emission) could execute in parallel; M3 (vacancy interim) depended on M2 because vacant-head envelope computation required compiled org data from TODO-05. M5 (red team + close) was the convergence gate depending on all four prior milestones. The entry also shows the discipline of ordering by dependency rather than severity — CRITICAL issues (M4) ran in parallel with HIGH issues (M2) rather than being forced first.
- **kailash-rs 0002** (POSITIVE) — A single high-level todo (TSG-602) was replaced with 10 granular todos in 6 milestones. The decomposition made the dependency chain inspectable: NP-001 (trait + registry) was the root; NP-002/003/004 depended on it; NP-009 (verification) depended on everything; NP-010 (bindings) depended on NP-001 and NP-009. Independent todos (NP-006 EventBus config, NP-007 MCP validation) were explicitly marked as parallelisable. The entry also shows the gate evidence pattern: NP-009 is a full-test-suite verification milestone with explicit acceptance criteria, not a "check it works" placeholder.
- **kz-engage herald 0007** (POSITIVE) — The Herald project was decomposed into 8 milestones with 35 todos, with an explicit parallelisation map: M1 (knowledge), M2 (delegate), M3 (adapter), and M5 (governance) could all progress concurrently after M0 (project setup). M4 (Nexus integration) was identified as the convergence point that wired everything together. The entry explicitly names the parallelisation as "the highest-leverage parallelisation in the project" and maps it to the autonomous execution model (parallel agent sessions). The dependency structure prevents an agent from attempting integration (M4) before the components exist.

## How to drill it

Give the practitioner a feature brief with 12-15 requirements that have non-obvious dependencies (e.g., "implement a governance engine" where vacancy enforcement depends on role compilation, bridge consent depends on org compilation, and EATP emission depends on both). The drill: (a) decompose into milestones with explicit dependency arrows, (b) identify which milestones can run in parallel, (c) for each milestone, name the gate and the evidence required to pass it (not "tests pass" — specific acceptance criteria like "all four vacancy paths enforce the check" or "cross-SDK parity confirmed on all 5 abstractions"), (d) identify the convergence milestone that depends on all others. Score on whether the dependency graph is correct (no missing edges), the parallelisation is maximal (no artificial sequencing), and every gate has concrete evidence requirements rather than "done" or "verified."

## Common failure mode

The practitioner lists milestones in a flat sequence (M1 then M2 then M3) without dependency analysis, forcing serial execution of work that could run in parallel. The second failure mode is gates without evidence: "M1 complete" instead of "M1 passes the three-question test on all 4 spec requirements." The kailash-py 0016 entry shows the correction for both: milestones are ordered by dependency graph (not sequence number), and the convergence gate (M5) has explicit criteria (all tests, red team, PR, issue closure).

## Related atoms

- SC-P-007 (session completion state definition) — completion state is what gates test; this atom covers how to structure milestones with gates, while SC-P-007 covers how to define the completion state that each gate verifies.
- SC-B-006 (multi-round red team convergence) — the convergence gate at the end of a milestone requires a numeric convergence threshold; this atom structures the milestone, SC-B-006 structures the gate.
- SC-B-013 (map cross-workspace dependency surface) — when milestones span workspaces, the dependency mapping from SC-B-013 feeds the milestone decomposition in this atom.
- SC-P-018 (split the long-pole into fast-unblock + parallel-completion) — a technique for restructuring milestones when one is blocking all others; complementary to the decomposition move here.
