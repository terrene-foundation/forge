---
drill_id: DR-SC-A-005
source_atom: SC-A-005
name: "Encapsulation as security boundary in signed structs — drill"
craft_area: attend-how, intervene-how
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a Rust or Python struct that has five fields, three of which are security-sensitive (a signature, a revocation flag, and a scope set). All fields are public. The drill: (a) identify which fields must be private, (b) design the accessor API (read-only getters vs. approved-mutation methods), (c) write one test that demonstrates the bypass if the fields remain public (clone the record, flip the revocation flag, confirm the chain accepts the modified record), and (d) write one test that demonstrates the fix blocks the bypass. Score on: all three security-sensitive fields identified, the approved-mutation path is a constructor or builder (not a setter), and the bypass test is a real executable scenario, not a hypothetical.
