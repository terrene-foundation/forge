---
title: Red team round 2 — framing compliance
agent: gold-standards-validator
date: 2026-04-08
git_commit: fbea2c4
scope: production FORGE library at /Users/esperie/repos/lyceum/programs/forge/
---

# Summary

- 0 findings at CRITICAL
- 0 findings at HIGH
- 4 findings at MEDIUM
- 2 findings at LOW
- **Verdict: FINDINGS** — all are status/staleness drift in CLAUDE.md and brief.md, none touch the canonical framing (quartet, four standards, seven applications, dual destination, specs-as-index). Catalog, drills, cases, and the four in-scope rule files pass clean.

## Per-item check

### 1. All atoms `status: verified`

- Result: **PASS**
- Evidence: Grep across `catalog/` returned 69 matches for `^status:` (57 atoms + 12 of the 13 co-codegen-variants that carry a local status). Zero occurrences of `status: draft` or `status: needs-second-source`. The only non-`verified` match is `catalog/future-passes.md:4 status: planning artifact, not authoring todo`, which is correct for a planning artifact.

### 2. Drill library

- Result: **PASS**
- Evidence: `drills/exemplars/` contains exactly 10 files (SC-B-003, SC-B-005, SC-B-012, SC-A-011, SC-A-014, SC-P-004, SC-P-009, SC-P-015, SC-P-019, SC-P-022). Spot-checked SC-B-003 exemplar at 81 lines / ≫500 words. `drills/README.md` carries four indices: craft area (lines 14+), practice modality (line 133), difficulty (line 217), spec lineage (line 291). `drills/specs/` holds 57 drill specs (one per atom). Expected: 10 exemplars + 57 specs + README with 4 views — all present.

### 3. Case library

- Result: **PASS**
- Evidence: `cases/exemplars/` contains exactly 10 files (CASE-01 through CASE-10). Spot-checked CASE-01 and CASE-10 — both substantially exceed 300 words. `cases/README.md` and `cases/case-candidates.md` present.

### 4. Routing manifest matches atom destination tags

- Result: **PASS**
- Evidence: Actual destination distribution from atom frontmatter: `forge=21`, `co-codegen=23` (excluding 13 files under `co-codegen-variants/`), `both=13`. Total = 57. Matches `catalog/routing-manifest.yaml` summary footer (lines 295–299). Count of `both` atoms (13) equals count of files in `catalog/co-codegen-variants/` (13). IDs match 1:1: SC-B-004, SC-A-001, SC-A-002, SC-A-003, SC-A-004, SC-A-006, SC-A-007, SC-A-011, SC-A-012, SC-A-014, SC-P-013, SC-P-015, SC-P-019. No mismatches detected between manifest entries and atom file frontmatter.

### 5. `CLAUDE.md` references are current

- Result: **FINDING** (see F1, F2, F3)
- Evidence: Rules index at `CLAUDE.md:109-125` references 13 rule files; all 13 exist under `.claude/rules/`. `catalog/`, `drills/`, `cases/` are all mentioned. However: line 19 corpus count is stale; lines 105–106 mark drills/cases as "pending M2/M3" when they are built; line 107 and line 129 reference a decision log that is out of date (D1-D26 in one place, D1-D19+ in another).

### 6. No stale pre-D1 framing

- Result: **PASS** (in scope)
- Evidence: Grep for `F-track|E-track|C-track|12 modules|foundation module|dual-track` returns matches only in (a) `.claude/rules/dual-track.md` which explicitly supersedes and labels the old framing, (b) `.claude/workspaces/forge-curriculum/brief.md` line 72 which explicitly labels the old framing as rejected by D2, (c) `.claude/workspaces/forge-curriculum/todos/active/M5-quality-and-completeness.md` lines 34, 36 which describe the M5 task of removing the stale framing, and (d) the out-of-scope decision log. All are allowed. One informational observation: the out-of-scope file `.claude/agents/project/curriculum-designer.md:12,14` still carries active E-track/C-track language; not flagged here because it sits outside the round-2 scope list, but it is a candidate for a future cleanup pass.

### 7. No trinity framing

- Result: **PASS** (in scope)
- Evidence: Grep for `trinity` in scope files: `CLAUDE.md:15` uses the word only to negate it ("quartet, not a trinity"); `CLAUDE.md:119` flags terrene-naming.md's stale wording as upstream debt; `.claude/rules/forge-scope.md:9,24,33,39` all use the word in rule-explaining context. `upstream-flags.md` and `decision-log.md` are excluded from the check per task instructions. `.claude/rules/terrene-naming.md` is excluded per task instructions. One informational observation: `.claude/skills/co-reference/behavioral-guidelines.md:13` actively asserts "Connect frameworks through the trinity — CARE (philosophy), EATP (protocol), CO (methodology) are peers, not competitors"; this file is out of scope for round 2 but is a candidate for upstream-flag documentation since it asserts the stale trinity framing as current guidance rather than flagging it as superseded.

### 8. No fabricated applications

- Result: **PASS** (in scope)
- Evidence: Grep for `COComp|COL\b` in scope files returns matches only in `.claude/rules/forge-scope.md` lines 76, 81, 86, 174 — all in `DO NOT` or `Why` sections explicitly naming them as fabricated and telling authors not to cite them. `catalog/future-passes.md` names all six future passes (research, education, compliance, finance, governance, learners) plus the current COC pass = seven real applications, no fabrications. Out-of-scope files under `01-analysis/03-spec-index/` and `01-analysis/04-reverse-index/` retain the strings but with explicit "previously-fabricated" annotations and are correctly excluded.

### 9. Four-standard quartet framing

- Result: **PASS**
- Evidence: `CLAUDE.md:15` ("CARE / EATP / CO / PACT are a quartet, not a trinity. All four are peers"), `.claude/rules/forge-scope.md:9` ("standards stack is a quartet, not a trinity"), `.claude/workspaces/forge-curriculum/brief.md:11-18` ("FORGE teaches practitioner craft moored in the four Terrene Foundation standards... All four are peers") — consistent across all three authoritative documents.

### 10. All 5 CO layers have ≥2 dedicated atoms

- Result: **PASS**
- Evidence: `catalog/spec-coverage.md:15-21` tabulates Layer 1 = 3 atoms, Layer 2 = 4, Layer 3 = 6, Layer 4 = 4, Layer 5 = 5. Sampled 2 atoms per layer and confirmed frontmatter `spec_lineage` cites the claimed CO §§:
  - Layer 1: `SC-A-013` cites `CO §9 — Layer 1 — Intent`, `CO §11 — Routing Rules`; `SC-B-004` cites `CO §9 — Layer 1 — Intent`, `CO §11 — Routing Rules`. Confirmed.
  - Layer 2: `SC-A-012` cites `CO §13 — Layer 2 — Context`, `CO §14 — Master Directive`, `CO §15 — Progressive Disclosure`. Confirmed.
  - Layer 3: `SC-A-002` cites `CO §19 — Layer 3 — Guardrails`. Confirmed.
  - Layer 4: `SC-A-014` cites `CO §26 — Layer 4 — Instructions`, `CO §27`, `CO §28`. Confirmed.
  - Layer 5: `SC-A-001` cites `CO §39 — Layer 5 — Learning`, `CO §47 — Knowledge Curator`. Confirmed.

### 11. All atoms tagged `[COC]` or `[COC, COR, COE]`

- Result: **PASS**
- Evidence: Grep across `catalog/**` for `^applications:` returned 69 matches (57 primary atom files + 13 co-codegen-variant files). Distinct values present: only `[COC]` and `[COC, COR, COE]`. No other application tags.

### 12. Independence check — no commercial product references

- Result: **PASS**
- Evidence: Grep for `Aegis|Aether` across `catalog/`, `drills/`, `cases/` returned zero matches in all three directories. The only references to Aegis in the program are in `CLAUDE.md:11` ("Aegis is not in FORGE scope") and `.claude/rules/forge-scope.md`'s historical framing section, both of which correctly name it as out of scope. Same for Aether.

### 13. PACT three-layer qualification (spot check)

- Result: **PASS with LOW finding** (see F5)
- Evidence:
  - `SC-P-001`: body text at lines 33, 43, 44, 60 uses `PACT (spec)` and `PACT (primitives)` correctly. Unqualified "PACT" occurs only inside verbatim `quote:` fields (lines 14, 17) which are excerpts from journal entries — preserving original text is acceptable.
  - `SC-B-005`: body at lines 41, 46 uses `PACT (primitives)` and `PACT (spec)`. Unqualified bare PACT only in `quote:` fields.
  - `SC-B-017`: body clean; only bare PACT is in quoted journal text at line 20.
  - `SC-B-004`: body at line 25 uses `PACT primitives` (no parentheses) and line 44 uses `PACT spec conformance` (no parentheses). The qualifier is present but not in the canonical parenthetical form. LOW severity consistency finding.

### 14. Decision log D4/D5/D6 superseded annotation

- Result: **PASS**
- Evidence: `02-decision-log.md:29` = `## D4. COC is inside FORGE, not beside it — [SUPERSEDED by D20]` with strikethrough on the old content plus explicit `**Superseded**:` paragraph. `:34` = `## D5. v1 application scope = COC only — [SUPERSEDED by D20]` with strikethrough. `:39` = `## D6. Spec lineage is the 5th capability dimension — [PARTIALLY SUPERSEDED by D20]` with `**Correction**:` paragraph. All three entries are correctly annotated.

## Findings

### F1 — CLAUDE.md corpus count is stale

- **Severity**: MEDIUM
- **Location**: `CLAUDE.md:19`
- **Current state**: "The **journal corpus** supplies the _substance_: ~253 CO/COC practitioner craft entries across `loom/` ... and `terrene/` ..."
- **Expected state**: Per `.claude/rules/specs-first.md` and decision log entries D20-D21, the correct count is **232 CO/COC practitioner-craft journal entries**. The `~253` figure is an earlier round number that predates the 2026-04-07 read-and-classify pass.
- **Fix**: Replace `~253 CO/COC practitioner craft entries` with `232 CO/COC practitioner craft entries` and align parenthetical with `specs-first.md` lines 3-11.

### F2 — CLAUDE.md marks drills and cases as "pending"

- **Severity**: MEDIUM
- **Location**: `CLAUDE.md:105-106`
- **Current state**:
  - Line 105: "`drills/` | Recurring micro-exercises extracted from atom drill sections (pending M2)"
  - Line 106: "`cases/` | DISCOVERY/RISK journal-derived teaching cases with spec-mooring overlay (pending M3)"
- **Expected state**: Drills and cases are both built. "(pending M2/M3)" is status drift.
- **Fix**: Replace with current status describing 57 specs + 10 drill exemplars and 10 case exemplars.

### F3 — CLAUDE.md decision-log range references drift

- **Severity**: MEDIUM
- **Location**: `CLAUDE.md:107` and `:129`
- **Current state**: Line 107 says "D1-D26"; line 129 says "D1–D19+". They disagree with each other, and D1–D19+ is stale.
- **Fix**: Update line 129 to "D1–D26+".

### F4 — Brief destination counts drift from catalog

- **Severity**: MEDIUM
- **Location**: `.claude/workspaces/forge-curriculum/brief.md:44-47`
- **Current state**: "- `forge` — uniquely FORGE practitioner pedagogy (Layer 1 — 24 atoms) / - `co-codegen` — 22 atoms / - `both` — 11 atoms"
- **Expected state**: Actual distribution after the M1 red team routing is `forge=21, co-codegen=23, both=13`.
- **Fix**: Update the three lines to the current counts and decouple the "Layer 1 = 24 atoms" claim from the `forge` destination count (practitioner atoms SC-P-013, SC-P-015, SC-P-019 are `both`, not `forge`).

### F5 — PACT layer qualification format inconsistency in SC-B-004

- **Severity**: LOW
- **Location**: `catalog/brokerage/SC-B-004-specialist-delegation.md:25, :44`
- **Current state**:
  - Line 25: "...Core SDK, DataFlow, Nexus, Kaizen, MCP platform, PACT primitives, ML, Align..."
  - Line 44: "...one-line context 'this touches PACT spec conformance — TrustStore, DelegationRecord, GovernanceEngine'..."
- **Expected state**: `rules/forge-scope.md §3` specifies parenthetical form: `PACT (spec)`, `PACT (primitives)`, `PACT (platform)`. Both uses here drop the parentheses.
- **Fix**: Rewrite to use parenthetical form — `PACT (primitives)` on line 25 and `PACT (spec) conformance` on line 44.

### F6 — Brief's M1-M5 deliverable reference understates completed scope

- **Severity**: LOW
- **Location**: `.claude/workspaces/forge-curriculum/brief.md:53-59`
- **Current state**: Deliverable tree lists `catalog/` / `drills/` / `cases/` without mentioning the additional production artifacts. "(57 verified atoms + indexes)" gestures at indexes but is vague.
- **Fix**: Expand the tree snippet to reference the catalog substructure or add a cross-reference to `catalog/README.md`.

## Convergence verdict

**Not yet converged.** Round 2 finds 4 MEDIUM and 2 LOW issues, all concentrated in two files that drift from the catalog/spec-coverage/routing-manifest state: `CLAUDE.md` (F1, F2, F3) and the workspace `brief.md` (F4, F6), plus one residual PACT-format issue in `SC-B-004` (F5) that round 1 partially fixed.

**Nothing in the canonical framing is broken.** The quartet (CARE/EATP/CO/PACT), the four-standard peer status, the seven real CO applications, the specs-as-index craft-as-substance principle, the library-not-tracks organization, the dual-destination routing discipline, the rigor bar, the 5-layer coverage, the status:verified state of all 57 atoms, and the decision-log supersession annotations are all intact.

Convergence criterion from `M5-quality-and-completeness.md` T5.7 is "0 CRITICAL + 0 HIGH ... Two consecutive clean rounds." CRITICAL and HIGH are both zero, so the CRITICAL/HIGH gate is met. However, the round is not "clean" in the strict sense (clean = 0 findings at any severity), so the two-consecutive-clean-rounds clock resets after F1–F6 are fixed.

**Recommended next step**: Fix F1-F6 in one pass, then run round 3 against the post-fix state. Round 3 should be the first clean round; round 4 would be the confirming second clean round.

## Out-of-scope observations (candidates for future cleanup)

- `.claude/agents/project/curriculum-designer.md:12-14` — still uses active "E-track / C-track / foundation modules" language; candidate for upstream-flags or agent rewrite.
- `.claude/skills/co-reference/behavioral-guidelines.md:13` — actively asserts the trinity framing; candidate for upstream-flags or skill rewrite.
- `.claude/skills/co-reference/co-spec.md` and `SKILL.md` — carry "COComp" and "COL" as active CO applications; candidate for upstream-flags (these originate from a template sync predating D20).
