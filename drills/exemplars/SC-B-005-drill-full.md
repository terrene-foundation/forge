---
drill_id: DR-SC-B-005-FULL
source_atom: SC-B-005
difficulty: intermediate
time_box_minutes: 30
---

# Spec-to-Code Conformance Audit

## Setup

The practitioner receives two documents:

**Document 1: A normative spec excerpt** (modelled on PACT-Core-Thesis.md, Sections 4.2, 4.4, and 5.7). The excerpt contains 8 normative statements, each marked MUST or SHOULD:

1. MUST: Every governance action creates a corresponding EATP record (GenesisRecord, DelegationRecord, or CapabilityAttestation).
2. MUST: Write-time tightening validation checks all 7 envelope dimensions (Financial, Confidentiality, Operational, Delegation, Temporal, Data Access, Communication).
3. MUST: `compile_org()` auto-creates a vacant head for any department/team without one, rather than silently dropping the unit.
4. SHOULD: `create_bridge()` validates bilateral consent from both endpoint roles, not just LCA approval.
5. MUST: Bridge scope is validated against role envelopes of both endpoints.
6. MUST: `verify_action()` blocks vacant roles from executing actions within their envelope.
7. MUST: Vacancy handling provides an interim envelope (degraded-but-operational) during the designation deadline window.
8. SHOULD: The designation deadline is configurable, not hardcoded to 24 hours.

**Document 2: Three source files** from a governance module (simplified versions of real files from kailash-py's PACT primitives):

- `engine.py` (~200 lines) -- contains `verify_action()` and `check_access()` functions. `check_access()` has a vacancy check at line 112-119. `verify_action()` has no vacancy check.
- `envelopes.py` (~150 lines) -- contains `validate_tightening()` that checks 4 of 7 dimensions (Financial, Confidentiality, Operational, Delegation). Temporal, Data Access, and Communication dimensions have runtime intersection functions (lines 80-110) but are not called during write-time validation.
- `governance.py` (~100 lines) -- contains `compile_org()` that silently drops departments without a head, and `create_bridge()` that validates LCA approval but not bilateral consent. Zero imports from any EATP module.

## Task

1. Extract each MUST and SHOULD from the spec excerpt into a numbered checklist row. For each row, record the spec section reference, the normative keyword (MUST or SHOULD), and the expected enforcement behaviour.
2. For each checklist row, search the three source files for the code location where enforcement should occur. Record one of three dispositions: CONFIRMED (code enforces the spec requirement, with file:line reference), GAP (code does not enforce the requirement, with the specific missing behaviour), or PARTIAL (code partially enforces, with what is present and what is missing).
3. For requirements disposed as GAP or PARTIAL, write a one-paragraph gap entry naming: the spec section, the expected behaviour, the actual behaviour found, and the impact of the gap.
4. Produce a summary gap list ordered by severity (MUST gaps first, SHOULD gaps second).
5. For each gap, draft a one-sentence issue title suitable for filing in a GitHub issue tracker.

## Model Answer

| # | Spec Requirement | Disposition | Evidence |
|---|---|---|---|
| 1 | EATP record emission | GAP | `governance.py` has zero imports from any EATP module. GovernanceEngine never instantiates a TrustStore. No call chain from governance actions to EATP records exists. |
| 2 | Write-time tightening (7 dims) | PARTIAL | `envelopes.py:validate_tightening()` checks 4 of 7 dimensions. Temporal, Data Access, and Communication have runtime intersection functions (lines 80-110) but these are not called during write-time validation. |
| 3 | `compile_org()` vacant head | GAP | `governance.py:compile_org()` silently drops departments without a head role. No auto-creation of vacant heads. |
| 4 | Bilateral consent | GAP | `governance.py:create_bridge()` validates LCA approval at line ~85 but has no consent mechanism for endpoint roles. |
| 5 | Bridge scope vs envelopes | GAP | No envelope validation in `create_bridge()`. Bridge scope is accepted without checking role envelope boundaries. |
| 6 | `verify_action()` vacancy | GAP | `engine.py:verify_action()` has zero vacancy checks. Compare with `check_access()` at lines 112-119 which correctly blocks vacant roles. Asymmetric enforcement. |
| 7 | Interim envelope | GAP | `engine.py:check_access()` returns a hard block for any vacancy without designation. No interim envelope logic exists. |
| 8 | Configurable deadline | GAP | The 24-hour deadline is hardcoded. No configuration parameter exists. |

**Gap summary:** 6 full GAPS (items 1, 3, 4, 5, 6, 7), 1 PARTIAL (item 2), 1 SHOULD-level GAP (item 8). Zero CONFIRMED items. This matches the real findings from journal entries 0014 and 0001: the PACT Python module had systematic spec-conformance gaps despite passing all existing tests.

**Issue titles:**
- "PACT: GovernanceEngine never emits EATP records (normative per spec Section 5.7)"
- "PACT: validate_tightening() checks 4 of 7 envelope dimensions"
- "PACT: compile_org() silently drops headless departments instead of auto-creating vacant heads"
- "PACT: create_bridge() validates LCA but not bilateral consent"
- "PACT: Bridge scope not validated against role envelopes"
- "PACT: verify_action() does not check vacancy (check_access does)"
- "PACT: No interim envelope during vacancy designation window"
- "PACT: 24-hour vacancy deadline is hardcoded, not configurable"

## Scoring Criteria

1. **Checklist completeness** (pass/fail): The practitioner produces a row for every one of the 8 normative statements. Practitioners who spot 4-5 "obvious" gaps and stop without systematic coverage are failing -- the checklist discipline is the point.
2. **Asymmetry detection** (pass/fail): The practitioner identifies that `check_access()` enforces the vacancy check but `verify_action()` does not. This is the canonical trap from kailash-rs journal 0001: two code paths, one enforced, one not. Missing this gap is the single most common failure.
3. **Specificity of gap entries** (pass: 6/8 specific): Each gap entry names the file, the specific function, and the concrete missing behaviour. "EATP records not emitted" scores zero. "governance.py has zero imports from EATP modules; GovernanceEngine never instantiates TrustStore; no call chain exists from governance actions to GenesisRecord/DelegationRecord/CapabilityAttestation" scores full.
4. **MUST vs SHOULD ordering** (pass/fail): The summary orders MUST-level gaps before SHOULD-level gaps. Item 8 (configurable deadline, SHOULD) must appear below the MUST gaps.

## Common Mistakes

1. **Sampling instead of systematic coverage.** The practitioner reads the spec, notices 3-4 obvious gaps (EATP emission, vacancy check, tightening dimensions), and files issues without walking every normative statement. The remaining gaps survive. In the real case (journal 0014), a systematic audit of PACT-Core-Thesis.md found exactly 4 numbered gaps across the full spec; a sampling approach would have found 2 and missed the compilation and bridge gaps.

2. **Trusting existing tests as evidence of conformance.** The PACT Python module had 1,139 passing tests at the time of the audit. A practitioner who sees "tests pass" and infers "spec is satisfied" will miss every gap. Tests verify the implemented behaviour, not the specified behaviour. The spec-to-code audit exists precisely for the delta between what was specified and what was implemented.

3. **Checking only the "happy path" code location.** The vacancy gap (item 6) is invisible if the practitioner checks only `check_access()` (which correctly enforces vacancy) and assumes `verify_action()` does the same. The audit must check ALL code paths that the spec requirement could apply to, not just the first one found.

## Extensions

1. **Cross-SDK comparison.** Give the practitioner a second set of source files from the Rust implementation of the same module, where all 8 requirements are correctly enforced. The practitioner must produce a cross-SDK conformance matrix showing which requirements Python satisfies vs which Rust satisfies, and draft a cross-SDK brokerage issue for each Python-only gap. This mirrors the real finding from journal 0014: "All 4 are areas where the Rust SDK is already compliant but the Python SDK is not."

2. **Write the negative test.** For each GAP, the practitioner writes a test case that would fail against the current code and pass against a conformant implementation. For the vacancy asymmetry gap: call `verify_action()` with a vacant role and assert it raises a VacancyError. For the tightening gap: attempt to widen the Temporal dimension and assert rejection.

3. **Spec ambiguity detection.** Add a 9th normative statement that is genuinely ambiguous ("SHOULD support delegation chains of arbitrary depth"). The practitioner must identify this as untestable without clarification and write a spec-clarification request rather than a gap entry. The ability to distinguish gaps (spec is clear, code is wrong) from ambiguities (spec is unclear, code may be correct) is a higher-order skill.
