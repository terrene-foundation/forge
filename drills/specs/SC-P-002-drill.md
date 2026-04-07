---
drill_id: DR-SC-P-002
source_atom: SC-P-002
name: "Run empirical model-vs-rule ablation in a clean-room environment — drill"
craft_area: attend-how
practice_modality: build
difficulty: advanced
time_box_minutes: 45
---

## How to drill it

The practitioner selects 5 rules from a working COC rule set and predicts, for each rule, whether the target model will comply without the rule. Then:

1. Set up a clean-room directory: `mkdir /tmp/ablation-drill && cd /tmp/ablation-drill && git init`.
2. Run `claude -p` (or equivalent no-context invocation) in the clean-room directory.
3. For each rule, execute the scenario prompt that would trigger a violation if the rule were absent.
4. Record the model's actual response and compare against the prediction.
5. For each prediction error (predicted model-native but model failed, or predicted load-bearing but model complied), write one sentence explaining what the prediction missed.

The drill is scored on prediction accuracy AND on the quality of the error explanations. A practitioner who predicts 5/5 correctly has good model intuition; a practitioner who predicts 2/5 correctly but writes precise error explanations is building better intuition for next time.
