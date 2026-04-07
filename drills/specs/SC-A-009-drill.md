---
drill_id: DR-SC-A-009
source_atom: SC-A-009
name: "Progressive disclosure architecture for CC artifacts — drill"
craft_area: institutional-memory
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner a flat CC artifact set (15-20 rules, 5-10 agents, all loaded globally). The drill: (a) classify each artifact into one of the four levels, (b) add `paths:` frontmatter to every topic-level rule so it only loads when relevant, (c) extract any agent over 400 lines into an agent stub (index level) plus a skill file (topic level), (d) audit CLAUDE.md for restated rules and remove them, and (e) calculate the before/after always-loaded token count. The 0003 audit findings are the before state; the 0018 convergence is the after state. Score on whether the practitioner achieves a measurable token reduction without removing any institutional knowledge.
