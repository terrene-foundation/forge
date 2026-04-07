---
atom_id: SC-P-001
name: Identify adjacent-but-disconnected modules as spec conformance gaps
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - PACT §40 — EATP Integration (Governance-Protocol Mapping)
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md
    polarity: NEGATIVE
    quote: "The modules are structurally adjacent but functionally isolated. PACT manages organizational governance (D/T/R, envelopes, bridges) and emits its own audit trail. EATP manages trust lineage (genesis, capabilities, delegations) with cryptographic verification."
  - path: loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md
    polarity: NEGATIVE
    quote: "A red team audit of the PACT governance module against PACT-Core-Thesis.md revealed 4 spec-conformance gaps, filed as GitHub issues #199-#202. All 4 are areas where the Rust SDK (`kailash-rs`) is already compliant but the Python SDK is not."
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md
    polarity: NEGATIVE
    quote: "Vacancy enforcement in kailash-governance is asymmetric: `check_access()` (knowledge access path) correctly blocks vacant roles at Step 0 of `can_access` (access.rs:112-119). `verify_action()` (action envelope path) has ZERO vacancy checks — a vacant role passes envelope verification."
practice_modality: case
status: draft
---

## The move

When two modules live in the same package tree and serve complementary spec concepts, check whether they actually call each other. Structural adjacency (same directory, shared imports of parent types, similar naming) is not functional integration. Run a directed search: count the cross-module import lines, trace the call graph at the boundary, and compare the spec's normative integration requirements against what the code actually wires. Flag every place the spec says "MUST create a corresponding record in [the other module]" but the code has zero imports from it.

## When it fires

Three trigger conditions:

1. A spec-conformance audit names a "MUST" requirement that crosses module boundaries (e.g., PACT §40 requires every PACT governance action to create a corresponding EATP record).
2. A red team finds that two governance paths enforce the same invariant asymmetrically — one path checks, the other does not — because they were developed as independent features.
3. A cross-SDK parity audit reveals that one SDK implements the bridge between modules but the other SDK does not, despite both having the individual modules.

## What it serves

PACT §40 (EATP Integration) is the direct normative requirement: "Every PACT governance action — creating an envelope, granting clearance, computing an address, activating a bypass — must create a corresponding EATP record." When two modules satisfy their individual specs but never wire the bridge the spec requires, the system has all the pieces and none of the integration. CO §17 (Single Source of Truth) is the structural reason the gap persists — without a single authoritative integration point, each module's audit trail becomes an independent source of truth that can diverge.

## Evidence walkthrough

- **0015-CONNECTION-pact-eatp-bridge-never-wired** (NEGATIVE) — The PACT governance module and the EATP trust chain module sit in the same `trust/` package tree, have zero cross-imports, and each maintain their own `AuditAnchor` class with different semantics. The move was entirely missed: the spec's Section 5.7 calls the mapping "normative," but no code implements it.
- **0014-DISCOVERY-pact-spec-conformance-four-gaps** (NEGATIVE) — A systematic gap audit found four spec-conformance holes, all of which the Rust SDK already implemented. Gap 1 (EATP record emission) is the exact adjacent-but-disconnected pattern — the PACT module builds its own audit chain but never emits the EATP record types the spec requires. The module-level isolation was invisible until someone read every "MUST" in the spec and grep'd for its implementation.
- **0001-DISCOVERY-vacancy-asymmetric-enforcement** (NEGATIVE) — A different surface of the same pattern: `check_access()` enforces vacancy blocking but `verify_action()` does not, because the two code paths were developed independently. The invariant (vacant roles cannot execute) holds in one path and fails in the other, exactly because no one tested the cross-path interaction.

## How to drill it

Give the practitioner two module directories from a real codebase (e.g., `trust/pact/` and `trust/chain.py`) plus the relevant spec section that defines their integration contract. The practitioner must:

1. Count the cross-module import lines (target: exact number, not "a few").
2. List every normative "MUST" in the spec that requires Module A to call Module B or vice versa.
3. For each "MUST," produce a grep result showing whether the call exists in the code.
4. Write a one-paragraph finding for each missing integration, naming the spec section, the expected call site, and the consequence of the gap.

Score on completeness of the "MUST" enumeration, not on whether the practitioner found "a gap." Finding one gap is easy; finding all four (as in the kailash-py case) requires systematic spec-to-code tracing.

## Common failure mode

The practitioner sees that both modules exist and are individually well-tested (the PACT module had 1,139 tests) and concludes they are "integrated" because they share a package namespace. High test counts on individual modules actively mask integration gaps — each module's test suite validates its own invariants without ever crossing the boundary. The adjacent-but-disconnected pattern is specifically invisible to per-module test coverage metrics.

## Related atoms

- SC-B-001 (read-then-merge sync semantics) — a sync-level manifestation of the same pattern: two artifacts that should compose but are maintained independently.
- SC-A-001 (codify-with-explicit-NOT-codified) — the codify-phase move that would have caught the gap by asking "what integration contracts did we NOT codify?"
