---
title: Drill modality
date: 2026-04-10
destination: forge
modality: drill
---

# Drill

A drill is structured practice with a knowable-correct answer. The practitioner is given a setup, a task, and must produce an answer before seeing the model answer. Scoring is objective enough that two independent graders would score a response within one point of each other.

## What a drill is

A drill has five required sections, matching the format used in every file under `drills/exemplars/`:

1. **Setup** — the concrete scenario the practitioner is placed into. Enough detail that the task is well-defined without being a research exercise. Often includes code snippets, plan excerpts, or artifact samples.
2. **Task** — a numbered list of what the practitioner must produce. Each item is verifiable.
3. **Model answer** — the reference response. Detailed enough that a grader can compare the practitioner's answer against it and identify specific omissions or errors.
4. **Scoring rubric** — a point-weighted criteria table. Total points are typically 10.
5. **Extensions** — 2-3 harder variants that stretch the practitioner beyond the core task.

See `drills/exemplars/SC-P-023-drill-full.md` for the canonical reference.

## When to pick a drill

Choose the drill modality for an atom when:

- There is a procedure the practitioner must execute correctly (grep this, trace that, list the N things).
- The move has clear pass/fail criteria independent of interpretation.
- The practitioner benefits from repetition on varied inputs.
- Mastery can be demonstrated on a constructed scenario without a real project context.

Do not pick a drill when:

- The move only exists in real workflow context (use `brokerage-rep`).
- The move is interpretive — the right answer depends on the situation (use `case`).
- The output is an artifact, not an answer (use `build`).

## How to run a drill

**Setup phase (practitioner):**

1. Read the Setup section fully. Do not read the Task until the Setup is clear.
2. Read the Task.
3. Commit to an answer before looking at the model answer. If the practitioner runs a drill by reading the model answer first, the drill is wasted.

**Execution phase:** 4. Produce the answer. Write it down (or type it). Spoken answers do not count — the drill is an act of deliberate articulation. 5. For multi-step drills, complete each step before moving to the next. Do not jump ahead.

**Review phase:** 6. Compare the practitioner's answer to the model answer section by section. 7. Apply the scoring rubric honestly. Self-grading is acceptable if the rubric is objective; peer grading is better. 8. For each point lost, write one sentence explaining what the practitioner missed and why. This is where the drill produces learning, not in the score itself.

**Time box:** Most drills in the catalog are tagged 15 or 30 minutes. Advanced drills may run 45. Time boxes are suggestions; exceeding them is a signal that the practitioner is still learning the atom, not that the drill is broken.

## What counts as complete

A drill is complete when:

- The practitioner has committed to an answer in writing.
- The answer has been scored against the rubric.
- The practitioner has articulated what they got wrong and why.

A drill is not complete when the practitioner has read the model answer and agreed with it. Agreement is not learning.

## Common failure modes

1. **Reading the model answer first.** Turns the drill into a lecture. The fix is structural: present the Setup + Task first, withhold the Model Answer until the practitioner has committed to their own answer.

2. **Skipping the scoring rubric.** The practitioner reads the model answer, thinks "I had most of that," and moves on. The rubric forces specificity — which criterion was missed, worth how many points. Without scoring, there is no feedback signal.

3. **Running the drill once and declaring the atom learned.** Drills are designed for repetition. A practitioner who drills SC-P-023 once in one scenario has not practiced the atom — they have seen it once. Multiple drill variants (via the Extensions section, or via different drills from the same atom) are the minimum reasonable practice.

4. **Grading for completeness instead of correctness.** A rubric that awards points for "attempted all steps" is not a rubric. If the practitioner's step 3 is wrong, they lose the points for step 3.

## Assessment compatibility

Drills work as both practice and assessment. A drill used for assessment should:

- Use a scenario the practitioner has not seen before (distinct from the Setup in the drill exemplar).
- Be scored by someone other than the practitioner.
- Have the model answer withheld until after grading is complete.

The Extensions section of each drill exemplar is well-suited for assessment use — the core drill is practice, the extensions are the novel scenario.

## Atoms using this modality

See `catalog/README.md` Index 4 (Practice Modality) for the full list. ~32 atoms across all three craft layers use the drill modality.
