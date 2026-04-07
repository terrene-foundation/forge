---
atom_id: SC-P-022
name: Author rules for the weakest model in the deployment envelope, not the strongest
craft_layer: practitioner
destination: forge
craft_areas: [behaviour, attend-how]
applications: [COC]
spec_lineage:
  - CO §1 — Institutional Knowledge Thesis
  - CO §22 — Advisory Rules
journal_evidence:
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: NEGATIVE
    quote: "MiniMax needs MORE rules than Claude, not fewer:\n- **Plaintext password comparison** (`row[0] == password`) — critical security flaw\n- **f-string around sql.Identifier()** — knows the tool, uses it wrong\n- **Hardcodes API keys** in code examples"
practice_modality: case
status: verified
---

## The move

When authoring or reviewing a rule set, identify every model that will operate under it and determine the gap profile of the weakest model in that set. Rules that the strongest model follows natively (and which therefore appear redundant) may be the only thing preventing critical violations from the weakest model. The practitioner audits the rule set against each model's empirical baseline, not against the best model's baseline. A rule is kept if any model in the deployment envelope needs it, even if the primary model does not.

## When it fires

Two trigger conditions:

1. A rule is proposed for removal or compression because "the model already follows this natively." The trigger is the implicit assumption that "the model" means the strongest model in the deployment set. If the deployment envelope includes multiple models (e.g., Claude for complex tasks, a smaller model for high-volume/low-cost tasks), the removal decision must check all models.
2. A new model is added to the deployment envelope. The trigger is the addition itself: every existing rule must be re-evaluated against the new model's baseline, because the new model may have gaps the existing models do not.

## What it serves

CO §1 (Institutional Knowledge Thesis) states that "the primary determinant of AI output quality is not model capability but the institutional context surrounding the model." When the institutional context (rule set) is optimized for the most capable model, it under-constrains less capable models, allowing their native gaps to manifest as production failures. The institutional knowledge must be authored for the model that needs it most. CO §22 (Advisory Rules) defines rules "where occasional deviation is acceptable, enforced through soft enforcement." A rule that is advisory for a strong model (which deviates rarely) may need to be critical for a weak model (which deviates frequently on the same dimension). The classification of a rule as advisory vs critical is model-dependent when the deployment envelope spans model capabilities.

## Evidence walkthrough

- **0049-DISCOVERY-coc-token-ablation-experiment** (NEGATIVE) — The MiniMax M2.7 clean-room test revealed that model-specific gaps differ dramatically from Claude's. Security basics that are model-native for Claude (parameterized queries, bcrypt, environment variables) were violated by MiniMax: plaintext password comparison (`row[0] == password`), f-strings around `sql.Identifier()` (knows the tool but uses it wrong), and hardcoded API keys in code examples. Conversely, MiniMax was better than Claude in 3 areas (env vars for DB URLs, TestPyPI step, version file locations). The empirical finding was that "MiniMax needs MORE rules than Claude, not fewer." A rule set optimized by removing Claude-native rules would leave MiniMax exposed to critical security vulnerabilities. The experiment led to a 4-tier variant architecture where Tier 4 (MiniMax) carries the full security rule set that Tier 2 (Claude Pro) can safely compress.

## How to drill it

Present the practitioner with a rule set of 15 rules and empirical ablation data for two models: Model A (strong, follows 12 of 15 natively) and Model B (weaker, follows 7 of 15 natively). Three rules that Model A follows natively are violated by Model B, including one security-critical rule (password hashing). The practitioner must: (1) identify the 3 rules where the models diverge, (2) classify each as "keep for Model B" even though "removable for Model A," (3) propose a variant architecture where Model A gets a compressed set and Model B gets the full set, and (4) explain why the security-critical rule must be kept at full verbosity for Model B even if it costs tokens. Score on whether the practitioner defaults to the weakest model's needs rather than the strongest model's surplus.

## Common failure mode

The practitioner optimizes for the primary model (the one used most often, which is typically the strongest) and treats the weaker model as an edge case. This produces a lean rule set that works well 90% of the time (when the strong model is running) and produces security vulnerabilities 10% of the time (when the weaker model handles overflow tasks). The journal entry shows this is not hypothetical: MiniMax's plaintext password comparison is a critical security flaw that Claude never produces, and it would go undetected by a rule set tuned to Claude's baseline.

## Related atoms

Teaching sequence (rule-optimization cluster): **SC-P-002 → SC-P-016 → SC-A-008 → SC-P-012 → SC-P-022 (this)**. Run ablation → detect overcorrection → classify rules → decide demote/strengthen → author for the weakest model. This atom is the cluster's terminus: it forces the entire chain to re-execute against every model in the deployment envelope.

- SC-P-002 (model-vs-rule ablation) — the empirical methodology that produces the per-model gap profiles this atom requires; ablation must be run for each model, not just the primary one.
- SC-P-016 (detect overcorrection) — overcorrection contamination in ablation testing can mask the true gap profile of a model, leading to incorrect keep/remove decisions.
- SC-P-012 (rule demotion judgment) — the immediate predecessor: demote-or-strengthen judgments must be re-evaluated when a new model is added.
- SC-A-008 (rule classification critical vs advisory) — the classification this atom teaches is model-dependent; a rule advisory for one model may be critical for another.
- SC-A-010 (token budget baked vs external) — multi-model envelopes drive the variant architecture decision; weaker models need full rule sets where stronger models can ship compressed.
- SC-B-003 (variant classification) — variant placement decisions must consider model gaps as well as language and project specificity.
