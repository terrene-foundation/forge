---
drill_id: DR-SC-A-002
source_atom: SC-A-002
name: "Frontend mock data detection — drill"
craft_area: attend-how, intervene-how
practice_modality: build
difficulty: advanced
time_box_minutes: 45
---

## How to drill it

Hand the practitioner a small Next.js/React project with three pages, all of which appear to render real data. One page calls a real API. One page uses `generateHourly*()` as a synthetic data source. One page uses `Math.random()` for the display values that look like a placeholder gauge. Their task:

1. Identify which pages are blocked by the `checkFrontendMockData()` rule and which would pass.
2. For each blocked page, write the regex that catches it and the import statement / variable name that the regex must match against.
3. For each passing page, prove it passes by walking through the network call or props chain.

Score on (a) zero false negatives (every mock pattern caught), (b) regex specificity (no false positives on legitimate `generateUUID()`-style utilities), and (c) correct identification that the cure is hook-layer enforcement, not just review. The 0047 entry's "for discussion" item 1 is where the practitioner should be tested on the false-positive trade-off.
