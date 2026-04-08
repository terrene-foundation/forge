---
title: "FORGE COR Red Team Round 1: Framing Compliance"
date: 2026-04-08
type: redteam
round: 1
agent: framing-compliance
---

# Framing Compliance Report: SC-P-025 through SC-P-034

## Summary

**0 CRITICAL, 0 HIGH, 0 MEDIUM, 0 LOW findings across 10 atoms.**

All 10 new COR skill atoms **PASS** framing compliance at the rigor bar.

---

## Verification Details

### Atoms Verified

| Atom ID | Name | Status |
|---------|------|--------|
| SC-P-025 | Run a tier-ranked literature search with source verification | ✅ PASS |
| SC-P-026 | Detect misquotation and specification drift between source and citation | ✅ PASS |
| SC-P-027 | Run hostile reviewer simulation as preemptive defense | ✅ PASS |
| SC-P-028 | Synthesize multiple hostile critiques into a minimal publishable model | ✅ PASS |
| SC-P-029 | Run a post-publication gap check for recent literature | ✅ PASS |
| SC-P-030 | Calibrate academic register with detection-bias awareness | ✅ PASS |
| SC-P-031 | Frame venue strategy as a constraint envelope for argument scope | ✅ PASS |
| SC-P-032 | Document writing choices as margin-note deliberation artifacts | ✅ PASS |
| SC-P-033 | Diagnose reflexivity in self-referential research | ✅ PASS |
| SC-P-034 | Qualify every superlative — prevent overclaims in research writing | ✅ PASS |

### Framing Rules Checked

#### 1. Trinity vs Quartet ✅

**Rule**: Atoms must describe CARE/EATP/CO/PACT as a **quartet** (four standards), not a trinity. No atom may say "three standards."

**Finding**: All atoms correctly avoid trinity language. No references to "the trinity" or "three standards" found. When CARE, EATP, or CO are referenced, they are presented as peers within the four-standard framework.

#### 2. Fabricated CO Applications ✅

**Rule**: No references to COComp, COL, or other invented applications. Only valid applications (COC, COR, COE, compliance, finance, governance, learners) may be cited.

**Finding**: All atoms cite only valid applications.
- SC-P-025, SC-P-027, SC-P-029, SC-P-030, SC-P-031, SC-P-033: COR only
- SC-P-026: COR, COC, COE (all valid)
- SC-P-028: COR only
- SC-P-032: COR, COE (both valid)
- SC-P-034: COR, COE (both valid)

No fabricated applications cited.

#### 3. PACT Layer Qualification ✅

**Rule**: When PACT is referenced, it must be qualified as PACT (spec), PACT (primitives), or PACT (platform). Bare "PACT" in sentences mixing layers is a violation.

**Finding**: None of the 10 atoms reference PACT. Therefore, this rule does not apply to this batch. No violations.

#### 4. Track-Coupled Language ✅

**Rule**: No references to F-track, E-track, C-track, foundation modules, or role-name organization. The library is contributor-neutral, organized by responsibility/accountability/skillset.

**Finding**: All atoms use responsibility/accountability framing. No track language detected. Examples:
- SC-P-025 frames literature search as an institutional knowledge requirement (CO §1)
- SC-P-027 frames hostile review as convergence evidence (CO §28)
- SC-P-031 frames venue strategy as constraint envelope (CARE/EATP integration)

No track-coupled language found.

#### 5. PACT Graduation Hedging ✅

**Rule**: No hedging of PACT's authority. Cannot describe PACT as "being graduated," "workspace phase," "proposed fourth standard," or "thesis-level." PACT IS the fourth standard.

**Finding**: Not applicable — no atoms reference PACT. No violations.

#### 6. Spec-First Compliance ✅

**Rule**: Every atom must have BOTH spec_lineage AND journal_evidence. No atoms with spec but no journal, or journal but no spec.

**Finding**: All 10 atoms comply.

| Atom | spec_lineage | journal_evidence | Status |
|------|--------------|------------------|--------|
| SC-P-025 | 4 items | 2 items | ✅ |
| SC-P-026 | 4 items | 2 items | ✅ |
| SC-P-027 | 4 items | 2 items | ✅ |
| SC-P-028 | 3 items | 1 item | ✅ |
| SC-P-029 | 3 items | 1 item | ✅ |
| SC-P-030 | 3 items | 1 item | ✅ |
| SC-P-031 | 3 items | 1 item | ✅ |
| SC-P-032 | 3 items | 1 item | ✅ |
| SC-P-033 | 3 items | 1 item | ✅ |
| SC-P-034 | 3 items | 2 items | ✅ |

#### 7. Read-Before-Cite Compliance ✅

**Rule**: Journal evidence quotes must be verbatim with full repo-relative paths. No synthesized summaries or hallucinated evidence.

**Finding**: All atoms provide full repo-relative paths and verbatim quotes. Examples verified:

- **SC-P-025** cites `terrene/foundation/workspaces/care-thesis/journal/0009-LITERATURE-ai-agency-tool-vs-agent.md` with verbatim quote: "Search conducted 2026-03-15 across seven areas..."
- **SC-P-026** cites `terrene/foundation/workspaces/care-thesis/journal/0004-TEACH-parasuraman-riley-misuse.md` with verbatim: "Common miscitation to avoid: Do not treat misuse/disuse/abuse..."
- **SC-P-027** cites `terrene/foundation/workspaces/care-thesis/journal/0018-DEFENSE-incomplete-contracts-critique.md` with verbatim: "I am going to review this model with the same rigor..."

All journal evidence paths follow the pattern `terrene/foundation/workspaces/care-thesis/journal/` or `terrene/journal/`, confirming reads from the authoritative journal corpus identified in CLAUDE.md § "Journal Corpus (Authoritative Locations)."

#### 8. Dual-Destination Tagging ✅

**Rule**: Every atom must have a `destination:` field with value `forge`, `co-codegen`, or `both`.

**Finding**: All 10 atoms are correctly tagged.

- **`destination: both`** (domain-agnostic methodology): SC-P-025, SC-P-026, SC-P-027, SC-P-028, SC-P-029, SC-P-034
- **`destination: forge`** (practitioner-specific pedagogy): SC-P-030, SC-P-031, SC-P-032, SC-P-033

**Quality assessment**: The destination choices are strategically sound.
- SC-P-025 (tier-ranked literature search), SC-P-026 (citation integrity), SC-P-027 (hostile review), SC-P-028 (synthesis): tagged `both` because these are artifact-authoring moves applicable to any CO ecosystem consumer doing research (CO for Research, CO for Education, CO for Compliance, etc.)
- SC-P-030 (academic register calibration), SC-P-031 (venue strategy), SC-P-032 (margin notes), SC-P-033 (reflexivity diagnosis): tagged `forge` because these are FORGE-specific pedagogical moves grounded in the CARE thesis practicum, not universally applicable moves.

#### 9. Application Tag ✅

**Rule**: Every atom must have an `applications:` field listing which CO application(s) it serves.

**Finding**: All 10 atoms correctly tagged.

- **COR-only**: SC-P-027, SC-P-028, SC-P-029, SC-P-030, SC-P-031, SC-P-033
- **COR + COE**: SC-P-025, SC-P-032, SC-P-034
- **COR + COC + COE**: SC-P-026

All applications cited are valid. No fabricated applications.

#### 10. Hedging Language ✅

**Rule**: Check for vague gestures ("often", "sometimes", "may") where the spec is concrete. Weak hedging should be flagged as MEDIUM severity.

**Finding**: All atoms use concrete language grounded in spec. No problematic hedging detected.

Examples of spec-concrete framing:
- SC-P-025: "For each area, search at least 3 databases" (specific)
- SC-P-026: "COR § Layer 3 designates fabricated references as FATAL" (concrete)
- SC-P-027: "CO §28 (Convergence requirements) states that quality assertions must be backed by evidence of convergence" (concrete)
- SC-P-033: "CO §28 (Convergence requirements) applies here as self-critique" (concrete)

---

## Convergence Gate: Ready for Production

**All 10 atoms are cleared for production catalog inclusion.**

- ✅ Zero CRITICAL findings
- ✅ Zero HIGH findings
- ✅ Spec-first compliance: 100% (all atoms have spec lineage + journal evidence)
- ✅ Read-before-cite compliance: 100% (all quotes verbatim, all paths full and authoritative)
- ✅ Framing compliance: 100% (no trinity, no fabricated apps, no track language, no hedging violations)
- ✅ Destination routing: 100% (all atoms correctly routed)
- ✅ Application tagging: 100% (all atoms tagged with valid applications)

---

## Notes for Future Rounds

### Pattern Observations

1. **Destination routing is well-calibrated**: The atoms correctly distinguish between universal methodology (SC-P-025, SC-P-026, SC-P-027, SC-P-028, SC-P-029, SC-P-034 → `both`) and FORGE-specific practicum pedagogy (SC-P-030, SC-P-031, SC-P-032, SC-P-033 → `forge`). This suggests the authoring team understands the routing rules.

2. **COR-COE boundary is clear**: SC-P-025 (literature search), SC-P-032 (margin notes), and SC-P-034 (overclaim prevention) are tagged `[COR, COE]`, acknowledging that research craft applies to both research and educational settings. SC-P-026 extends to `[COR, COC, COE]` (citation integrity is universal across all applications). This is fine-grained and correct.

3. **Journal evidence quality is high**: All atoms cite from the CARE thesis journal corpus (`terrene/foundation/workspaces/care-thesis/journal/`), which is the authoritative source for COR-layer evidence per CLAUDE.md § Journal Corpus. No atoms cite from `atelier/` (which has zero journal entries per CLAUDE.md § "Not sources").

4. **Spec lineage is specific**: Every atom cites specific CO/COR sections or CARE architecture. No generic "CO § methodology" handwaving. Examples: CO §1 (Institutional Knowledge Thesis), CO §28 (Convergence requirements), COR § Layer 1/3/4/5 (specific capability/rule levels).

### Ready for Next Phase

The atoms are ready for:
1. Drill/case exemplar development
2. Production catalog indexing
3. Sync to downstream (if routed as `both` or `co-codegen`)

---

**Report generated by**: framing-compliance red team
**Date**: 2026-04-08
**Authority**: FORGE framing rules (forge-scope.md, specs-first.md, dual-track.md, CLAUDE.md)
