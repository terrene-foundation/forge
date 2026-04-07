---
atom_id: SC-B-011
name: Version tracking cascade across the repo chain
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - CO §13 — Layer 2 — Context
journal_evidence:
  - path: loom/journal/0015-DECISION-version-tracking-cascade.md
    polarity: POSITIVE
    quote: "Implemented a version cascade where each repo tracks its upstream's version and checks for updates on session start via GitHub raw URL fetch."
  - path: loom/journal/0022-DECISION-sdk-version-awareness.md
    polarity: POSITIVE
    quote: "Link COC artifact versions to SDK code versions by having /codify stamp sdk_version (from pyproject.toml/Cargo.toml) into proposals, and /sync cross-check it at Gate 1."
  - path: loom/journal/0023-DECISION-sync-sdk-dependency-pins.md
    polarity: POSITIVE
    quote: "Added step 9 to /sync Gate 2: after updating VERSION, read all Kailash package versions from the BUILD repo's pyproject.toml/Cargo.toml (root + workspace members) and update the template's dependency pins to match."
practice_modality: brokerage-rep
status: draft
---

## The move

Wire every repo in the distribution chain so it knows (a) its own version, (b) its upstream's version, and (c) the SDK version the artifacts were authored against. Surface version mismatches at session start and at sync gates so that stale artifacts are never silently applied to a newer SDK and newer artifacts are never silently applied to a project pinned to an older SDK.

## When it fires

At three moments: session start (does this repo's upstream have a newer version?), /codify (stamp the SDK version the artifacts were authored against into the proposal), and /sync Gate 1 + Gate 2 (cross-check proposal SDK version against BUILD repo version; update template dependency pins). The trigger is any transition between "authoring artifacts" and "distributing artifacts."

## What it serves

CO §17 (Single Source of Truth) requires no contradictions in institutional knowledge. A VERSION file that disagrees with pyproject.toml is a contradiction at the identity level — the repo does not know what version of itself it is, and downstream consumers inherit that confusion. CO §13 (Layer 2 — Context) requires that the master directive document be loaded at start of every session; version awareness is a sub-requirement — the session context must include which SDK version the artifacts assume, or the context is incomplete.

## Evidence walkthrough

- **0015-DECISION-version-tracking-cascade** (POSITIVE) — Records the implementation of a cascade chain: co/ v1.0.0 -> co-template, kailash/ -> kailash-coc-claude-py/rs -> downstream projects. Each repo has a `.claude/VERSION` (JSON) with own version + upstream version + changelog, checked at session start via curl with a 3-second timeout. Alternatives considered included git submodules (too heavy) and manual changelog checking (humans forget).
- **0022-DECISION-sdk-version-awareness** (POSITIVE) — Links COC artifact versions to SDK code versions. Before this decision, two version tracks existed independently: `.claude/VERSION` (COC artifact currency) and `pyproject.toml` (SDK release). When /codify runs after implementing SDK v2.2.1 features, the resulting artifacts reference APIs that may not exist in projects still on v2.0.0. The fix was to stamp sdk_version into proposals and cross-check at Gate 1.
- **0023-DECISION-sync-sdk-dependency-pins** (POSITIVE) — Extends the cascade to dependency pins in USE template repos. The py template had `kailash>=2.2.1` while the BUILD repo was at 2.3.0. /sync Gate 2 now reads BUILD repo versions and updates template dependency pins so that downstream projects scaffolded from the template get correct minimum version pins.

## How to drill it

Set up a three-repo chain: source S (version 2.3.0 in pyproject.toml, VERSION says 1.5), template T (version 1.4 in VERSION, dependency pin says kailash>=2.0.0), downstream D (version 1.3 in VERSION). Run through the three trigger points: (1) session start on D — what mismatch should surface? (2) /codify on S — what should the proposal stamp? (3) /sync from S through T — what should Gate 2 update in T's pyproject.toml? Score: does the practitioner identify all three version tracks (COC artifact, SDK release, dependency pin) and the specific mismatch at each gate?

## Common failure mode

Practitioner maintains the VERSION file but forgets to link it to the SDK version in pyproject.toml, creating two independent version tracks that drift silently. The artifact-level VERSION looks current, but the artifacts reference API features that do not exist in the SDK version the downstream project actually uses. The cascade must link all three tracks, not just one.

## Related atoms

- SC-B-010 (drift detection audit) — version drift is a special case of cross-repo divergence; the audit catches it, the cascade prevents it.
- SC-B-001 (read-then-merge sync) — the sync mechanism that the version check gates.
