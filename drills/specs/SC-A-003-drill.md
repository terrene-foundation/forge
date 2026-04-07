---
drill_id: DR-SC-A-003
source_atom: SC-A-003
name: "Defense-in-depth codification across multiple artifact locations — drill"
craft_area: institutional-memory, attend-how
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a single new rule to codify (e.g., "all database queries must use parameterized statements, never string interpolation"). The drill: produce a codification plan that names at least four distinct artifact landing locations, explains what each artifact type contributes that the others cannot, and identifies the failure mode if any one is missing. Score on: (a) at least four distinct artifact types named, (b) each landing has a different enforcement surface (e.g., rule = constraint text, hook = deterministic block, agent = contextual review, skill = teaching material), (c) the failure mode for each missing landing is specific, not generic ("if the hook is missing, the rule is probabilistic" — specific; "things might go wrong" — generic).
