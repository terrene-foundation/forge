---
drill_id: DR-SC-P-008
source_atom: SC-P-008
name: "Trace deep dependency chains to find where a single broken link breaks the install path — drill"
craft_area: attend-how
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a `pyproject.toml` for a hypothetical package that depends on 3 libraries, each with loose version pins (`>=X`). Provide a dependency tree showing the transitive closure (8-12 total packages). Embed three fragility points:

1. One package where the minimum pinned version lacks an API that the code uses (the TRL pattern).
2. One package that requires source compilation on one major platform (no binary wheel for ARM Linux, for example).
3. One package maintained by a single contributor with irregular release cadence.

The practitioner must:

1. Trace the full dependency tree and identify all three fragility points.
2. For each fragility, state which of the three questions it fails and what the concrete risk is.
3. Propose tightened version pins with a rationale for each bound.
4. Describe what a CI version-matrix test for this dependency set would look like (which combinations to test).

Score on whether all three fragility points are found, whether the version pins are specific enough to prevent the failure but not so tight that they block legitimate upgrades, and whether the CI matrix covers the minimum and maximum of each tightened range.
