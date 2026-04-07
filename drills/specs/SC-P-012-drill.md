---
drill_id: DR-SC-P-012
source_atom: SC-P-012
name: "Decide whether to demote or strengthen a rule when ablation shows partial redundancy — drill"
craft_area: intervene-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with 5 rules and their ablation data (model compliance rate without the rule, failure-mode description when the rule is absent). For each rule, the practitioner must:

1. State whether they would demote or strengthen the rule.
2. Give a one-sentence rationale citing the failure cost, not the compliance percentage.
3. For demoted rules, state the monitoring mechanism that would catch a regression (e.g., "if the model starts producing human-weeks estimates, the rule re-enters the always-in-context set").
4. For strengthened rules, state which additional enforcement mechanism they would add (e.g., "add a pre-commit hook that rejects plaintext passwords").

Example cases:

- Rule: "No hardcoded API keys." Model compliance: 90%. Failure mode: credential leak. Correct answer: strengthen (add hook), because the 10% failure rate on a credential-leak rule is unacceptable.
- Rule: "Use outcome-focused summaries." Model compliance: 95%. Failure mode: PM gets technical jargon. Correct answer: demote (compress to one line), because the failure mode is low-cost and recoverable.
- Rule: "No time estimates in human-weeks." Model compliance: 98% (CC system prompt handles it). Failure mode: misleading effort framing. Correct answer: depends on whether the CC system prompt is guaranteed to persist across model versions — if yes, demote; if no, keep.

Score on the quality of the rationale (does it reference failure cost?) and on whether the monitoring/enforcement mechanism is concrete, not abstract.
