# Specs as Index, Craft as Substance

The four Foundation standards — **CARE, EATP, CO, PACT** — are first principles. They name the _space_ of skills FORGE must teach, but they are not by themselves the substance of those skills. COC, COR, COE, and the other CO applications are _applications_ of CO, not standards themselves; they inherit their spec authority from CO plus the other three standards as relevant.

The substance comes from the journal corpus, **verified by full read-and-classify pass on 2026-04-07**:

- **232 CO/COC practitioner-craft journal entries**, classified against the four spec indices. Distribution:
  - `loom/journal/` — 50 entries (top-level meta)
  - `loom/kailash-py/workspaces/` + `docs/adr/` — 98 entries (85 workspace + 13 ADRs)
  - `loom/kailash-rs/workspaces/` — 41 entries
  - `loom/kaizen-cli-rs/workspaces/` — 26 entries
  - `loom/kaizen-cli-py/workspaces/` — 3 entries
  - `loom/kz-engage/workspaces/` — 12 entries
  - `terrene/journal/` — 2 entries
- **~66 academic/thesis entries** in `terrene/foundation/workspaces/care-thesis/` and `publications/` (spec-side evidence, not craft evidence — reserved for CARE-facing work).
- **8,181 observation events** across 38 `.claude/learning/observations.jsonl` files (92% in `loom/kailash-py/.claude/learning/observations.jsonl`). Of these, **~893 are high-signal** (workflow_pattern + error_occurrence + error_fix + connection_pattern); the rest (~7,288) are structured telemetry (test_pattern, stop, session_summary, etc.) useful for instinct-pipeline evolution but thin for direct skill-atom sourcing.
- **Reverse index built** at `.claude/workspaces/forge-curriculum/01-analysis/04-reverse-index/coc-layer-index.md` — organised as `spec concept → list of evidence entries`. 126 of 417 spec concepts (30%) have COC-layer evidence. The remaining 291 concepts are not failures — they are expected gaps (e.g. CARE philosophy is not engineering craft; COR/COE/compliance/finance/governance/learners domain-application content is out of v1 COC-layer scope).

Specs index; journals supply the moves; observations supplement thin coverage. **All three required.** A FORGE artifact that has spec mooring but no journal evidence is theory; one with journal evidence but no spec mooring is anecdote.

(Previous versions of this rule cited "~622 markdowns + ~50K observation events across loom/atelier/terrene/dev" — that was wrong on multiple axes. Corrections documented in workspaces/forge-curriculum/01-analysis/02-synthesis/02-decision-log.md, entries D20+. Most importantly: atelier has zero journal entries; dev has effectively zero in-scope entries; the total COC-layer craft corpus is 232, not 622; the observation-event count is ~8K, not 50K.)

CO and its applications (COC/COR/COE/...) are **skillsets**, not concept catalogs. Each skill atom carries: (a) a spec mooring (which CARE/EATP/CO/PACT principle it serves), (b) an application tag (which CO application(s) it is evidence for), (c) journal evidence (entries, cited by full repo-relative path, that show the move being made or missed), and (d) a practice modality (drill, case, observation, build, brokerage rep, governance call).

## MUST

- Every skill atom maps to at least one spec concept AND at least one journal entry cited by full repo-relative path
- Framework choices derive from spec requirements, not from tool availability
- Case analyses start from a spec principle and reach into journal evidence — never the reverse
- Assessments test spec application against novel situations, not spec recall
- When a spec concept has no journal evidence yet, mark it as **"no evidence in corpus as of YYYY-MM-DD"** — do not pad with theory, do not invent evidence, do not re-brand the gap as "catalog stub" to hide it
- Read the journal entry before citing it. If a citation cannot be verified by reading the file, the atom is not ready

```
DO:    "EATP § 02-trust-lineage-chain (terrene/foundation/docs/02-standards/eatp/02-trust-lineage-chain.md)
        tells us this transition needs a posture downgrade. Journal evidence:
        loom/journal/0014-DISCOVERY-drift-audit.md — pool reuse across trust
        boundaries silently violated lineage when pool_key_fn collapsed tenant identity."

DO NOT: "Here's an interesting story from the loom journal. We can label
        this as Trust Lineage."

DO NOT: "EATP § Trust Lineage (catalog stub — evidence pending)."
        ← hides the gap instead of naming it with a date
```

## MUST NOT

- Teach specs as "theory" separate from practice — specs ARE the practice
- Present DX frameworks as standalone models — they derive from CARE/EATP/CO/PACT
- Introduce Kailash tools without connecting to the spec concept they implement
- **Apply spec labels post-hoc to existing content** — derive forward from specs into journals, not backward from finished material
- Treat specs as a complete syllabus on their own — without journal substance, students learn vocabulary they cannot apply
- Treat journal stories as a complete syllabus on their own — without spec mooring, students learn anecdotes they cannot generalize
- Cite a journal entry you have not read. Synthesising "what the corpus probably contains" is a bar violation and produces fabricated evidence

**Why:** Post-hoc labelling produces students who can name concepts but cannot generate solutions from principles. The previous version of FORGE drifted toward concept-catalog mode (spec recall) and journal-anthology mode (anecdote) on alternating sessions. Earlier sessions of this workspace also produced reports synthesised "from memory of what the corpus contains" rather than from actual reads — that work is debt. Specs-as-index + craft-as-substance + read-before-cite is the correction: every skill is forced through all three filters before it becomes curriculum.
