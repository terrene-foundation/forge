---
drill_id: DR-SC-B-011
source_atom: SC-B-011
name: "Version tracking cascade across the repo chain — drill"
craft_area: institutional-memory
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Set up a three-repo chain: source S (version 2.3.0 in pyproject.toml, VERSION says 1.5), template T (version 1.4 in VERSION, dependency pin says kailash>=2.0.0), downstream D (version 1.3 in VERSION). Run through the three trigger points: (1) session start on D — what mismatch should surface? (2) /codify on S — what should the proposal stamp? (3) /sync from S through T — what should Gate 2 update in T's pyproject.toml? Score: does the practitioner identify all three version tracks (COC artifact, SDK release, dependency pin) and the specific mismatch at each gate?
