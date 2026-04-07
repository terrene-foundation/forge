---
drill_id: DR-SC-P-001
source_atom: SC-P-001
name: "Identify adjacent-but-disconnected modules as spec conformance gaps — drill"
craft_area: attend-how
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner two module directories from a real codebase (e.g., `trust/pact/` and `trust/chain.py`) plus the relevant spec section that defines their integration contract. The practitioner must:

1. Count the cross-module import lines (target: exact number, not "a few").
2. List every normative "MUST" in the spec that requires Module A to call Module B or vice versa.
3. For each "MUST," produce a grep result showing whether the call exists in the code.
4. Write a one-paragraph finding for each missing integration, naming the spec section, the expected call site, and the consequence of the gap.

Score on completeness of the "MUST" enumeration, not on whether the practitioner found "a gap." Finding one gap is easy; finding all four (as in the kailash-py case) requires systematic spec-to-code tracing.
