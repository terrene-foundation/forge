---
destination: forge
layer: skill-catalog-root
date: 2026-04-07
status: 57 atoms authored (18 brokerage + 15 artifact + 24 practitioner) — red team fixes applied
sources:
  - 01-analysis/03-spec-index/{care,eatp,co,pact}-concepts.md (concept numbering)
  - 01-analysis/04-reverse-index/coc-layer-index.md (heavy-cited concepts + 20 seed candidates + contradictions)
  - 01-analysis/00-corpus-cache/ (read-before-cite source for journal evidence)
---

# FORGE Skill Catalog

The skill catalog is the **substance** of FORGE. Specs name the space; this catalog populates it. Each entry is a single skill atom — the minimum teachable unit of practitioner, artifact, or brokerage craft — moored in spec lineage and grounded in journal evidence.

## What this directory holds

```
05-skill-catalog/
  README.md                          (this file)
  practitioner/                      (Layer 1 — uniquely FORGE; destination: forge)
  artifact/                          (Layer 2 — what makes a CC artifact work; destination: co-codegen | both)
  brokerage/                         (Layer 3 — operator + governance moves; destination: co-codegen | both)
```

Subdirectories follow the three-layer craft decomposition (`02-synthesis/01-three-layer-craft-decomposition.md`). Each `.md` file in a subdirectory is one skill atom.

## Skill atom format

Every atom uses this exact frontmatter:

```yaml
---
atom_id: SC-LAYER-NNN # SC = skill catalog; LAYER = P|A|B; NNN = sequential within layer
name: <imperative skill name> # what the practitioner does
craft_layer: practitioner | artifact | brokerage
destination: forge | co-codegen | both
craft_areas: [list] # from the 7 user-named areas (prompt / behaviour / intervene-how / intervene-when / attend-how / attend-when / institutional-memory)
applications: [COC, COR, COE, ...] # which CO applications this is evidence for (multiple allowed)
spec_lineage: # at least one entry; cite by concept number + name
  - <STANDARD §N — Concept Name>
journal_evidence: # at least one entry; cite by full repo-relative path
  - path: <full repo-relative path>
    polarity: POSITIVE | NEGATIVE | MIXED
    quote: <verbatim ≤2 sentences from the entry>
practice_modality: drill | case | observation | build | brokerage-rep | governance-call
status: draft | verified | needs-second-source
---
```

Body sections (kept tight — atoms are catalog entries, not essays):

1. **The move** — 2-4 sentences naming the skill in operational terms. Imperative voice. What the practitioner _does_, not what they _understand_.
2. **When it fires** — the trigger condition. Diagnostic, not narrative.
3. **What it serves** — why this move exists at all, in spec terms.
4. **Evidence walkthrough** — for each cited journal entry, one sentence connecting the entry's content to the move. Read the entry before writing this sentence.
5. **How to drill it** — one concrete exercise. The drill is what makes the atom teachable, not the description.
6. **Common failure mode** — the most-likely way a practitioner skips or fakes the move.
7. **Related atoms** — by atom_id only. Built up over time as the catalog grows.

## Authoring rules (rigor bar)

These rules are hard. They are why the previous specialist reports are debt — they were written before the bar was set.

1. **Read before cite.** Every journal entry referenced in `journal_evidence` MUST be read in this session before its citation is written. Synthesising "from memory of what the entry probably says" is a bar violation. The corpus cache at `01-analysis/00-corpus-cache/` exists so this is always possible.
2. **Verbatim quotes only.** The `quote:` field is verbatim from the entry, ≤2 sentences. Paraphrases go in the body, not the frontmatter.
3. **Spec mooring is concrete, not aesthetic.** Cite by concept number AND concept name. "CARE § Dual Plane (loose mooring)" is not allowed; use "CARE §31 — Dual Plane Architecture" or whatever the spec index gives you. If you cannot find the concept in `03-spec-index/{care,eatp,co,pact}-concepts.md` by exact number, the citation is wrong — fix it before saving the atom.
4. **Polarity is mandatory.** Each piece of evidence is POSITIVE (the move was made and worked), NEGATIVE (the move was missed and the failure proves it should have been), or MIXED (the move was partially made; outcome was partial). Polarity is what makes the corpus teachable — anecdotes without polarity are just stories.
5. **Destination is set per-atom, not per-batch.** Layer 1 → almost always `forge`. Layer 2 → almost always `co-codegen` or `both`. Layer 3 → almost always `co-codegen` or `both`. If unsure, the test from `rules/forge-scope.md` § 4 applies: "Would a non-FORGE CO ecosystem consumer want this exact artifact?"
6. **Status defaults to `draft`.** Promote to `verified` only after a second pass that re-reads the cited entries and confirms each quote against the cache file.
7. **No catalog stubs.** If an atom has spec mooring but no journal evidence yet, do not create the file — record it as a queued atom in this README's § Queue section instead. Stubs hide gaps.
8. **PACT three-layer qualification.** Per `rules/forge-scope.md` §3, qualify PACT on first body-text use as PACT (spec), PACT (primitives), or PACT (platform). Verbatim journal quotes are exempt. Concept-number-qualified references (e.g. "PACT §10") are self-qualifying. Bare "PACT" in body-text evidence walkthroughs that refers to the SDK module should say "PACT (primitives)". The verification pass should catch remaining bare usages.

## Authored atoms

Fifty-seven atoms across all three layers, authored in six passes:

1. **Seed list** (20 atoms) — the 20-candidate list from the reverse index, hand-authored then parallelized
2. **Heavy-cited concepts** (12 atoms) — mined from CO §17, §16, §19, §3.3, §5, §15, §27, §49 entry clusters
3. **All 9 contradictions** (8 atoms, C3+C7 merged) — convergence-craft and judgment-call teaching atoms
4. **Reports 02/03 gap-fill** (10 atoms) — practitioner prompting, codegen behaviour, attention timing, institutional memory moves uncovered by the pre-rigor-bar reports but not yet captured
5. **PACT/EATP governance** (2 atoms) — verification gradient zone assignment and PDP/PEP separation
6. **Red team gap-fill** (5 atoms) — CO §16 Framework-First, §13 Layer 2 Context, §9 Layer 1 Intent, §26 Layer 4 Instructions, §14 Master Directive

### Brokerage (18 atoms)

| atom_id  | name                                                               | destination | source               |
| -------- | ------------------------------------------------------------------ | ----------- | -------------------- |
| SC-B-001 | Read-then-merge sync semantics                                     | co-codegen  | seed #8              |
| SC-B-002 | Authority-chain escalation when local fix gets reverted            | co-codegen  | seed #7              |
| SC-B-003 | Variant classification — single-sentence test rule                 | co-codegen  | seed #2              |
| SC-B-004 | Specialist delegation as required first move                       | both        | seed #19             |
| SC-B-005 | Spec-to-code conformance audit                                     | co-codegen  | seed #1              |
| SC-B-006 | Multi-round red team to numeric convergence                        | co-codegen  | seed #3              |
| SC-B-007 | Worktree-per-agent for safe parallel execution                     | co-codegen  | seed #15             |
| SC-B-008 | Convergence-as-validation across independent reds                  | co-codegen  | seed #18             |
| SC-B-009 | Cross-SDK upstream brokerage                                       | co-codegen  | seed #20             |
| SC-B-010 | Cross-repo divergence audit with numbered drift-gap list           | co-codegen  | heavy-cited §17, §49 |
| SC-B-011 | Version tracking cascade across the repo chain                     | co-codegen  | heavy-cited §17, §13 |
| SC-B-012 | Automate manifest-to-filesystem parity check and halt on staleness | co-codegen  | heavy-cited §49, §17 |
| SC-B-013 | Map cross-workspace dependency surface before parallelizing        | co-codegen  | heavy-cited §27, §17 |
| SC-B-014 | Journal decision reversals with explicit supersession references   | co-codegen  | contradiction C3+C7  |
| SC-B-015 | Reinterpret MUST rules from "human performs" to "human approves"   | co-codegen  | contradiction C5     |
| SC-B-016 | Audit cross-language parity bidirectionally                        | co-codegen  | contradiction C9     |
| SC-B-017 | Verification gradient zone assignment before delegation            | co-codegen  | PACT §19             |
| SC-B-018 | PDP/PEP separation in access control                               | co-codegen  | EATP §40             |

### Artifact (15 atoms)

| atom_id  | name                                                             | destination | source            |
| -------- | ---------------------------------------------------------------- | ----------- | ----------------- |
| SC-A-001 | Codify-with-explicit-NOT-codified                                | both        | seed #4           |
| SC-A-002 | Frontend mock data detection                                     | both        | seed #10          |
| SC-A-003 | Defense-in-depth codification across multiple artifact locations | both        | seed #5           |
| SC-A-004 | Layer 3 hook security audit                                      | both        | seed #11          |
| SC-A-005 | Encapsulation as security boundary in signed structs             | co-codegen  | seed #13          |
| SC-A-006 | Negative tests for deny-by-default                               | both        | seed #14          |
| SC-A-007 | Zero-config security audit                                       | both        | heavy-cited §3.3  |
| SC-A-008 | Rule classification — critical vs advisory                       | co-codegen  | heavy-cited §19   |
| SC-A-009 | Progressive disclosure architecture for CC artifacts             | co-codegen  | heavy-cited §15   |
| SC-A-010 | Token budget — baked-in vs external boundary decision            | co-codegen  | contradiction C8  |
| SC-A-011 | Framework-First check before building                            | both        | red team §16      |
| SC-A-012 | CLAUDE.md authoring as Layer 2 Context                           | both        | red team §13, §14 |
| SC-A-013 | Agent specialization routing as Layer 1 Intent                   | co-codegen  | red team §9, §11  |
| SC-A-014 | Milestone decomposition with phase-skip prevention               | both        | red team §26, §27 |
| SC-A-015 | Layer 2 Context corpus structuring (rules/skills/agents/cmds)    | co-codegen  | red team §13, §14 |

### Practitioner (24 atoms)

| atom_id  | name                                                        | destination | source               |
| -------- | ----------------------------------------------------------- | ----------- | -------------------- |
| SC-P-001 | Adjacent-but-disconnected module pattern                    | forge       | seed #6              |
| SC-P-002 | Empirical model-vs-rule ablation                            | forge       | seed #9              |
| SC-P-003 | Cross-feature interaction red team                          | forge       | seed #12             |
| SC-P-004 | Per-file disposition labelling pre-implementation           | forge       | seed #16             |
| SC-P-005 | Friction-gradient inversion diagnosis                       | forge       | seed #17             |
| SC-P-006 | Package boundary decision — module vs separate package      | forge       | heavy-cited §16, §17 |
| SC-P-007 | Session completion state definition before starting         | forge       | heavy-cited §28, §29 |
| SC-P-008 | Dependency chain fragility tracing                          | forge       | heavy-cited §3.3     |
| SC-P-009 | Architecture doc as testable contract against codebase      | forge       | heavy-cited §29, §16 |
| SC-P-010 | Timing race condition red team                              | forge       | heavy-cited §3.3, §5 |
| SC-P-011 | Audit categorization blindness to progressive disclosure    | forge       | contradiction C1     |
| SC-P-012 | Rule demotion vs strengthening judgment                     | forge       | contradiction C2     |
| SC-P-013 | Estimate self-contradiction as high-confidence signal       | both        | contradiction C4     |
| SC-P-014 | Zero-tolerance scope discipline — failures vs gaps          | forge       | contradiction C6     |
| SC-P-015 | Ask for the contradictions — "what would invalidate this?"  | both        | Report 02 §1.4       |
| SC-P-016 | Detect overcorrection — bidirectional confidence signal     | forge       | Report 03 A1         |
| SC-P-017 | False-positive learning detection                           | forge       | Report 02 §4.4       |
| SC-P-018 | Split the long-pole into fast-unblock + parallel-completion | forge       | Report 02 §3.2       |
| SC-P-019 | Cold-start continuity — notes as instructions to next-self  | both        | Report 02 §6.1, §7.3 |
| SC-P-020 | Second occurrence as structural signal                      | forge       | Report 02 §6.6       |
| SC-P-021 | Reference sweep after rename                                | forge       | Report 03 I10        |
| SC-P-022 | Cross-model gap reading — target the weakest model          | forge       | Report 02 §2.6       |
| SC-P-023 | Insert a gate, not a check                                  | forge       | Report 02 §3.3       |
| SC-P-024 | Fallback permanence detection                               | forge       | Report 02 §4.5       |

## Queue

All major sources exhausted for the COC layer. Red team round 1 findings resolved:

- 20/20 seed candidates authored
- 9/9 cross-entry contradictions authored
- All top 15 heavy-cited concepts (≥10 entries) now have at least 1 dedicated atom — including CO §16 (SC-A-011), §13 (SC-A-012, SC-A-015), §9 (SC-A-013), §26 (SC-A-014), §14 (SC-A-012, SC-A-015)
- Reports 02/03 gap-filled (10 additional practitioner atoms)
- PACT §19 and EATP §40 governance concepts authored
- 18 application-general atoms cross-tagged `[COC, COR, COE]`
- 3 practitioner atoms rerouted from `forge` to `both` (SC-P-013, SC-P-015, SC-P-019)
- PACT body-text qualification rule added (rule 8) for verification pass

Remaining potential sources (diminishing returns):

- Deeper mining of CO §17 (47 entries) and CO §16 (45 entries) — bulk of entries already cited by multiple atoms
- Medium-cited CARE concepts (§44 Failure Modes 9 entries, §6 Trust Verification Bridge 8 entries, §30 Constraint Envelopes 8 entries) — largely philosophy, not engineering craft; better suited for a CARE-facing pass when COR/COE applications are built
- Verification pass to promote all 52 draft atoms to `verified` status

## How this catalog feeds downstream

- **`lyceum/courses/`** — role-tailored sequences pull atoms by `craft_areas`, `applications`, and `spec_lineage`. The same atom can appear in multiple courses without forking.
- **`atelier/co-codegen/`** — atoms tagged `destination: co-codegen` or `both` get their canonical write-up in co-codegen on the next sync pass. FORGE references those write-ups; it does not duplicate them.
- **Drill library** — each atom's "How to drill it" section is the seed for a recurring micro-exercise. Drills land in `lyceum/programs/forge/drills/` once the catalog is dense enough to route from.
- **Case story library** — DISCOVERY/RISK journals cited as evidence become the case-story corpus. Cases are pre-formatted by the journal authors; FORGE just adds the spec-mooring overlay.
