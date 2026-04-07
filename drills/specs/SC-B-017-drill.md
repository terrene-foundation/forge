---
drill_id: DR-SC-B-017
source_atom: SC-B-017
name: "Assign verification-gradient zone explicitly before delegating to an agent — drill"
craft_area: intervene-when, institutional-memory
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a scenario: three agents are about to work in parallel on (1) updating documentation, (2) refactoring a public API's internal implementation, and (3) adding a new CI pipeline stage that gates releases. Ask the practitioner to assign a verification-gradient zone to each agent and justify the assignment. Score: does the practitioner correctly distinguish that documentation is auto-approved or flagged (low risk, easily reversible), the API refactoring is flagged or held (behaviour could change even if the interface is stable), and the CI pipeline change is held (it gates all future releases and cannot be easily reversed)? Does the practitioner name a different zone for each agent rather than defaulting all three to the same zone?
