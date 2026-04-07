---
drill_id: DR-SC-P-004
source_atom: SC-P-004
name: "Label every file with a disposition before starting implementation — drill"
craft_area: attend-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner a module with 10-15 source files plus an architecture document or brief that describes the intended implementation. The architecture document should contain at least 3 inaccuracies about the current codebase state (version mismatches, features described as missing that already exist, features described as complete that are stubs). The practitioner must:

1. Read each source file and assign a disposition (SOLID / EXTEND / ENHANCE / REWRITE / IMPLEMENT / NEEDS MIGRATION).
2. Produce a table with file path, line count, disposition, and one-sentence rationale.
3. Compare their table against the architecture document and list every discrepancy.
4. Revise the effort estimate based on the table, not the document.

Score on the number of architecture-vs-codebase discrepancies found (target: all 3+) and on whether the dispositions are justified by file content rather than by the architecture document's claims. A practitioner who assigns dispositions by reading the architecture document instead of the source files is failing the drill even if the labels happen to be correct.
