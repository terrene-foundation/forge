---
title: Practice Modalities — how FORGE atoms are practiced
date: 2026-04-10
destination: forge
---

# Practice Modalities

Every atom in the FORGE catalog is tagged with a `practice_modality` that specifies _how_ the skill is learned. The modality is not a teaching technique — it is a structural property of the atom: some atoms are drillable (there is a right answer), others must be practiced in situ (the move only exists in a real workflow context), others are discussed (the move is an interpretive act best examined through narrative).

This directory defines what each modality is, how to run it, and what counts as complete. Course designers reading the modalities before selecting atoms can match their delivery format to the atom's actual shape.

## Five modalities in use

| Modality        | Atoms | Character                                                                 |
| --------------- | ----- | ------------------------------------------------------------------------- |
| `drill`         | ~32   | Structured practice with a knowable-correct answer                        |
| `case`          | ~22   | Journal-sourced narrative dissected for the move it contains (or missed)  |
| `brokerage-rep` | ~14   | Real governance or sync move performed as part of the practitioner's work |
| `build`         | ~6    | Construct an artifact end-to-end; the output is a real deliverable        |
| `observation`   | 1     | Watch a move unfold live and classify it against the atom's definition    |

A sixth modality, `governance-call`, is declared in the FORGE framework but has no atoms tagged with it in the current catalog.

## How to pick the right modality

The catalog has already picked the modality for each atom — the question for a course designer is whether the delivery format matches what the modality requires.

- If an atom is tagged `drill`, the practitioner must _do_ the exercise. Reading the drill exemplar and discussing the model answer is not equivalent.
- If an atom is tagged `case`, the practitioner must _read the journal entry first_, then work through the 6-section structure. Skipping straight to "what was the move?" collapses the case into a lecture.
- If an atom is tagged `brokerage-rep`, the practitioner must be in a workflow where the move is _actually required_. Simulating a sync is not a brokerage-rep — it is at best a drill.
- If an atom is tagged `build`, the practitioner must _ship a real artifact_ at the end. The output is inspectable and reusable.
- If an atom is tagged `observation`, the practitioner must _watch a live session or red team round_, not a recording or a transcript.

The modality constrains the format. Downstream courses that change the modality lose the atom's pedagogy — the atom is still "covered" but the skill is not practiced.

## Cross-modality notes

- **Drills and cases compose well**: a case can be assigned as pre-reading, with a drill following that exercises the same atom in a new scenario. This gives the practitioner one narrative exposure and one active repetition without either modality replacing the other.
- **Brokerage-reps require scheduling**: unlike drills and cases, brokerage-reps cannot be scheduled at the course's convenience — they fire when the workflow fires. Courses that include brokerage-rep atoms must either run alongside a real project or defer the rep until the practitioner is on one.
- **Builds are the longest**: build atoms typically take hours to a full session to complete. Courses should budget accordingly and avoid stacking multiple builds in one session.
- **Observation is rarest**: only one atom uses it. Courses covering that atom can substitute a well-annotated recording only if they accept the trade-off that live observation is the canonical modality.

## What not to do

- Do not convert a `case` into a lecture by summarizing the journal entry. The narrative is the vehicle; removing it removes the move.
- Do not convert a `brokerage-rep` into a drill by scripting the governance move. The rep's validity comes from its real-world consequences.
- Do not convert a `drill` into a read-along by walking through the model answer. The practitioner must commit to an answer before seeing the model.
- Do not convert a `build` into a code walkthrough. The practitioner must construct the artifact themselves, even if their version is uglier than the reference.
