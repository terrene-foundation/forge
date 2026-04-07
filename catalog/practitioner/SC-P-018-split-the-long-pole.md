---
atom_id: SC-P-018
name: Split a multi-session blocker into fast-unblock and parallel-completion halves
craft_layer: practitioner
destination: forge
craft_areas: [intervene-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §27 — Structured Workflow
  - CO §28 — Approval Gate
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: POSITIVE
    quote: "Split B0 into two deliverables with independent gates:\n- **B0a**: Extract HandlerRegistry + EventBus as standalone modules (1 session). This unblocks WS-2A immediately.\n- **B0b**: Extract HTTPTransport + MCPTransport as Transport implementations (2-3 sessions). This can run in parallel with Phase 2 work that only needs the registry."
practice_modality: drill
status: verified
---

## The move

When a single work item sits on the critical path and blocks multiple downstream items, split it into two deliverables: (a) the minimum subset that unblocks the most downstream work, gated independently, and (b) the remainder, which can proceed in parallel with the now-unblocked downstream work. Do not wait for the entire blocker to complete before unblocking. The split is not a scope reduction — both halves must complete — but a sequencing intervention that collapses the critical path.

## When it fires

A plan or dependency graph shows a single item that (a) is estimated at more than one session, (b) blocks three or more downstream items, and (c) contains a natural seam where a partial deliverable would satisfy some but not all downstream dependencies. The trigger is the combination of duration, fan-out, and separability. If the blocker is atomic (no natural seam), this move does not apply.

## What it serves

CO §27 (Structured Workflow) requires that implementations "define a structured workflow with at least two phases and at least one approval gate." Splitting a blocker into two gated deliverables creates structure where a monolithic task had none: the first deliverable gets its own gate (is the registry extract correct? does it unblock WS-2A?), and the second deliverable gets a separate gate. CO §28 (Approval Gate) is served because each half can now be reviewed independently rather than waiting for the entire scope to be reviewable. The split also enables parallel execution, which CO §27 supports through its phased structure — the phases need not be sequential if their dependencies are satisfied.

## Evidence walkthrough

- **0034-RISK-stack-gaps-redteam-attack** (POSITIVE) — Finding 1 identified B0 (Nexus transport refactor) as a critical-path blocker estimated at 2-3 sessions in the plan but 7 sessions in the detailed analysis, blocking 7+ downstream items. The red team recommended splitting B0 into B0a (HandlerRegistry + EventBus extraction, 1 session, unblocks Phase 2 immediately) and B0b (HTTPTransport + MCPTransport extraction, 2-3 sessions, runs in parallel with Phase 2). This split collapsed the critical path by allowing WS-2A to start after 1 session instead of waiting for 4-6 sessions. The split preserved the full scope (both B0a and B0b must complete) while eliminating the sequencing bottleneck.

## How to drill it

Present the practitioner with a dependency graph containing 12 work items across 3 phases. One Phase 1 item (estimated 4 sessions) blocks 5 Phase 2 items, but only 2 of those 5 actually depend on the full scope — the other 3 only need a subset. The practitioner must: (1) identify the blocker and its fan-out, (2) determine which downstream items depend on which parts of the blocker, (3) propose a split that maximizes the number of items unblocked by the fast half, (4) define independent gates for each half, and (5) redraw the critical path showing the time saved. Score on whether the split is clean (no circular dependency between halves), the gates are meaningful (not rubber-stamps), and the time savings are correctly computed.

## Common failure mode

The practitioner splits the blocker but makes the second half depend on the first half completing, reintroducing the sequential bottleneck. The effective split requires that B0a and B0b are independently completable — B0b can start without B0a finishing, and vice versa. If the practitioner draws B0a → B0b as a sequential chain, the split saves zero time on the critical path and only adds gate overhead. The journal entry explicitly specifies that B0b "can run in parallel with Phase 2 work," meaning it does not depend on B0a's output.

## Related atoms

- SC-P-013 (estimate self-contradiction) — the estimate contradiction that surfaces the blocker's true duration is what motivates the split; without catching the 2-3 vs 7 session disagreement, the split would never be proposed
- SC-A-014 (milestone decomposition) — splitting a blocker restructures the milestone graph; the decomposition move is the structural counterpart that ensures the split halves have proper gates.
- SC-B-013 (map cross-workspace dependency surface) — mapping dependencies is the prerequisite move that reveals which blocker to split
- SC-P-023 (insert gate not check) — the integration gate at the seam between the fast-unblock half and the parallel-completion half validates that the two compose correctly.
- SC-P-024 (fallback permanence detection) — Finding 6 from the same journal entry warns that if the fast-unblock half becomes a permanent workaround, the second half never completes
