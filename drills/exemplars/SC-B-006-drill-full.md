---
drill_id: DR-SC-B-006-FULL
source_atom: SC-B-006
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC, COR, COE]
---

# Multi-Round Red Team to Numeric Convergence

## Setup

The practitioner receives a **red team log** from a plan review. The plan covers a 12-workspace implementation with 8 modified files per workspace. Round 1 has been completed by three independent red team agents (architecture, security, specification compliance). Their combined findings are:

**Round 1 findings (18 total):**

| #   | Severity | Finding                                                                                                | Source          |
| --- | -------- | ------------------------------------------------------------------------------------------------------ | --------------- |
| 1   | CRITICAL | TrustStore API returns Option<Posture> but plan assumes Posture — unwrap will panic on missing entries | spec-compliance |
| 2   | CRITICAL | Bridge bilateral consent not validated — only LCA approval checked                                     | spec-compliance |
| 3   | CRITICAL | Session token stored in localStorage, not httpOnly cookie                                              | security        |
| 4   | HIGH     | Pool exhaustion under concurrent requests — no connection limit configured                             | architecture    |
| 5   | HIGH     | Delegation record missing cascade revocation when parent revoked                                       | spec-compliance |
| 6   | HIGH     | No rate limiting on authentication endpoint                                                            | security        |
| 7   | HIGH     | Envelope dimension check covers 4 of 7 dimensions (missing Temporal, Data Access, Communication)       | spec-compliance |
| 8   | HIGH     | Shared mutable state between async handlers without synchronization                                    | architecture    |
| 9   | HIGH     | Error responses leak internal stack traces                                                             | security        |
| 10  | MEDIUM   | Inconsistent naming: `verify_action` vs `check_access` for similar operations                          | architecture    |
| 11  | MEDIUM   | Missing index on `delegation_records.parent_id` — query performance on large tables                    | architecture    |
| 12  | MEDIUM   | Healthcheck endpoint returns 200 even when database is unreachable                                     | architecture    |
| 13  | MEDIUM   | Log messages contain full envelope contents including financial limits                                 | security        |
| 14  | MEDIUM   | Test fixtures use hardcoded timestamps that will fail after 2027                                       | architecture    |
| 15  | MEDIUM   | OpenAPI spec missing 403 response codes for governed endpoints                                         | spec-compliance |
| 16  | LOW      | README installation section references deprecated `pip install` instead of `uv sync`                   | architecture    |
| 17  | LOW      | Copyright header missing from 3 new files                                                              | architecture    |
| 18  | LOW      | Spelling error in error message: "Authenication failed"                                                | architecture    |

The plan author has produced **Round 2 responses** addressing all 18 findings. The practitioner must now run Round 2 and determine whether the responses resolve the findings or create new issues.

**Round 2 responses from plan author:**

1. (#1) Changed to pattern match with explicit error on None: `posture.ok_or(Error::MissingPosture)?`
2. (#2) Added bilateral consent check: both endpoint roles must approve bridge creation
3. (#3) Moved to httpOnly secure cookie with SameSite=Strict
4. (#4) Added connection pool with max_connections=25, acquire timeout 5s
5. (#5) Added recursive cascade: revoking parent revokes all children depth-first
6. (#6) Added rate limiter: 5 attempts per minute per IP, sliding window
7. (#7) Added Temporal, Data Access, Communication dimension checks to `validate_tightening()`
8. (#8) Wrapped shared state in `Arc<RwLock<T>>` with read-priority locking
9. (#9) Error responses now return generic message; full trace to structured log only
10. (#10) Renamed `check_access` to `verify_access` for consistency
11. (#11) Added composite index on `(parent_id, created_at)`
12. (#12) Added database connectivity check to health endpoint; returns 503 if unreachable
13. (#13) Envelope financial limits redacted in logs; replaced with hash
14. (#14) Changed to relative timestamps: `Utc::now() + Duration::days(30)`
15. (#15) Added 403 responses to all governed endpoints in OpenAPI spec
16. (#16) Updated to `uv sync`
17. (#17) Added copyright headers
18. (#18) Fixed spelling

## Task

1. For each of the 18 Round 2 responses, classify as one of:
   - **RESOLVED** — the response fully addresses the finding
   - **PARTIALLY RESOLVED** — the response addresses the symptom but not the root cause, or introduces a new concern
   - **NEW ISSUE** — the response creates a new problem not present in Round 1

2. Count findings remaining by severity after Round 2.

3. Determine whether convergence has been achieved. Convergence requires: (a) zero CRITICAL findings remaining, (b) monotonic decrease in total finding count, (c) no NEW ISSUE at equal or higher severity than the finding it resolves.

4. If convergence is not achieved, specify what Round 3 must address.

## Model Answer

| #   | Disposition        | Rationale                                                                                                                                                                    |
| --- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | RESOLVED           | Pattern match with explicit error is the correct fix for Option unwrap.                                                                                                      |
| 2   | RESOLVED           | Bilateral consent directly addresses the spec gap.                                                                                                                           |
| 3   | RESOLVED           | httpOnly + Secure + SameSite=Strict is the standard mitigation.                                                                                                              |
| 4   | PARTIALLY RESOLVED | max_connections=25 is arbitrary. No evidence this was sized for expected load. What happens when pool is exhausted — does the caller get a clear error or a hang?            |
| 5   | PARTIALLY RESOLVED | Recursive cascade is correct, but depth-first on a deep delegation tree could blow the stack. No depth limit mentioned. Also: what about concurrent revocations of siblings? |
| 6   | RESOLVED           | 5/minute sliding window is reasonable for auth endpoints.                                                                                                                    |
| 7   | RESOLVED           | Adding the 3 missing dimensions completes the 7-dimension check.                                                                                                             |
| 8   | PARTIALLY RESOLVED | `Arc<RwLock<T>>` with read-priority can starve writers under heavy read load. Should specify fairness policy or use tokio::sync::RwLock which is fair by default.            |
| 9   | RESOLVED           | Generic message + structured log is the correct pattern.                                                                                                                     |
| 10  | RESOLVED           | Consistent naming addresses the finding.                                                                                                                                     |
| 11  | RESOLVED           | Composite index on the right columns.                                                                                                                                        |
| 12  | RESOLVED           | 503 on database unreachable is correct.                                                                                                                                      |
| 13  | RESOLVED           | Hash redaction preserves debuggability without leaking limits.                                                                                                               |
| 14  | RESOLVED           | Relative timestamps eliminate the hardcoded expiry.                                                                                                                          |
| 15  | RESOLVED           | OpenAPI spec now documents the 403 responses.                                                                                                                                |
| 16  | RESOLVED           | Correct update.                                                                                                                                                              |
| 17  | RESOLVED           | Addressed.                                                                                                                                                                   |
| 18  | RESOLVED           | Addressed.                                                                                                                                                                   |

**Round 2 score: 15 RESOLVED, 3 PARTIALLY RESOLVED, 0 NEW ISSUE.**

**Severity after Round 2:** 0 CRITICAL, 2 HIGH (items 4, 5), 1 MEDIUM (item 8). Total: 3 findings remaining (down from 18).

**Convergence check:** (a) Zero CRITICAL remaining — PASS. (b) Monotonic decrease: 18 → 3 — PASS. (c) No new issues at equal or higher severity — PASS. **Convergence achieved at Round 2.** Round 3 would address the 3 remaining items but is not structurally required — the system is safe to proceed with the 3 partial resolutions tracked as follow-up issues.

## Scoring

| Criterion                                                                     | Points |
| ----------------------------------------------------------------------------- | ------ |
| Correctly identified all 15 RESOLVED items                                    | 3      |
| Correctly identified items 4, 5, 8 as PARTIALLY RESOLVED with valid rationale | 3      |
| Severity count after Round 2 is accurate                                      | 1      |
| Convergence determination is correct with all three criteria checked          | 2      |
| Round 3 scope (if applicable) is specific and minimal                         | 1      |
| **Total**                                                                     | **10** |

## Extensions

- **Variant A (harder):** Add a Round 2 response that introduces a NEW ISSUE (e.g., the rate limiter stores IP addresses in an unencrypted log, creating a GDPR concern). Practitioner must catch it and reclassify convergence.
- **Variant B (meta):** After completing the drill, the practitioner writes a 3-sentence convergence declaration suitable for a decision log entry. Compare against the declaration pattern in loom-meta 0039.
