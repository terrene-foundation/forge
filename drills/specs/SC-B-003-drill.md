---
drill_id: DR-SC-B-003
source_atom: SC-B-003
name: "Variant classification — single-sentence test rule — drill"
craft_area: institutional-memory
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Hand the practitioner ten unfamiliar artifact names (mix of file paths from real repos — five rules, three agents, two skills) with no hint about content. For each, they must:

1. Predict global / variant-py / variant-rs / skip in under 30 seconds, citing the one-sentence test.
2. Open the file and verify or revise.
3. If revised, write one sentence on why their first instinct was wrong — was it a content surprise, or did they apply a secondary test (size, author, sync convenience) before the primary one?

Score on the _order_ of reasoning, not just the final answer. Practitioners who reach the right answer via "this looks like a Rust file" are failing the drill even when correct, because they will misclassify the next ambiguous case. The Gate 1 rs review (entry 0048) is a ready-made answer key — 43 directories with disposition + reason already recorded.
