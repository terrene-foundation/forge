---
drill_id: DR-SC-B-004
source_atom: SC-B-004
name: "Specialist delegation as required first move (framework work) — drill"
craft_area: intervene-when, institutional-memory
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Pick a feature plan from the corpus that already has a corrections list attached (entry 0017 is ideal because it lists C1/C2/C3/H1-H5 explicitly). Strip the corrections out and hand the practitioner the original plan + a one-line context ("this touches PACT spec conformance — TrustStore, DelegationRecord, GovernanceEngine"). They have 20 minutes to:

1. Identify which API surfaces the plan depends on.
2. Open the actual SDK source for each one in the corpus cache.
3. Produce their own corrections list.
4. Compare against the recorded list (the entry's findings section is the answer key).

Score on (a) coverage — did they catch all 8 findings, (b) discrimination — did they raise the criticals as criticals, and (c) source-citation — did each correction reference the actual source file/line they read, or did they hallucinate from API shape memory?
