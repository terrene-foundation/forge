---
atom_id: SC-P-015
variant_of: SC-P-015
name: End complex prompts with "what would invalidate this?" rather than "is this right?"
craft_layer: practitioner
destination: co-codegen
craft_areas: [prompt]
applications: [COC, COR, COE]
spec_lineage:
  - CO §28 — Approval Gate
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: POSITIVE
    quote: "The Nexus transport refactor deep analysis estimates 7 sessions internally but the dependency analysis compresses this to 2-3 sessions in WS-1C. Given that the deep analysis was produced by the same analytical process and examined every coupling point in the 2,436-line `core.py`, what justification exists for the compressed estimate -- and if none, should the dependency analysis document be considered unreliable on other estimates as well?"
  - path: loom/journal/0046-DECISION-downstream-proposal-guard.md
    polarity: POSITIVE
    quote: 'The detection heuristic checks git remote for "kailash-py" or "kailash-rs" — if a downstream project happens to have "kailash-py" in its remote URL (e.g., a fork), would this produce a false positive?'
  - path: loom/journal/0036-RISK-kailash-rl-redteam-round2.md
    polarity: POSITIVE
    quote: "If the 1-2 session estimate had been applied to a `kailash-align` package (without the serving integration and on-prem workflow), what critical capability gaps would have shipped as missing-on-day-one?"
practice_modality: drill
status: verified
observation_search: "no corroborating high-signal observation event found as of 2026-04-08; observation telemetry captures code-execution patterns (workflow_pattern, error_occurrence, error_fix, connection_pattern), not prompt-level practitioner moves. Structural corroboration is in journal rules (rules/journal.md mandates a 'For Discussion' section with at least one counterfactual question per entry — the falsification-prompting move is institutionally encoded in the journal format itself)"
---

## The move

When reviewing or soliciting agent output on a complex analysis, end the prompt with a falsification question — "what would invalidate this?", "what evidence would prove this wrong?", "name the weakest assumption" — rather than a confirmation question like "is this right?" or "does this look good?". Falsification questions force the model to adversarially test its own output instead of pattern-completing toward agreement. The practitioner makes this the default prompt suffix on any output that will feed an approval gate.

## When it fires

The practitioner has received a complex deliverable (plan, analysis, estimate, architecture proposal) from an agent session and needs to decide whether to approve it at a gate. The trigger is the moment before sending the approval-or-rejection response. Instead of "looks good, proceed" or "is this correct?", the practitioner appends the falsification question.

## What it serves

CO §28 (Approval Gate) defines "a structured point where human judgment is exercised before the workflow proceeds to the next phase." A confirmation question ("is this right?") biases the gate toward rubber-stamping because the model's default completion for "is X right?" skews toward "yes, because..." A falsification question restructures the task: the model must now find the strongest counterargument to its own output, surfacing the weaknesses that a confirmation question would bury. The approval gate's value depends on the quality of adversarial scrutiny, which depends on the prompt that invokes it.

## Evidence walkthrough

- **0034-RISK-stack-gaps-redteam-attack** (POSITIVE) — The "For Discussion" section at the end of this red team report demonstrates the move in action. Each of the three discussion questions is a falsification prompt: "what justification exists for the compressed estimate — and if none, should the dependency analysis document be considered unreliable on other estimates as well?" This question structure forced the red team to identify the internal estimate contradiction (7 sessions vs 2-3 sessions) that a confirmation question would have passed through.
- **0046-DECISION-downstream-proposal-guard** (POSITIVE) — A different domain (artifact-flow governance, not estimation) but the same prompt structure. The decision entry resolves a real bug — `/codify` Step 7 was creating upstream proposals from downstream repos, violating the authority chain — and the For Discussion section ends with three falsification questions, including "if a downstream project happens to have 'kailash-py' in its remote URL (e.g., a fork), would this produce a false positive?" and "if the authority chain had been designed with a `.claude/repo-type` marker file from the start, would the runtime detection be unnecessary?". Each is counterfactual or stress-tests the chosen design against an alternative. None ask "is this fix right?" — that question would have surfaced agreement, not the false-positive risk that the first question surfaces. The move generalises across domains: it works on architectural decisions, governance fixes, and any other deliverable that needs adversarial scrutiny at a gate.
- **0036-RISK-kailash-rl-redteam-round2** (POSITIVE) — A third independent instance, this time on a scope-redefinition decision. Question 1 is a counterfactual: "If the 1-2 session estimate had been applied to a `kailash-align` package (without the serving integration and on-prem workflow), what critical capability gaps would have shipped as missing-on-day-one?" The question forces the responder to enumerate concrete capabilities that would have been missing, not to evaluate whether the new scope is "good enough." This is the specificity that the common failure mode warns against losing.
- **Structural backing in journal rules** — The journal rule (rules/journal.md) MANDATES that every entry include a "For Discussion" section with at least one counterfactual question and at least one referencing specific data. The falsification-prompting move is encoded as an institutional requirement, not an optional habit. Every well-formed journal entry in the corpus is therefore a positive instance of the move.

## How to apply

When reviewing any complex agent output at an approval gate, replace confirmation questions ("is this right?", "does this look good?") with specific falsification questions ("which estimate has the weakest evidence base?", "what would change if this assumption were wrong?", "name the single finding most likely to be incorrect and explain why"). Specificity matters — broad falsification questions ("anything wrong?") get treated as confirmation questions and produce token hedging rather than genuine adversarial analysis.

## Common failure mode

The practitioner writes a falsification question that is too broad ("is there anything wrong with this?") which the model treats as equivalent to a confirmation question and answers with a token list of minor issues followed by "overall the analysis is sound." Effective falsification questions are specific: "which estimate in this plan has the weakest evidence base, and what would change if that estimate were 2x higher?" Specificity forces the model to commit to a concrete vulnerability rather than hedging.

## Related atoms

- SC-P-013 (estimate self-contradiction) — the falsification question in 0034's For Discussion section is what surfaced the estimate contradiction that SC-P-013 teaches practitioners to catch
- SC-P-009 (architecture doc as contract) — falsification questions applied to architecture documents surface the same class of hidden-assumption errors that treating docs as testable contracts catches.
- SC-B-006 (multi-round red team) — red team rounds are the structural mechanism; this atom teaches the prompt-level skill that makes each round produce adversarial rather than confirmatory output
- SC-B-008 (convergence-as-validation) — asking "what would invalidate this?" from independent perspectives increases convergence signal; convergent invalidation attempts are high-confidence findings
