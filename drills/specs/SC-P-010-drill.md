---
drill_id: DR-SC-P-010
source_atom: SC-P-010
name: "Red team CI, auth, and release processes for timing-dependent failures — drill"
craft_area: attend-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner a system description with three components: (1) a CI publish pipeline that processes 3 artifacts triggered by git tags, (2) an auth middleware that validates bearer tokens against a list of valid tokens using an iterator, and (3) a database transaction wrapper that uses Drop for rollback. Each component has a timing bug embedded:

1. The CI pipeline pushes all tags simultaneously (race condition).
2. The auth middleware uses `.find()` which short-circuits (timing side-channel).
3. The transaction Drop calls an async rollback from a sync context (runtime absence).

The practitioner must:

1. Identify the timing assumption in each component.
2. Construct a concrete scenario where the assumption fails (e.g., "attacker sends 1000 requests with key at index 0 vs index 5, measures median response time").
3. Propose a fix for each that eliminates the timing dependency (sequential tag push, constant-time iteration, owned runtime in Drop).
4. Describe how to test the fix (how do you verify that the race condition is resolved, that the iteration is constant-time, that Drop works without an ambient runtime?).

Score on whether all three timing bugs are found, whether the attack scenarios are concrete enough to be reproducible, and whether the fixes address the root cause (timing dependency) rather than the symptom (specific failure mode).
