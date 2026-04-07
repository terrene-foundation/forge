---
drill_id: DR-SC-P-007
source_atom: SC-P-007
name: "Define session completion state before starting, not after convergence is claimed — drill"
craft_area: attend-how, intervene-when
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a brief describing a session with 5-8 deliverables across 2-3 milestones. Some deliverables have dependencies on each other; one has an external dependency that may not be available. The practitioner must:

1. Write a completion-state checklist BEFORE looking at the code. Each item must be verifiable (not "implement auth" but "auth middleware returns 401 for expired tokens, test_auth_expired passes").
2. Identify which items can be done in parallel and which are sequential.
3. Name what is explicitly NOT in scope, with a one-sentence reason for each exclusion.
4. After a simulated "session end" (presented as a narrative of what was accomplished), evaluate the narrative against the checklist and score: how many items are done, how many are partial, how many are gaps.

Score on the specificity of the checklist items (vague items like "tests pass" score zero; specific items like "test_pypi_publish_single_tag passes" score full), the accuracy of the dependency graph, and the quality of the post-session evaluation (did the practitioner catch the gaps in the narrative, or did they rubber-stamp "done"?).
