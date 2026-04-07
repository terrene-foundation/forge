---
drill_id: DR-SC-B-005
source_atom: SC-B-005
name: "Spec-to-code conformance audit — drill"
craft_area: attend-when, institutional-memory
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Take a module with a published spec (PACT (spec), EATP, or a smaller internal spec). Extract every MUST/SHOULD/MUST NOT into a numbered checklist. For each item, find the code location where enforcement should occur. Write the gap entry (or "confirmed: enforced at file:line") for each. Score the drill on coverage completeness — did the practitioner check all code paths, or only the obvious one? The kailash-rs 0001 vacancy case (two paths, one enforced, one not) is the canonical trap.
