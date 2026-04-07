---
atom_id: SC-P-023
name: Insert a gating workspace at integration seams rather than adding assertions to existing tests
craft_layer: practitioner
destination: forge
craft_areas: [intervene-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §27 — Structured Workflow
  - CO §28 — Approval Gate
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: NEGATIVE
    quote: "Each workspace passes its own tests. But integration seams -- the exact points where domains meet -- are never tested together until Phase 5 Rust alignment discovers them. By then, the Python API surface has been committed and Rust is implementing against a contract that was never validated end-to-end."
practice_modality: drill
status: verified
---

## The move

When integration between two domains is implicit — each domain's tests pass in isolation but the seam between them is untested — add a gating workspace or phase that exercises the seam. Do not add an assertion to an existing test within one domain's workspace. A gate is structural: it appears in the workflow, blocks downstream work, and is visible to everyone reviewing the plan. An assertion is ephemeral: it lives inside one domain's test suite, is invisible to the other domain, and can be deleted without anyone noticing.

## When it fires

A plan or dependency graph shows N independent workspaces whose outputs must compose at a later phase. Each workspace has its own test suite that validates its domain in isolation. No workspace owns the integration points between domains. The trigger is the gap between "all workspaces pass" and "the composed system works" — the gap that only becomes visible when a downstream consumer (e.g., a Rust implementation, a deployment, a user) tries to use the composed output and discovers that the seams were never validated.

## What it serves

CO §27 (Structured Workflow) requires "at least two phases and at least one approval gate." A plan with N parallel workspaces followed by a downstream phase has an implicit integration seam between the parallel work and the downstream phase. Without an explicit integration gate, the workflow structure assumes that parallel completion implies integrated correctness — which is false when the domains interact. The integration gate makes the seam explicit and blocks downstream work until it is validated. CO §28 (Approval Gate) defines "a structured point where human judgment is exercised before the workflow proceeds." The integration gate is the approval point where the human verifies that the composed system works, not just that each component works.

## Evidence walkthrough

- **0034-RISK-stack-gaps-redteam-attack** (NEGATIVE) — Finding 2 identified that 17 workspaces across 5 phases produced 35 items with 7 cross-domain integration points (EventBus semantics, DataFlow-Nexus events, kailash-ml storage/serving/agents, MCP introspection, Polars interop). Each integration point was verified only within its own workspace. No workspace ran end-to-end validation across domains. The red team recommended inserting WS-4.5 (Integration Validation) as a gate: a reference application exercising all 7 integration points, with Phase 5 (Rust alignment) blocked until WS-4.5 passes. Without this gate, the Rust team would arrive in Phase 5 implementing against Python APIs that were individually tested but never validated together — discovering seam failures when the cost of fixing them (changing committed Python APIs) is highest.

## How to drill it

Present the practitioner with 4 workspaces: WS-A produces a data model, WS-B produces an event system, WS-C produces an API layer, and WS-D (downstream) builds a client against all three. Each of WS-A, WS-B, WS-C has passing tests. The practitioner is asked: "Is the plan ready for WS-D?" The correct answer is no: there is no integration gate that verifies A's data model is compatible with B's event payloads, B's events are routable through C's API, and C's API returns data in the shape D expects. The practitioner must: (1) identify the missing integration seams, (2) propose a gating workspace (not assertions within A, B, or C), (3) define what the gate tests (a reference app or integration test suite exercising all seams), and (4) specify that WS-D is blocked until the gate passes. Score on whether the practitioner adds a structural gate rather than sprinkling assertions into existing workspaces.

## Common failure mode

The practitioner adds integration assertions to WS-C (the API layer) because "C is closest to the consumer." This makes C responsible for validating A and B's contracts — a responsibility that C's workspace was not scoped for and that C's owner may not have the context to verify. It also means the integration tests are invisible to WS-A and WS-B: if A changes its data model, C's tests break but A's team does not know until C runs. A dedicated integration gate owns the seam explicitly, is visible in the plan, and blocks downstream work structurally rather than hiding inside one domain's test suite.

## Related atoms

- SC-P-018 (split the long pole) — the integration gate is the structural complement to splitting blockers: splitting creates parallel work, the gate validates that the parallel outputs compose
- SC-A-014 (milestone decomposition) — gating workspaces are milestones in the decomposition graph; the gate's completion criteria feed the milestone's pass/fail decision.
- SC-B-013 (map cross-workspace dependency surface) — dependency mapping reveals which seams need gating; this atom teaches the structural intervention once the seams are identified
- SC-P-013 (estimate self-contradiction) — the same journal entry's Finding 1 shows estimate contradictions that compound when integration validation is absent
