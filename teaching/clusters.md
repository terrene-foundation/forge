---
title: Atom clusters — themed bundles for course selection
date: 2026-04-10
destination: forge
---

# FORGE Atom Clusters

Themed atom bundles that group atoms by shared concern. Unlike the ordered teaching sequences in `catalog/teaching-sequences.md`, clusters are **unordered** — they tell a course designer "if you are covering topic X, these are the atoms you need." Sequencing within the cluster comes from the prerequisite graph in `prereqs.md`.

Clusters are not partitions. An atom can appear in multiple clusters when it serves multiple concerns. The clusters here complement (not duplicate) the 5 teaching sequences already in `catalog/teaching-sequences.md`.

## How to use clusters

1. Pick the cluster(s) relevant to your course audience
2. Pull all atoms in the cluster
3. Check `prereqs.md` for ordering constraints within the cluster
4. Add any hard prereqs from outside the cluster that unblock the atoms you selected
5. Sequence using the topological layer view

## Cluster 1 — Security & Integrity (7 atoms)

Atoms teaching defensive security craft: auditing, testing, integrity verification.

| Atom     | Name                                                 | Modality      | Layer        |
| -------- | ---------------------------------------------------- | ------------- | ------------ |
| SC-A-002 | Frontend mock data detection                         | build         | Artifact     |
| SC-A-004 | Layer 3 hook security audit                          | case          | Artifact     |
| SC-A-005 | Encapsulation as security boundary in signed structs | case          | Artifact     |
| SC-A-006 | Negative tests for deny-by-default                   | drill         | Artifact     |
| SC-A-007 | Zero-config security audit                           | drill         | Artifact     |
| SC-B-018 | PDP/PEP separation                                   | brokerage-rep | Brokerage    |
| SC-P-010 | Timing and race condition red team                   | drill         | Practitioner |

**Spec thread**: CO §5 (Deterministic Enforcement) → CO §5.4 (Defense in Depth) → CO §19 (Layer 3 Guardrails) → PACT §23 (Blocked Zone). All seven atoms enforce the same principle from different angles: enforcement that is not tested by intentional violation is assumed broken.

**When to pull this cluster**: Any course teaching artifact-authoring craft where the practitioners will author rules, hooks, or security configurations. Also relevant for courses covering PACT governance where security is a concern.

**Prereqs from outside the cluster**: None — all 7 atoms are Layer 0 (no hard prereqs). This cluster is a clean standalone selection.

**Overlap**: SC-A-005 also appears in PACT Governance (Cluster 2). SC-A-006 also appears in PACT Governance.

## Cluster 2 — PACT Governance (10 atoms)

Atoms that directly serve PACT spec concepts — D/T/R accountability, operating envelopes, verification gradient, monotonic tightening.

| Atom     | Name                                         | Modality      | Layer        |
| -------- | -------------------------------------------- | ------------- | ------------ |
| SC-B-002 | Authority-chain escalation                   | brokerage-rep | Brokerage    |
| SC-B-004 | Specialist delegation as required first move | brokerage-rep | Brokerage    |
| SC-B-005 | Spec-to-code conformance audit               | brokerage-rep | Brokerage    |
| SC-B-015 | MUST rule semantic reinterpretation          | brokerage-rep | Brokerage    |
| SC-B-017 | Verification gradient zone assignment        | brokerage-rep | Brokerage    |
| SC-B-018 | PDP/PEP separation                           | brokerage-rep | Brokerage    |
| SC-A-005 | Encapsulation as security boundary           | case          | Artifact     |
| SC-A-006 | Negative tests for deny-by-default           | drill         | Artifact     |
| SC-P-001 | Adjacent-but-disconnected modules            | case          | Practitioner |
| SC-P-003 | Cross-feature interaction red team           | drill         | Practitioner |

**Spec thread**: PACT §10 (Monotonic Tightening) → PACT §11 (Role Envelope) → PACT §19 (Verification Gradient) → PACT §23 (Blocked Zone) → PACT §35 (Bridges) → PACT §40 (EATP Integration) → PACT §51 (Vacant Roles).

**When to pull this cluster**: Any course where practitioners need to understand governance primitives — how D/T/R accountability is enforced, how operating envelopes constrain agents, and how verification gradient zones determine what gets audited at what frequency. Also relevant for courses covering the PACT platform (the reference implementation).

**Prereqs within the cluster**: SC-B-002 requires SC-B-003 → SC-B-001 → SC-B-010 (the sync/drift chain). SC-B-005 is Layer 0. Most governance atoms are foundational and can be taught early.

**Overlap**: SC-B-018, SC-A-005, SC-A-006 also in Security & Integrity. SC-B-005, SC-B-015, SC-B-017 also in Cross-Application (Cluster 7).

## Cluster 3 — Cross-SDK & Multi-Repo Operations (4 atoms)

Atoms for operating across repository boundaries, SDK boundaries, and worktree boundaries.

| Atom     | Name                                           | Modality      | Layer     |
| -------- | ---------------------------------------------- | ------------- | --------- |
| SC-B-007 | Worktree-per-agent for safe parallel execution | build         | Brokerage |
| SC-B-009 | Cross-SDK upstream brokerage                   | brokerage-rep | Brokerage |
| SC-B-013 | Cross-workspace dependency mapping             | brokerage-rep | Brokerage |
| SC-B-016 | Bidirectional cross-language parity audit      | brokerage-rep | Brokerage |

**Spec thread**: CO §17 (Single Source of Truth across repos) → CO §49 (Enforcement Reliability across boundaries) → PACT §10 (Monotonic Tightening across the chain).

**When to pull this cluster**: Any course where practitioners work on multi-repo or multi-SDK projects (e.g., maintaining both Python and Rust implementations of the same spec). This cluster is less relevant for single-repo courses.

**Prereqs within the cluster**: SC-B-007 requires SC-B-013. The other three are Layer 0.

**Overlap**: SC-B-013 and SC-B-007 are also in the prereq chains for Sync/Drift (teaching sequence 2).

## Cluster 4 — Research Craft (COR) (10 atoms)

All 10 atoms from the COR application pass. These teach research-as-scholarship craft rather than research-as-engineering.

| Atom     | Name                                               | Modality | Layer        |
| -------- | -------------------------------------------------- | -------- | ------------ |
| SC-P-025 | Tier-ranked literature search                      | drill    | Practitioner |
| SC-P-026 | Citation integrity audit                           | case     | Practitioner |
| SC-P-027 | Hostile reviewer simulation                        | drill    | Practitioner |
| SC-P-028 | Multi-perspective synthesis                        | case     | Practitioner |
| SC-P-029 | Post-publication gap check                         | drill    | Practitioner |
| SC-P-030 | Academic register calibration                      | case     | Practitioner |
| SC-P-031 | Venue strategy as constraint envelope              | case     | Practitioner |
| SC-P-032 | Margin note as deliberation artifact               | drill    | Practitioner |
| SC-P-033 | Reflexivity diagnosis in self-referential research | case     | Practitioner |
| SC-P-034 | Overclaim prevention in research writing           | drill    | Practitioner |

**Spec thread**: COR-specific instantiation of the CO 5-layer architecture. Layer 2 (Context) = institutional literature corpus + prior-work ledger. Layer 3 (Guardrails) = citation integrity, overclaim prevention, register calibration.

**When to pull this cluster**: Any course with a research-writing or academic-publishing component. Four of these atoms (SC-P-025, SC-P-026, SC-P-032, SC-P-034) are also tagged COE, making them relevant for courses where practitioners teach research methods.

**Prereqs within the cluster**: SC-P-025 is the foundation — SC-P-026, SC-P-029, and SC-P-034 all require it. SC-P-028 requires SC-P-027. The rest are Layer 0.

**Overlap**: SC-P-025, SC-P-026, SC-P-032, SC-P-034 are tagged `[COR, COE]` and relevant for the COE application pass.

## Cluster 5 — Institutional Memory & Layer 5 (10 atoms)

Atoms about building, maintaining, and verifying institutional knowledge across sessions. Maps to CO Layer 5 (Learning) and the `institutional-memory` craft area.

| Atom     | Name                                | Modality      | Layer        |
| -------- | ----------------------------------- | ------------- | ------------ |
| SC-A-001 | Codify-with-explicit-NOT-codified   | drill         | Artifact     |
| SC-A-009 | Progressive disclosure architecture | drill         | Artifact     |
| SC-A-010 | Token budget: baked vs external     | case          | Artifact     |
| SC-A-012 | CLAUDE.md as Layer 2 context        | build         | Artifact     |
| SC-A-015 | Layer 2 context structuring         | build         | Artifact     |
| SC-B-003 | Variant classification              | drill         | Brokerage    |
| SC-B-011 | Version tracking cascade            | brokerage-rep | Brokerage    |
| SC-B-014 | Explicit supersession journaling    | brokerage-rep | Brokerage    |
| SC-P-017 | False-positive learning detection   | case          | Practitioner |
| SC-P-019 | Cold-start continuity               | drill         | Practitioner |

**Spec thread**: CO §39 (Layer 5 Learning) → CO §40 (Observe-Capture-Evolve) → CO §42 (Knowledge Growth) → CO §7 (Knowledge Compounds). This cluster teaches the craft of making knowledge survive across sessions.

**When to pull this cluster**: Any course where practitioners will author or maintain `.claude/` artifacts, CLAUDE.md files, or learning pipelines. Especially relevant for COC courses where institutional memory is the primary differentiator.

**Prereqs within the cluster**: Deep chain — SC-A-012 → SC-A-001 → SC-A-014 → SC-A-008 (from 5-layer sequence). SC-B-003 → SC-B-001 → SC-B-010. Not all atoms can be taught simultaneously; check `prereqs.md` before sequencing.

**Overlap**: SC-A-001, SC-A-012, SC-A-015 also in 5-Layer Architecture (teaching sequence 4). SC-P-019 also in Attention-Timing (teaching sequence 5). SC-B-003, SC-B-011, SC-B-014 also in Sync/Drift (teaching sequence 2).

## Cluster 6 — Diagnostic Attention (8 atoms)

Atoms about noticing patterns — what to look at, what to notice, and how to read what the code or the model is telling you.

| Atom     | Name                                                  | Modality | Layer        |
| -------- | ----------------------------------------------------- | -------- | ------------ |
| SC-P-001 | Adjacent-but-disconnected modules                     | case     | Practitioner |
| SC-P-005 | Friction-gradient inversion                           | case     | Practitioner |
| SC-P-008 | Dependency chain fragility assessment                 | case     | Practitioner |
| SC-P-011 | Audit categorization blindness                        | case     | Practitioner |
| SC-P-013 | Estimate self-contradiction as high-confidence signal | drill    | Practitioner |
| SC-P-016 | Detect overcorrection in model behaviour              | case     | Practitioner |
| SC-P-017 | False-positive learning detection                     | case     | Practitioner |
| SC-P-020 | Second occurrence as structural signal                | case     | Practitioner |

**Spec thread**: CO §3.2 (Convention Drift) → CO §3.3 (Safety Blindness) → CO §15 (Progressive Disclosure) → CO §1 (Institutional Knowledge Thesis). These atoms teach practitioners to see what is normally invisible.

**When to pull this cluster**: Any course with a strong analytical or diagnostic focus. Pairs well with Red Team / Convergence (teaching sequence 3) — diagnostic atoms are what practitioners use between red team rounds.

**Prereqs within the cluster**: SC-P-016 requires SC-P-002. SC-P-011 requires SC-B-008 → SC-B-006 → SC-B-005. SC-P-020 requires SC-P-004 → SC-P-019. Most diagnostic atoms have upstream prereqs from other clusters.

**Overlap**: SC-P-001 also in PACT Governance. SC-P-017 also in Institutional Memory. SC-P-020 also in Attention-Timing (teaching sequence 5).

## Cluster 7 — Cross-Application Atoms (22 atoms)

Atoms tagged with two or more CO applications (`[COC, COR, COE]` or `[COR, COE]`). These are the atoms that transfer directly across application boundaries — the portable core of practitioner craft.

| Atom     | Name                                  | Applications  |
| -------- | ------------------------------------- | ------------- |
| SC-A-001 | Codify-with-explicit-NOT-codified     | COC, COR, COE |
| SC-A-011 | Framework-first check                 | COC, COR, COE |
| SC-A-014 | Milestone decomposition               | COC, COR, COE |
| SC-B-005 | Spec-to-code conformance audit        | COC, COR, COE |
| SC-B-006 | Multi-round red team                  | COC, COR, COE |
| SC-B-008 | Convergence-as-validation             | COC, COR, COE |
| SC-B-015 | MUST rule semantic reinterpretation   | COC, COR, COE |
| SC-B-017 | Verification gradient zone assignment | COC, COR, COE |
| SC-P-002 | Model-vs-rule ablation                | COC, COR, COE |
| SC-P-007 | Session completion state              | COC, COR, COE |
| SC-P-011 | Audit categorization blindness        | COC, COR, COE |
| SC-P-012 | Rule demotion judgment                | COC, COR, COE |
| SC-P-013 | Estimate self-contradiction signal    | COC, COR, COE |
| SC-P-014 | Zero-tolerance scope discipline       | COC, COR, COE |
| SC-P-015 | Ask for contradictions                | COC, COR, COE |
| SC-P-016 | Detect overcorrection                 | COC, COR, COE |
| SC-P-018 | Split the long-pole                   | COC, COR, COE |
| SC-P-020 | Second occurrence structural signal   | COC, COR, COE |
| SC-P-023 | Insert a gate, not a check            | COC, COR, COE |
| SC-P-024 | Fallback permanence detection         | COC, COR, COE |
| SC-P-026 | Citation integrity audit              | COR, COC, COE |
| SC-P-032 | Margin note as deliberation           | COR, COE      |
| SC-P-034 | Overclaim prevention                  | COR, COE      |

**When to pull this cluster**: When designing a course that spans multiple CO applications. These 22 atoms are the transferable core — skills that work in codegen, research, and education contexts. A COE course, for example, can start with these 22 before adding COE-specific atoms from the future COE pass.

**Note**: This cluster is a view, not a teaching unit. It is too large (22 atoms) for a single course segment. Use it as a selection filter — "which atoms from my course are also applicable elsewhere?" — not as a bundle to teach in sequence.

## Session Discipline (5 atoms)

Atoms about session hygiene — what to do at the start, during, and at the close of a working session.

| Atom     | Name                            | Modality | Layer        |
| -------- | ------------------------------- | -------- | ------------ |
| SC-P-004 | Per-file disposition labelling  | drill    | Practitioner |
| SC-P-007 | Session completion state        | drill    | Practitioner |
| SC-P-014 | Zero-tolerance scope discipline | case     | Practitioner |
| SC-P-015 | Ask for contradictions          | drill    | Practitioner |
| SC-P-019 | Cold-start continuity           | drill    | Practitioner |

**Spec thread**: CO §33 (Phase Analyze) → CO §29 (Evidence-Based Completion) → CO §39 (Layer 5 Learning). These atoms enforce the discipline that makes every session legible to the next one.

**When to pull this cluster**: Any introductory or onboarding course. Session discipline atoms are the minimum viable practitioner skillset — without them, nothing else sticks.

**Prereqs within the cluster**: SC-P-004 requires SC-P-019 (cold-start before disposition). The rest are Layer 0. This cluster is nearly self-contained.

**Overlap**: SC-P-004 and SC-P-019 also in Attention-Timing (teaching sequence 5). SC-P-007 also in Cross-Application.

## Summary

| Cluster                   | Atoms | Primary spec            | Recommended for                 |
| ------------------------- | ----- | ----------------------- | ------------------------------- |
| 1. Security & Integrity   | 7     | CO §5, PACT §23         | Artifact authoring courses      |
| 2. PACT Governance        | 10    | PACT §10-§51            | Governance and platform courses |
| 3. Cross-SDK & Multi-Repo | 4     | CO §17, CO §49          | Multi-repo project courses      |
| 4. Research Craft (COR)   | 10    | COR-specific CO 5-layer | Research writing courses        |
| 5. Institutional Memory   | 10    | CO §39, CO §7           | COC artifact-authoring courses  |
| 6. Diagnostic Attention   | 8     | CO §3.2, CO §3.3        | Analytical/diagnostic courses   |
| 7. Cross-Application      | 22    | Mixed                   | Cross-domain CO courses         |
| 8. Session Discipline     | 5     | CO §33, CO §29          | Introductory/onboarding courses |

Total unique atoms across all clusters: 62 of 67. The 5 atoms not appearing in any cluster are highly specialized atoms that fit better into teaching sequences than into thematic bundles:

- SC-A-003 (defense-in-depth codification) — part of 5-Layer Architecture sequence
- SC-A-008 (rule classification) — convergence point in two teaching sequences
- SC-A-013 (agent specialization routing) — part of 5-Layer Architecture sequence
- SC-P-006 (package boundary decision) — standalone decision atom
- SC-P-021 (reference sweep after rename) — standalone intervention atom
