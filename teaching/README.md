---
title: FORGE Teaching Scaffolding — reusable course-design support
date: 2026-04-10
destination: forge
---

# FORGE Teaching Scaffolding

Reusable scaffolding that lets downstream courses consume the FORGE library without reinventing learning objectives, modality definitions, or sequencing logic. The content here is **knowledge about how to teach the atoms**, not delivery for any specific audience.

## Scope

This directory contains material that:

1. Every downstream course would otherwise have to write from scratch
2. Is derived from the catalog, not invented alongside it
3. Stays audience-neutral — no cohort, no week, no assignment schedule

Audience-specific material (session plans, slide decks, cohort rubrics, facilitator cues for a particular group) belongs in `lyceum/courses/{course-name}/`, not here. The lane rule from `lyceum/.claude/rules/lane-isolation.md` applies: programs hold the knowledge; courses package it for instructors.

## Contents

```
teaching/
  README.md           This file — what the scaffolding layer is and is not
  outcomes.md         Learning outcome statement for each of the 67 atoms
  prereqs.md          Atom-to-atom prerequisite graph (hard + soft edges)
  rubrics.md          Mastery rubric per atom (novice → advanced)
  clusters.md         8 themed atom bundles for course selection
  facilitation.md     Case facilitation notes (25 cases — common wrong answers, probes, push-backs)
  modalities/         How each practice modality runs
    drill.md          Structured practice with known-correct answer
    case.md           Narrative-first discussion moves
    build.md          Construct an artifact end-to-end
    brokerage-rep.md  Real governance or sync move in the workflow
    observation.md    Watch a move unfold and classify it
    README.md         Index and cross-modality notes
```

Already existing in `catalog/`:

- `catalog/teaching-sequences.md` — 7 curated atom sequences where atom A's output feeds atom B's input (5 COC: rule optimization, sync/drift, red team, 5-layer walkthrough, attention-timing; 2 COR: literature integrity, peer review resilience)

## How to use this scaffolding

**Course designer workflow:**

1. Read `outcomes.md` to scan what each atom actually teaches (one sentence each)
2. Browse `clusters.md` for themed bundles that match your course focus
3. Pull atoms relevant to your audience from `catalog/README.md`
4. Check `prereqs.md` to see which atoms build on which — topological layer view for quick sequencing
5. Read the relevant modality guides in `modalities/` to understand how to run the practice
6. Use `rubrics.md` for assessment criteria (novice / developing / proficient / advanced per atom)
7. Use `catalog/teaching-sequences.md` if your course touches one of the 5 existing ordered sequences
8. Build your course-specific session plan in `lyceum/courses/{your-course}/`, referencing atom IDs back into forge

**What not to do here:**

- Do not add session plans, time boxes, or cohort-specific pacing to this directory
- Do not duplicate atom content — always reference back to `catalog/`
- Do not add domain-specific examples that only fit one course's audience

## Lane boundary note

The facilitation notes in `facilitation.md` sit at the edge of the programs/courses boundary. They stay in forge because they are generic to the case — not cohort-specific, not audience-specific. If an instructor needs facilitation guidance tailored to a particular group (e.g., "what MBA students typically get wrong on CASE-04"), that goes in the course, not here.
