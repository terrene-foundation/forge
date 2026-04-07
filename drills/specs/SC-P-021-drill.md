---
drill_id: DR-SC-P-021
source_atom: SC-P-021
name: "Grep the entire repo for dangling references immediately after every rename or delete — drill"
craft_area: intervene-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner a repo with 5 skill directories, 3 agent files, and a CLAUDE.md that references all 5 skills. Instruct them to delete one skill directory and rename another. After the practitioner makes the changes, run `grep -r` for the old names across the repo. If any hits remain, the drill fails. Then introduce a subtlety: one reference is in a YAML frontmatter `references:` list (not a prose mention), and one is in a markdown link `[text](path)`. Score on whether the practitioner's grep pattern catches both structured and unstructured references, and whether they update all hits in the same commit as the rename.
