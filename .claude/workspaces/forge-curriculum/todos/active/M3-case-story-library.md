---
milestone: 3
title: Case Story Library — DISCOVERY/RISK journals as teaching material
dependency: M1 partial (T3.1 survey can start from workspace copy + reverse index; T3.2-T3.3 need M1 T1.4 production directory)
estimated_sessions: 1-2
---

# Milestone 3: Case Story Library

The DISCOVERY/RISK journal entries cited as evidence in the catalog are pre-formatted case stories. The case library lifts them as primary teaching material, adds a spec-mooring overlay (which CARE/EATP/CO/PACT concepts each case exercises), and maps each case to the skill atoms it instantiates.

## Todos

### T3.1 Identify standalone teaching cases from journal evidence

Read the reverse index (`01-analysis/04-reverse-index/coc-layer-index.md`) and the 57 atoms' evidence entries. Identify journal entries that work as standalone teaching cases — entries that have a clear narrative arc (situation → discovery → resolution or situation → failure → lesson), enough context to be self-contained, and connection to ≥2 spec concepts.

Filter criteria: DISCOVERY and RISK entries are primary candidates. DECISION entries are secondary (they're conclusions, not stories). CONNECTION entries are tertiary (they're observations, not narratives).

**Acceptance**: A ranked list of ≥20 candidate case entries with source path, narrative type, spec concepts exercised, and atoms served. Stored in `cases/case-candidates.md`.

### T3.2 Write case index

For each candidate case: case_id, source_path, title, one-line synopsis, spec concepts exercised, atoms it instantiates, craft layer(s), recommended use (teaching case / discussion case / assessment case), CO application(s) it demonstrates.

**Acceptance**: `cases/README.md` with index table. Every case maps to ≥1 atom and ≥2 spec concepts.

### T3.3 Write 10 exemplar cases with spec-mooring overlay

Lift the 10 strongest cases from the journal corpus. For each, produce:
1. **The situation** — what was happening when the case started (project, phase, team)
2. **The trigger** — what the practitioner noticed (the attentional pattern)
3. **The move** — what was done or should have been done (the skill atom)
4. **The outcome** — what happened as a result
5. **The spec connection** — which CARE/EATP/CO/PACT concepts this case exercises, with concrete enumeration
6. **Discussion questions** — 2-3 probing questions (at least one counterfactual)

Priority candidates (richest narrative arc, strongest spec connection):
- loom-meta 0014 (drift audit — 85% divergence discovered)
- loom-meta 0019 (sync merge not copy — the rsync damage)
- loom-meta 0047 (spec coverage gate — 11 red team rounds passed, still shipped mock data)
- loom-meta 0049 (token ablation — clean-room experiment)
- loom-meta 0050 (session-start command injection)
- kailash-py 0017 (PACT todos red team — 3 critical API mismatches)
- kailash-rs v3.8-issues 0005 (artifact red team convergence — GLOBAL BUG in loom source)
- kz-engage herald 0008 (SDK API corrections — 10 wrong assumptions)
- kailash-rs cross-sdk-governance 0008 (cross-feature vacancy bridge)
- loom-meta 0004 (learning system broken — 4579 observations, 8 duplicate instincts)

**Acceptance**: 10 case documents in `cases/exemplars/`, each with all 6 sections, ≥300 words, read-before-cite verified.
