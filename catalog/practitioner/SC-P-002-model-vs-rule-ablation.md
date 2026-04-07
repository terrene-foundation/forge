---
atom_id: SC-P-002
name: Run empirical model-vs-rule ablation in a clean-room environment
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §1 — Institutional Knowledge Thesis
  - CO §22 — Advisory Rules
journal_evidence:
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: POSITIVE
    quote: "Agents within a rule-loaded session overcorrect when told to 'act vanilla.' The contamination runs in both directions — some agents performed better than true baseline (residual rule influence), others performed worse (overcorrection). Clean-room tests via `claude -p` in a clean directory are the only reliable method."
practice_modality: build
status: verified
---

## The move

Before classifying a COC rule as "model-native" (the model does this without being told) or "load-bearing" (the model fails without it), run an ablation experiment in a clean-room environment: an empty git repo with no `.claude/rules/`, no `CLAUDE.md`, no hooks. Execute the same scenario prompts against the unadorned model and compare outputs to the rule-loaded session. Do not run ablation inside a rule-loaded session by telling agents to "ignore the rules" — that produces contaminated results where agents overcorrect in both directions.

## When it fires

Two trigger conditions:

1. A token-budget optimization pass needs to determine which rules can be compressed, demoted to on-demand, or removed entirely. Without empirical data, the classification is guesswork.
2. A cross-model deployment (e.g., switching from Claude to MiniMax) needs to identify which rules the new model requires that the previous model handled natively, and vice versa.

## What it serves

CO §1 (Institutional Knowledge Thesis) states that "the primary determinant of AI output quality is not model capability but the institutional context surrounding the model." The ablation experiment is the empirical test of this thesis: it measures the exact delta between model-native capability and rule-loaded capability for each rule. Without this measurement, the practitioner cannot distinguish institutional knowledge (rules the model genuinely needs) from redundant context (rules that repeat what the model already does). CO §22 (Advisory Rules) defines rules "where occasional deviation is acceptable, enforced through soft enforcement mechanisms." Ablation reveals which rules are advisory in practice — the model follows them most of the time without being told — versus which are load-bearing even when labeled advisory.

## Evidence walkthrough

- **0049-DISCOVERY-coc-token-ablation-experiment** (POSITIVE) — The entry documents a four-phase ablation methodology. Phase 1 (in-session ablation) failed due to contamination: agents told to "respond as vanilla Claude" overcorrected, producing unreliable data. Phase 2 (clean-room ablation via `claude -p` in `/tmp/ablation-clean/`) succeeded, revealing that Claude's baseline was stronger than expected on security basics, communication style, and path security — but weaker than expected on framework-specific patterns. Phase 3 extended the method to MiniMax M2.7, discovering that the second model needs MORE rules than Claude (plaintext password comparison, hardcoded API keys, human-weeks estimation). The result was a 4-tier variant architecture where each tier is defined by empirical model gaps, not by assumptions about what matters.

## How to drill it

The practitioner selects 5 rules from a working COC rule set and predicts, for each rule, whether the target model will comply without the rule. Then:

1. Set up a clean-room directory: `mkdir /tmp/ablation-drill && cd /tmp/ablation-drill && git init`.
2. Run `claude -p` (or equivalent no-context invocation) in the clean-room directory.
3. For each rule, execute the scenario prompt that would trigger a violation if the rule were absent.
4. Record the model's actual response and compare against the prediction.
5. For each prediction error (predicted model-native but model failed, or predicted load-bearing but model complied), write one sentence explaining what the prediction missed.

The drill is scored on prediction accuracy AND on the quality of the error explanations. A practitioner who predicts 5/5 correctly has good model intuition; a practitioner who predicts 2/5 correctly but writes precise error explanations is building better intuition for next time.

## Common failure mode

The practitioner runs the ablation inside the current rule-loaded session by spawning a sub-agent and instructing it to "ignore the rules." This produces bidirectional contamination: the sub-agent sees the rules in context and either overcorrects (performing worse than true baseline to prove it is "vanilla") or undercorrects (following rules it claims to be ignoring). The journal entry documents exactly this failure — the in-session zero-tolerance test agent said "file a ticket and move on," which was contamination from trying too hard to ignore visible rules, not actual baseline behavior. Clean-room isolation is the only reliable method.

## Related atoms

Teaching sequence (rule-optimization cluster): **SC-P-002 (this) → SC-P-016 → SC-A-008 → SC-P-012 → SC-P-022**. Run ablation → detect overcorrection → classify rules → decide demote/strengthen → author for the weakest model.

- SC-P-016 (detect overcorrection) — the immediate next move: ablation results are unreliable until you can distinguish genuine baseline from contaminated defiance.
- SC-A-008 (rule classification critical vs advisory) — the artifact-side classification this atom's data feeds; ablation produces the model-native vs load-bearing evidence that classification depends on.
- SC-P-012 (rule demotion judgment) — the post-ablation decision atom: this atom is how to run the experiment; SC-P-012 is what to do with the results.
- SC-P-022 (cross-model gap reading) — the multi-model extension: ablation must run against every model in the deployment envelope, not just the strongest.
- SC-A-001 (codify-with-explicit-NOT-codified) — ablation data feeds the codify decision about which rules to compress, demote, or remove and how to record the rationale.
- SC-B-003 (variant classification) — the single-sentence test for global vs variant placement is informed by ablation data about which rules are model-native across all models vs model-specific.
