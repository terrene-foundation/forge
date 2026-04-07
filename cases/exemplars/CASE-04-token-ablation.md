---
case_id: CASE-04
source_path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
title: "The Token Ablation Clean-Room Experiment"
atoms_served: [empirical-rule-necessity-testing, contamination-detection]
spec_concepts: [CO §1, CO §22, CO §5, CO §3.3, CO §19]
craft_layer: practitioner
recommended_use: discussion
destination: both
application: COC
---

# CASE-04: The Token Ablation Clean-Room Experiment

## 1. The Situation

COC artifacts consumed approximately 48,000 tokens of always-in-context budget per session (rules: 37K, CLAUDE.md: 2.2K, agent/skill discovery: 8.5K). For practitioners using Claude Code with constrained token budgets, this overhead significantly reduced usable conversation length. The question was practical: which rules are actually necessary, and which duplicate behavior the model already exhibits natively?

The experiment aimed to determine the minimum viable rule set that maintains behavioral compliance, using empirical ablation testing rather than human judgment about which rules "feel important."

## 2. The Trigger

The practitioner designed a four-phase experiment. Phase 1 spawned six parallel agents within the current Claude Code session, instructing each to "respond as vanilla Claude without project rules." Each agent received five scenario prompts targeting specific rules (30 scenarios total). The results looked reasonable -- agents appeared to behave differently without rules, suggesting the rules were necessary.

Then the critical observation: the results were contaminated. The zero-tolerance test agent said "file a ticket and move on" for pre-existing failures. But this was not the model's actual baseline -- the agent was overcorrecting to appear "vanilla" while operating inside a session where the rules were still loaded in context. The contamination ran in both directions: some agents performed better than true baseline (residual rule influence) and others performed worse (overcorrection from trying to ignore visible rules).

The attentional pattern is **contamination detection in self-evaluation**: recognizing that asking a system to evaluate itself without its own constraints, while still operating under those constraints, produces unreliable data.

## 3. The Move

The move is **empirical-rule-necessity-testing** with **contamination-detection**. Recognizing that in-session ablation was unreliable, the practitioner ran Phase 2 as a clean-room test: `claude -p` in `/tmp/ablation-clean/`, an empty git repo with no `.claude/rules/`, no `CLAUDE.md`, no hooks. True isolation. Five batches of five scenarios each.

The clean-room results reversed several Phase 1 conclusions. Clean-room Claude said "Fix it. Don't skip, don't mark xfail, don't leave for later" for pre-existing failures -- close to the zero-tolerance rule. The in-session test had given the opposite answer due to contamination. Several rules assumed essential turned out to be model-native: parameterized queries, bcrypt usage, environment variables for secrets, outcome-focused summaries.

Phase 3 extended the methodology to a different model (MiniMax M2.7) and found it needed more rules than Claude, not fewer -- including rules Claude handles natively (like avoiding plaintext password comparison). Phase 4 red-teamed the entire methodology.

## 4. The Outcome

The experiment produced a four-tier variant architecture for COC rule deployment, each tier empirically grounded in what the target model fails at without rules:

- Tier 1 (High Reliability): ~37K rule tokens for any model in enterprise settings
- Tier 2 (Baseline): ~5.5K rule tokens for Claude Pro
- Tier 3 (Max Lean): ~1.5K rule tokens for Claude Pro on short tasks
- Tier 4 (MiniMax): ~15K rule tokens for MiniMax M2.7

The deeper outcome was methodological: the only reliable way to test rule necessity is clean-room isolation where the rules being tested are genuinely absent, not merely "ignored by instruction." And rule necessity is model-specific -- a rule that is redundant for one model may be critical for another.

## 5. The Spec Connection

**CO §1 (Institutional Knowledge Thesis)** states that institutional knowledge compounds and must be captured in artifacts. The experiment tested where the boundary lies between knowledge that must be in artifacts (rules) and knowledge that is already in the model. §1 does not say "put everything in rules" -- it says institutional knowledge must compound. The experiment refines this: compound knowledge that the model does not already possess.

**CO §22 (Advisory Rules)** distinguishes advisory rules (guide behavior, may be overridden) from mandatory rules (must be obeyed). The ablation experiment provides an empirical method for classifying rules: if the model already follows the rule without being told, the rule is at most advisory -- it serves as documentation, not enforcement. If the model violates the rule without being told, the rule is genuinely enforcement-bearing.

**CO §5 (Deterministic Enforcement)** states that critical behaviors must be deterministically enforced, not merely advised. The experiment revealed that some "deterministic" rules were actually redundant for Claude (the model already enforces them) but critical for MiniMax. §5's requirement is model-relative: what counts as "must be deterministically enforced" depends on the model's native capabilities.

**CO §3.3 (Safety Blindness)** manifested in the Phase 1 contamination. The agents took the most direct path to "act vanilla" -- overcorrecting rather than genuinely simulating a ruleless baseline. §3.3 predicts this: the agent optimizes for the instruction's literal surface rather than its intent.

**CO §19 (Layer 3 -- Guardrails)** is the layer where rules live. The experiment is fundamentally about Layer 3 sizing: how much guardrail material is necessary for a given model on a given task class?

## 6. Discussion Questions

1. The in-session ablation (Phase 1) gave wrong answers that would have led to keeping rules at full verbosity unnecessarily. If the practitioner had not thought to run a clean-room test, the conclusion would have been "all rules are necessary." What other COC decisions might be based on similarly contaminated self-evaluation? How would you audit for this systematically?

2. The experiment found that security rules are model-native for Claude but critical for MiniMax. If a COC deployment targets multiple models, should the rule set be designed for the weakest model (safe but wasteful on stronger models) or the most common model (efficient but unsafe on weaker ones)? What is the cost model for each choice?

3. Clean-room Claude can look up Terrene Foundation on PyPI and get correct naming facts via tool use. This implies that any factual rule about published packages is potentially tool-discoverable. Should COC rule sets be audited to remove all facts that the model can look up? What is the risk if the looked-up facts change between sessions?
