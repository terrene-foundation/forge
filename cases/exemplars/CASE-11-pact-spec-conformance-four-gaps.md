---
case_id: CASE-11
source_path: loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md
title: "Four Gaps Between Spec and Code"
atoms_served: [spec-to-code-conformance-audit, gap-list-methodology]
spec_concepts: [PACT §10, PACT §35, PACT §40, PACT §44, CO §49, CO §5, CO §29]
craft_layer: brokerage
recommended_use: assessment
destination: both
application: COC
---

# CASE-11: Four Gaps Between Spec and Code

## 1. The Situation

The Kailash Python SDK's PACT governance module was a mature codebase: 22 source files, 1,139 tests, covering organizational compilation, role envelopes, bridge protocols, and vacancy handling. The module had been through multiple development cycles and was considered production-grade. A parallel Rust SDK implementation (`kailash-rs`) had been developed from the same PACT-Core-Thesis.md specification.

No systematic audit had been performed to verify that the Python module's implementation actually conformed to every normative requirement in the PACT specification. The test suite validated internal correctness (do the functions do what the code says they should do) but not spec conformance (does the code do what the spec says it must do). These are different questions, and only one had been asked.

## 2. The Trigger

A red team audit was run against the PACT module using the PACT-Core-Thesis.md specification as the checklist. Instead of testing whether the code worked correctly on its own terms, the auditor tested whether the code implemented what the spec required. This attentional shift -- from "does it work" to "does it conform" -- immediately surfaced four gaps, filed as GitHub issues #199 through #202.

The critical discovery was Gap 1: the PACT module built its own audit chain (`AuditAnchor`/`AuditChain` in `audit.py`) but had zero imports from `kailash.trust.chain`. It never emitted the EATP record types (`GenesisRecord`, `DelegationRecord`, `CapabilityAttestation`) that Section 5.7 of the spec calls "normative." The module had a working audit system that was disconnected from the trust protocol it was supposed to integrate with.

## 3. The Move

The move is **spec-to-code-conformance-audit** combined with **gap-list-methodology**. The practitioner did not simply report "there are conformance issues." Instead, the audit produced a structured gap list with four entries, each carrying: a severity classification (CRITICAL, HIGH, MEDIUM), the specific spec section violated, the exact lines of code involved, and a cross-SDK comparison showing that the Rust implementation was already compliant on all four points.

The gap list methodology is the skill here. Gap 1 (EATP record emission) was classified CRITICAL because Section 5.7 calls the mapping "normative" -- meaning the spec does not leave this as optional. Gap 2 (write-time tightening) was HIGH because 3 of 7 dimensions were missing from validation despite the runtime intersection functions for those dimensions already existing at lines 240-307 of `envelopes.py`. Gap 3 (compilation and bridge protocol) was HIGH because `compile_org()` silently dropped departments without a head role instead of auto-creating vacant heads per Section 4.2. Gap 4 (vacancy handling) was MEDIUM because the 24-hour deadline was hardcoded rather than configurable.

Each gap was filed as a separate GitHub issue with enough detail for a fix to begin immediately.

## 4. The Outcome

The audit transformed a module that appeared complete (1,139 passing tests) into one with four known spec-conformance gaps at known severity levels. The cross-SDK comparison established that all four gaps were catch-up work, not novel design problems -- the Rust SDK had already solved them. This reframed the remediation from "design four new features" to "port four proven patterns," reducing the decision overhead significantly.

The gap list also raised a deeper question captured in the journal's discussion section: why was the PACT module released without EATP record emission if Section 5.7 calls it normative? Was there a conscious decision to defer, or was it simply missed? The audit could not answer this -- it could only surface the gap -- but asking the question is itself a brokerage move: it forces the institutional memory to account for the omission rather than treating it as an anonymous defect.

## 5. The Spec Connection

**PACT §10 (Organizational Compilation)** specifies how organizational structures are compiled into governance artifacts. The audit found that `compile_org()` silently dropped departments and teams without a head role instead of auto-creating vacant heads as Section 4.2 requires. This is a direct violation: the spec says incomplete structures should be handled gracefully with interim arrangements, not silently discarded.

**PACT §35 (Envelope Dimensions)** defines seven constraint dimensions that operating envelopes must enforce. The audit found that `validate_tightening()` checked only four of seven: Financial, Confidentiality, Operational, and Delegation. Temporal, Data Access, and Communication were missing from write-time validation even though the runtime intersection functions for those three dimensions already existed in the same file. The spec makes all seven normative; the implementation made four mandatory and three optional-by-omission.

**PACT §40 (Bridge Protocol)** requires bilateral consent from both endpoint roles when creating a cross-functional bridge. The audit found that `create_bridge()` validated LCA (lowest common ancestor) approval but not bilateral consent. Additionally, bridge scope was not validated against the role envelopes of the endpoints. The spec's consent requirement exists to prevent unilateral authority extension; the implementation enforced the weaker LCA check only.

**PACT §44 (Vacancy Handling)** specifies that vacancies should trigger an interim envelope (degraded-but-operational) during a configurable deadline window. The implementation returned a hard block for any vacancy without designation, and the 24-hour deadline was hardcoded. The spec envisions graceful degradation; the code enforced a binary present/absent check.

**CO §49 (Red Team Validation)** is the principle that artifacts should be validated adversarially, not just functionally. This case is a direct demonstration: the module's 1,139 functional tests all passed, but a red team audit against the spec found four gaps. Functional testing asks "does it break?" Red team validation asks "does it conform to what it claims to implement?"

**CO §5 (Guardrails)** states that constraints should be enforced automatically, not left to manual compliance. The missing write-time validation for three envelope dimensions is a guardrail gap: the spec says the constraints exist, the runtime knows how to intersect them, but no guardrail enforces them at the point where violations are introduced.

**CO §29 (Knowledge Compounding)** is relevant because the cross-SDK comparison turned a Python-only defect list into institutional knowledge. The fact that the Rust SDK was already compliant on all four points means the knowledge of how to implement these features correctly already existed in the organization -- it just had not flowed to the Python codebase. The gap list made that knowledge flow explicit.

## 6. Discussion Questions

1. The module had 1,139 passing tests and zero spec-conformance tests. If you were designing the test suite from scratch, what fraction of tests would you allocate to spec conformance versus functional correctness? Is there a principled way to decide, or does it depend on how normative the spec is?

2. Gap 2 reveals that the runtime intersection functions for the three missing dimensions already existed (lines 240-307 of `envelopes.py`) but were not wired into write-time validation. What organizational or process failure mode produces code that can do something but does not enforce it? How would you detect this class of gap systematically?

3. The journal asks whether EATP record emission was consciously deferred or simply missed. If there is no decision record for the omission, how should the remediation team treat it? Does the answer change how they implement the fix?

4. All four gaps were already solved in the Rust SDK. The journal frames this as "a Python catch-up sprint -- no novel design decisions required." Under what conditions would treating cross-SDK compliance as a mechanical port be dangerous? What signals would tell you to re-derive from spec instead of porting from the other implementation?
