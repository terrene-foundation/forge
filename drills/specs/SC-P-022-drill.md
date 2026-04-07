---
drill_id: DR-SC-P-022
source_atom: SC-P-022
name: "Author rules for the weakest model in the deployment envelope, not the strongest — drill"
craft_area: behaviour, attend-how
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a rule set of 15 rules and empirical ablation data for two models: Model A (strong, follows 12 of 15 natively) and Model B (weaker, follows 7 of 15 natively). Three rules that Model A follows natively are violated by Model B, including one security-critical rule (password hashing). The practitioner must: (1) identify the 3 rules where the models diverge, (2) classify each as "keep for Model B" even though "removable for Model A," (3) propose a variant architecture where Model A gets a compressed set and Model B gets the full set, and (4) explain why the security-critical rule must be kept at full verbosity for Model B even if it costs tokens. Score on whether the practitioner defaults to the weakest model's needs rather than the strongest model's surplus.
