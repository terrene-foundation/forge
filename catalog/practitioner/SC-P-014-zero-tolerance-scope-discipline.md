---
atom_id: SC-P-014
name: Distinguish pre-existing failures from pre-existing gaps when applying zero-tolerance Rule 1
craft_layer: practitioner
destination: forge
craft_areas: [intervene-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §29 — Evidence-Based Completion
  - CO §21 — Critical Rules
journal_evidence:
  - path: loom/kailash-rs/workspaces/sync-express/journal/0002-DISCOVERY-binding-parity-gaps.md
    polarity: POSITIVE
    quote: "These are pre-existing gaps, not introduced by #115."
practice_modality: case
status: verified
---

## The move

When zero-tolerance Rule 1 ("if you found it, you own it") fires during a session, classify the discovered issue as either a FAILURE (something that was supposed to work but does not — a broken test, a regression, a violated invariant) or a GAP (something that was never built — missing feature parity, unimplemented binding, absent capability). Fix failures in the current session. Track gaps as separate issues with explicit scope boundaries. The rule's intent is to prevent failure ratcheting — each session leaving more broken things than it found — not to mandate that every session expands scope to build features that were never started.

## When it fires

During analysis or implementation, the practitioner discovers a pre-existing problem that is not part of the current session's scope. The zero-tolerance instinct says "fix it now." The scope-discipline instinct says "this is a separate workstream." The conflict between these two instincts is the trigger for this move: the practitioner must make an explicit classification before acting on either instinct.

## What it serves

CO §29 (Evidence-Based Completion) requires "verifiable proof" for completion claims, not just assertions. The failure/gap distinction is an evidence question: a failure has evidence that something was attempted and broke (a failing test, a regression from a prior working state, a violated spec requirement). A gap has evidence that something was never attempted (no tests exist, no implementation exists, no prior working state exists). These are different categories of evidence and require different responses. Conflating them produces either infinite scope expansion (treating every gap as an urgent failure) or failure normalization (treating every failure as a deferrable gap). CO §21 (Critical Rules) establishes that some rules require hard enforcement with zero deviation. Zero-tolerance Rule 1 is itself a critical rule — but its enforcement boundary must be precisely defined to remain enforceable. A rule that means "fix everything you encounter regardless of scope" is unenforceable in practice and degrades into being ignored.

## Evidence walkthrough

- **sync-express 0002-DISCOVERY-binding-parity-gaps** (POSITIVE) — During analysis of issue #115 (sync Express API), the requirements analyst discovered significant parity gaps across language bindings: Ruby missing five CRUD operations, Node.js missing all CRUD, C ABI missing CRUD and aggregation, WASM having no DataFlow bindings at all. The entry explicitly classifies these as "pre-existing gaps, not introduced by #115" and recommends they "be tracked as separate issues." The session scope stayed focused on the Rust `DataFlowExpressSync` struct, C ABI improvement, and Ruby sync wrapper. This is the rule being correctly interpreted: the parity gaps are not failures (nothing broke — these bindings never had the features), so they do not trigger the must-fix-now obligation. They are gaps that need their own analysis, their own scoping, and their own implementation sessions. Filing them as separate issues preserves traceability without blowing up the current session's scope.

## How to drill it

Present the practitioner with a list of 6 issues discovered during a sync-focused session. Three are failures, three are gaps. The practitioner must classify each and state the action:

1. A Ruby binding test for `upsert` that exists but fails with a `NotImplementedError` at runtime. (FAILURE — the test asserts behavior that was promised; fix now.)
2. Node.js has no CRUD bindings at all — no tests, no implementation, no prior art. (GAP — never built; file as separate issue.)
3. A C ABI function `dataflow_query` that segfaults on NULL input. (FAILURE — the function exists and is broken; fix now.)
4. WASM has no DataFlow bindings and no tracking issue. (GAP — never started; file as separate issue.)
5. The Ruby binding's `aggregate` function returns incorrect results when given an empty collection. (FAILURE — the function exists and returns wrong results; fix now.)
6. Go bindings follow the C ABI but lack a high-level wrapper that other Go consumers expect. (GAP — the wrapper was never built; file as separate issue.)

Score on classification accuracy and on whether the practitioner's action statement matches the classification: failures get "fix in this session" with a concrete next step; gaps get "file as issue #NNN with scope description" without attempting implementation.

## Common failure mode

Two symmetric failure modes:

1. **Scope explosion**: The practitioner treats every gap as a failure and attempts to build all missing bindings in the current sync session. The session runs out of budget, the original sync task is incomplete, and the gap work is half-done — worse than not starting it. This is zero-tolerance Rule 1 interpreted as "fix everything" rather than "fix every failure."

2. **Failure normalization**: The practitioner treats every failure as a gap and files them all as separate issues. The segfaulting C ABI function and the broken Ruby test survive another session, accumulating into the failure ratchet that Rule 1 was designed to prevent. This is scope discipline interpreted as "defer everything uncomfortable."

The move is the middle path: failures are non-negotiable (fix now), gaps are legitimate separate work (track explicitly).

## Related atoms

- SC-P-004 (per-file disposition labelling) — the failure/gap classification is a per-item disposition label applied to discovered issues, not a batch decision.
- SC-B-005 (spec-to-code conformance audit) — a conformance audit may surface both failures (spec says MUST, code violates) and gaps (spec says MUST, code does not attempt); the same classification applies.
- SC-B-009 (cross-SDK upstream brokerage) — when a failure traces back to an SDK defect, zero-tolerance Rule 4 requires filing upstream rather than working around it locally.
- SC-P-024 (fallback permanence detection) — a temporary workaround that accumulates consumers becomes a zero-tolerance violation of the intended architecture; the permanence signal triggers the same failure-not-gap classification.
