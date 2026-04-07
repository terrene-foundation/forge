---
drill_id: DR-SC-A-008
source_atom: SC-A-008
name: "Rule classification — critical vs advisory — drill"
craft_area: intervene-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner 10 rules from a real CC artifact set (use the loom/kailash-py rule set). For each rule, the drill: (a) state the consequence of a single violation (irreversible or correctable?), (b) classify as critical or advisory, (c) for each critical rule, name the enforcement mechanism (hook, pre-commit, CI check, or session-start injection), and (d) estimate the token savings if all advisory rules are moved to on-demand loading. Score on whether the classification matches the ablation data from 0049 — if the practitioner classifies a model-native rule as critical, they are over-enforcing; if they classify a non-model-native rule as advisory, they are under-enforcing.
