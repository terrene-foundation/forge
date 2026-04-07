---
milestone: 5
title: Quality & Completeness — final passes before the library is publishable
dependency: M1 (verified catalog), M2 (drill library), M3 (case library)
estimated_sessions: 1
---

# Milestone 5: Quality & Completeness

Final quality passes that make the FORGE library publishable alongside CARE / EATP / CO / PACT in the Foundation's public presentation.

## Todos

### T5.1 Spec coverage reconciliation

Compare the 57-atom catalog against the full 417-concept spec index. For each of the top 15 heavy-cited concepts, verify that at least one atom directly addresses the concept (not just cites it as secondary lineage). For concepts without atoms, record them as explicit gaps with "no atom as of YYYY-MM-DD" and the reason (e.g. "CARE philosophy — not engineering craft" or "COR domain — out of COC-layer scope").

**Acceptance**: A coverage report in `catalog/spec-coverage.md` showing: 57 atoms, N concepts covered, M concepts explicitly named as gaps with reasons.

### T5.2 Craft area balance audit

Count atoms per craft area. The 7 user-named areas (prompt, behaviour, intervene-how, intervene-when, attend-how, attend-when, institutional-memory) should each have meaningful coverage. If any area has <3 atoms, flag it as thin and document whether this is a corpus gap (the journals don't have evidence for that area) or an authoring gap (evidence exists but wasn't mined).

**Acceptance**: A balance report in the production catalog README showing counts per craft area. Any area with <3 atoms has a documented reason.

### T5.3 Teaching-sequence links for clustered atoms

The overlap detector identified a 5-atom cluster (SC-P-002, SC-P-012, SC-P-016, SC-P-022, SC-A-008) around ablation/rule-optimization. Write an explicit teaching-sequence document that links these atoms in a workflow order (run ablation → detect contamination → classify rules → decide demote/strengthen → author for weakest model). Identify any other clusters (e.g. the sync/drift cluster: SC-B-001, SC-B-010, SC-B-012).

**Acceptance**: `catalog/teaching-sequences.md` with ≥2 identified clusters, each showing workflow order + the spec concepts that thread through the sequence.

### T5.4 Update stale brief

The workspace brief (`brief.md`) describes the pre-D1 framing (12 modules, F/E/C tracks). Replace it with a current brief that accurately describes what FORGE is (library, not course), what it contains (57 atoms, 3 layers, dual-destination), and how it feeds downstream (courses pull atoms, co-codegen receives methodology).

**Acceptance**: `brief.md` matches the D1-D26 framing. No references to F-track, E-track, C-track, or module numbers.

### T5.5 Label specialist reports as pre-rigor-bar

The 7 specialist reports in `01-analysis/01-research/` were written before the rigor bar was set (D22). They remain useful as evidence drafts but are NOT curriculum sources. Add a frontmatter `status: pre-rigor-bar — do not use as curriculum source` to each. This prevents future sessions from treating them as production content.

**Acceptance**: All 7 reports have the pre-rigor-bar status in frontmatter.

### T5.6 CO 5-layer coverage verification

Verify that all 5 CO layers have meaningful atom coverage, not just the heavy-cited concepts. Specifically check Layer 4 (Instructions, CO §26-§30) which the red team flagged as having only 1 dedicated atom (SC-A-014). Also identify the 5-layer architecture as a required teaching sequence in T5.3 (teaching-sequence links), alongside the ablation cluster.

**Acceptance**: T5.1's spec coverage report includes a "CO 5-layer coverage" section. Each layer has ≥2 atoms that directly teach a layer-specific skill (not just cite the layer as secondary lineage). If Layer 4 is thin, document it as a known gap with a forward pointer to the COR/COE pass.

### T5.7 Red team round 2

Deploy red team agents against the full production catalog (not the workspace copy). Configuration:
- **Agents**: quote-verifier (sample 15 atoms including all 5 gap atoms), overlap-detector (all atoms), framing-compliance (rules + coverage + routing)
- **Scope**: production `catalog/` directory, `drills/`, `cases/`, routing manifest, CLAUDE.md
- **Check list**:
  - All atoms `status: verified` and in production directory
  - Drill library has ≥10 exemplars + full index
  - Case library has ≥10 exemplars + full index
  - Routing manifest matches atom destination tags
  - CLAUDE.md references are current
  - No stale references to the pre-D1 framing anywhere in the program
  - Decision log D4/D5/D6 are annotated as superseded
  - 5 CO layers all have ≥2 dedicated atoms

**Convergence criteria**: 0 CRITICAL + 0 HIGH across all agents. "Clean round" = 0 new findings at any severity. Two consecutive clean rounds on the same artifact state (no fixes between rounds). If round 1 produces findings, fix them, then restart the count.

**Acceptance**: 2 consecutive clean rounds documented with agent names, finding counts, and artifact state (git commit hash or file checksums).

### T5.8 COR/COE pass placeholder

Create `catalog/future-passes.md` documenting the next application layers to build after the COC layer is complete. Per `forge-scope.md` §2: build order is COC → COR → COE → the rest. Each entry: application name, estimated evidence sources, known coverage from cross-tagged atoms (18 atoms already tagged `[COC, COR, COE]`), and estimated new atoms needed. This is a planning artifact, not an authoring todo — it queues the work for a future session.

**Acceptance**: `catalog/future-passes.md` exists with entries for COR, COE, compliance, finance, governance, learners. Each entry references the cross-tagged atoms that already serve that application.
