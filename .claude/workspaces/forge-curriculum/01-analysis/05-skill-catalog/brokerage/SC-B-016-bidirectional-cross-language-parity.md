---
atom_id: SC-B-016
name: Audit cross-language parity bidirectionally — the other SDK may be the reference
craft_layer: brokerage
destination: co-codegen
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - CO §29 — Evidence-Based Completion
journal_evidence:
  - path: loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md
    polarity: POSITIVE
    quote: "After auditing all 30 source files (21,835 lines) in kailash-nexus and the related event systems in kailash-core, the Rust Nexus does not merely match the Python model -- it exceeds it in 4 of 5 abstractions."
practice_modality: brokerage-rep
status: draft
---

## The move

When auditing cross-language parity between two SDK implementations, do not assume a fixed direction (Language A is always the reference, Language B always follows). Audit bidirectionally: for each abstraction, determine which implementation is more complete, more type-safe, or more featureful. The audit may reveal that the "follower" SDK is ahead — making it the reference for that abstraction. Record the parity direction per-abstraction, not per-SDK.

## When it fires

Any cross-SDK parity audit, alignment review, or feature-gap analysis that compares two implementations of the same spec. The trigger is "I am about to list what B needs to adopt from A" — the move is to first check whether A needs to adopt anything from B. The assumption that parity flows in one direction is the default mental model; this move overrides it with evidence.

## What it serves

CO §17 (Single Source of Truth) requires that institutional knowledge live in exactly one place. In a cross-SDK context, the "one place" for a given abstraction is whichever implementation is more complete — not whichever SDK was started first. Assuming a fixed parity direction makes the wrong SDK the source of truth for abstractions where it is behind. CO §29 (Evidence-Based Completion) requires evidence for completion claims, not just assertions. Claiming "B is at parity with A" without checking whether A should adopt from B is an assertion without evidence — the completion claim applies to the wrong baseline.

## Evidence walkthrough

- **nexus-parity 0001-DISCOVERY** (POSITIVE) — The brief framed the audit as a parity concern: Rust's 4-abstraction Nexus model vs Python's 5-abstraction model, with an estimated 2 sessions to close the gap. The actual audit found that Rust exceeds Python in 4 of 5 abstractions: trait-based handler system is more type-safe, Rust has 3 channels vs Python's 2, Rust has two event systems (lightweight lifecycle bus + full domain event bus) while Python has one, and Rust adds dependency resolution and hot-reload to its plugin system. The only gap was BackgroundService (~360 lines). The entry explicitly records the consequence: "Python B2 will adopt Rust DomainEventBus semantics." The parity direction reversed — Rust became the reference for EventBus, not the follower. The estimated effort dropped from 2 sessions to 1 session because the audit discovered there was almost nothing to implement in the "follower" direction.

## How to drill it

Present the practitioner with two mock SDK implementations (SDK-Alpha and SDK-Beta) of a spec with 5 abstractions. SDK-Alpha was started 6 months earlier and is assumed to be the reference. For each abstraction, provide a feature-completeness summary. Seed the data so that SDK-Beta exceeds SDK-Alpha in 2 of 5 abstractions. Ask the practitioner to produce a parity report. Score: does the report identify per-abstraction parity direction (not a blanket "Beta follows Alpha")? Does it identify the 2 abstractions where Alpha should adopt from Beta? Does it adjust the effort estimate downward for the "follower" direction where the follower is already ahead?

## Common failure mode

Practitioner audits only in the assumed direction (what does B need from A?) and produces a gap list that omits the abstractions where A is behind. The parity plan then implements features in B that B already has (wasted work) while leaving A's gaps unaddressed. The bidirectional audit catches both directions in a single pass, producing a smaller and more accurate gap list.

## Related atoms

- SC-B-009 (cross-SDK upstream brokerage) — the governance mechanism for deciding which SDK's implementation becomes the upstream reference for a given abstraction.
- SC-B-005 (spec-to-code conformance) — the spec is the shared reference that both SDKs implement; the parity audit compares them against each other and against the spec.
