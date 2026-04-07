---
atom_id: SC-B-015
name: Reinterpret MUST rules from "human performs" to "human approves" and amend the rule text
craft_layer: brokerage
destination: co-codegen
craft_areas: [intervene-when]
applications: [COC, COR, COE]
spec_lineage:
  - CARE §4 — Human-on-the-Loop (Framework Model)
  - CO §28 — Approval Gate
  - CO §4 — Human-on-the-Loop
journal_evidence:
  - path: loom/journal/0021-DECISION-autonomous-gate1-classification.md
    polarity: POSITIVE
    quote: 'artifact-flow.md rule 4 ("Human Classifies Every Inbound Change") is now interpreted as "Human approves classification" not "Human performs classification".'
practice_modality: brokerage-rep
status: verified
---

## The move

When a MUST rule's literal text says "human performs X" but an agent team can produce trustworthy parallel proposals for X, reinterpret the rule to "human approves X" — and then amend the rule text so that the reinterpretation is durable and visible to future sessions. The reinterpretation preserves the human authority the rule intended (the human still gates the outcome) while removing the human as a throughput bottleneck on the execution. Do not silently ignore the rule. Do not leave the old text in place with an undocumented exception.

## When it fires

When a MUST rule in the artifact-flow, spec, or process model requires a human to perform a task that an agent team can execute more reliably and faster — and the human's unique contribution is judgment on the result, not the execution itself. The trigger is "this rule is blocking autonomous throughput, and the human's value is in approval, not in performance." The counter-trigger is "this rule requires human judgment that agents cannot replicate (e.g., aesthetic decisions, political trade-offs, stakeholder empathy)" — in that case, do not reinterpret.

## What it serves

CARE §4 (Human-on-the-Loop) defines the position: humans define the operating envelope, observe execution patterns, and refine boundaries based on evidence. A rule that says "human performs classification" puts the human in-the-loop (bottleneck), not on-the-loop (observer + approver). Reinterpreting to "human approves" restores the on-the-loop position. CO §28 (Approval Gate) requires human judgment at structural transitions; the reinterpretation preserves this by keeping the human at the gate while removing them from the execution path. CO §4 (Human-on-the-Loop as CO principle) reinforces that the optimal position is neither in-the-loop nor out-of-the-loop.

## Evidence walkthrough

- **0021-DECISION-autonomous-gate1-classification** (POSITIVE) — The original artifact-flow.md rule 4 said "Human Classifies Every Inbound Change." Five sync-reviewer agents processed 79 files in ~2 minutes with unanimous agreement (0 global, 64 variant:rs, 15 skip). The human approved the batch result rather than classifying each file individually. The entry records the explicit reinterpretation: "Human approves classification" not "Human performs classification." The reinterpretation was then written back into the rule text so future sessions would not re-encounter the same friction. This is the move in its complete form: identify the bottleneck, verify agent reliability, reinterpret, and amend the rule.

## How to drill it

Present the practitioner with a rule file containing three MUST rules: (1) "Human MUST review every test result before merge" — agent teams can run tests and summarise, human value is in judging pass/fail on edge cases. (2) "Human MUST write the changelog entry" — agent teams can draft, human value is in tone and audience judgment. (3) "Human MUST decide the deployment window" — requires business context, stakeholder negotiation, calendar awareness that agents cannot replicate. Ask the practitioner to classify each as reinterpretable or not, and for the reinterpretable ones, write the amended rule text. Score: does the practitioner correctly identify rule 3 as non-reinterpretable (human judgment is the execution, not just the approval)? Does the amended text for rules 1 and 2 preserve the approval gate while removing the execution bottleneck?

## Common failure mode

Practitioner reinterprets the rule but does not amend the rule text. The next session reads the original text, encounters the same bottleneck, and either re-derives the reinterpretation (wasting a turn) or follows the literal text (re-creating the bottleneck). The amendment is what makes the reinterpretation durable — without it, the institutional knowledge has not actually changed.

## Related atoms

- SC-B-002 (authority-chain escalation) — if the reinterpretation is contested, escalation determines who has authority to amend.
- SC-B-003 (variant classification) — the specific Gate 1 task that was the subject of the canonical reinterpretation.
- SC-B-014 (explicit supersession journaling) — a rule reinterpretation is a decision reversal; the amended rule text must journal the supersession explicitly.
- SC-B-017 (verification-gradient zone assignment) — the Gate 1 delegation in 0021 illustrates both moves: SC-B-015 teaches the rule-amendment side, SC-B-017 teaches the zone-assignment side of the same event.
- SC-B-009 (cross-SDK upstream brokerage) — rules that block upstream-first approaches are prime candidates for reinterpretation from "human performs" to "human approves."
