---
milestone: 2
title: Drill Library — recurring micro-exercises from atom drill sections
dependency: M1 partial (T2.1 survey can start from workspace copy; T2.2-T2.3 need M1 T1.4 production directory)
estimated_sessions: 1-2
---

# Milestone 2: Drill Library

Each of the 57 atoms has a "How to drill it" section with a concrete exercise specification. The drill library extracts, expands, and indexes these into a standalone set of recurring exercises that downstream courses can sequence.

## Todos

### T2.1 Extract drill specifications from all 57 atoms

Read every atom's "How to drill it" section. Produce a structured drill spec for each: drill_id, source_atom, setup (what the practitioner gets), task (what they do), scoring criteria (what correct looks like), time_box (suggested minutes), difficulty (beginner/intermediate/advanced), craft_area, practice_modality.

**Acceptance**: 57 drill spec files in `drills/specs/`, one per atom.

### T2.2 Write drill index

Index drills by 4 views: by craft area (7 areas), by practice modality (6 types), by difficulty (3 levels), and by spec lineage (which standard each drill exercises). The index is the entry point for downstream course builders selecting drills for their sequence.

**Acceptance**: `drills/README.md` with 4 index tables. Every drill appears in at least 2 index views.

### T2.3 Expand 10 exemplar drills to full form

The atom drill sections are seeds (~100 words each). Pick the 10 highest-leverage drills (prioritize: diverse craft areas, strong evidence, concrete scoring) and expand each to a full drill document: setup instructions, materials (which journal entries or code samples the practitioner needs), step-by-step walkthrough, model answer, common mistakes with explanations, extension questions.

Priority candidates:
- SC-B-003 (variant classification — answer key from entry 0048)
- SC-B-005 (spec-to-code conformance — answer key from entries 0014/0015)
- SC-A-011 (framework-first check — answer key from "already exists" entries)
- SC-P-004 (per-file disposition labelling — worked example from kaizen-kz 0014)
- SC-P-009 (architecture doc as contract — answer key from kailash-rs 0001)
- SC-P-015 (ask for contradictions — generative exercise)
- SC-P-019 (cold-start continuity — diagnostic exercise from entry 0017)
- SC-B-012 (manifest parity automation — build exercise)
- SC-A-014 (milestone decomposition — structured planning exercise)
- SC-P-022 (cross-model gap reading — comparative exercise from entry 0049)

**Acceptance**: 10 full-form drill documents in `drills/exemplars/`, each ≥500 words with setup + task + model answer + scoring + extensions.
