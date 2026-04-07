---
drill_id: DR-SC-B-001
source_atom: SC-B-001
name: "Read-then-merge sync semantics — drill"
craft_area: institutional-memory
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Take a small mock pair of repos (source A, target B). Seed B with three files that disagree with A: one drift (B has newer content), one numbering collision (different file at the same canonical slot), one true duplicate. The drill: write the merge plan as a per-file diff with three columns — `source`, `target`, `decision (global/variant/skip/halt)` — and refuse to produce a single bulk command. Score the drill on whether the practitioner halted at the numbering collision or tried to "fix" it inline.
