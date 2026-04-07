---
drill_id: DR-SC-P-009
source_atom: SC-P-009
name: "Treat architecture documents as testable claims against the actual codebase — drill"
craft_area: attend-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Provide the practitioner with a 2-page architecture document describing a module of 8-12 source files. The document should contain: 2 claims about dependency versions that are wrong (e.g., says "uses library X 2.0" when the code uses 1.8), 2 claims about capabilities that already exist but are described as "to be implemented," and 1 claim about a code pattern that is described incorrectly (e.g., says "uses pattern A" when the code actually uses pattern B). The practitioner must:

1. Read each factual claim in the architecture document.
2. For each claim, read the relevant source file and classify the claim as CONFIRMED, CORRECTED, or ABSENT.
3. Produce a numbered correction list with the specific discrepancy for each CORRECTED claim.
4. Revise the effort estimate based on the corrections (should decrease if capabilities already exist, should increase if dependency versions require migration).

Score on the number of discrepancies found (target: all 5), the specificity of the corrections (vague corrections like "version is wrong" score zero; specific corrections like "doc says polars 0.39, codebase uses 0.53, 14 minor versions of API changes" score full), and whether the effort estimate revision is directionally correct.
