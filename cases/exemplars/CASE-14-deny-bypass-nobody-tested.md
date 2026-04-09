---
case_id: CASE-14
source_path: loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md
title: "The Deny Bypass Nobody Tested"
atoms_served: [negative-test-for-deny-default, auth-enforcement-verification]
spec_concepts: [CO §49, CO §5, PACT §23, EATP §36]
craft_layer: artifact
recommended_use: assessment
destination: both
application: COC
---

# CASE-14: The Deny Bypass Nobody Tested

## 1. The Situation

The Kailash Rust SDK's Nexus module included RBAC (Role-Based Access Control) middleware in `crates/kailash-nexus/src/auth/rbac.rs`. The middleware supported a `deny_by_default` flag intended to control what happens when a route has no explicit permission mapping. When the flag was `true`, unmapped routes should be blocked -- only routes with explicit permission entries would be accessible. When `false`, unmapped routes would be allowed through. This is a standard security pattern: deny-by-default is the conservative posture, allow-by-default is the permissive one.

The middleware had been implemented, tested, and deployed. The test suite verified that mapped routes behaved correctly -- users with the right role could access protected endpoints, users without the right role were denied. The tests passed. The middleware was considered complete.

## 2. The Trigger

A security-reviewer agent during a post-implementation red team (Round 1) examined the `deny_by_default = true` branch at lines 296-305 of `rbac.rs`. The agent found that the `deny_by_default = true` branch and the `deny_by_default = false` branch executed identical code: both passed the request to the inner handler. The flag was completely meaningless. When `deny_by_default` was set to `true` and a route had no explicit permission mapping and no default permission, the request was allowed through -- the exact opposite of what the flag's name promised.

The attentional pattern is **negative-case verification**: the reviewer asked "what happens when the security flag is on and a route is NOT in the permission map?" The existing tests had only asked "what happens when a route IS in the permission map?"

## 3. The Move

The move is **negative-test-for-deny-default** combined with **auth-enforcement-verification**. The fix itself was minimal: change the `deny_by_default = true` branch to return `forbidden_response()` instead of passing through to the inner handler, and add a tracing debug log for visibility. The code change was a one-line behavioral fix.

The deeper skill is recognizing the class of bug. Auth middleware with deny-by-default semantics requires negative tests -- tests that confirm unmapped routes are blocked when the flag is on. The existing test suite had comprehensive positive tests (mapped routes work correctly) but zero negative tests (unmapped routes are denied). The test suite was testing the happy path of the security system, not the security guarantee itself. Writing a test that verifies "this request is denied" is as important as writing a test that verifies "this request succeeds."

## 4. The Outcome

The fix closed a CRITICAL authorization bypass. Before the fix, any route omitted from `route_permissions` by mistake was accessible to any authenticated user regardless of role. An admin-only endpoint that a developer forgot to add to the permission map would be wide open. After the fix, the `deny_by_default` flag behaved as documented: unmapped routes return a 403 Forbidden response.

The classification as CRITICAL was warranted because the vulnerability was silent -- no error, no log, no warning. A developer setting `deny_by_default = true` would believe they had a secure-by-default posture. Every unmapped route would appear to work correctly in normal testing because authorized users could still access it. Only an attacker (or a red team) probing with an unauthorized user on an unmapped route would discover the bypass.

## 5. The Spec Connection

**CO §49 (Red Team Validation)** requires that artifacts be validated adversarially. This case demonstrates why: the middleware passed all its functional tests. Only a red team review that asked "what if the security flag is on and the route is missing from the map?" found the bypass. Functional testing validated that the code worked; red team validation revealed that the security guarantee was hollow.

**CO §5 (Guardrails)** states that constraints must be enforced automatically. The `deny_by_default` flag was supposed to be a guardrail -- it should have automatically blocked unmapped routes without requiring the developer to remember to add every route to the permission map. The implementation reduced the guardrail to a label: the flag existed, it was set to `true`, but the enforcement was identical to `false`. A guardrail that does not change behavior is not a guardrail.

**PACT §23 (Verification Gradient)** defines different levels of verification rigor for different trust levels. Authorization middleware sits at the highest trust level -- it is the boundary between authenticated and authorized access. The verification gradient principle says that higher-trust boundaries require more rigorous verification. The test suite applied standard functional verification (positive tests only) to a boundary that required adversarial verification (positive and negative tests). The verification was under-calibrated for the trust level.

**EATP §36 (Default Deny Posture)** specifies that the default security posture should deny access unless explicitly granted. The implementation violated this principle at the most literal level: the code path labeled "deny by default" did not deny. The spec principle exists precisely because this class of bug is common -- developers frequently implement allow-by-default behavior while believing they have implemented deny-by-default, and only negative tests can catch the discrepancy.

## 6. Discussion Questions

1. The bug was that the `deny_by_default = true` and `deny_by_default = false` branches had identical code. A linter or static analysis tool could detect "two branches of a conditional that do the same thing." Should this class of check be standard for security-critical code paths? What other static patterns would catch similar authorization bugs?

2. The test suite had zero negative tests for the deny-by-default flag. Write the test case (in pseudocode) that would have caught this bug. What is the minimum set of negative tests that any deny-by-default authorization system should have?

3. The fix was a one-line change (return `forbidden_response()` instead of passing through). The discovery was a multi-hour red team review. What does the asymmetry between discovery cost and fix cost tell you about where to invest in security -- in code review, in testing, or in red team exercises?

4. The vulnerability was silent -- no errors, no logs, no warnings. If you were designing an authorization middleware from scratch, how would you make deny-by-default failures visible during development without creating noise in production?
