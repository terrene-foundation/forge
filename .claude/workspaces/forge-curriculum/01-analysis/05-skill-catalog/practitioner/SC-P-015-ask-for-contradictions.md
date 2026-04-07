---
atom_id: SC-P-015
name: End complex prompts with "what would invalidate this?" rather than "is this right?"
craft_layer: practitioner
destination: both
craft_areas: [prompt]
applications: [COC, COR, COE]
spec_lineage:
  - CO §28 — Approval Gate
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: POSITIVE
    quote: "The Nexus transport refactor deep analysis estimates 7 sessions internally but the dependency analysis compresses this to 2-3 sessions in WS-1C. Given that the deep analysis was produced by the same analytical process and examined every coupling point in the 2,436-line core.py, what justification exists for the compressed estimate — and if none, should the dependency analysis document be considered unreliable on other estimates as well?"
practice_modality: drill
status: draft
---

## The move

When reviewing or soliciting agent output on a complex analysis, end the prompt with a falsification question — "what would invalidate this?", "what evidence would prove this wrong?", "name the weakest assumption" — rather than a confirmation question like "is this right?" or "does this look good?". Falsification questions force the model to adversarially test its own output instead of pattern-completing toward agreement. The practitioner makes this the default prompt suffix on any output that will feed an approval gate.

## When it fires

The practitioner has received a complex deliverable (plan, analysis, estimate, architecture proposal) from an agent session and needs to decide whether to approve it at a gate. The trigger is the moment before sending the approval-or-rejection response. Instead of "looks good, proceed" or "is this correct?", the practitioner appends the falsification question.

## What it serves

CO §28 (Approval Gate) defines "a structured point where human judgment is exercised before the workflow proceeds to the next phase." A confirmation question ("is this right?") biases the gate toward rubber-stamping because the model's default completion for "is X right?" skews toward "yes, because..." A falsification question restructures the task: the model must now find the strongest counterargument to its own output, surfacing the weaknesses that a confirmation question would bury. The approval gate's value depends on the quality of adversarial scrutiny, which depends on the prompt that invokes it.

## Evidence walkthrough

- **0034-RISK-stack-gaps-redteam-attack** (POSITIVE) — The "For Discussion" section at the end of this red team report demonstrates the move in action. Each of the three discussion questions is a falsification prompt: "what justification exists for the compressed estimate — and if none, should the dependency analysis document be considered unreliable on other estimates as well?" This question structure forced the red team to identify the internal estimate contradiction (7 sessions vs 2-3 sessions) that a confirmation question would have passed through. The pattern recurs across all journal entries with "For Discussion" sections — they consistently use counterfactual and invalidation framings rather than confirmation framings.

## How to drill it

Give the practitioner three agent outputs: a dependency analysis, a risk assessment, and an architecture proposal. For each, they must write two prompts: one confirmation question and one falsification question. Then run both against the same model and compare the responses. Score on: (a) whether the falsification question surfaces a real weakness not mentioned in the confirmation response, (b) whether the falsification question is specific enough to be answerable (not just "what's wrong with this?"), and (c) whether the practitioner can articulate why the falsification framing produced different output. The drill succeeds when the practitioner defaults to falsification prompts without being told to.

## Common failure mode

The practitioner writes a falsification question that is too broad ("is there anything wrong with this?") which the model treats as equivalent to a confirmation question and answers with a token list of minor issues followed by "overall the analysis is sound." Effective falsification questions are specific: "which estimate in this plan has the weakest evidence base, and what would change if that estimate were 2x higher?" Specificity forces the model to commit to a concrete vulnerability rather than hedging.

## Related atoms

- SC-P-013 (estimate self-contradiction) — the falsification question in 0034's For Discussion section is what surfaced the estimate contradiction that SC-P-013 teaches practitioners to catch
- SC-B-006 (multi-round red team) — red team rounds are the structural mechanism; this atom teaches the prompt-level skill that makes each round produce adversarial rather than confirmatory output
