---
drill_id: DR-SC-P-017
source_atom: SC-P-017
name: "Detect false-positive learning pipelines that report healthy metrics but produce noise — drill"
craft_area: attend-how, attend-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a learning pipeline with: 5,000 logged observations, a processing script that generates instinct files, and a `/codify` command that reads from those instinct files. The observation logs contain 4,800 session-start/stop events and 200 genuine decision-point events. The processing script outputs 6 instinct files, all with confidence 0.9 and near-identical descriptions. The practitioner must: (1) identify the input-quality problem (99% noise), (2) identify the processing-quality problem (hardcoded confidence, duplicate output), (3) identify the promotion-quality problem (empty evolved directory), and (4) propose a fix that addresses all three stages. The drill fails if the practitioner proposes only "capture better observations" without checking the processing and promotion stages.
