---
drill_id: DR-SC-A-011
source_atom: SC-A-011
name: "Framework-first check before building new functionality — drill"
craft_area: intervene-when, attend-how
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a brief describing a new feature ("add local model serving to the alignment framework"). Before they write any code, they must: (a) grep the existing codebase for primitives that already handle the core functionality, (b) produce a one-paragraph assessment of what exists vs what is genuinely missing, and (c) write a revised scope statement that composes with existing primitives instead of rebuilding. Score on whether the practitioner's revised scope is smaller than the original brief. The kailash-align 0005 case is the canonical example: the brief implied adapter creation, but the check revealed existing adapters, and the scope shrank from "build serving infrastructure" to "build a 200-line convenience wrapper."
