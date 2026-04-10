---
drill_id: DR-SC-B-004-FULL
source_atom: SC-B-004
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Specialist Delegation as Required First Move

## Setup

The practitioner receives a **feature plan** for implementing a PACT (primitives) governance module. The plan was authored without specialist review and is about to enter the todo phase. The plan covers 4 files and enumerates specific SDK API calls.

**Feature plan excerpt — "Implement PACT Delegation with Cascade Revocation":**

```
File 1: delegation_service.py
  - Import TrustStore from kailash_pact
  - Create DelegationRecord with fields: delegator, delegate, scope, created_at
  - Call trust_store.emit(record) synchronously for each delegation event
  - On revocation, query trust_store.get_children(record_id) and revoke each

File 2: envelope_checker.py
  - Import GovernanceEngine from kailash_pact
  - Call engine.validate_tightening(old_envelope, new_envelope)
  - Check 4 dimensions: Financial, Confidentiality, Operational, Delegation

File 3: bridge_service.py
  - Import create_bridge from kailash_pact.bridges
  - Call create_bridge(role_a, role_b, scope, approver=lca_role)
  - Bridge creation requires only LCA approval

File 4: test_governance.py
  - Use GovernanceEngine() with no constructor arguments
  - Assert delegation records have 'created_at' timestamp field
  - Assert validate_tightening returns True for valid narrowing
```

**Context the practitioner receives:**

- "This plan touches PACT (primitives) — TrustStore, DelegationRecord, GovernanceEngine, and the bridges module."
- The practitioner has access to the actual kailash-pact SDK source files (provided as a reference appendix).
- The practitioner has NOT been told there are errors in the plan.

**SDK source reference (the ground truth):**

- `TrustStore` is async — `await trust_store.emit(record)`, not synchronous.
- `DelegationRecord` uses `delegated_at`, not `created_at`. It also requires a `delegation_chain_id` field that the plan omits.
- `GovernanceEngine()` requires a `config` argument: `GovernanceEngine(config=PactConfig(...))`.
- `validate_tightening()` checks all 7 envelope dimensions (Financial, Confidentiality, Operational, Delegation, Temporal, Data Access, Communication), not 4.
- `create_bridge()` signature changed in v3.7: it now requires `consent_a` and `consent_b` parameters for bilateral consent — LCA-only approval is no longer sufficient.
- `create_bridge` lives in `kailash_pact.governance`, not `kailash_pact.bridges` (the bridges submodule was merged into governance in v3.6).
- The test file's `GovernanceEngine()` with no arguments will raise `TypeError: __init__() missing 1 required keyword argument: 'config'`.

## Task

1. **Identify the framework surfaces.** Read the plan and list every SDK API call, constructor, import path, and field name the plan depends on. Produce a checklist of API assumptions the plan makes.

2. **Read the actual source.** For each API assumption on your checklist, open the SDK source reference and verify whether the assumption is correct. Record each finding as CONFIRMED (plan matches source) or CORRECTION (plan differs from source, with the specific discrepancy).

3. **Produce a corrections list.** For each CORRECTION, write a structured entry with: (a) severity (CRITICAL if it would cause a runtime failure, HIGH if it would cause incorrect behaviour, MEDIUM if it is a naming/import issue that would fail at import time), (b) the plan's assumption, (c) the SDK's actual API, and (d) the corrected plan text.

4. **Classify the corrections by discoverability.** For each correction, state: would this error have been caught by (a) a type checker or linter at import time, (b) a unit test at test time, (c) an integration test at runtime, or (d) only by reading the SDK source? Corrections in category (d) are the ones that specialist delegation specifically prevents.

5. **Rewrite the plan.** Produce a corrected version of the 4-file plan that incorporates all corrections. The corrected plan should be ready for the todo phase with no further specialist review needed.

## Model Answer

**Step 1 — API assumption checklist:**

| #   | Assumption                                                           | File   | Type         |
| --- | -------------------------------------------------------------------- | ------ | ------------ |
| 1   | `from kailash_pact import TrustStore`                                | File 1 | import       |
| 2   | `DelegationRecord` has field `created_at`                            | File 1 | field name   |
| 3   | `trust_store.emit(record)` is synchronous                            | File 1 | sync/async   |
| 4   | `trust_store.get_children(record_id)` exists                         | File 1 | method       |
| 5   | `from kailash_pact import GovernanceEngine`                          | File 2 | import       |
| 6   | `engine.validate_tightening()` checks 4 dimensions                   | File 2 | behaviour    |
| 7   | `from kailash_pact.bridges import create_bridge`                     | File 3 | import path  |
| 8   | `create_bridge(role_a, role_b, scope, approver=lca_role)` — LCA-only | File 3 | signature    |
| 9   | `GovernanceEngine()` takes no constructor arguments                  | File 4 | constructor  |
| 10  | `DelegationRecord` has no other required fields                      | File 1 | completeness |

**Step 2 — Source verification:**

| #   | Assumption                  | Disposition | Discrepancy                                                             |
| --- | --------------------------- | ----------- | ----------------------------------------------------------------------- |
| 1   | TrustStore import           | CONFIRMED   | Import path is correct                                                  |
| 2   | `created_at` field          | CORRECTION  | Field is `delegated_at`, not `created_at`                               |
| 3   | Synchronous emit            | CORRECTION  | `TrustStore.emit()` is async — requires `await`                         |
| 4   | `get_children()` method     | CONFIRMED   | Method exists on TrustStore                                             |
| 5   | GovernanceEngine import     | CONFIRMED   | Import path is correct                                                  |
| 6   | 4-dimension check           | CORRECTION  | `validate_tightening()` checks all 7 dimensions, not 4                  |
| 7   | `kailash_pact.bridges` path | CORRECTION  | Merged into `kailash_pact.governance` in v3.6                           |
| 8   | LCA-only approval           | CORRECTION  | v3.7 requires bilateral consent: `consent_a` and `consent_b` parameters |
| 9   | No-argument constructor     | CORRECTION  | Requires `config=PactConfig(...)`                                       |
| 10  | No other required fields    | CORRECTION  | `delegation_chain_id` is required                                       |

**Step 3 — Corrections list:**

| #   | Severity | Plan assumed                                     | SDK actual                                                                        | Corrected text                                                                      |
| --- | -------- | ------------------------------------------------ | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| C1  | CRITICAL | `trust_store.emit(record)` synchronous           | `await trust_store.emit(record)` async                                            | Change to `await trust_store.emit(record)` and mark service function as `async def` |
| C2  | CRITICAL | `GovernanceEngine()` no args                     | `GovernanceEngine(config=PactConfig(...))`                                        | Add `config` parameter with required PactConfig                                     |
| C3  | CRITICAL | `create_bridge(... approver=lca_role)` LCA-only  | `create_bridge(... consent_a=role_a_consent, consent_b=role_b_consent)` bilateral | Add bilateral consent parameters, remove LCA-only approval                          |
| C4  | HIGH     | `validate_tightening` checks 4 dimensions        | Checks all 7 dimensions                                                           | Update plan to state 7 dimensions; add Temporal, Data Access, Communication         |
| C5  | HIGH     | `DelegationRecord` missing `delegation_chain_id` | Required field                                                                    | Add `delegation_chain_id` to record construction                                    |
| C6  | MEDIUM   | `created_at` field name                          | `delegated_at` field name                                                         | Rename field throughout                                                             |
| C7  | MEDIUM   | `from kailash_pact.bridges`                      | `from kailash_pact.governance`                                                    | Fix import path                                                                     |

**Step 4 — Discoverability classification:**

| #   | Caught by                                                                                                                                                                                            | Reasoning    |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| C1  | (c) integration test — async call without `await` may return a coroutine object instead of raising immediately, depending on the call site. Some linters catch this but not all.                     | Category (c) |
| C2  | (a) type checker — `TypeError` on construction, caught at import/instantiation time                                                                                                                  | Category (a) |
| C3  | (d) source read only — the old signature still type-checks if `approver` is accepted as a keyword; the bilateral consent requirement is a semantic change, not a type change                         | Category (d) |
| C4  | (d) source read only — calling `validate_tightening()` with an envelope that omits 3 dimensions will still succeed for the 4 checked dimensions; the missing checks produce false-pass, not an error | Category (d) |
| C5  | (a) type checker — missing required constructor argument raises `TypeError`                                                                                                                          | Category (a) |
| C6  | (c) integration test — `record.created_at` raises `AttributeError` at runtime                                                                                                                        | Category (c) |
| C7  | (a) type checker — `ModuleNotFoundError` at import time                                                                                                                                              | Category (a) |

**Category (d) corrections — the ones only specialist delegation prevents:** C3 (bilateral consent) and C4 (7-dimension check). These are the highest-value findings from the specialist review. They would have passed type checking, passed unit tests with mocks, and produced silently incorrect behaviour in production — a governance module that approves unilateral bridges and validates only 4 of 7 envelope dimensions.

**Step 5 — Corrected plan:**

```
File 1: delegation_service.py
  - Import TrustStore from kailash_pact
  - Create DelegationRecord with fields: delegator, delegate, scope,
    delegated_at, delegation_chain_id
  - Call await trust_store.emit(record) asynchronously for each delegation event
  - On revocation, query trust_store.get_children(record_id) and revoke each

File 2: envelope_checker.py
  - Import GovernanceEngine from kailash_pact
  - Instantiate engine = GovernanceEngine(config=PactConfig(...))
  - Call engine.validate_tightening(old_envelope, new_envelope)
  - Check all 7 dimensions: Financial, Confidentiality, Operational,
    Delegation, Temporal, Data Access, Communication

File 3: bridge_service.py
  - Import create_bridge from kailash_pact.governance
  - Call create_bridge(role_a, role_b, scope,
    consent_a=role_a_consent, consent_b=role_b_consent)
  - Bridge creation requires bilateral consent from both endpoint roles

File 4: test_governance.py
  - Use GovernanceEngine(config=PactConfig(...))
  - Assert delegation records have 'delegated_at' timestamp and
    'delegation_chain_id' chain reference
  - Assert validate_tightening returns True for valid narrowing across
    all 7 dimensions
```

## Scoring

| Criterion                                                                                              | Points |
| ------------------------------------------------------------------------------------------------------ | ------ |
| API assumption checklist covers all 10 surfaces (imports, fields, sync/async, signatures, constructor) | 2      |
| All 7 corrections identified with correct severity classification (3 CRITICAL, 2 HIGH, 2 MEDIUM)       | 3      |
| Discoverability classification correctly identifies C3 and C4 as source-read-only (category d)         | 2      |
| Corrected plan incorporates all 7 fixes and is ready for the todo phase                                | 2      |
| Corrections reference the actual SDK source, not API shape memory                                      | 1      |
| **Total**                                                                                              | **10** |

## Extensions

- **Variant A (delegation timing):** The practitioner receives the same plan but is told "the specialist is unavailable for 2 hours." They must decide: (a) wait for the specialist and block the session, (b) proceed without specialist review and accept the risk, or (c) perform the specialist review themselves by reading the SDK source. The correct answer is (c) — the rule is "route through a specialist," and the practitioner can act as their own specialist if they read the actual source. The move that fails is (b) — proceeding without reading the source is the exact failure mode the atom describes.
- **Variant B (cascading corrections):** After the specialist review, one of the corrections (bilateral consent) reveals that the upstream PACT (spec) has changed since the plan was written. The practitioner must now decide: is this a plan correction (fix the plan to match current spec), or an upstream brokerage issue (the spec change was not communicated to downstream consumers)? This links SC-B-004 to SC-B-002 (authority-chain escalation) and SC-B-009 (cross-SDK upstream brokerage).
