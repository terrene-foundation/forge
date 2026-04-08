# FORGE Skill Catalog

The canonical, contributor-neutral library of **67 practitioner skill atoms** for the CO ecosystem (57 COC-layer + 10 COR-layer). This catalog is the **substance** of FORGE — moored in the four Foundation standards (CARE / EATP / CO / PACT) and grounded in journal entries of real craft evidence from both codegen (232 entries) and research (~58 entries) corpora.

**This is not a course.** Downstream role-tailored sequences live in `lyceum/courses/` and pull atoms from here. Different courses mix and match the same atoms without forking. Methodology content tagged `co-codegen` or `both` is also routed to `atelier/co-codegen/` for the broader CO ecosystem.

## What is a skill atom?

A skill atom is the minimum teachable unit of practitioner, artifact, or brokerage craft. Each atom has:

- **A name** — imperative, describing what the practitioner _does_
- **Spec lineage** — which CARE/EATP/CO/PACT concepts it derives from
- **Journal evidence** — verbatim quotes from real craft journals, with polarity (POSITIVE/NEGATIVE/MIXED)
- **A drill** — a concrete exercise that makes the atom teachable
- **A common failure mode** — the most-likely way practitioners skip or fake the move
- **Related atoms** — how this atom composes with others

## Three craft layers

| Layer            | What it is                                                                                                               | Count | Destination             |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------ | ----- | ----------------------- |
| **Brokerage**    | Operator + governance moves: sync, authority, variant classification, cross-SDK coordination                             | 18    | mostly `co-codegen`     |
| **Artifact**     | What makes a CC artifact work: rules, hooks, defense-in-depth, progressive disclosure                                    | 15    | mostly `both`           |
| **Practitioner** | Session-level moves: prompting, attention, intervention, reading codegen behaviour, institutional memory, research craft | 34    | mixed (24 COC + 10 COR) |

## Index 1 — By craft layer

See subdirectories: `brokerage/`, `artifact/`, `practitioner/`.

## Index 2 — By craft area

The seven craft areas from the programme owner. Counts are derived from each atom's `craft_areas` frontmatter (atoms may carry multiple craft-area tags, so counts sum to more than 57).

| Craft area               | Count | Description                                                      | Representative atoms                                                                                                                                                                  |
| ------------------------ | ----- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **attend-how**           | 25    | How to pay attention — what to look at                           | SC-P-001 (adjacent modules), SC-P-003 (cross-feature), SC-P-008 (dependency fragility), SC-P-009 (architecture as contract), SC-P-010 (timing races), SC-P-011 (audit categorization) |
| **institutional-memory** | 21    | How to keep knowledge across sessions                            | SC-A-001 (codify NOT-codified), SC-A-009 (progressive disclosure), SC-A-012 (CLAUDE.md authoring), SC-B-010 (drift audit), SC-B-011 (version tracking)                                |
| **intervene-how**        | 13    | How to intervene once you've decided to                          | SC-P-018 (split long-pole), SC-P-021 (reference sweep), SC-P-023 (gate not check), SC-A-008 (rule classification)                                                                     |
| **intervene-when**       | 13    | When to intervene — timing signals                               | SC-B-002 (authority escalation), SC-B-015 (MUST-rule reinterpretation), SC-P-012 (rule demotion), SC-P-024 (fallback permanence)                                                      |
| **attend-when**          | 7     | When to pay attention — triggering moments                       | SC-P-019 (cold start), SC-P-020 (second occurrence), SC-B-005 (spec-to-code audit)                                                                                                    |
| **behaviour** ⚠️ thin    | 2     | How codegen behaves — reading the model's native floor and drift | SC-P-016 (overcorrection), SC-P-022 (cross-model gap) — both dual-tagged `attend-how`                                                                                                 |
| **prompt** ⚠️ thin       | 1     | How to frame requests to the model                               | SC-P-015 (ask for contradictions)                                                                                                                                                     |

### Balance audit — thin areas

Two craft areas sit below the <3-atom threshold (T5.2). Both are **corpus gaps, not authoring gaps**: the COC journals capture intervention and attention craft in depth but rarely capture prompt-level and model-behaviour craft as explicit first-class moves.

- **prompt (1 atom)** — The corpus has many entries where a prompt-level move drove an outcome, but the move is usually implicit (the entry shows the result, not the prompt text that produced it). SC-P-015 (ask for contradictions) is the one clear explicit case because it was structurally encoded in the journal rule requiring a "For Discussion" section with falsification questions. Expanding this area requires either (a) prompt-level telemetry that captures the actual prompts sent to the model, which the current observation pipeline does not produce, or (b) a dedicated pass mining conversation logs from past sessions for recurring prompt moves. Neither source is available in the current COC corpus.
- **behaviour (2 atoms)** — Reading the model's native floor and drift is a subtle craft area. SC-P-016 (detect overcorrection) and SC-P-022 (cross-model gap reading) cover the two clearest cases. The corpus has additional scattered evidence of behaviour-reading moves (e.g. noticing that a model invented a plausible-but-wrong API, or that a refactor "drifted" toward a style the model prefers) but the evidence rarely has the clarity needed for a rigor-bar atom. A dedicated pass mining contamination and drift patterns would likely surface 2-4 more atoms; this is queued for a future cycle rather than forced now at the risk of lowering the bar.

Neither thin area is a blocker for M5 — both are documented corpus gaps with forward paths. COR and COE passes may surface additional evidence (research workflows have explicit prompt-engineering moves; education workflows have explicit behaviour-reading moves for learner models), and those applications can back-fill the COC-layer gap when their passes complete.

## Index 3 — By spec lineage

Atoms by which standard they primarily serve. Most atoms cite 2-3 concepts across multiple standards.

### CARE (philosophy)

- CARE §4 Human-on-the-Loop: SC-B-015
- CARE §6 Trust Verification Bridge: SC-B-001

### EATP (protocol)

- EATP §16 Delegation Record: SC-A-005
- EATP §40 Enforcement Model (PDP/PEP): SC-B-018

### CO (methodology)

- **CO §17 Single Source of Truth** (most cited): SC-B-001, SC-B-002, SC-B-003, SC-B-005, SC-B-009, SC-B-010, SC-B-011, SC-B-012, SC-B-013, SC-A-001, SC-A-009, SC-A-011, SC-P-001, SC-P-006, SC-P-009
- **CO §16 Framework-First**: SC-A-011, SC-B-003, SC-P-006, SC-P-009
- **CO §19 Layer 3 Guardrails**: SC-A-002, SC-A-003, SC-A-004, SC-A-007, SC-A-008, SC-P-010
- **CO §29 Evidence-Based Completion**: SC-A-002, SC-P-004, SC-P-007, SC-P-009, SC-P-014
- **CO §28 Approval Gate**: SC-B-001, SC-B-006, SC-B-014, SC-P-007, SC-P-013, SC-P-015, SC-A-014
- **CO §3.3 Safety Blindness**: SC-A-002, SC-A-004, SC-A-007, SC-P-003, SC-P-005, SC-P-008, SC-P-010, SC-P-017
- **CO §13 Layer 2 Context**: SC-A-009, SC-A-012, SC-A-015, SC-B-011
- **CO §9 Layer 1 Intent**: SC-A-013, SC-B-004, SC-P-019
- **CO §26 Layer 4 Instructions**: SC-A-014
- **CO §14 Master Directive**: SC-A-012, SC-A-015
- **CO §15 Progressive Disclosure**: SC-A-009, SC-A-012, SC-P-011
- **CO §5 Deterministic Enforcement**: SC-A-005, SC-A-003, SC-A-004, SC-A-008, SC-B-018, SC-P-010, SC-P-024
- **CO §39 Layer 5 Learning**: SC-A-001, SC-A-015, SC-P-012, SC-P-017, SC-P-019
- **CO §47 Knowledge Curator**: SC-A-001
- **CO §1 Institutional Knowledge Thesis**: SC-P-002, SC-P-005, SC-P-012, SC-P-016, SC-P-022
- **CO §22 Advisory Rules**: SC-P-002, SC-P-012, SC-P-022
- **CO §3.2 Convention Drift**: SC-P-005, SC-P-020, SC-P-024
- **CO §11 Routing Rules**: SC-A-013, SC-B-004
- **CO §27 Structured Workflow**: SC-A-014, SC-B-013, SC-P-018
- **CO §49 Enforcement Reliability**: SC-B-005, SC-B-006, SC-B-008, SC-B-010, SC-B-012, SC-A-006, SC-P-011
- **CO §33 Phase Analyze**: SC-P-004

### PACT (governance)

- **PACT §10 Monotonic Tightening**: SC-A-005, SC-B-002, SC-B-009
- **PACT §11 Role Envelope**: SC-B-002, SC-B-004, SC-B-017
- **PACT §19 Verification Gradient**: SC-B-017
- **PACT §23 Blocked Zone**: SC-A-006
- **PACT §35 Bridges**: SC-P-003
- **PACT §40 EATP Integration**: SC-B-005, SC-P-001
- **PACT §51 Vacant Roles**: SC-P-003

## Index 4 — By practice modality

How the atom is learned:

| Modality          | Description                                           | Count | Atoms                                                                                                                                                          |
| ----------------- | ----------------------------------------------------- | ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **drill**         | Recurring micro-exercise with a concrete answer key   | ~20   | SC-B-003, SC-B-012, SC-A-011, SC-P-003, SC-P-004, SC-P-007, SC-P-009, SC-P-010, SC-P-013, SC-P-015, SC-P-021, SC-P-023, ...                                    |
| **case**          | Narrative journal entry with spec-mooring overlay     | ~15   | SC-P-001, SC-P-005, SC-P-008, SC-P-011, SC-P-012, SC-P-014, SC-P-016, SC-P-020, SC-P-022, SC-P-024, SC-A-010, ...                                              |
| **brokerage-rep** | Reproducible operator move performed at a gate        | ~14   | SC-B-001, SC-B-002, SC-B-004, SC-B-005, SC-B-006, SC-B-007, SC-B-008, SC-B-009, SC-B-010, SC-B-011, SC-B-013, SC-B-014, SC-B-015, SC-B-016, SC-B-017, SC-B-018 |
| **build**         | The practitioner builds a CC artifact as the exercise | ~4    | SC-P-002, SC-A-012, SC-A-013, SC-A-015                                                                                                                         |
| **observation**   | Watch-and-classify exercise                           | 1     | (one atom)                                                                                                                                                     |

## Index 5 — By application

All 57 COC-layer atoms are evidence for COC (codegen). **18 atoms** are cross-tagged `[COC, COR, COE]` — they apply directly to research and education without reformulation:

- **Brokerage (5)**: SC-B-005 spec-to-code conformance, SC-B-006 multi-round red team, SC-B-008 convergence-as-validation, SC-B-015 MUST-rule reinterpretation, SC-B-017 verification gradient zone assignment
- **Artifact (1)**: SC-A-001 codify-with-NOT-codified
- **Practitioner (12)**: SC-P-002, SC-P-007, SC-P-011, SC-P-012, SC-P-013, SC-P-014, SC-P-015, SC-P-016, SC-P-018, SC-P-020, SC-P-023, SC-P-024

### COR layer (10 atoms, added 2026-04-08)

The COR application pass added 10 new practitioner atoms (SC-P-025 through SC-P-034) capturing research-specific craft not surfaced by the COC corpus. Evidence base: ~58 entries from `terrene/foundation/workspaces/care-thesis/journal/` plus `terrene/journal/0006-DISCOVERY-pact-skill-thesis-drift.md`.

| Atom     | Title                                                       | Application     | Destination |
| -------- | ----------------------------------------------------------- | --------------- | ----------- |
| SC-P-025 | Tier-ranked literature search with verification             | [COR, COE]      | both        |
| SC-P-026 | Citation integrity audit — misquotation and spec drift      | [COR, COC, COE] | both        |
| SC-P-027 | Hostile reviewer simulation as preemptive defense           | [COR]           | both        |
| SC-P-028 | Multi-perspective synthesis into minimal publishable model  | [COR]           | both        |
| SC-P-029 | Post-publication gap check for recent literature            | [COR]           | both        |
| SC-P-030 | Academic register calibration with detection-bias awareness | [COR]           | forge       |
| SC-P-031 | Venue strategy as constraint envelope for argument scope    | [COR]           | forge       |
| SC-P-032 | Margin note as deliberation artifact                        | [COR, COE]      | forge       |
| SC-P-033 | Reflexivity diagnosis in self-referential research          | [COR]           | forge       |
| SC-P-034 | Overclaim prevention — qualify every superlative            | [COR, COE]      | both        |

The COR pass built on the 18 cross-tagged atoms as foundation rather than re-authoring them — see `.claude/workspaces/forge-cor/01-analysis/01-research/03-existing-cor-atoms-audit.md` for the audit.

Future application passes (COE, compliance, finance, governance, learners) will follow the same pattern. See `future-passes.md`.

## How downstream consumers use this catalog

### lyceum/courses/

Role-tailored sequences (AI Engineer, AI Business Consultant, etc.) select atoms by craft area, application, or spec lineage and sequence them into course modules. The same atom can appear in multiple courses without forking. See `lyceum/courses/*/sequence.md` for course-specific atom selections.

### atelier/co-codegen/

Atoms tagged `destination: co-codegen` or `destination: both` are routed to co-codegen on each sync pass. Co-codegen is the contributor-neutral methodology repo for the broader CO ecosystem. FORGE references co-codegen's canonical versions; it does not duplicate them.

### Drill library (`../drills/`)

Each atom's "How to drill it" section is extracted, expanded, and indexed in the drill library. 10 exemplar drills are written in full form.

### Case story library (`../cases/`)

DISCOVERY and RISK journal entries cited as evidence become standalone teaching cases with a spec-mooring overlay.

## Status

- **67 atoms total**: 18 brokerage + 15 artifact + 34 practitioner (24 COC-layer + 10 COR-layer)
- **COC-layer status**: verified (M1-M5 complete, red team rounds 2-4 converged)
- **COR-layer status**: verified (initial pass, red team rounds 1-N converged 2026-04-08)
- **Evidence base**: 232 COC-layer journal entries from loom/terrene + ~58 COR-layer entries from terrene/foundation/workspaces/care-thesis/
- **Spec coverage**: all top 15 heavy-cited COC concepts have ≥1 dedicated atom; 10 COR-specific concepts have ≥1 dedicated atom

Analysis history lives in `../.claude/workspaces/`:

- COC pass: `forge-curriculum/01-analysis/02-synthesis/02-decision-log.md` (D1–D27)
- COR pass: `forge-cor/01-analysis/`
