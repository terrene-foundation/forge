---
atom_id: SC-P-007
name: Define session completion state before starting, not after convergence is claimed
craft_layer: practitioner
destination: forge
craft_areas: [attend-how, intervene-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §28 — Approval Gate
  - CO §29 — Evidence-Based Completion
journal_evidence:
  - path: loom/journal/0012-DECISION-session-completion-state.md
    polarity: POSITIVE
    quote: "All findings fixed. No blocking issues."
  - path: loom/journal/0031-DECISION-stack-gaps-dependency-sequencing.md
    polarity: POSITIVE
    quote: "The problem: ~35 items across 5 domains (DataFlow, Nexus, kailash-ml, MCP, kailash-rs) will be implemented by parallel autonomous agent teams. Without a dependency map, teams will block each other or build on interfaces that do not yet exist."
practice_modality: drill
status: verified
---

## The move

Before the first implementation action in a session, write down what "done" means as a concrete checklist: what code ships (files, functions, tests), what tests must pass (by name or pattern), what documentation updates are required, and what is explicitly NOT in scope for this session. The checklist is the contract. When the session claims completion, evaluate the claim against the checklist -- not against a feeling of convergence.

## When it fires

Two trigger conditions:

1. A session begins with a brief, a todo list, or a set of milestones. The practitioner is about to start executing. This is the moment to convert the brief into a testable completion definition.
2. A session claims completion ("all findings fixed," "implementation done," "red team converged") but the claim is not anchored to a pre-defined checklist. The absence of a checklist is itself the signal that the move was skipped.

## What it serves

CO §28 (Approval Gate) defines a structured point where human judgment is exercised before the workflow proceeds. A completion-state definition is the artifact that makes the approval gate actionable: the human reviews the checklist, not a narrative summary. Without it, the gate is a rubber stamp. CO §29 (Evidence-Based Completion) requires evidence for completion claims, not just assertions. A pre-defined completion checklist turns "done" from an assertion into a verifiable claim: each item is either done (evidence exists) or not done (gap named).

## Evidence walkthrough

- **0012-DECISION-session-completion-state** (POSITIVE) -- This entry is itself a worked example of the move. It records the completion state of a multi-milestone session: M2 complete (6/6), M3 complete (2/2 with one invalid item deleted), M4 partial (2/7 with specifics on what remains), M5 substantial (7/8 with only /sync remaining). The entry also records what is running (4 background agents on a specific task) and what the next session should prioritize. The format -- milestone table with scores, correction applied, running tasks, next priorities -- is the template for a completion-state definition. The entry demonstrates that partial completion is acceptable when the gaps are named and quantified.
- **0031-DECISION-stack-gaps-dependency-sequencing** (POSITIVE) -- This entry defines session completion at a higher level: 17 workspaces across 5 phases, with explicit dependency ordering and two named keystones (B0 and C1) that must start on day 1. The critical path is traced 8 items deep. This is completion-state definition applied to a multi-session programme, not just a single session. The entry shows the move scaling: "15 of 35 items (43%) can start immediately with zero dependencies" is a testable claim. "B0 is a single point of failure -- if it slips, 7 items are blocked" is a named risk with a quantified blast radius. The negative consequence section names three specific ways the plan could fail, which is itself part of the completion definition -- if those failures occur, the session did not complete as planned.

## How to drill it

Give the practitioner a brief describing a session with 5-8 deliverables across 2-3 milestones. Some deliverables have dependencies on each other; one has an external dependency that may not be available. The practitioner must:

1. Write a completion-state checklist BEFORE looking at the code. Each item must be verifiable (not "implement auth" but "auth middleware returns 401 for expired tokens, test_auth_expired passes").
2. Identify which items can be done in parallel and which are sequential.
3. Name what is explicitly NOT in scope, with a one-sentence reason for each exclusion.
4. After a simulated "session end" (presented as a narrative of what was accomplished), evaluate the narrative against the checklist and score: how many items are done, how many are partial, how many are gaps.

Score on the specificity of the checklist items (vague items like "tests pass" score zero; specific items like "test_pypi_publish_single_tag passes" score full), the accuracy of the dependency graph, and the quality of the post-session evaluation (did the practitioner catch the gaps in the narrative, or did they rubber-stamp "done"?).

## Common failure mode

The practitioner starts implementing immediately, makes good progress, and declares the session "done" based on the feeling that the major work is complete. When reviewed, 2-3 items were silently dropped: a test that was never written, a doc that was never updated, an edge case that was acknowledged in analysis but never implemented. The entry 0012 shows the corrective: the session explicitly records M4 as PARTIAL (2/7) rather than omitting it. Without a pre-defined checklist, the practitioner would have reported "M2, M3, M5 complete" and never mentioned M4.

## Related atoms

- SC-P-004 (per-file disposition labelling) -- disposition labelling is a specific instance of completion-state definition applied to codebase files rather than session milestones.
- SC-A-014 (milestone decomposition) -- milestones with gates are the structural counterpart; completion state defines what "done" means, milestone decomposition defines the gate checkpoints.
- SC-B-006 (multi-round red team to numeric convergence) -- red team convergence is a completion claim that must be evaluated against pre-defined numeric criteria, not narrative assertions.
- SC-P-019 (cold-start continuity) -- completion-state definition is the precondition for useful session notes; without it, the notes have nothing actionable to convey at the next session start.
