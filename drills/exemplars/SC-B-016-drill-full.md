---
drill_id: DR-SC-B-016-FULL
source_atom: SC-B-016
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Bidirectional Cross-Language Parity Audit

## Setup

The practitioner receives feature-completeness summaries for two SDK implementations of the same specification: **SDK-Alpha** (Python, started 8 months ago, 14 contributors, assumed to be the reference) and **SDK-Beta** (Rust, started 2 months ago, 3 contributors, assumed to be the follower). Both implement a governance engine specified in `PACT-Core-Thesis.md` with five abstractions.

**Abstraction 1 — Role Resolution:**

| Dimension          | SDK-Alpha (Python)                          | SDK-Beta (Rust)                                             |
| ------------------ | ------------------------------------------- | ----------------------------------------------------------- |
| Role types         | 4 (Owner, Delegate, Vacant, Observer)       | 4 (Owner, Delegate, Vacant, Observer)                       |
| Hierarchy depth    | Unlimited (recursive)                       | Unlimited (recursive)                                       |
| Vacancy handling   | Cascades to parent; no deadline enforcement | Cascades to parent; deadline enforcement via `VacancyTimer` |
| Resolution caching | None — re-resolves on every call            | LRU cache with configurable TTL                             |

**Abstraction 2 — Envelope Enforcement:**

| Dimension           | SDK-Alpha (Python)                                | SDK-Beta (Rust)                                                  |
| ------------------- | ------------------------------------------------- | ---------------------------------------------------------------- |
| Envelope dimensions | 4 (Financial, Operational, Temporal, Data Access) | 5 (Financial, Operational, Temporal, Data Access, Communication) |
| Enforcement mode    | Strict only                                       | Strict + Shadow (observe without blocking)                       |
| Nested envelopes    | Not supported                                     | Supported (child envelope inherits parent constraints)           |
| Decorator API       | `@envelope(financial=...)`                        | `#[envelope(financial = ...)]`                                   |

**Abstraction 3 — Action Verification:**

| Dimension           | SDK-Alpha (Python)    | SDK-Beta (Rust)                                                    |
| ------------------- | --------------------- | ------------------------------------------------------------------ |
| Verification points | 1 (submit-time only)  | 1 (submit-time only)                                               |
| Verdict types       | 2 (approved, blocked) | 4 (auto-approved, flagged, held, blocked)                          |
| Audit trail         | Log-based (stdout)    | Structured (typed `VerificationRecord` with timestamps)            |
| Bypass detection    | None                  | Compile-time check: governed operations must pass through verifier |

**Abstraction 4 — Delegation Chain:**

| Dimension            | SDK-Alpha (Python)               | SDK-Beta (Rust)                                                        |
| -------------------- | -------------------------------- | ---------------------------------------------------------------------- |
| Delegation depth     | Unlimited                        | 3 levels max (configurable)                                            |
| Revocation           | Immediate (no cascade)           | Cascade revocation (revoking a delegation revokes all sub-delegations) |
| Signature validation | HMAC-SHA256 on delegation record | Ed25519 on delegation record                                           |
| Temporal constraints | None                             | Delegation expiry with auto-revocation                                 |

**Abstraction 5 — Bridge Protocol:**

| Dimension        | SDK-Alpha (Python)                                  | SDK-Beta (Rust)                                          |
| ---------------- | --------------------------------------------------- | -------------------------------------------------------- |
| Bridge types     | 3 (data, action, observation)                       | 2 (data, action)                                         |
| Consent model    | Bilateral (both sides approve)                      | Unilateral (initiator only)                              |
| LCA verification | Full (lowest common ancestor computed and verified) | Partial (LCA computed but not verified against org tree) |
| Rate limiting    | Per-bridge token bucket                             | None                                                     |

The practitioner also receives a project brief stating: "SDK-Beta was created to bring the governance engine to Rust. SDK-Alpha is the reference. Estimate the effort to bring SDK-Beta to parity with SDK-Alpha. Expected: 2 sessions."

## Task

1. For each of the five abstractions, determine the **parity direction**: which SDK is the reference for that specific abstraction? Record it as "Alpha leads", "Beta leads", or "At parity".

2. Produce a **parity report table** with columns: Abstraction, Parity Direction, Gaps in the Follower, Adoption Recommendation.

3. Revise the project brief's effort estimate. The brief assumed 2 sessions of "Beta catching up to Alpha." Given your bidirectional audit, what is the actual effort and in which directions?

4. Identify the one abstraction where **neither SDK should be the reference** — both have gaps relative to the spec. What does the spec require that both implementations miss?

## Model Answer

**1. Parity direction per abstraction:**

| Abstraction          | Direction                                                    | Rationale                                                                                                                                                                                                                                    |
| -------------------- | ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Role Resolution      | **Beta leads**                                               | Beta has vacancy deadline enforcement and resolution caching — both absent in Alpha. The four role types and recursive hierarchy are at parity.                                                                                              |
| Envelope Enforcement | **Beta leads**                                               | Beta has 5 dimensions (vs 4), shadow enforcement mode, and nested envelopes — all absent in Alpha. Alpha's decorator API is equivalent in its 4-dimension scope but less featureful.                                                         |
| Action Verification  | **Beta leads**                                               | Beta has 4 verdict types (vs 2), structured audit trail (vs log-based), and compile-time bypass detection (vs none). The single verification point is a shared gap (see task 4).                                                             |
| Delegation Chain     | **Mixed — Beta leads on safety, Alpha leads on flexibility** | Beta's cascade revocation, temporal constraints, and Ed25519 signatures are security advances. But Alpha's unlimited delegation depth is more flexible, and for some deployments, the 3-level limit is too restrictive. Neither fully leads. |
| Bridge Protocol      | **Alpha leads**                                              | Alpha has 3 bridge types (vs 2 — observation bridge missing in Beta), bilateral consent (vs unilateral — a security regression in Beta), and rate limiting (vs none). Alpha's LCA verification is also more complete.                        |

**2. Parity report:**

| Abstraction          | Parity Direction | Gaps in Follower                                                                                                | Adoption Recommendation                                                                                                                                                 |
| -------------------- | ---------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Role Resolution      | Beta leads       | Alpha lacks: vacancy deadline enforcement, resolution caching                                                   | Alpha adopts Beta's `VacancyTimer` design and adds LRU cache. Estimated: 0.5 session.                                                                                   |
| Envelope Enforcement | Beta leads       | Alpha lacks: Communication dimension, shadow mode, nested envelopes                                             | Alpha adopts all three. Communication dimension is a spec requirement (PACT-Core-Thesis names five dimensions). Shadow mode enables safe rollout. Estimated: 1 session. |
| Action Verification  | Beta leads       | Alpha lacks: flagged/held verdict types, structured audit, bypass detection                                     | Alpha adopts 4-verdict model and structured records. Bypass detection is language-dependent (compile-time in Rust, runtime linting in Python). Estimated: 0.5 session.  |
| Delegation Chain     | Mixed            | Alpha lacks: cascade revocation, temporal constraints, Ed25519. Beta lacks: configurable depth beyond 3 levels. | Both adopt from each other. Alpha adds cascade revocation and expiry. Beta makes depth limit configurable (remove hardcoded 3). Estimated: 0.5 session each direction.  |
| Bridge Protocol      | Alpha leads      | Beta lacks: observation bridge type, bilateral consent, rate limiting, full LCA verification                    | Beta adopts all four. Bilateral consent is the highest priority — unilateral bridge creation is a security gap. Estimated: 1 session.                                   |

**3. Revised effort estimate:**

The brief estimated **2 sessions, one direction** (Beta catching up to Alpha). The actual effort:

- **Beta catching up to Alpha:** 1 session (Bridge Protocol + delegation depth configurability). Beta is only behind in one abstraction and one sub-dimension.
- **Alpha catching up to Beta:** 2 sessions (Role Resolution caching + Envelope Enforcement's three gaps + Action Verification's three gaps + Delegation Chain safety features). Alpha is behind in three full abstractions and one sub-dimension.

**Revised total: 3 sessions, both directions.** The brief's estimate was wrong in both magnitude (2 vs 3) and direction (unidirectional vs bidirectional). The brief assumed Beta was the follower everywhere; the audit shows Alpha is the follower in 3 of 5 abstractions. This matches the journal evidence for SC-B-016: the kailash-rs Nexus parity audit found that Rust exceeded Python in 4 of 5 abstractions, and the estimated effort dropped from 2 sessions to 1 session for the Rust direction while revealing work needed in the Python direction.

**4. Shared gap — Action Verification:**

Both SDKs have only 1 verification point (submit-time). The PACT-Core-Thesis specification requires per-node verification: the policy decision must be re-evaluated at each execution node, not just at submission. This is the single-gate governance gap documented in `gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md` — `verify_action()` is called once at submit, and envelope constraints (blocked_actions, temporal, compartments) are bypassed after the gate. Neither SDK should be the reference for verification-point count; both need per-node enforcement. This is a spec-to-code conformance gap (SC-B-005), not a parity gap.

## Scoring

| Criterion                                                                                              | Points |
| ------------------------------------------------------------------------------------------------------ | ------ |
| Correctly identifies Beta as leading in Role Resolution, Envelope Enforcement, and Action Verification | 2      |
| Correctly identifies Alpha as leading in Bridge Protocol                                               | 1      |
| Delegation Chain recognised as mixed direction (neither fully leads)                                   | 1      |
| Parity report includes per-abstraction adoption recommendations, not a blanket "Beta follows Alpha"    | 2      |
| Effort estimate revised in both magnitude and direction with specific session counts                   | 2      |
| Shared gap identified (single verification point) with reference to spec requirement                   | 2      |
| **Total**                                                                                              | **10** |

## Extensions

- **Variant A (priority sequencing):** Given the revised effort estimate (3 sessions, bidirectional), the programme owner can fund only 2 sessions. Which direction gets priority? Argue from risk: Alpha's security gaps (no cascade revocation, no bypass detection, 2-verdict model) vs Beta's functional gaps (missing observation bridge, no bilateral consent). Produce a prioritised 2-session plan.
- **Variant B (variant vs drift):** For each abstraction where the two SDKs differ, classify the difference as **intentional variant** (language-idiomatic design choice — should be preserved) or **unintentional drift** (one SDK is simply behind). The delegation chain's signature algorithm (HMAC-SHA256 vs Ed25519) is the ambiguous case: is it a variant (Python ecosystem prefers HMAC, Rust ecosystem prefers Ed25519) or drift (Ed25519 is objectively superior)?
- **Variant C (three-SDK extension):** A third SDK (Ruby) is starting from scratch. Using the parity report, which SDK should it use as reference for each abstraction? Produce a per-abstraction reference map: `{Role Resolution: Beta, Envelope Enforcement: Beta, Action Verification: Beta + spec, Delegation Chain: merge, Bridge Protocol: Alpha}`.
