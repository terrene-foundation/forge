---
atom_id: SC-P-012
name: Decide whether to demote or strengthen a rule when ablation shows partial redundancy
craft_layer: practitioner
destination: forge
craft_areas: [intervene-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §1 — Institutional Knowledge Thesis
  - CO §39 — Layer 5 — Learning
  - CO §22 — Advisory Rules
journal_evidence:
  - path: loom/journal/0006-DECISION-learning-must-work.md
    polarity: POSITIVE
    quote: 'The L5 Learning System is NOT aspirational — it is a core CO principle that MUST work. The fix path is mandatory. "Simplify" and "remove" are rejected.'
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: MIXED
    quote: "Several rules we considered essential turned out to be model-native for Claude:\n- **Security basics**: Parameterized queries, bcrypt, env vars — all model-native\n- **Communication style**: PM summaries are naturally outcome-focused"
practice_modality: case
status: verified
---

## The move

When ablation data shows that a rule is partially redundant — the model complies without the rule most of the time — make an explicit judgment call: demote the rule (move to on-demand, compress, or remove) if the model's baseline fully covers the rule's intent, OR strengthen the rule (add hard enforcement or defense-in-depth) if the model covers the common case but misses edge cases where failure is costly. The judgment hinges on the failure cost of the remaining uncovered percentage, not on the coverage percentage itself. A rule that the model follows 95% of the time but whose 5% failure mode is a security breach should be strengthened, not demoted.

## When it fires

Two trigger conditions:

1. An ablation experiment (SC-P-002) has produced per-rule data showing that some rules are model-native. The practitioner must now classify each partially-redundant rule as "demote" or "strengthen." This is the post-ablation decision point.
2. A new model version is deployed and the practitioner observes that previously load-bearing rules now appear to be followed without enforcement. The question is whether to trust the new model's compliance and reduce the rule set, or to maintain the rules as insurance against regression in future model versions.

## What it serves

CO §1 (Institutional Knowledge Thesis) states that "the primary determinant of AI output quality is not model capability but the institutional context surrounding the model." This creates a default posture: institutional knowledge is non-negotiable. CO §39 (Layer 5 — Learning) defines a system where "knowledge compounds across sessions, practitioners, and time periods" — demoting a rule is an act of de-compounding that must be justified. CO §22 (Advisory Rules) provides the classification mechanism: rules "where occasional deviation is acceptable." The judgment call is whether a specific rule, shown to be partially redundant by empirical data, belongs in the advisory category (demote) or remains in the critical category (strengthen) despite the model's apparent compliance.

## Evidence walkthrough

- **0006-DECISION-learning-must-work** (POSITIVE) — This entry establishes the baseline posture: Layer 5 is "NOT aspirational" and the fix path is "mandatory." The rationale is that without working institutional knowledge, "every session starts cold" and "the 10x multiplier cannot compound." This entry is the voice arguing against demotion: if you strip rules because the model seems to handle them, you lose the compounding benefit and the system degrades gracefully until it fails catastrophically on an edge case no one remembers the rule was guarding.

- **0049-DISCOVERY-coc-token-ablation-experiment** (MIXED) — This entry provides the empirical counterweight. Clean-room ablation revealed that Claude's baseline covers security basics, communication style, and path security without rules. But the same experiment showed that the initial assumption ("security.md is model-native and eliminable") was wrong: "Even partially model-native rules add value through absolute enforcement." The entry documents 10 rules demoted to on-demand and 1 rule removed — but also documents the user challenging premature elimination. The MIXED polarity reflects the tension: the data supports demotion for some rules, but the entry's own red-team phase found that zero rules could be eliminated without testing, and the user pushed back on overcorrection.

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

## Common failure mode

The practitioner averages the compliance percentages and demotes everything above a threshold (e.g., "anything above 90% compliance is model-native, demote it"). This ignores the asymmetry between low-cost failures (communication style drift) and high-cost failures (security breaches, scope creep from human-weeks estimates). The compliance percentage tells you how often the model gets it right; the failure cost tells you how much damage the remaining wrong cases do. A 99%-compliant rule with catastrophic failure mode needs strengthening; a 50%-compliant rule with trivial failure mode might not even need to exist.

## Related atoms

Teaching sequence (rule-optimization cluster): **SC-P-002 → SC-P-016 → SC-A-008 → SC-P-012 (this) → SC-P-022**. Run ablation → detect overcorrection → classify rules → decide demote/strengthen → author for the weakest model.

- SC-P-002 (model-vs-rule ablation) — provides the methodology that produces the data this atom acts on. SC-P-002 is how to run the experiment; SC-P-012 is what to do with the results.
- SC-P-016 (detect overcorrection) — guards against acting on contaminated ablation data; demoting a rule based on a contaminated baseline removes a load-bearing constraint.
- SC-A-008 (rule classification critical vs advisory) — the upstream classification that this atom revisits when ablation reveals partial redundancy.
- SC-P-022 (cross-model gap reading) — the cluster's terminus: never demote a rule on the strongest model's compliance alone if a weaker model in the envelope still needs it.
- SC-A-001 (codify-with-explicit-NOT-codified) — when a rule is demoted, the demotion rationale should be codified in the NOT-codified section to prevent future practitioners from re-adding the rule without data.
- SC-A-003 (defense-in-depth codification) — for rules classified as "strengthen," the defense-in-depth pattern is the implementation mechanism.
- SC-A-015 (Layer 2 context structuring) — demotion is a relocation from rule artifact type to skill or command artifact type; this atom feeds the artifact-type decision.
