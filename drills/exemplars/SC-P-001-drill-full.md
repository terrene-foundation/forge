---
drill_id: DR-SC-P-001-FULL
source_atom: SC-P-001
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Identify Adjacent-but-Disconnected Modules as Spec Conformance Gaps

## Setup

The practitioner receives two module directories from the same package tree and an extract from the governing spec that defines their integration contract.

**Module A: `trust/pact/`** (governance engine, 4 files):

1. `engine.py` (280 lines) -- GovernanceEngine with create_envelope, grant_clearance, compute_address, activate_bypass. Each method emits a `PactAuditAnchor` to the engine's internal audit trail.
2. `models.py` (190 lines) -- Envelope, Clearance, Address, Bypass dataclasses. Each carries a `pact_audit_id: str` field.
3. `bridges.py` (150 lines) -- BridgeManager with establish_bridge, approve_bridge, dissolve_bridge. Uses the role graph from `engine.py` but never imports from `trust/chain.py`.
4. `__init__.py` (20 lines) -- exports GovernanceEngine, BridgeManager.

**Module B: `trust/chain.py`** (trust lineage, single file):

1. `chain.py` (320 lines) -- TrustChain with create_genesis, delegate_capability, verify_lineage. Each operation creates an `EatpAuditRecord` with cryptographic hash. Has its own `EatpAuditRecord` class with different fields than `PactAuditAnchor`. Zero imports from `trust/pact/`.

**Spec extract** (PACT (spec) Section 5.7 — EATP Integration, 6 normative statements):

> S5.7.1: "Every PACT governance action — creating an envelope, granting clearance, computing an address, activating a bypass — MUST create a corresponding EATP record in the trust lineage chain."
>
> S5.7.2: "The EATP record MUST include the PACT audit anchor ID as a cross-reference, enabling bidirectional traversal between governance and trust lineage audit trails."
>
> S5.7.3: "Bridge establishment MUST be recorded as a delegation event in the EATP trust chain, with the bridge's bilateral consent captured as dual capability delegations."
>
> S5.7.4: "Clearance grants MUST be verifiable through EATP lineage — a clearance is valid only if its EATP record traces back to an unbroken genesis chain."
>
> S5.7.5: "The two audit trail types (PactAuditAnchor and EatpAuditRecord) MUST share a common identifier scheme or implement a mapping table, not maintain independent ID namespaces."
>
> S5.7.6: "Bypass activation MUST emit an EATP record with an elevated trust posture flag, enabling downstream systems to distinguish normal governance from bypass governance."

## Task

1. Count the exact number of cross-module import lines between `trust/pact/` and `trust/chain.py`. Report imports in both directions (pact importing chain, chain importing pact).
2. List every normative "MUST" from the spec extract (S5.7.1 through S5.7.6). For each MUST, produce a grep-style result showing whether the required cross-module call exists in either module's code.
3. For each MUST that has no corresponding implementation, write a one-paragraph finding that names: (a) the spec section, (b) the expected call site (which function in which module should make the call), (c) the target (which function in the other module should be called), and (d) the consequence of the gap (what goes wrong in production if the integration is never wired).
4. Assess whether the modules' individual test coverage would have caught these gaps. State what kind of test (unit, integration, cross-module) is needed to surface each gap.
5. Write a one-paragraph summary of the overall integration state using the formula: "N of M normative requirements are implemented, leaving K gaps. The modules are [structurally adjacent but functionally isolated / partially integrated / fully integrated]."

## Model Answer

**Step 1 — Cross-module imports:**

- `trust/pact/` → `trust/chain.py`: 0 import lines
- `trust/chain.py` → `trust/pact/`: 0 import lines
- Total cross-module imports: **0**

**Step 2 — MUST enumeration with grep results:**

| MUST                                    | Spec Section | Grep target                                                                                                | Found?                                                                                 |
| --------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| PACT actions create EATP records        | S5.7.1       | `grep -rn "EatpAuditRecord\|create_.*eatp\|chain\." trust/pact/engine.py`                                  | NO — zero references to EATP or chain in engine.py                                     |
| EATP record includes PACT audit ID      | S5.7.2       | `grep -rn "pact_audit_id" trust/chain.py`                                                                  | NO — chain.py has no awareness of pact_audit_id                                        |
| Bridge establishment creates delegation | S5.7.3       | `grep -rn "delegate_capability\|trust_chain\|chain\." trust/pact/bridges.py`                               | NO — bridges.py never calls into chain                                                 |
| Clearance verifiable through lineage    | S5.7.4       | `grep -rn "verify_lineage" trust/pact/engine.py`                                                           | NO — engine.py never calls verify_lineage                                              |
| Common identifier scheme                | S5.7.5       | `grep -rn "pact_audit_id\|mapping" trust/chain.py` and `grep -rn "eatp.*id\|mapping" trust/pact/models.py` | NO — PactAuditAnchor and EatpAuditRecord use independent ID namespaces with no mapping |
| Bypass emits elevated posture flag      | S5.7.6       | `grep -rn "posture\|elevated\|bypass" trust/chain.py`                                                      | NO — chain.py has no concept of bypass or posture                                      |

**Step 3 — Findings for each missing integration:**

1. **S5.7.1 (action-to-record):** `engine.py::create_envelope()`, `grant_clearance()`, `compute_address()`, and `activate_bypass()` should each call `chain.py::TrustChain.delegate_capability()` (or a dedicated `create_eatp_record()` method) after emitting the PactAuditAnchor. Without this, governance actions are auditable within PACT but invisible to the trust lineage chain — a downstream system that queries EATP for "all governance events" gets zero results despite active governance.

2. **S5.7.2 (cross-reference ID):** The EATP record created in the call above should include `pact_audit_id` as a field. Without bidirectional cross-referencing, an auditor who finds an EATP anomaly cannot trace it back to the governance action that caused it. The two audit trails become parallel histories with no join key.

3. **S5.7.3 (bridge-to-delegation):** `bridges.py::establish_bridge()` should call `chain.py::TrustChain.delegate_capability()` twice — once per side of the bilateral consent. Without this, bridge establishment is governmentally valid but trust-lineage-invisible: the bridge exists in the role graph but has no provenance in the chain.

4. **S5.7.4 (clearance verification):** `engine.py::grant_clearance()` should call `chain.py::TrustChain.verify_lineage()` to confirm the genesis chain is unbroken before issuing the clearance. Without this verification, a clearance can be granted even after the trust chain has been compromised — the governance layer trusts its own state without checking the trust layer.

5. **S5.7.5 (identifier scheme):** `models.py::PactAuditAnchor` and `chain.py::EatpAuditRecord` should either share a base ID type or implement a mapping table in a third location. Without this, correlating audit events across the two systems requires manual name matching — unreliable and impossible to automate.

6. **S5.7.6 (bypass posture flag):** `engine.py::activate_bypass()` should create an EATP record with a `posture=ELEVATED` flag. Without this, downstream systems cannot distinguish normal governance events from bypass events in the trust lineage — a bypass that should trigger heightened scrutiny looks identical to a routine delegation.

**Step 4 — Test coverage analysis:**

Per-module unit tests cannot catch any of these gaps. Both modules can have 100% line coverage and 100% branch coverage within their own boundaries. The gaps exist at the cross-module call boundary, which is never exercised. Surfacing these gaps requires:

- S5.7.1, S5.7.2, S5.7.6: Cross-module integration test that calls a PACT governance action and asserts an EATP record was created.
- S5.7.3: Cross-module integration test that establishes a bridge and asserts two delegation events in the chain.
- S5.7.4: Cross-module integration test that corrupts the chain, then attempts a clearance grant and asserts rejection.
- S5.7.5: Cross-module integration test that creates records in both systems and asserts they can be correlated by ID.

**Step 5 — Summary:**

0 of 6 normative requirements from PACT (spec) S5.7 are implemented. The modules are structurally adjacent (same `trust/` package tree, complementary spec domains, parallel audit trail classes) but functionally isolated (zero cross-module imports, independent ID namespaces, no shared state). The 1,139+ unit tests across both modules would all pass while every integration requirement is violated.

## Scoring

| Criterion                                                                             | Points |
| ------------------------------------------------------------------------------------- | ------ |
| Cross-module import count is exactly 0 in both directions                             | 1      |
| All 6 MUSTs extracted and enumerated (not just "several gaps found")                  | 2      |
| Grep results show specific search patterns, not just "not found"                      | 1      |
| Findings name the specific call site (function in source module)                      | 1      |
| Findings name the specific target (function in destination module)                    | 1      |
| Consequence of each gap stated in operational terms (what breaks)                     | 1      |
| Test coverage analysis correctly identifies cross-module integration tests as the gap | 1      |
| Summary uses the "N of M" formula with correct counts                                 | 1      |
| No gap dismissed as low-priority without spec justification                           | 1      |
| **Total**                                                                             | **10** |

## Extensions

1. **Asymmetric enforcement detection.** Add a third code path to `engine.py` — a `check_access()` method that DOES call `chain.py::verify_lineage()` (one of the six integrations is wired). The practitioner must now detect that 1 of 6 integrations exists but is applied asymmetrically: `check_access` verifies lineage but `grant_clearance` does not. This mirrors the kailash-rs vacancy finding where `check_access()` enforced vacancy but `verify_action()` did not.

2. **Cross-SDK parity gap.** Provide a second implementation of the same modules in Rust (`trust/pact/mod.rs` and `trust/chain.rs`) where 4 of 6 integrations ARE wired. The practitioner must produce a parity matrix showing which integrations exist in each SDK and identify the Python SDK as the lagging implementation. This mirrors the real PACT finding where the Rust SDK was already compliant on gaps the Python SDK had not addressed.

3. **Integration wiring plan.** After completing the gap analysis, the practitioner writes a todo list with one item per missing integration. Each todo must specify: the source function, the target function, the data passed at the boundary, and the test that verifies it. Score on whether the todo list covers all 6 gaps and whether each todo is implementable without ambiguity.
