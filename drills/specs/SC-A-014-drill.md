---
drill_id: DR-SC-A-014
source_atom: SC-A-014
name: "Decompose milestones with hard-blocking approval gates — drill"
craft_area: intervene-how, institutional-memory
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a feature brief with 12-15 requirements that have non-obvious dependencies (e.g., "implement a governance engine" where vacancy enforcement depends on role compilation, bridge consent depends on org compilation, and EATP emission depends on both). The drill: (a) decompose into milestones with explicit dependency arrows, (b) identify which milestones can run in parallel, (c) for each milestone, name the gate and the evidence required to pass it (not "tests pass" — specific acceptance criteria like "all four vacancy paths enforce the check" or "cross-SDK parity confirmed on all 5 abstractions"), (d) identify the convergence milestone that depends on all others. Score on whether the dependency graph is correct (no missing edges), the parallelisation is maximal (no artificial sequencing), and every gate has concrete evidence requirements rather than "done" or "verified."
