---
drill_id: DR-SC-B-013
source_atom: SC-B-013
name: "Map cross-workspace dependency surface before parallelizing — drill"
craft_area: attend-how
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Take three mock workspaces (W1, W2, W3) with a shared codebase of 10 files. W1 modifies files A, B, C. W2 modifies files B, D, E. W3 modifies files C, E, F. W2 also consumes an abstraction W1 produces. Produce a dependency graph: which pairs conflict on shared files? Which have a producer-consumer sequencing constraint? Which can truly run in parallel? Present a concrete execution plan (sequential, parallel, or mixed) with justification for each decision. Score: does the practitioner identify B and E as merge-conflict risks, C as a separate conflict, and the W1-before-W2 sequencing constraint?
