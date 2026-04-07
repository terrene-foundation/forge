---
case_id: CASE-06
source_path: loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md
title: "Three Critical API Mismatches Before a Line of Code"
atoms_served: [specialist-delegation, todos-red-team, api-surface-verification]
spec_concepts: [CO §36, CO §29, EATP §16, PACT §35]
craft_layer: artifact
recommended_use: teaching
destination: both
application: COC
---

# CASE-06: Three Critical API Mismatches Before a Line of Code

## 1. The Situation

The Kailash Python SDK needed to add PACT spec-conformance features to its governance engine. A sprint plan (S12) had been written: 14 todos organized into 5 milestones, covering vacancy enforcement, bridge bilateral consent, EATP trust emission, and verification gradient. The plan was detailed -- it specified which functions to modify, which fields to add, which modules to wire together.

Before any implementation began, the plan was submitted to a red team review. This is the `/todos` phase gate in the COC process: the plan is vetted for feasibility before execution starts.

## 2. The Trigger

The red team agent reviewed the todo specifications against the actual SDK source code and found three critical issues that would have caused runtime failures had implementation proceeded as planned. It also found five high-severity issues requiring significant revisions.

**Critical 1 (TrustStore API mismatch)**: The todos assumed synchronous, per-record emission to `TrustStore`. But `TrustStore` is asynchronous and stores complete `TrustLineageChain` objects, not individual records. The `GovernanceEngine` is synchronous. Implementing the todos as written would have produced a synchronous caller trying to use an asynchronous API -- a guaranteed failure.

**Critical 2 (Field name wrong)**: The todos referenced `created_at` as the field name on `DelegationRecord`. The actual field is `delegated_at`. Every constructor call would have raised `TypeError`.

**Critical 3 (Breaking change)**: The plan called for adding required `consent` parameters to `create_bridge()`. But 14+ test sites and the university example call `create_bridge()` without consent. The change would have broken every existing caller.

The attentional pattern is **plan-versus-reality verification**: checking whether the plan's assumptions about the existing codebase are actually true before executing the plan.

## 3. The Move

The move is **todos-red-team** combined with **specialist-delegation** and **api-surface-verification**. The red team did not just check the plan for logical coherence -- it checked the plan's concrete API assumptions against the actual source code. This is the difference between "does this plan make sense?" (which it did) and "will this plan work against the code that actually exists?" (which it would not have).

For C1, the fix was to create a `PactEatpEmitter` synchronous protocol with an `InMemoryPactEmitter` default implementation, bridging the sync/async gap. For C2, the field name was corrected. For C3, a `require_bilateral_consent=False` default was added for backward compatibility.

The five high findings were similarly concrete: a type collision with an existing `GradientEngine`, a return-type change that would silently break a truthiness check, a frozen dataclass that could not accept a new field, unspecified None-handling semantics, and 8 of 14 todos modifying the same file without acknowledging serialization requirements.

## 4. The Outcome

All findings were addressed in revision 2 of the milestones before any implementation began. No findings were deferred. The revised implementation order ensured that the 8 todos touching `engine.py` would not conflict. The total cost of the red team was one review cycle. The cost of not doing the red team would have been at least three critical runtime failures discovered during implementation, each requiring plan revision and partial rollback.

The ratio is instructive: 3 critical + 5 high findings caught in review versus 0 lines of code written. The leverage of pre-implementation verification against actual source is enormous.

## 5. The Spec Connection

**CO §36 (Phase -- Vet)** defines the vetting phase as a mandatory step between planning and implementation. The sprint plan was well-structured, the milestones were logical, and the dependencies were mapped. It would have been tempting to skip vetting and start coding. §36 requires the vet phase precisely for cases like this: when the plan looks complete but its assumptions have not been verified against the implementation target.

**CO §29 (Evidence-Based Completion)** requires that claims of readiness be backed by evidence. The plan claimed readiness for implementation. The red team asked "what is the evidence that these API calls will work?" and found that the evidence was the plan author's memory of the API, not a verified read of the current source. §29 applied to the plan, not just to the final product.

**EATP §16 (Delegation Record)** is directly relevant because the field-name error (C2: `created_at` vs `delegated_at`) was on the `DelegationRecord` type itself. The spec defines the delegation record as a trust artifact; getting its field names wrong in a plan means the plan was not grounded in the actual spec implementation.

**PACT §35 (Bridges)** defines the bilateral-consent bridge that C3 addresses. The spec says bridge creation requires consent. The existing code did not enforce consent. The plan called for adding enforcement but missed the backward-compatibility impact. §35 establishes what the bridge must do; the case shows the practitioner must also consider what existing callers expect.

## 6. Discussion Questions

1. The TrustStore API mismatch (C1) arose because the plan author assumed synchronous, per-record semantics. This is a plausible assumption if you have not read the TrustStore code recently. What organizational or process mechanism would make API assumptions visible and checkable before they become plan commitments? Is "read the source before writing the plan" sufficient, or does it need to be formalized?

2. Eight of 14 todos modified `engine.py`, a 1,618-line file carrying governance logic, bridge logic, vacancy logic, audit emission, and EATP emission. The red team flagged the serialization requirement but did not recommend decomposing the file. At what point does a file's responsibility count become a structural risk that should be addressed before new features are added to it?

3. The `require_bilateral_consent=False` default is a pragmatic backward-compatibility choice, but the PACT spec says consent is required. This creates a gap between spec and implementation that exists for practical reasons. How long should such gaps be allowed to persist? What is the trigger for flipping the default to `True` and breaking existing callers?
