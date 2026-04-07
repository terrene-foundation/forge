---
atom_id: SC-B-009
name: Cross-SDK upstream brokerage
craft_layer: brokerage
destination: co-codegen
craft_areas: [intervene-how, institutional-memory]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - PACT §10 — Monotonic Tightening Invariant
journal_evidence:
  - path: loom/kz-engage/workspaces/herald/journal/0010-DISCOVERY-nexus-dedup-fingerprint-bug.md
    polarity: POSITIVE
    quote: "Herald's Telegram bot returned the same 'What is CO?' answer for every question. The user asked about anti-capture, CARE, EATP — all got the cached first response."
  - path: loom/kailash-py/workspaces/kailash/journal/0013-DISCOVERY-cross-sdk-pseudo-posture-shared-bug.md
    polarity: POSITIVE
    quote: "kailash-rs#118 ('pseudo' posture rejected) DOES exist in kailash-py — `TrustPosture('pseudo')` raised ValueError because the enum value was `'pseudo_agent'`, not `'pseudo'`. All other CARE-spec L1-L5 names parsed correctly."
practice_modality: brokerage-rep
status: draft
---

## The move

When a discovered bug crosses SDK language boundaries (the same defect exists in both the Python and Rust SDKs, or a defect in one SDK's shared framework component affects all consumers), file an issue against each affected repository rather than working around it locally. The upstream issue preserves the authority chain for the fix: the SDK maintainer decides the correct resolution, and all downstream consumers inherit it. A local workaround (disabling a feature, patching a single consumer) breaks the authority chain by silently diverging the consumer from the canonical SDK behaviour.

## When it fires

Whenever a bug discovered during application development traces back to an SDK-level component shared across multiple SDKs or multiple consumers. The specific trigger is: "the fix I am about to apply locally would need to be applied independently by every other consumer of this SDK component." If the answer is yes, the fix belongs upstream, not in the application. Also fires when cross-SDK issue triage reveals that a bug filed against one SDK also exists in the other.

## What it serves

CO §17 (Single Source of Truth) requires that institutional knowledge — including bug fixes — live in exactly one place. A local workaround creates a second source of truth for the bug's resolution, one that drifts from the SDK's eventual fix. PACT §10 (Monotonic Tightening Invariant) applies at the cross-repo level: the SDK's constraints should only tighten over time, and a local workaround that disables a feature (e.g., `enable_durability=False`) loosens the constraint for that consumer without signalling the loosening to the SDK. The `rules/zero-tolerance.md` Rule 4 (no workarounds for core SDK issues) is the enforcement rule: file the issue, do not work around it.

## Evidence walkthrough

- **kz-engage 0010** (POSITIVE) — Demonstrates the move executed correctly. The Nexus `DurableWorkflowServer` had a dedup fingerprint bug: every POST to the same endpoint got an identical fingerprint because the request body stream was consumed by earlier middleware. The local fix was `enable_durability=False`, but the entry also filed kailash-py#175 and kailash-rs#110 to fix the root cause upstream. The dual filing (Python + Rust) ensures both SDKs address the shared design flaw rather than leaving one with the silent regression.
- **kailash-py 0013** (POSITIVE) — Cross-SDK triage revealed that a Rust-filed issue (kailash-rs#118, "pseudo" posture rejected) also existed in Python. The Python `TrustPosture` enum accepted `"pseudo_agent"` but rejected the CARE-spec shorthand `"pseudo"`. The fix (adding a `_missing_` classmethod for alias resolution) was applied to kailash-py and filed as kailash-py#191. The entry also notes that three other cross-SDK issues (py#145-147) were already fixed, demonstrating the value of checking current codebase state rather than assuming filed issues are still live.

## How to drill it

Simulate a bug discovery in an application that uses the SDK. The bug traces to an SDK middleware component. The drill has two paths: (a) the practitioner applies a local workaround and moves on, or (b) the practitioner files an issue against the SDK repo, applies a temporary local mitigation with a comment linking to the issue, and notes the issue number in the session journal. Score on: did the practitioner file upstream? Did they file against all affected SDKs (not just the one they are using)? Did they tag the local mitigation as temporary with a link to the upstream issue?

## Common failure mode

Practitioner discovers the bug, applies a local workaround that fixes their immediate problem, and never files the upstream issue because "someone else will notice it." The workaround persists, the SDK ships without the fix, and other consumers hit the same bug independently. The kz-engage 0010 case is instructive: without the upstream filing, every Nexus consumer using durable mode with POST requests would have silently received cached responses, and each would have independently discovered and worked around the same fingerprint bug.

## Related atoms

- SC-B-002 (authority-chain escalation) — upstream brokerage is a form of authority-chain escalation: the fix belongs at the authority level (SDK repo), not at the consumer level.
- SC-B-005 (spec-to-code conformance) — cross-SDK upstream issues often surface during conformance audits when one SDK implements a spec requirement and the other does not.
