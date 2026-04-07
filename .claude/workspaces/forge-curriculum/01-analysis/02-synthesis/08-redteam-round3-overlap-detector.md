---
title: Red team round 3 — overlap detection (confirmation pass)
agent: reviewer (overlap-detector role)
date: 2026-04-08
atoms_scanned: 57
pairs_examined: 9
base_commit: 43da41b
round_2_report: .claude/workspaces/forge-curriculum/01-analysis/02-synthesis/05-redteam-round2-overlap-detector.md
---

# Summary

- 0 HIGH overlaps (target: 0)
- 0 new MEDIUM overlaps
- 0 new LOW overlaps
- Verdict: CLEAN — round 2 assessment holds unchanged

Round 3 is a confirmation pass over the catalog after commit `43da41b`. The round-2 edits were all non-structural: status/staleness drift in CLAUDE.md and brief.md, and one format-cleanup in SC-B-004 changing "PACT primitives" → "PACT (primitives)" and "PACT spec conformance" → "PACT (spec) conformance" to comply with the three-layer-qualification rule in `rules/forge-scope.md` §3. No atoms were added, deleted, renamed, re-tagged, or given new spec lineage. The catalog still holds 57 atoms (18 brokerage + 15 artifact + 24 practitioner) plus 13 co-codegen variants.

The round-3 re-verification confirms:

1. The SC-B-004 format edit did not shift the atom's meaning or scope, and did not create any new overlap with the other PACT-themed atoms (SC-B-017 verification-gradient zone assignment, SC-B-018 PDP/PEP separation).
2. All 4 MEDIUM "keep both distinct" pairs from round 2 remain distinct.
3. Spot checks against 5 previously-unexamined pairs with similar craft areas or shared spec lineage returned no HIGH overlap.

# HIGH overlaps

(none)

# SC-B-004 re-check

The SC-B-004 atom text was read in full. The two edits are at line 25 ("Kailash framework (Core SDK, DataFlow, Nexus, Kaizen, MCP platform, **PACT (primitives)**, ML, Align)") and line 44 ("this touches **PACT (spec) conformance** — TrustStore, DelegationRecord, GovernanceEngine"). Both are pure format cleanups to comply with the three-layer qualification rule. The surrounding prose, the "what it serves" argument, the journal evidence, the drill, the common failure mode, and the Related atoms section are all unchanged from round 2.

The atom's meaning is unchanged:

- Line 25 is still an enumeration of Kailash framework packages that warrant specialist delegation. Adding the "(primitives)" qualifier clarifies that the framework-work trigger is specifically about the SDK package layer, not the spec or the reference platform — which is exactly the layer a framework specialist would operate on. The edit narrows nothing; it makes an implicit distinction explicit.
- Line 44 is still describing the drill's test scenario (a PACT-conformance plan that touches TrustStore, DelegationRecord, GovernanceEngine). Adding "(spec)" clarifies that the conformance target is the normative spec, not the primitives or platform — which is correct since the referenced journal 0017 is a red team of spec-conformance todos.

Neither edit alters the craft move (delegate framework work to the specialist before writing the todo) or its trigger (a todo that names SDK API calls / code about to import from an un-opened framework module). The Related atoms section still cites SC-B-002, SC-B-009, SC-B-017, SC-A-001, and SC-A-013 in the same order and with the same distinctions.

**Overlap re-check against PACT-themed atoms:**

- **SC-B-004 ↔ SC-B-017** (verification-gradient zone assignment). Both cite PACT in their lineage. SC-B-004 teaches "which specialist handles this framework work"; SC-B-017 teaches "which verification zone does this delegation sit in." The "(primitives)" edit to SC-B-004 does not touch zone-assignment territory — it specifies the SDK package layer, not the trust posture of the delegation. SC-B-017's Related atoms explicitly names SC-B-004 as "specialist delegation triggers zone assignment: delegating to a specialist with a narrow Role Envelope may warrant a wider zone (auto-approved) than delegating to a generalist." The complementarity is unchanged. LOW overlap, keep both; no action.

- **SC-B-004 ↔ SC-B-018** (PDP/PEP separation). SC-B-018's lineage is EATP §40 + CO §5, not PACT. The shared topic is "governance-bearing modules" but the moves are entirely different: SC-B-004 is a delegation-timing move for framework work, SC-B-018 is a refactor move on access-control module structure. The "(primitives)" edit in SC-B-004 does not move its scope toward PDP/PEP territory. No overlap. No cross-reference needed (and none existed in round 2 or now).

- **SC-B-004 ↔ any other PACT-themed atom**: the catalog has only two other atoms with PACT in the lineage: SC-B-017 (covered above) and SC-B-005 (spec-to-code conformance). SC-B-005 vs SC-B-004 is already covered by round 2 implicitly — SC-B-005 is the spec-to-code audit _move_, SC-B-004 is the delegation move that feeds the correct API-shape information into the todo before any code is written. The two are sequential complements (delegate-then-audit) rather than overlapping. The "(spec)" edit to SC-B-004 line 44 brings its terminology in line with SC-B-005 but does not duplicate its move.

**Verdict on SC-B-004 edit**: Clean. The two format edits tightened compliance with the qualification rule without shifting the atom's craft content. No new overlaps introduced.

# MEDIUM pair re-verification

All 4 MEDIUM pairs from round 2 were re-read and confirmed still distinct.

## SC-A-014 ↔ SC-P-018

Re-read both atoms in full. SC-A-014 is the **authoring** move (produce a milestone graph with gates, evidence requirements, and dependency arrows at the start of a multi-session feature). SC-P-018 is the **intervention** move (split a single long-pole item after a milestone graph already exists and one item has become a critical-path bottleneck). The temporal ordering is explicit in both Related atoms sections (SC-A-014 cites SC-P-018 as "a technique for restructuring milestones when one is blocking all others"; SC-P-018 cites SC-A-014 as "the decomposition move is the structural counterpart that ensures the split halves have proper gates"). The shared journal evidence (0034) is used for different findings: SC-A-014 also cites kailash-py 0016 and kailash-rs 0002 as independent evidence, while SC-P-018 cites only 0034. Keep both.

## SC-B-005 ↔ SC-B-016

Re-read both atoms. SC-B-005 is the **spec-referenced** audit (compare code against the normative spec, produce a numbered gap list). SC-B-016 is the **cross-language parity** audit (compare two implementations bidirectionally, determine per-abstraction which is ahead). SC-B-005's journal evidence is three entries (kailash-py 0014, kailash-py 0015, kailash-rs 0001) about spec-to-code gaps; SC-B-016's journal evidence is one entry (kailash-rs nexus-parity 0001) about Rust exceeding Python in 4 of 5 abstractions. The moves are intentionally complementary — SC-B-016's Related atoms explicitly says "the spec is the shared reference that both SDKs implement; the parity audit compares them against each other and against the spec," naming the orthogonality. Keep both.

## SC-A-011 ↔ SC-P-006

Re-read both atoms. SC-A-011 is the **framework-first check** (grep the framework for existing primitives before creating new functionality) and fires at the start of any implementation session. SC-P-006 is the **package boundary decision** (three-question test for "module vs separate package") and fires only after framework-first has confirmed the functionality is genuinely new. SC-A-011's Related atoms explicitly names this sequencing ("a downstream decision that fires only after the framework-first check has confirmed the functionality is genuinely new"). SC-P-006's Related atoms is not shown in this excerpt but round 2 verified the symmetric reference. Keep both.

## SC-P-020 ↔ SC-P-024

Re-read both atoms. SC-P-020 is the **second occurrence of a drift class** move (fix the mechanism, not another instance). SC-P-024 is the **second consumer of a workaround** move (choose between fast-completing the proper implementation or promoting the workaround to production-grade). Both share CO §3.2 (Convention Drift) in their lineage and use the "second-is-structural" framing, but the "thing being counted" is different: SC-P-020 counts incidents of the same failure class across different artifacts; SC-P-024 counts consumers of a single workaround artifact. The interventions produce different artifacts (a validation check / automated scan vs. a binary decision about workaround permanence). Both Related atoms sections name the relationship as "parallel principle applied to drift classes" vs "parallel principle applied to workaround consumers." Keep both.

# Spot check — 5 previously-unexamined pairs

Five random pairs were selected from atoms not deeply examined in round 2, biased toward shared craft areas or shared spec lineage to maximise the chance of catching an overlap.

## 1. SC-A-004 ↔ SC-A-007 (both Layer 3 security audit atoms)

Both atoms cite CO §3.3 (Safety Blindness) + CO §19 (Layer 3 — Guardrails) as primary spec lineage and both fire during red team security review. SC-A-004 is the **hook-code audit** move (audit hooks for shell injection, path traversal, DoS because hooks run with elevated privilege and cannot be caught by themselves). SC-A-007 is the **zero-config default posture** move (audit `start()` / `serve()` convenience APIs for default-open exposure of write endpoints and network-reachable resources). Different surfaces: SC-A-004 operates on hook source files; SC-A-007 operates on framework entry-point defaults. Different journal evidence: SC-A-004 cites loom-meta 0050 (version-utils execSync injection) and loom-meta 0007 (enforcers-own-rules); SC-A-007 cites data-fabric-engine 0008 (db.start write endpoints), kz-foundation 0002 (mixed security posture), and kailash-rs 0003 (timing side-channel). The Related atoms sections in both atoms cross-reference each other explicitly and name the distinct surfaces. No overlap.

## 2. SC-P-008 ↔ SC-P-021 (both attend-how practitioner atoms about stale references)

SC-P-008 is the **transitive dependency chain audit** move (trace heavyweight deps N levels deep, identify single-point-of-failure links, tighten pins and add version-matrix tests). SC-P-021 is the **same-turn reference sweep after rename** move (grep the repo immediately after any rename/delete/move, update every reference in the same commit). Superficially both are about "references" but the target is completely different: SC-P-008 is about external package version pins and their transitive closure; SC-P-021 is about internal file path / command name / skill directory references within a single repo. Different spec lineage too (CO §3.3 for SC-P-008; CO §17 + §15 for SC-P-021). Different journal evidence (kailash-align 0007/0002 vs loom-meta 0011). Different practice modality (case vs drill). No overlap.

## 3. SC-B-011 ↔ SC-B-014 (both institutional-memory brokerage atoms)

Both atoms have `institutional-memory` as their sole craft area and both cite CO §17 (Single Source of Truth). SC-B-011 is the **version tracking cascade** move (wire every repo in the chain so it knows its own version, its upstream's version, and the SDK version artifacts were authored against; surface mismatches at session start and at sync gates). SC-B-014 is the **supersession journaling** move (write a new journal entry naming the prior entry by number and stating "this supersedes NNNN" whenever a decision is reversed). These are completely different institutional-memory mechanisms: SC-B-011 operates on version metadata in `.claude/VERSION` + pyproject.toml; SC-B-014 operates on decision-log journal entries. The `institutional-memory` craft area is broad enough to hold both without collision — they are the "version state" and "decision history" facets of institutional memory respectively. No overlap.

## 4. SC-A-002 ↔ SC-P-017 (both about systems reporting healthy metrics while shipping broken output)

Both atoms cite CO §3.3 (Safety Blindness). SC-A-002 is the **frontend mock detection hook extension** move (author a Layer 3 hook that detects `MOCK_*`, `generate*()`, `Math.random()` patterns in JS/TS that Python-focused validators miss). SC-P-017 is the **learning pipeline false-positive inspection** move (read the output end of an observation pipeline, check for duplicate instincts, hardcoded confidence values, empty evolved directory). Both share the meta-framing "the system reports green but is actually broken" but the artifacts under inspection are completely different: frontend JS/TS source code vs Layer 5 learning pipeline outputs. Different practice modality (build vs case). Different journal evidence (loom-meta 0047 kz-engage shipping vs loom-meta 0004 learning-system-broken). No overlap.

## 5. SC-P-005 ↔ SC-P-009 (both practitioner attend-how moves about doc-vs-code mismatches)

SC-P-005 is the **friction-gradient inversion** move (diagnose why practitioners converge on a worse API; trace the inversion through COC layers 2/3/5 to find which artifact reinforces the wrong path). SC-P-009 is the **architecture-doc-as-contract** move (read every factual claim in a brief or architecture doc, verify each against the actual codebase, produce a numbered correction list labelled CONFIRMED/CORRECTED/ABSENT). Both touch the theme "the written doc misstates what the code does" but at different granularities: SC-P-005 diagnoses a systemic routing failure across the COC layers (the inversion is the symptom, the layer failure is the root cause); SC-P-009 is a line-by-line claim verification at the start of implementation. Different spec lineage (CO §1/§3.2/§64 for SC-P-005; CO §29/§16 for SC-P-009). Different journal evidence (kailash-py 0006/0007 engine-broken vs kailash-rs kailash-ml-crate 0001 + nexus-parity 0003). No overlap.

# Convergence verdict

**Zero HIGH overlaps. Round 3 is CLEAN. Round 2 assessment holds unchanged.**

- Round 1 found 0 HIGH and 3 MEDIUM (all keep-both).
- Round 2 found 0 HIGH and 4 MEDIUM (all keep-both).
- Round 3 found 0 HIGH and 0 new findings at any severity.

The catalog satisfies the multi-round red team convergence criterion over three rounds. The SC-B-004 format edit (the only atom-level change between round 2 and round 3) was confirmed as non-scope-shifting and introduced no new overlaps. The 4 MEDIUM "keep both distinct" pairs from round 2 remain distinct with unchanged cross-references. Five previously-unexamined spot-check pairs across different craft areas and spec lineages all returned clean.

No collapse-into-one recommendations. No new cross-references required. No re-tagging required. The catalog can advance past T5.3 (round 3 overlap detection) on the M5 path.
