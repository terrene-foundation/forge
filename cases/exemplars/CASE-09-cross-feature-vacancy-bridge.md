---
case_id: CASE-09
source_path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md
title: "The Vacant Approver Who Could Approve Bridges"
atoms_served: [cross-feature-interaction-redteam, governance-primitive-integrity]
spec_concepts: [PACT §51, PACT §35, CO §3.3, CO §49]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-09: The Vacant Approver Who Could Approve Bridges

## 1. The Situation

The Kailash Rust SDK governance engine implemented two features that had been developed as independent work items:

**Feature #106 (Bridge LCA Approval)**: Cross-boundary access bridges require approval from the lowest common ancestor (LCA) in the organizational hierarchy. The `approve_bridge()` and `reject_bridge()` functions enforce this approval gate.

**Feature #107 (Vacancy Enforcement)**: When an organizational role is vacant (no occupant), that role cannot perform governance actions. The `verify_action()` function checks vacancy status before allowing any action to proceed.

Both features worked correctly in isolation. Both had passing test suites. Both had been reviewed and merged independently.

## 2. The Trigger

Two independent red team agents -- a security-reviewer and a deep-analyst -- discovered the same gap from different angles: `approve_bridge()` and `reject_bridge()` did not check whether the approver or rejector role was vacant. A role with no occupant could approve cross-boundary access bridges.

The vacancy check had been added to `verify_action()` but was never propagated to the bridge approval path. The two features shared the governance engine's state (the organizational tree, role occupancy, bridge registry) but their implementations did not share enforcement logic.

The attentional pattern is **cross-feature interaction detection**: recognizing that two features that share state will interact even if their requirements documents and todo specifications treat them as independent.

## 3. The Move

The move is **cross-feature-interaction-redteam** combined with **governance-primitive-integrity**. The fix itself was straightforward: add vacancy and designation checks to both `approve_bridge()` and `reject_bridge()`, mirroring the pattern already established in `verify_action()`. A guard was also added to `set_vacancy_designation()` to prevent pre-staging designations on filled roles.

The deeper skill is in how the gap was found. Two independent red team agents converged on the same finding from different perspectives. The security-reviewer asked "what can a vacant role do?" and enumerated all governance actions. The deep-analyst asked "what assumptions does bridge approval make about the approver?" and found that it assumed the approver was a real occupant without checking. Cross-feature interaction testing is not a single technique -- it is the habit of asking "what state does this feature share with other features, and do the invariants hold across all access paths?"

## 4. The Outcome

The vacancy check was propagated to bridge approval and rejection. The governance engine now enforces a consistent invariant: vacant roles cannot perform any governance action, whether that action is a general `verify_action()` call, a bridge approval, or a bridge rejection.

The journal records a structural lesson: "Cross-feature interaction testing must be explicit in todo specifications. Three features that share the governance engine's state (vacancy, bridge, delegation) will interact whether or not their todos acknowledge it."

This is a process recommendation, not just a bug fix. The features were developed as independent todos with independent test suites. Nothing in the process required anyone to ask "how do these features interact?" The fix addresses the bug; the lesson addresses the process gap that allowed the bug to exist.

## 5. The Spec Connection

**PACT §51 (Vacant Roles)** defines the governance behavior of vacant roles: a vacant role cannot perform actions that require an occupant. The spec is unambiguous -- vacancy is a constraint on all governance actions. The implementation applied this constraint to `verify_action()` but not to `approve_bridge()`, creating a gap between spec and code that existed because the features were implemented as separate work items rather than as aspects of a single constraint.

**PACT §35 (Bridges)** defines cross-boundary access bridges and their approval requirements. §35 says bridges require approval from the LCA. It does not explicitly say "and the approver must not be vacant" because that constraint comes from §51, not from §35. The interaction between §35 and §51 is the source of the bug: each spec section was implemented faithfully in isolation, but the interaction between sections was not implemented.

**CO §3.3 (Safety Blindness)** applies because the feature developers took the most direct path to implementing each feature. Feature #106 implemented bridge approval. Feature #107 implemented vacancy enforcement. Neither developer asked "what happens when these two features interact?" because the most direct path to "feature complete" does not include cross-feature analysis. §3.3 predicts this: the agent optimizes for the task at hand and does not spontaneously consider adjacent tasks.

**CO §49 (Enforcement Reliability)** states that enforcement mechanisms must actually enforce. The vacancy enforcement was reliable for `verify_action()` but unreliable for bridge approval. §49's requirement is not "enforce on the code path you tested" but "enforce on every code path where the constraint applies."

## 6. Discussion Questions

1. The two red team agents found the same bug independently. If only one agent had been assigned, would the bug still have been found? The security-reviewer approached from "what can vacant roles do?" and the deep-analyst from "what does bridge approval assume?" Are these approaches interchangeable, or does each surface different classes of bugs?

2. The journal says cross-feature interaction testing must be explicit in todo specifications. Concretely, what would that look like? For a todo that adds vacancy enforcement, should the specification list every other feature that uses the governance engine and require interaction testing with each? At what scale does that become impractical?

3. The bridge approval path and `verify_action()` both needed vacancy checks, but they were implemented by copying the pattern rather than by extracting a shared enforcement function. If the vacancy check logic changes in the future (e.g., adding suspension as a separate state), every copy must be updated. Should the fix have been a shared function instead of a copied pattern? What are the trade-offs between code reuse and code locality in governance enforcement?
