---
drill_id: DR-SC-P-020
source_atom: SC-P-020
name: "Treat the second occurrence of a drift class as a structural signal, not another incident — drill"
craft_area: attend-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a sequence of three journal entries from the same project, spaced across sessions. Entry 1: "found 5 agents referencing deleted skill directories, fixed the references." Entry 2 (different session): "found 3 rules referencing a renamed command, fixed the references." Entry 3 (different session): "found 2 skills referencing a moved file path, fixed the references." The practitioner must: (1) identify that all three are the same class (dangling references after rename/delete), (2) explain why entry 2 — not entry 3 — was the moment to shift from instance-fix to mechanism-fix, and (3) propose the mechanism (a post-rename reference sweep, a pre-commit hook that checks for dangling paths, or a structural rename tool that updates references atomically). Score on timing: the practitioner who waits until entry 3 to recognize the pattern has already let one preventable instance through.
