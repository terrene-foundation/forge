---
drill_id: DR-SC-B-008-FULL
source_atom: SC-B-008
difficulty: intermediate
time_box_minutes: 30
modality: observation
application: [COC, COR, COE]
---

# Convergence-as-Validation Across Independent Reds

## Setup

The practitioner receives findings from **two independent red team agents** who reviewed the same implementation plan without seeing each other's work. The plan is for a new PACT governance module with 6 files and ~1,200 lines of code.

**Agent Alpha** (architecture specialist) produced 8 findings:

| #   | Severity | Finding                                                                                  |
| --- | -------- | ---------------------------------------------------------------------------------------- |
| A1  | CRITICAL | `verify_action()` does not check vacancy status — vacant roles can execute actions       |
| A2  | HIGH     | No connection pooling for GovernanceStore — each request opens a new connection          |
| A3  | HIGH     | `compile_org()` traverses the full org tree on every action verification (O(n) per call) |
| A4  | HIGH     | Bridge endpoints lack any form of rate limiting                                          |
| A5  | MEDIUM   | Error type is a single `GovernanceError` enum with 14 variants — should split by domain  |
| A6  | MEDIUM   | Integration tests use an in-memory store that doesn't enforce foreign key constraints    |
| A7  | LOW      | `DelegationRecord` Debug impl leaks envelope financial limits to logs                    |
| A8  | LOW      | No structured logging — all output via `println!`                                        |

**Agent Beta** (security specialist) produced 6 findings:

| #   | Severity | Finding                                                                                    |
| --- | -------- | ------------------------------------------------------------------------------------------ |
| B1  | CRITICAL | Vacant role check missing in `verify_action()` — governance bypass via vacant designation  |
| B2  | CRITICAL | `approve_bridge()` validates LCA but not bilateral consent — unilateral bridge creation    |
| B3  | HIGH     | `DelegationRecord` fields are `pub` — post-creation mutation bypasses signature validation |
| B4  | HIGH     | API key comparison uses `==` — timing side-channel leaks key length                        |
| B5  | MEDIUM   | Rate limiter absent on bridge creation endpoint — DoS via bridge spam                      |
| B6  | MEDIUM   | Debug output includes financial envelope limits in plaintext                               |

## Task

1. **Map convergent findings**: Identify findings that appear in both agents' reports (same underlying issue, possibly different framing). List each convergent pair with the common root cause.

2. **Map unique findings**: Identify findings that appear in only one agent's report. For each, explain why the other agent's specialisation would make them less likely to catch it.

3. **Calculate convergence metrics**:
   - Convergence rate: (convergent pairs) / (total unique issues)
   - Unique coverage contributed by Alpha only
   - Unique coverage contributed by Beta only

4. **Assess completeness**: Based on the convergence pattern, estimate whether a third agent with a different specialisation (e.g., specification compliance) would likely surface additional findings. Justify your estimate.

## Model Answer

**1. Convergent findings (3 pairs):**

| Alpha                             | Beta                               | Common Root Cause                                                                                                                            |
| --------------------------------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| A1 (vacancy check missing)        | B1 (vacancy bypass)                | `verify_action()` has no vacancy guard — both agents independently identified the same governance bypass                                     |
| A4 (bridge rate limiting)         | B5 (bridge DoS via spam)           | Bridge creation endpoint has no rate limiting — architecture sees it as missing infrastructure, security sees it as DoS vector               |
| A7 (Debug leaks financial limits) | B6 (financial limits in plaintext) | `DelegationRecord` Debug impl exposes sensitive envelope data — architecture sees it as a logging concern, security sees it as data exposure |

**2. Unique findings:**

**Alpha-only (5 findings):**

- A2 (no connection pooling): Infrastructure concern — security specialists focus on access control, not resource management
- A3 (O(n) org tree traversal): Performance concern — security specialists don't typically profile algorithmic complexity
- A5 (monolithic error enum): Code organisation — security specialists care about error handling behaviour, not error type design
- A6 (in-memory store skips FK constraints): Test infrastructure concern — security specialists test against the real system, not the test harness
- A8 (no structured logging): Operational concern — security specialists check what IS logged (sensitive data), not HOW it's logged

**Beta-only (3 findings):**

- B2 (bilateral consent missing in bridge): Governance-specific spec violation — architecture specialists verify structure, not spec-conformance of individual governance operations
- B3 (pub fields bypass signatures): Security-boundary violation at the language level — architecture specialists see field visibility as API design, not as a security boundary
- B4 (timing side-channel in key comparison): Low-level security vulnerability — architecture specialists don't test for timing attacks

**3. Convergence metrics:**

- **Total unique issues**: 3 convergent + 5 Alpha-only + 3 Beta-only = **11 unique issues**
- **Convergence rate**: 3 / 11 = **27%**
- **Alpha unique coverage**: 5 / 11 = **45%**
- **Beta unique coverage**: 3 / 11 = **27%**
- **Combined coverage**: 11 / 11 = **100%** (by definition, but the question is whether this is the full surface)

**4. Completeness assessment:**

A third agent (specification compliance) would likely surface **2-4 additional findings**. Rationale:

- Neither agent performed a systematic spec-to-code mapping. B2 (bilateral consent) was caught by security instinct, not by checking every PACT-Core-Thesis MUST statement. A spec-compliance agent would enumerate all normative requirements and disposition each one, likely catching gaps that neither architecture nor security would frame as their concern (e.g., EATP record emission, cascade revocation semantics, designation deadline configurability).
- The convergence rate of 27% is moderate — it suggests the two perspectives cover significantly different territory. Adding a third orthogonal perspective (spec compliance vs architecture vs security) would extend coverage further.
- **Calibration**: In the kz-engage case (journal evidence for this atom), two agents converged on 3/5 critical issues with 2 non-overlapping, giving a 60% convergence rate. This drill's 27% suggests wider specialisation gaps, making a third perspective more valuable.

## Scoring

| Criterion                                                                                           | Points |
| --------------------------------------------------------------------------------------------------- | ------ |
| All 3 convergent pairs correctly identified                                                         | 2      |
| Unique findings correctly attributed with valid specialisation reasoning                            | 3      |
| Convergence metrics calculated correctly                                                            | 2      |
| Completeness assessment references the coverage gap pattern, not just "more agents = more findings" | 2      |
| Calibration against known convergence rates (optional bonus)                                        | 1      |
| **Total**                                                                                           | **10** |

## Extensions

- **Variant A (meta):** The practitioner IS the third agent. Run a 15-minute spec-compliance review of the same plan and see how many additional findings surface. Compare actual yield against the estimate from step 4.
- **Variant B (COR application):** Two peer reviewers independently review the same research manuscript. Map their convergent and unique feedback. Does the convergence rate predict whether a third reviewer would add value?
