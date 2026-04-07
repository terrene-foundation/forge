---
drill_id: DR-SC-B-010
source_atom: SC-B-010
name: "Cross-repo divergence audit with numbered drift-gap list — drill"
craft_area: attend-how, institutional-memory
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Take two mock repos (upstream U, downstream D) with 20 files in scope. Seed D with five divergences: one orphan (file on disk, not in manifest), one stale entry (in manifest, not on disk), one intentional variant (language-specific content), one unintentional drift (older version of a global file), and one true duplicate. Produce a numbered drift-gap list with columns: number, filename, classification (orphan/stale/variant/drift/aligned), one-line description. Score: did the practitioner correctly classify all five, and did they halt before proposing a sync for the unintentional drift?
