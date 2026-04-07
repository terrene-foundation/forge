---
atom_id: SC-A-006
name: Negative tests for deny-by-default
craft_layer: artifact
destination: both
craft_areas: [attend-how, intervene-how]
applications: [COC]
spec_lineage:
  - CO §49 — Quality Criteria — Enforcement Reliability
  - PACT §23 — Blocked Zone
journal_evidence:
  - path: loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md
    polarity: NEGATIVE
    quote: "In crates/kailash-nexus/src/auth/rbac.rs:296-305, when deny_by_default = true and a route had no explicit permission mapping and no default permission, the code allowed the request through instead of denying it. Both the deny_by_default = true and deny_by_default = false branches executed identical code (passing to inner handler), making the flag meaningless."
  - path: loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md
    polarity: NEGATIVE
    quote: "--timeout and --warning-threshold CLI params not validated with math.isfinite(). Budget is validated but these two aren't. NaN could bypass governance thresholds."
practice_modality: drill
status: verified
---

## The move

When implementing auth middleware, governance gates, or any enforcement point with "deny by default" semantics, write the negative test first: a test that confirms an unmapped route, an unrecognized role, or an unvalidated input is blocked. Positive tests (mapped routes succeed, known roles pass) cannot detect a deny-by-default inversion because they exercise only the allow path. The negative test is the only test that verifies the enforcement mechanism's raison d'etre — that the default is actually denial, not passage.

## When it fires

Every time a new enforcement point is created or an existing one is modified. The trigger is the word "default" in the enforcement semantics — "deny by default", "fail closed", "block unless explicitly allowed." If the semantics include a default-deny clause, the negative test is mandatory before the implementation is considered complete.

## What it serves

CO §49 (Quality Criteria — Enforcement Reliability) defines the assessment method: "Intentionally attempt to violate critical rules and verify hard enforcement catches it." This is exactly the negative test — an intentional violation attempt that must be caught. Without the negative test, enforcement reliability is assumed, not verified. PACT §23 (Blocked Zone) defines the expected behavior when an action is denied: "Cannot proceed; action denied; explanation logged." The negative test verifies both the denial and the logging.

## Evidence walkthrough

- **kailash-rs 0002** (NEGATIVE) — The RBAC middleware in `crates/kailash-nexus/src/auth/rbac.rs` had a `deny_by_default` flag that was semantically meaningless: both the `true` and `false` branches executed identical code (passing the request to the inner handler). A route with no explicit permission mapping was accessible to any authenticated user regardless of role. The entry records: "An admin-only endpoint omitted from route_permissions by mistake would be wide open." The existing tests only verified mapped routes — no test confirmed that an unmapped route was blocked. This is the canonical case: the positive tests all passed, and the enforcement flag was inert.
- **kaizen-py 0002** (NEGATIVE) — The security review found that `--timeout` and `--warning-threshold` CLI parameters were not validated with `math.isfinite()`, while `budget` was. A `NaN` value would bypass governance thresholds because `NaN` comparisons are always false — a deny condition expressed as `if value > threshold` silently passes when either operand is `NaN`. This is the same pattern at the input-validation level: the positive case (valid numbers) works, but the negative case (NaN, Inf) was never tested, and the enforcement silently inverts.

## How to drill it

Give the practitioner a minimal auth middleware implementation (15-20 lines) with a `deny_by_default` flag and a route-permission map. The implementation has the kailash-rs 0002 bug: both branches of the flag execute the same code. The drill: (a) write a test that exercises an unmapped route with `deny_by_default = true` and expects denial, (b) run the test and observe it fail, (c) fix the implementation, (d) run the test and observe it pass, (e) write a second negative test for an unknown role on a mapped route. Score on: (a) the first test is written before reading the implementation (test-first, not test-after), (b) the test asserts both the HTTP status (403) and the presence of a denial log entry, (c) the fix changes only the `deny_by_default = true` branch, not both branches.

## Common failure mode

Practitioner writes positive tests for all mapped routes and all known roles, sees 100% of tests pass, and declares the enforcement complete. The deny-by-default flag is never exercised with an unmapped route because the test author knows which routes exist and tests only those. The enforcement gap is invisible to coverage metrics — the middleware function has high line coverage because the allow path is exercised. The cure is the standing rule: every enforcement point with deny semantics must have at least one test that exercises the deny path with an input the enforcement has never seen.

## Related atoms

- SC-A-003 (defense-in-depth codification) — each enforcement surface in the defense-in-depth stack needs negative testing to verify it blocks what it claims; this atom provides the testing methodology.
- SC-A-004 (Layer 3 hook security audit) — hooks are enforcement mechanisms that assume correct behavior; negative tests are the verification method that hooks actually enforce what they claim.
- SC-A-005 (encapsulation as security boundary) — both atoms address default-open vs. default-closed semantics. Encapsulation is the struct-level version (private by default); negative tests are the middleware-level version (deny by default). Both require explicit testing of the "closed" path.
- SC-A-007 (zero-config security audit) — once a zero-config default is changed to deny, negative tests verify the deny posture holds across all convenience entry points.
- SC-P-010 (timing race condition red team) — enforcement mechanisms with deny semantics can be bypassed via timing (e.g., NaN comparisons silently passing governance thresholds); timing-aware negative tests are the mitigation.
