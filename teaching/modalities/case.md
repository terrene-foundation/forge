---
title: Case modality
date: 2026-04-10
destination: forge
modality: case
---

# Case

A case is a narrative-first learning move. The practitioner reads a real journal entry from the COC corpus, works through what happened, and maps the situation onto the spec concepts the atom serves. The journal narrative is the vehicle — without the narrative, the atom collapses into a definition.

## What a case is

Every exemplar case in `cases/exemplars/` has six sections:

1. **The Situation** — project, phase, what was happening
2. **The Trigger** — what the practitioner noticed (the attentional pattern that fired the move)
3. **The Move** — what was done or should have been done
4. **The Outcome** — what happened as a result
5. **The Spec Connection** — which CARE / EATP / CO / PACT concepts connect, with concrete enumeration
6. **Discussion Questions** — 2-3 probing questions, at least one counterfactual

Each case carries frontmatter with `case_id`, `source_path`, `atoms_served`, `spec_concepts`, `craft_layer`, and `recommended_use` (teaching / discussion / assessment).

## When to pick a case

Choose the case modality for an atom when:

- The move is interpretive — the right response depends on noticing the right pattern at the right time.
- The skill is best taught by walking through a concrete instance where the move was executed (or conspicuously missed).
- The journal corpus contains a well-scoped narrative that naturally carries the move.
- The attentional quality of the move matters as much as the procedure — the practitioner must learn _what to notice_, not just _what to do_.

Do not pick a case when:

- The move is mechanical and has a right answer (use `drill`).
- The journal corpus has no clean narrative instance (author the atom with a different modality or defer it until evidence exists).
- The move requires the practitioner to actually execute it in a workflow (use `brokerage-rep`).

## How to run a case

**Assignment phase:**

1. Assign the case as pre-reading. The practitioner must read the full journal entry at the cited `source_path`, not a summary.
2. Practitioners come to the session having already read Sections 1-4 of the case file (Situation → Outcome), but _not_ Section 5 (Spec Connection) or Section 6 (Discussion Questions).

**Session phase:** 3. Open with the Situation. Re-ground the narrative in the practitioner's working memory; do not assume the pre-read sticks. 4. Walk through the Trigger. Ask: "What did the practitioner in this story notice first?" This is the attentional move — the moment of pattern recognition that made everything else possible. 5. Work through the Move. Distinguish what was actually done from what, in hindsight, should have been done. Many cases involve a missed move that was only identified after the fact. 6. Open the Discussion Questions. These are not rhetorical. The facilitator should push for specific answers, including the counterfactual. 7. _Only now_ reveal the Spec Connection section. The practitioners should have already been gesturing toward the concepts — the reveal confirms the connection rather than introducing it.

**Debrief phase:** 8. Ask the practitioners: what atom would you apply if you saw this pattern again? They should name the atom the case serves. 9. Ask: what nearby atoms might also apply? This surfaces composition — cases often serve multiple atoms.

**Time box:** Typically 45-60 minutes per case for discussion. A dense case with multiple atoms can fill 90 minutes.

## What counts as complete

A case is complete when the practitioners have:

- Articulated the trigger in their own words (not recited from the case).
- Named the move correctly.
- Connected the move to at least one spec concept without being prompted.
- Answered the counterfactual discussion question.

A case is not complete when the practitioners have listened to the facilitator walk through the case. Passive listening produces familiarity, not mastery.

## Common failure modes

1. **Summarizing the journal entry instead of reading it.** The case format depends on the practitioners having the original narrative in their head. A summary loses the texture that makes the move visible. Fix: make the full journal entry required pre-reading and verify it at the top of the session.

2. **Revealing the Spec Connection too early.** If the practitioners are told upfront which CO section the case illustrates, they stop doing the pattern-matching work and start looking for confirming evidence. The reveal must come after the discussion, not before.

3. **Treating discussion questions as rhetorical.** The questions are the case's assessment surface. Skipping them reduces the case to a story. Fix: require a specific answer from a specific practitioner for each question, not an open offer.

4. **Missing the counterfactual.** Every case has at least one counterfactual question ("what if the practitioner had not noticed the pattern?"). This is where the stakes of the move become visible. Facilitators tempted to skip counterfactuals for time are skipping the most valuable part.

5. **Running a case where the journal entry does not exist.** Every case cites a `source_path`. If the file is missing or has been moved, the case is broken. Fix: verify source paths before each session.

## Assessment compatibility

Cases work for assessment when the scenario is novel — either a different journal entry that illustrates the same atom, or a hypothetical situation the practitioner must analyze using the atom's framework. Straight re-use of the exemplar case for assessment rewards memory, not skill.

The `recommended_use` field in each case frontmatter indicates whether the case is primarily for teaching, discussion, or assessment. The 25 current cases split roughly 11 / 8 / 6.

## Atoms using this modality

See `catalog/README.md` Index 4 (Practice Modality) and `cases/README.md` for the full list. ~22 atoms use the case modality, with 25 fully-expanded exemplar cases available in `cases/exemplars/`.
