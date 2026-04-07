---
atom_id: SC-B-005
name: Spec-to-code conformance audit
craft_layer: brokerage
destination: co-codegen
craft_areas: [attend-when, institutional-memory]
applications: [COC, COR, COE]
spec_lineage:
  - PACT §40 — EATP Integration (Governance-Protocol Mapping)
  - PACT §10 — Monotonic Tightening Invariant
  - CO §49 — Quality Criteria — Enforcement Reliability
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md
    polarity: POSITIVE
    quote: "A red team audit of the PACT governance module against PACT-Core-Thesis.md revealed 4 spec-conformance gaps, filed as GitHub issues #199-#202. All 4 are areas where the Rust SDK (`kailash-rs`) is already compliant but the Python SDK is not."
  - path: loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md
    polarity: NEGATIVE
    quote: "The modules are structurally adjacent but functionally isolated. PACT manages organizational governance (D/T/R, envelopes, bridges) and emits its own audit trail. EATP manages trust lineage (genesis, capabilities, delegations) with cryptographic verification."
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md
    polarity: NEGATIVE
    quote: "Vacancy enforcement in kailash-governance is asymmetric: `check_access()` (knowledge access path) correctly blocks vacant roles at Step 0 of `can_access` (access.rs:112-119). `verify_action()` (action envelope path) has ZERO vacancy checks — a vacant role passes envelope verification."
practice_modality: brokerage-rep
status: draft
---

## The move

Read every MUST and SHOULD in the normative spec for a module, then grep for each one's implementation in the codebase. Produce a numbered gap list where each gap names the spec section, the expected enforcement, and the actual behaviour found (or not found). File each gap as a trackable issue against the implementing repo.

## When it fires

After a module reaches "functional complete" status — tests pass, features work, the team believes it is done. The trigger is the claim of completeness, not a calendar date. Also fires when a cross-SDK comparison reveals that one SDK implements a spec requirement that another does not.

## What it serves

PACT §40 (EATP Integration) requires that every PACT governance action creates a corresponding EATP record. CO §49 (Quality Criteria — Enforcement Reliability) demands that critical rules have hard enforcement, verified by intentionally attempting to violate them. PACT §10 (Monotonic Tightening Invariant) requires write-time rejection of envelope widening across all dimensions — not just the dimensions the implementer remembered. A conformance audit is the move that detects the gap between what the spec mandates and what the code actually enforces.

## Evidence walkthrough

- **kailash-py 0014** (POSITIVE) — Demonstrates the move executed well. The agent read PACT-Core-Thesis.md section by section, then audited the Python PACT module against each normative requirement. Result: four numbered gaps (EATP record emission, write-time tightening missing three dimensions, compile_org silent drop, vacancy hard-block without interim envelope), each filed as a GitHub issue. The Rust SDK was already compliant on all four, proving the gaps were implementation omissions rather than spec ambiguities.
- **kailash-py 0015** (NEGATIVE) — Shows the cost of never performing this audit on the integration boundary. PACT and EATP lived in the same `trust/` package tree with zero imports between them. The normative "EVERY governance action creates an EATP record" requirement was never wired. The entry names the specific absent call chain: GovernanceEngine never instantiates a TrustStore and never calls TrustOperations.
- **kailash-rs 0001** (NEGATIVE) — Demonstrates the move missed on a single function. `verify_action()` had zero vacancy checks while `check_access()` correctly blocked vacant roles. A spec-to-code audit would have caught the asymmetry by checking that every path through the engine enforces the vacancy invariant, not just the knowledge-access path.

## How to drill it

Take a module with a published spec (PACT, EATP, or a smaller internal spec). Extract every MUST/SHOULD/MUST NOT into a numbered checklist. For each item, find the code location where enforcement should occur. Write the gap entry (or "confirmed: enforced at file:line") for each. Score the drill on coverage completeness — did the practitioner check all code paths, or only the obvious one? The kailash-rs 0001 vacancy case (two paths, one enforced, one not) is the canonical trap.

## Common failure mode

Practitioner reads the spec, spots three or four obvious gaps, and files issues without systematic coverage. The remaining gaps survive until the next red team or the next cross-SDK comparison. The fix is the checklist: every normative statement gets a row, even if the practitioner is confident it is implemented.

## Related atoms

- SC-B-003 (variant classification) — conformance audits surface global-vs-variant questions when one SDK is compliant and another is not.
- SC-B-001 (read-then-merge sync) — sync operations can propagate a conformant implementation from one repo to a non-conformant one, but only if the gap is known.
