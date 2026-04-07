---
atom_id: SC-P-003
name: Write cross-feature interaction tests before individual feature tests
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - PACT §51 — Edge Case — Vacant Roles
  - PACT §35 — Bridges (Standing Cross-functional Relationships)
  - CO §3.3 — Safety Blindness (Failure Mode)
journal_evidence:
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md
    polarity: NEGATIVE
    quote: "Issues #106 (bridge LCA approval) and #107 (vacancy enforcement) were developed as independent features without cross-feature interaction requirements. The vacancy check was added to `verify_action()` but not propagated to the bridge approval path."
practice_modality: drill
status: draft
---

## The move

When two governance features share state in the same engine (e.g., vacancy handling and bridge approval both operate on the governance engine's role graph), write the cross-feature interaction test BEFORE writing either feature's primary tests. The interaction test asks: "If Feature A's safety invariant holds and Feature B's safety invariant holds, does the combined system still hold?" Specifically: construct a scenario where one feature's output is the other feature's input, and verify the safety property at the seam. Do not defer this to integration testing — by then, the features are independently "passing" and the interaction gap is invisible.

## When it fires

Two trigger conditions:

1. A todo specification introduces a new feature that shares mutable state with an existing feature in the same module. The signal is that both features read or write the same data structure (role graph, envelope set, clearance table, delegation chain).
2. A red team discovers that a safety property holds within each feature's test suite but fails when two features are composed. This is the retroactive trigger — the move should have fired at implementation time but was missed.

## What it serves

PACT §51 (Vacant Roles) defines that "a vacant role satisfies the grammatical constraint but cannot execute" — meaning vacancy must block ALL execution paths, not just the ones tested by the vacancy feature. PACT §35 (Bridges) defines bilateral establishment of cross-functional relationships requiring role-level approval. When these two features interact, the question becomes: can a vacant role approve a bridge? The spec answers no (vacant roles cannot execute), but neither feature's individual test suite asks this question. CO §3.3 (Safety Blindness) names the failure mode: "The AI optimizes for the most direct path to task completion. Safety measures are never the most direct path. The AI deprioritizes them unless explicitly enforced." Cross-feature interaction tests are the explicit enforcement that prevents safety from being deprioritized at the seam.

## Evidence walkthrough

- **0008-RISK-cross-feature-vacancy-bridge** (NEGATIVE) — Two independent red team agents discovered that `approve_bridge()` and `reject_bridge()` did not check whether the approver role was vacant. The root cause was that Issues #106 (bridge LCA approval) and #107 (vacancy enforcement) were developed independently without cross-feature interaction requirements. The vacancy check was added to `verify_action()` but never propagated to the bridge approval path. The fix was mechanical — adding the same vacancy check to two more methods — but the gap had existed since both features were merged, invisible to both features' passing test suites.

## How to drill it

Give the practitioner a module with two features that share state (use a simplified governance engine with vacancy + delegation, or any pair where Feature A's safety property should constrain Feature B's operation). The practitioner must:

1. List the shared mutable state between the two features (e.g., the role graph, the envelope set).
2. For each feature, state the safety invariant in one sentence (e.g., "vacant roles cannot execute," "bridge approval requires bilateral consent from non-vacant roles").
3. Write one test that constructs a scenario crossing the two features: Feature A's output becomes Feature B's input, and the combined safety property must hold.
4. Run the test against the module. If it passes, explain why. If it fails, identify the missing guard and write the fix.

Score on whether the practitioner identifies the interaction test BEFORE being prompted, and on whether the test targets the seam (the point where Feature A's output enters Feature B's logic) rather than testing each feature in isolation with shared setup.

## Common failure mode

The practitioner writes thorough tests for Feature A (vacancy handling: 20 tests, all pass) and thorough tests for Feature B (bridge approval: 15 tests, all pass) and reports "all green." The cross-feature gap is invisible because each feature's test suite uses its own fixtures — Feature A's fixtures never create bridges, Feature B's fixtures never create vacant roles. The combined scenario (vacant role approves a bridge) exists in neither fixture set. High per-feature coverage actively masks the interaction gap.

## Related atoms

- SC-P-001 (adjacent-but-disconnected modules) — the module-level manifestation of the same pattern: two modules that should integrate but don't. SC-P-003 is the intra-module version: two features within the same module that should constrain each other but don't.
- SC-A-002 (frontend mock data detection) — another case where per-feature validation misses cross-boundary issues.
