---
drill_id: DR-SC-P-006
source_atom: SC-P-006
name: "Decide whether new functionality is a module inside an existing package or a separate package — drill"
craft_area: intervene-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with three proposed features, each with a brief describing the feature, its expected consumers, its dependency footprint, and its release cadence. One should clearly be a separate package (different consumers, heavy unique deps, independent release cycle). One should clearly be an extension of an existing package (same consumers, no new deps, shared release cycle). One should be ambiguous (overlapping consumers, moderate deps, release cycle unclear). The practitioner must:

1. Apply the three-question test to each feature.
2. Write a one-paragraph rationale for each decision.
3. For the ambiguous case, state which direction they chose AND what would flip the decision.
4. Identify whether the ambiguous case has low or high extraction cost if they chose "start inside."

Score on whether the clear cases are correct, whether the ambiguous case's rationale addresses all three questions, and whether the extraction-cost assessment is grounded in specifics (API surface area, internal coupling depth) rather than hand-waving.
