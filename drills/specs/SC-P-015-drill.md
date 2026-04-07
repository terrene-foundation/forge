---
drill_id: DR-SC-P-015
source_atom: SC-P-015
name: "End complex prompts with "what would invalidate this?" rather than "is this right?" — drill"
craft_area: prompt
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner three agent outputs: a dependency analysis, a risk assessment, and an architecture proposal. For each, they must write two prompts: one confirmation question and one falsification question. Then run both against the same model and compare the responses. Score on: (a) whether the falsification question surfaces a real weakness not mentioned in the confirmation response, (b) whether the falsification question is specific enough to be answerable (not just "what's wrong with this?"), and (c) whether the practitioner can articulate why the falsification framing produced different output. The drill succeeds when the practitioner defaults to falsification prompts without being told to.
