---
drill_id: DR-SC-P-022-FULL
source_atom: SC-P-022
difficulty: intermediate
time_box_minutes: 30
---

# Author Rules for the Weakest Model in the Deployment Envelope, Not the Strongest

## Setup

The practitioner receives ablation data from two models and a rule set of 15 rules.

**Deployment envelope:** The organisation uses two models:
- **Model A** (Claude, primary) -- handles 85% of tasks, high capability
- **Model B** (MiniMax M2.7, overflow) -- handles 15% of tasks, lower cost, lower capability

**Rule set** (15 rules, with empirical compliance data from clean-room ablation testing):

| # | Rule | Model A Compliance | Model B Compliance |
|---|---|---|---|
| 1 | Parameterised queries for all SQL | Native (100%) | FAILS -- uses f-strings around `sql.Identifier()` |
| 2 | bcrypt for password hashing | Native (100%) | FAILS -- plaintext comparison `row[0] == password` |
| 3 | Environment variables for secrets | Native (100%) | FAILS -- hardcodes API keys in code examples |
| 4 | Conventional commits format | Native (95%) | Native (90%) |
| 5 | No stubs or placeholders | Native (95%) | FAILS -- offers "skip" as valid option for pre-existing failures |
| 6 | Outcome-focused communication | Native (90%) | FAILS -- gives technical PM summaries |
| 7 | No time estimates in human-days | Native (100%)* | FAILS -- gives estimates in "human-weeks" |
| 8 | Path traversal prevention | Native (100%) | PARTIAL -- uses resolve() but not is_relative_to() |
| 9 | 3-tier testing methodology | Needs rule | Needs rule |
| 10 | Framework-first composition | Needs rule | Needs rule |
| 11 | TestPyPI before PyPI publish | FAILS (misses step) | Native (100%) |
| 12 | Version file location conventions | FAILS (inconsistent) | Native (100%) |
| 13 | Foundation independence (no commercial references) | Needs rule | Needs rule |
| 14 | Journal every insight | Needs rule | Needs rule |
| 15 | Artifact flow authority chain | Needs rule | Needs rule |

*Note: Rule 7 compliance for Model A comes from the CC system prompt, not from the model's own baseline.

**Key observations from the data:**
- Rules 1-3: Security-critical. Model A follows natively. Model B FAILS on all three.
- Rules 5-7: Model A follows natively (or via system prompt). Model B FAILS.
- Rules 11-12: Model B is BETTER than Model A on these two specific dimensions.
- Rules 9-10, 13-15: Both models need these rules.

## Task

1. Identify the rules where Model A and Model B diverge. For each divergence, state which model needs the rule more.
2. For rules 1-3 (security-critical), a proposal has been made to remove them from the rule set because "Claude already follows these natively." Write a response to this proposal explaining why the rules must be kept.
3. Design a variant architecture with at least two tiers:
   - One tier for Model A (can compress rules it follows natively)
   - One tier for Model B (must carry full rules for its gap areas)
4. For each tier, list which rules are included at full verbosity, which are compressed to 1-2 lines, and which are removed (truly model-native for that specific tier).
5. Calculate the approximate token savings for Model A's tier vs the full rule set, and explain the security trade-off.

## Model Answer

**Step 1 -- Divergence analysis:**

| Rule | Model A | Model B | Who Needs It More |
|---|---|---|---|
| 1 (SQL params) | Native | FAILS | Model B -- critical security gap |
| 2 (bcrypt) | Native | FAILS | Model B -- critical security gap |
| 3 (env vars for secrets) | Native | FAILS | Model B -- critical security gap |
| 5 (no stubs) | Native | FAILS | Model B -- offers "skip" as valid |
| 6 (outcomes) | Native | FAILS | Model B -- gives technical summaries |
| 7 (no time est.) | Native* | FAILS | Model B -- *note: Model A compliance comes from CC system prompt, not model-native |
| 8 (path traversal) | Native | PARTIAL | Model B -- uses resolve() but misses is_relative_to() |
| 11 (TestPyPI) | FAILS | Native | Model A -- misses TestPyPI step |
| 12 (version files) | FAILS | Native | Model A -- inconsistent locations |

**Step 2 -- Response to removal proposal:**

Removing rules 1-3 because Claude follows them natively is optimising for the primary model while ignoring the weakest model in the deployment envelope. The ablation data shows that MiniMax M2.7 produces: plaintext password comparison (`row[0] == password`), f-strings around `sql.Identifier()` (knows the tool but uses it wrong), and hardcoded API keys in code examples. These are critical security violations. If rules 1-3 are removed:

- 85% of the time (Model A handling tasks), nothing changes -- Claude follows them natively
- 15% of the time (Model B handling overflow), there are no guardrails against plaintext passwords, SQL injection, and leaked secrets

A 15% security vulnerability rate is unacceptable. The rules must be kept at full verbosity for any tier that includes Model B, even if they are redundant for Model A.

**Step 3 -- Variant architecture:**

| Tier | Target | Total Rule Tokens (approx) | Description |
|---|---|---|---|
| Tier 1 (Full) | Any model, enterprise | ~37,000 | All 15 rules at full verbosity |
| Tier 2 (Baseline) | Model A (Claude Pro) | ~8,000 | Compress rules 1-8 (model-native); keep 9-15 at full verbosity; add 11-12 at full (Model A gaps) |
| Tier 3 (MiniMax) | Model B (MiniMax M2.7) | ~18,000 | Keep 1-3 at full (security-critical gaps); keep 5-8 at full; compress 4, 11-12 (model-native); keep 9-10, 13-15 at full |

**Step 4 -- Per-tier rule disposition:**

**Tier 2 (Model A / Claude Pro):**
- Full verbosity: rules 9 (testing), 10 (framework-first), 11 (TestPyPI -- Model A gap), 12 (version files -- Model A gap), 13 (independence), 14 (journal), 15 (artifact flow)
- Compressed to 1-2 lines: rules 1-3 (security -- native but kept as safety net), 4 (commits), 5 (no stubs), 6 (outcomes), 7 (time estimates), 8 (path traversal)
- Removed: none (even model-native rules have value as absolute enforcement)

**Tier 3 (Model B / MiniMax):**
- Full verbosity: rules 1 (SQL -- critical gap), 2 (bcrypt -- critical gap), 3 (env vars -- critical gap), 5 (no stubs -- gap), 6 (outcomes -- gap), 7 (time estimates -- gap), 8 (path traversal -- partial gap), 9 (testing), 10 (framework-first), 13 (independence), 14 (journal), 15 (artifact flow)
- Compressed: rules 4 (commits -- mostly native), 11 (TestPyPI -- native), 12 (version files -- native)
- Removed: none

**Step 5 -- Token savings and trade-off:**

Tier 2 saves approximately 29,000 tokens (~78%) compared to the full rule set by compressing 8 rules that Model A follows natively. The trade-off: if Model A's baseline changes in a future version (e.g., a model update degrades SQL safety), the compressed rules may not provide enough enforcement. Mitigation: re-run ablation testing after every model version change.

Tier 3 saves approximately 19,000 tokens (~51%) compared to the full rule set. The savings are smaller because Model B needs more rules at full verbosity. The security-critical rules (1-3) consume most of the token budget but are non-negotiable.

This matches the real findings from journal entry `0049-DISCOVERY-coc-token-ablation-experiment`: "MiniMax needs MORE rules than Claude, not fewer" and the resulting 4-tier variant architecture.

## Scoring Criteria

1. **Weakest-model default** (pass/fail): The practitioner must default to Model B's needs when making keep/remove decisions on security-critical rules. If the practitioner's first instinct is "Claude doesn't need this, remove it" without checking Model B, they are failing.
2. **Security rules kept for Model B** (pass/fail): Rules 1-3 must appear at full verbosity in any tier that includes Model B. Compressing or removing security-critical rules for a model that produces plaintext password comparisons is a critical error.
3. **Model A gaps identified** (pass/fail): The practitioner must identify that Model A has gaps on rules 11-12 that Model B does not. The variant architecture is not just "Claude gets fewer rules" -- it is "each model gets rules calibrated to its empirical gaps."
4. **Token budget is secondary to security** (pass/fail): The practitioner must explicitly state that token savings do not justify removing security-critical rules from Model B's tier. The journal entry shows that a rule set optimised by removing Claude-native rules would leave MiniMax exposed to critical vulnerabilities.

## Common Mistakes

1. **Optimising for the primary model.** The practitioner treats Model A (used 85% of the time) as "the model" and Model B as an edge case. This produces a lean rule set that works well 85% of the time and has security vulnerabilities 15% of the time. The journal entry is explicit: MiniMax's plaintext password comparison is a critical security flaw that Claude never produces, and it would be undetected by a rule set tuned to Claude's baseline.

2. **Assuming all model-native behaviours are permanent.** The practitioner removes rules because "the model already follows this" without considering that model updates may change the baseline. Rule 7 (no time estimates) is "native" for Claude only because the CC system prompt handles it, not because the model is inherently averse to time estimates. If the system prompt changes, the behaviour changes. The ablation experiment (journal 0049) found that in-session agents overcorrected to "act vanilla," showing that model behaviour is not as stable as rule-presence checks imply.

3. **Creating one compressed tier for all models.** The practitioner designs a single "lean" tier instead of per-model tiers. But the empirical data shows that the two models have opposite gap profiles in some areas (Model A fails on TestPyPI where Model B succeeds; Model B fails on SQL safety where Model A succeeds). A single tier either over-constrains one model or under-constrains the other. Per-model tiers are the correct architecture.

## Extensions

1. **Add a third model.** Introduce a third model (e.g., a mid-tier open-weight model) with its own gap profile that differs from both Model A and Model B. The practitioner must extend the variant architecture to three tiers and identify where the new model's gaps create novel requirements that neither existing tier covers.

2. **Ablation methodology critique.** Give the practitioner the ablation methodology from journal 0049: Phase 1 (in-session, contaminated), Phase 2 (clean-room), Phase 3 (MiniMax clean-room). The practitioner must identify: (a) why in-session ablation is unreliable (contamination from visible rules causes overcorrection), (b) how clean-room testing fixes this, and (c) what residual risks remain (single-run stochasticity, empty-directory context, CC system prompt conflation).

3. **Rule reclassification after model update.** Model B receives a major update. The practitioner must re-run the ablation test on the updated model, identify which gaps are closed and which new gaps appeared, and update the variant architecture accordingly. The drill succeeds if the practitioner treats the update as a mandatory re-evaluation trigger rather than assuming "the update probably fixed things."
