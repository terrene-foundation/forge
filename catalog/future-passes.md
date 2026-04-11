---
title: Future Application Passes — post-COC layer work queue
date: 2026-04-07
status: planning artifact, not authoring todo
destination: forge
---

# Future Application Passes

The current 57-atom catalog is the **COC layer** — craft evidence mined from the codegen application of CO. Per `rules/forge-scope.md` §2, all seven CO applications are in scope. Build order is usage-weighted: **COC → COR → COE → compliance → finance → governance → learners**. This file queues the next layers.

**This is a planning artifact, not an authoring todo.** It names what's next, not what's being worked on now.

## Cross-tagged atoms (already serve future applications)

18 atoms in the current catalog are tagged `applications: [COC, COR, COE]` — their moves transfer directly to research and education applications. Future COR and COE passes build on top of these, not alongside them:

**Brokerage (5)**:

- SC-B-005 Spec-to-code conformance audit — any spec compliance work
- SC-B-006 Multi-round red team to numeric convergence — universal red team craft
- SC-B-008 Convergence-as-validation across independent reds — universal
- SC-B-015 Reinterpret MUST rules from "human performs" to "human approves" — any rule system
- SC-B-017 Verification gradient zone assignment — PACT governance, applies to any CO app

**Artifact (1)**:

- SC-A-001 Codify-with-explicit-NOT-codified — the codify discipline

**Practitioner (12)**:

- SC-P-002 Empirical model-vs-rule ablation
- SC-P-007 Session completion state
- SC-P-011 Audit categorization blindness to progressive disclosure
- SC-P-012 Rule demotion vs strengthening judgment
- SC-P-013 Estimate self-contradiction as high-confidence signal
- SC-P-014 Zero-tolerance scope discipline
- SC-P-015 Ask for the contradictions
- SC-P-016 Detect overcorrection
- SC-P-018 Split the long-pole
- SC-P-020 Second occurrence as structural signal
- SC-P-023 Insert a gate, not a check
- SC-P-024 Fallback permanence detection

## Queued application passes

### Pass 2 — COR (CO for Research) ✅ COMPLETE (2026-04-08)

**Status**: Initial pass complete. 10 new atoms authored (SC-P-025 through SC-P-034). Workspace at `.claude/workspaces/forge-cor/`. See `catalog/README.md` Index 5 for the COR atom table.

**Spec source**: `terrene/foundation/docs/02-standards/co/applications/co-for-research.md`

**Estimated evidence sources**:

- Academic/thesis workspace at `terrene/foundation/workspaces/care-thesis/` (~66 entries flagged in D21 as spec-side evidence, not craft)
- Publications corpus at `terrene/foundation/docs/02-standards/publications/`
- Research-specific journal work in `loom/kailash-align/` (model evaluation, RLHF data collection) and `loom/kailash-ml/` (experiment tracking, ablation studies) — these are research-as-engineering, not research-as-scholarship, but may yield COR atoms
- Any future `terrene/research/` workspace journals

**Known coverage from cross-tagged atoms**: 18 atoms already tagged `[COC, COR, COE]` directly apply. Specifically for research: SC-P-002 (ablation), SC-P-011 (audit categorization), SC-P-013 (estimate contradiction), SC-P-022 (cross-model gap — especially relevant for research evaluations), SC-B-006 (multi-round red team).

**Estimated new atoms needed**: ~15-25 atoms covering research-specific craft not captured by the COC layer:

- Literature review as a move (not a phase)
- Prior-work ledger construction
- Reproducibility requirements as spec conformance
- Publication-ready vs research-internal artifact discrimination
- Citation as verification gradient
- Peer review as convergence-as-validation

**Readiness**: Medium. The CARE thesis workspace is a known source. A COR reverse-index pass analogous to the COC one (232 entries → 417 concepts → 30% coverage) would need to be built from scratch for COR sources.

### Pass 3 — COE (CO for Education) — coordinated from rr-coe

**Status**: Moved to `lyceum/courses/rr-coe/coe-application-pass.md` as of 2026-04-09. The COE application pass is now coordinated from the RR CoE course repo, where real teaching observations from Cohort 1 will supply first-person craft evidence. New COE atoms discovered during course design are proposed back to forge for authoring per lane isolation.

**Spec source**: `terrene/foundation/docs/02-standards/co/applications/co-for-education.md`

**Prep analysis**: `.claude/workspaces/forge-coe/coe-spec-index.md` enumerates 37 COE spec concepts. `.claude/workspaces/forge-coe/coe-coverage-map.md` maps the 24 cross-tagged atoms against those concepts: 5 COVERED, 18 PARTIAL, 14 GAP.

**Known coverage**: 24 atoms already tagged COE (20 cross-tagged `[COC, COR, COE]` + 4 COR atoms also tagged COE). 14 spec concepts have no existing atom. Critical gaps: COE Spectrum (4 levels), assessment workflow, assessment agent design, deliberation-log-as-assessment. See `rr-coe/coe-application-pass.md` for coordination and estimated 14-17 new atoms needed.

**Readiness**: Medium. Cohort 1 instruction (starting soon) will produce the craft journal evidence. Prep analysis identifies exactly which gaps need evidence — Cohort 1 journals should target the 7 gap clusters listed in the coverage map.

### Pass 4 — Compliance

**Spec source**: `terrene/foundation/docs/02-standards/co/applications/co-for-compliance.md`

**Known coverage**: SC-A-007 (zero-config security audit), SC-A-006 (negative tests for deny-by-default), SC-B-018 (PDP/PEP separation) are compliance-relevant even though tagged [COC] only.

**Estimated new atoms**: ~10-15 atoms covering regulatory mapping, audit trail construction, attestation craft.

**Readiness**: Low. No known workspace corpus. Would build from compliance framework documents (SOC2, ISO27001, GDPR) mapped to CO five-layer architecture.

### Pass 5 — Finance

**Spec source**: `terrene/foundation/docs/02-standards/co/applications/co-for-finance.md`

**Estimated new atoms**: ~8-12 atoms covering financial modelling as craft, monetary approval gates, budget envelope management (EATP §14 Financial Dimension is already cited by some atoms).

**Readiness**: Low. No known corpus source.

### Pass 6 — Governance

**Spec source**: `terrene/foundation/docs/02-standards/co/applications/co-for-governance.md`

**Known coverage**: PACT-themed atoms (SC-B-002, SC-B-004, SC-B-015, SC-B-017, SC-B-018, SC-A-005) are governance-adjacent.

**Estimated new atoms**: ~10 atoms covering organisational governance specifics not captured by the PACT primitives-level atoms.

**Readiness**: Medium. Overlaps with PACT platform work.

### Pass 7 — Learners

**Spec source**: `terrene/foundation/docs/02-standards/co/applications/co-for-learners.md`

**Estimated new atoms**: ~8-12 atoms covering CO as applied to individual learner self-direction (distinct from COE which is about teaching others).

**Readiness**: Low. No known corpus source.

## What each future pass looks like

Per the `rules/specs-first.md` methodology, each application pass repeats the COC pattern:

1. **Enumerate spec concepts** for the application (analogous to `01-analysis/03-spec-index/`)
2. **Identify corpus** of practitioner journals for the application (analogous to `01-analysis/04-reverse-index/`)
3. **Classify entries** against spec concepts (analogous to the 232 → 417 classification)
4. **Mine heavy-cited concepts** and contradictions for atom candidates
5. **Author atoms** at the rigor bar (read-before-cite, verbatim quotes, polarity, modality)
6. **Cross-tag** atoms that serve earlier applications
7. **Red team** and verify
8. **Route** to forge / co-codegen / both
9. **Integrate** into the catalog with updated indexes

Each pass is a full workspace cycle (analyze → todos → implement → redteam). The COC pass took ~D1 through D26 to reach 57 verified atoms. Subsequent passes should be faster because the format and rigor bar are now fixed.

## Why this matters

Without a queued list, the "all in scope" declaration from D20 would be a rule without a forward motion. Each application pass expands the library and the cross-tagging; the 18 already-cross-tagged atoms are the seed for every subsequent pass. The catalog grows by addition, not by replacement.
