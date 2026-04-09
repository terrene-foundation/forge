---
case_id: CASE-18
source_path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md
title: "Vacancy Enforcement Only Works One Way"
atoms_served: [governance-primitive-integrity, asymmetric-enforcement-detection]
spec_concepts: [PACT §51, PACT §13, CO §5, PACT §33]
craft_layer: brokerage
recommended_use: assessment
destination: both
application: COC
---

# CASE-18: Vacancy Enforcement Only Works One Way

## 1. The Situation

The Kailash Rust SDK implemented a governance engine (`kailash-governance`) that enforced PACT primitives -- roles, envelopes, knowledge clearance, and vacancy. The engine exposed two primary enforcement paths: `check_access()` for knowledge access (can this role read this item?) and `verify_action()` for action execution (can this role perform this action within its envelope?). Both paths were expected to enforce the same invariant: a vacant role -- one with no occupant -- should be blocked from both reading knowledge and executing actions.

The governance engine had been passing its test suite and was considered structurally complete for the cross-SDK governance milestone.

## 2. The Trigger

An audit of the vacancy enforcement code discovered that the two paths were asymmetric. `check_access()` correctly blocked vacant roles at Step 0 of the `can_access` function (`access.rs`, lines 112-119) -- before any other checks ran, it verified the role had an occupant. But `verify_action()` had zero vacancy checks. A vacant role passed straight through envelope verification in `engine.rs` at line 192, meaning it could execute any action its envelope permitted despite having no occupant.

The attentional skill here is **cross-path invariant verification**: checking that a constraint enforced on one code path is also enforced on every other code path that should share the same invariant. The discovery came not from a test failure but from reading the enforcement logic across both paths and noticing the absence.

## 3. The Move

The move is **governance-primitive-integrity** combined with **asymmetric-enforcement-detection**. The practitioner identified that the fix location is specific: `verify_action()` in `engine.rs` at line 192 should check vacancy before calling `compute_envelope`. The fix is a single check insertion, but the craft is in the diagnosis: recognising that two enforcement paths that should share an invariant do not, and that the asymmetry is invisible to any test that exercises only one path at a time.

The non-obvious aspect is the security implication. In a production governance scenario, a vacant role (no occupant assigned) could still execute governed actions -- actions taken under an authority that has no human behind it. This is not a crash or a test failure; it is a silent authorisation gap that only manifests when someone audits the enforcement logic rather than the enforcement outcome.

## 4. The Outcome

The audit pinpointed the exact fix location (`engine.rs:192`) and the exact fix: add a vacancy check before `compute_envelope`. The entry classifies this as a security gap -- actions taken under a vacant role's authority in a production governance scenario. The asymmetry existed because `check_access` was written with vacancy awareness from the start, while `verify_action` was written later without inheriting the same guard.

## 5. The Spec Connection

**PACT §51 (Vacant Roles)** defines the semantics of role vacancy: a role with no occupant should not exercise authority. The case directly violates this -- the action path allowed a vacant role to exercise its envelope authority. The spec does not distinguish between knowledge access and action execution when it comes to vacancy; both are exercises of role authority.

**PACT §13 (Effective Envelope)** defines the computed envelope that constrains what a role can do. The `verify_action()` path correctly computed the effective envelope but then applied it to a role that should never have reached envelope computation. The envelope was effective but applied to a non-entity -- the spec's envelope semantics presuppose a non-vacant occupant.

**CO §5 (Deterministic Enforcement)** requires that governance constraints produce the same result regardless of which code path evaluates them. The asymmetry between `check_access` and `verify_action` violates deterministic enforcement: the same role in the same vacancy state gets blocked on one path and permitted on another. The enforcement outcome depends on the path, not the policy.

**PACT §33 (Knowledge Cascade)** addresses how knowledge access permissions propagate through delegation chains. The fact that vacancy was enforced on the knowledge path but not the action path raises a cascade question: if a vacant role delegates to a subordinate, the subordinate is blocked from knowledge access (via `check_access`) but could the subordinate's actions still proceed (via `verify_action` on the delegating role's envelope)? The asymmetry may have cascade effects beyond the direct vacancy check.

## 6. Discussion Questions

1. If you were writing the test suite for `verify_action()`, what test case would have caught this asymmetry? Why is it likely that the existing tests did not -- what testing pattern produces tests that exercise each path independently but never compare their invariants?

2. The entry says the fix is adding a vacancy check before `compute_envelope`. But consider the alternative: what if vacancy were enforced AFTER envelope computation, as a final gate? What are the security and performance differences between checking vacancy first vs last, and which failure mode is worse -- a vacant role that computes an envelope it never uses, or a vacant role that skips computation but could be permitted by a future code change that inserts logic before the gate?

3. The asymmetry arose because `check_access` was written first with vacancy awareness and `verify_action` was written later without it. What structural pattern could prevent this class of error -- where a new enforcement path forgets to inherit an invariant from an existing path? Consider both code-level patterns (shared guard functions) and process-level patterns (invariant checklists at review time).
