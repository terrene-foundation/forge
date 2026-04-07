---
drill_id: DR-SC-B-012
source_atom: SC-B-012
name: "Automate manifest-to-filesystem parity check and halt on staleness — drill"
craft_area: institutional-memory
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Create a mock sync-manifest.yaml with 15 entries (10 global, 3 variant:py, 2 variant:rs). Create a filesystem with 16 files (the 15 plus one orphan variant not in the manifest) and remove one file that the manifest references (creating a stale entry). Write a bash or Python script that produces three lists: orphans, stale entries, parity gaps. The script must exit non-zero if orphans or stale entries are found. Score: does the script correctly identify both the orphan and the stale entry? Does it correctly report but not fail on parity gaps?
