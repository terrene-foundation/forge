---
drill_id: DR-SC-P-018
source_atom: SC-P-018
name: "Split a multi-session blocker into fast-unblock and parallel-completion halves — drill"
craft_area: intervene-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Present the practitioner with a dependency graph containing 12 work items across 3 phases. One Phase 1 item (estimated 4 sessions) blocks 5 Phase 2 items, but only 2 of those 5 actually depend on the full scope — the other 3 only need a subset. The practitioner must: (1) identify the blocker and its fan-out, (2) determine which downstream items depend on which parts of the blocker, (3) propose a split that maximizes the number of items unblocked by the fast half, (4) define independent gates for each half, and (5) redraw the critical path showing the time saved. Score on whether the split is clean (no circular dependency between halves), the gates are meaningful (not rubber-stamps), and the time savings are correctly computed.
