---
atom_id: SC-B-018
name: Separate the policy decision point from the policy enforcement point in access control
craft_layer: brokerage
destination: co-codegen
craft_areas: [intervene-how]
applications: [COC]
spec_lineage:
  - EATP §40 — Enforcement Model (PDP/PEP Separation)
  - CO §5 — Deterministic Enforcement Over Probabilistic Compliance
journal_evidence:
  - path: loom/kailash-py/workspaces/data-fabric-engine/journal/0009-DECISION-trusted-code-boundary.md
    polarity: POSITIVE
    quote: "Product functions are written by the application developer — the same person who writes @app.handler(), has SSH access to the server, and manages the database credentials. Restricting their access to sources would add complexity without security benefit."
  - path: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md
    polarity: NEGATIVE
    quote: "Single-gate governance (#234) — verify_action() called once at submit, not per-node. Envelope constraints (blocked_actions, temporal, compartments) bypassed after gate."
practice_modality: brokerage-rep
status: verified
---

## The move

When building or auditing access control in any governance-bearing module, verify that the layer that decides whether an action is permitted (the policy decision point, PDP) is a distinct module from the layer that blocks or allows the action at runtime (the policy enforcement point, PEP). The PDP produces a verdict (auto-approved, flagged, held, blocked). The PEP consumes that verdict and acts on it. If the two are merged into one function or class, split them. If they are already split, verify that the PEP cannot be bypassed by calling the underlying operation directly — every code path through the system must pass through a PEP that consults the PDP.

## When it fires

During implementation or audit of any module that enforces governance constraints: envelope checks, role-based access, trust verification, action authorization. The trigger is the presence of a check that both decides and enforces in the same function body — an `if` statement that evaluates policy and then either proceeds or raises in the same block. Also fires when a code path exists that reaches a governed operation without passing through any enforcement gate at all.

## What it serves

EATP §40 (Enforcement Model) requires that VERIFY produces a VerificationResult as a PDP, with enforcement as a separate concern — either a StrictEnforcer (blocks unauthorized actions in production) or a ShadowEnforcer (observes without blocking, for rollout). The separation means the PDP can be tested by inspecting its verdicts without triggering enforcement side effects, the PEP can be swapped between strict and shadow mode without changing the decision logic, and auditors can review the decision logic independently of the enforcement mechanism. CO §5 (Deterministic Enforcement Over Probabilistic Compliance) requires that critical rules be enforced by mechanisms operating outside the AI's context window. A merged PDP/PEP makes it impossible to verify that enforcement is deterministic because the decision and the enforcement are tangled — you cannot test one without triggering the other.

## Evidence walkthrough

- **data-fabric-engine 0009** (POSITIVE) — The entry records a deliberate PDP/PEP separation in the Data Fabric Engine. The PDP is the `trusted_code_patterns` list: a declarative policy that says "product functions written by the application developer are trusted code." The PEP is the DataFlow endpoint guard that decides at runtime whether a given function call is within the trusted boundary. The entry explicitly rejects merging these layers: hard access control (PEP blocking undeclared sources) was considered and rejected because the PDP already classifies product functions as trusted, and a hard block would add complexity without security benefit. The decision is clean because the two layers are reasoned about independently — the trust classification is one decision, the enforcement mechanism is another.
- **gh-issues 0001** (NEGATIVE) — The entry documents four security invariant violations in the PACT (primitives) engine, the first of which is a textbook PDP/PEP merge failure. `verify_action()` is called once at submit time (acting as both PDP and PEP in a single gate), and then the workflow proceeds through multiple nodes without further enforcement. Envelope constraints — blocked_actions, temporal windows, compartment boundaries — are bypassed after the initial gate because no PEP exists at the per-node level. The PDP made its decision at submit; no PEP re-checks that decision as the execution proceeds through nodes that may violate the constraints the PDP evaluated. A properly separated architecture would have the PDP produce a verdict at submit and a PEP enforce that verdict at every node boundary.

## How to drill it

Present the practitioner with a governance module that has a single `authorize_and_execute(action, role)` function. The function checks the role's envelope, checks temporal constraints, and if all pass, executes the action. Ask the practitioner to (1) identify the PDP and PEP boundaries within the merged function, (2) refactor into two modules — a `PolicyDecider.evaluate(action, role) -> Verdict` and an `Enforcer.enforce(verdict, action)`, and (3) write a test for the PDP that verifies a blocked verdict is returned for an out-of-envelope action without any side effects from execution. Score: does the practitioner produce a PDP that returns a data structure (not a raised exception) and a PEP that is the only call site for the actual action? Does the refactored PEP handle all four verdict types (auto-approved, flagged, held, blocked)?

## Common failure mode

Practitioner separates the PDP and PEP at the primary call site but leaves a secondary code path that calls the governed operation directly — typically an admin endpoint, a batch job, or a migration script. The secondary path bypasses the PEP entirely, meaning the PDP's verdict is irrelevant for that path. The fix is a code-level audit: grep for every call site of the governed operation and verify that each one passes through a PEP. The gh-issues 0001 per-node bypass is exactly this pattern at the workflow level.

## Related atoms

- SC-B-005 (spec-to-code conformance) — a conformance audit would catch the single-gate bypass documented in gh-issues 0001; PDP/PEP separation is one of the specific checks that audit should include.
- SC-B-017 (verification-gradient zone assignment) — zone assignment determines the verification policy (PDP); the enforcement point (PEP) must match the assigned zone's strictness level.
- SC-A-006 (negative tests for deny-by-default) — the PEP's deny path must be tested with negative tests; a PEP that defaults to allow when the PDP is unreachable is a deny-by-default inversion.
- SC-A-008 (rule classification critical vs advisory) — whether a rule is critical or advisory determines whether the PEP must be strict (block) or can be shadow (observe). The PDP/PEP separation is a prerequisite for making that distinction actionable.
- SC-P-010 (timing race condition red team) — timing attacks exploit the gap between PDP decision and PEP enforcement; if the two are separated, the gap must be minimized or made atomic.
