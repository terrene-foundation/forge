---
drill_id: DR-SC-P-003
source_atom: SC-P-003
name: "Write cross-feature interaction tests before individual feature tests — drill"
craft_area: attend-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Give the practitioner a module with two features that share state (use a simplified governance engine with vacancy + delegation, or any pair where Feature A's safety property should constrain Feature B's operation). The practitioner must:

1. List the shared mutable state between the two features (e.g., the role graph, the envelope set).
2. For each feature, state the safety invariant in one sentence (e.g., "vacant roles cannot execute," "bridge approval requires bilateral consent from non-vacant roles").
3. Write one test that constructs a scenario crossing the two features: Feature A's output becomes Feature B's input, and the combined safety property must hold.
4. Run the test against the module. If it passes, explain why. If it fails, identify the missing guard and write the fix.

Score on whether the practitioner identifies the interaction test BEFORE being prompted, and on whether the test targets the seam (the point where Feature A's output enters Feature B's logic) rather than testing each feature in isolation with shared setup.
