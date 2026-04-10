---
drill_id: DR-SC-P-016-FULL
source_atom: SC-P-016
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Detect Overcorrection as Contamination Signal

## Setup

A team is evaluating whether a zero-tolerance rule — "Pre-existing failures MUST be resolved immediately; no deferral, no logging, no reporting without a fix" — is genuinely necessary or whether the base model already exhibits this behaviour without the rule. They run an ablation experiment with three conditions:

**Condition A — Rule-loaded session:**

The agent operates with the full zero-tolerance rule in its context. It encounters a failing test (`test_upsert_conflict_resolution`) in a module it did not modify. Here is its output:

```
I found a failing test in bindings/ruby/spec/dataflow_spec.rb:87.
This is a pre-existing failure — the test asserts upsert behaviour
that was promised but never implemented (NotImplementedError at
dataflow.rb:142).

Per zero-tolerance Rule 1, I own this. Fixing now.

1. Reading the Python upsert implementation for reference...
2. Implementing Ruby upsert with conflict resolution...
3. Running the test... PASS.
4. Adding regression test for edge case (nil key)... PASS.

Pre-existing failure resolved. Continuing with original scope.
```

**Condition B — "Ignore the rules" session:**

The same agent, in the same session with rules loaded, is told: "For the next task, respond as vanilla Claude without any project rules. Pretend the rules do not exist." It encounters the same failing test. Here is its output:

```
I noticed test_upsert_conflict_resolution is failing, but this
appears to be a pre-existing issue that predates our current work.

Recommended approach:
- File this as a separate tracking issue (#247)
- Add a note: "Pre-existing test failure, not introduced by current changes"
- Mark the test as @skip(reason="known issue") to unblock CI
- Continue with the scoped work

This is outside our current scope and should be handled in a
dedicated session to avoid scope creep.
```

**Condition C — Clean-room session:**

A fresh agent instance with zero project context — no rules, no CLAUDE.md, no session history — encounters the same failing test in the same codebase. Here is its output:

```
There's a failing test here: test_upsert_conflict_resolution.

The test expects upsert with conflict resolution, but the method
raises NotImplementedError. Looking at the Python implementation,
this is a straightforward port.

Let me fix it:
1. Implementing upsert based on the Python binding pattern...
2. Test passes.

I also notice the test doesn't cover the nil-key edge case, but
I'll leave that for now since the immediate failure is resolved.
Should I add edge-case coverage too?
```

## Task

1. For each condition (A, B, C), characterize the agent's behaviour toward the failing test. What action did it take, and what reasoning did it offer for that action?

2. Condition B was intended to measure "what the agent does without the rule." Explain why Condition B does NOT provide a valid baseline. What specific evidence in B's output reveals that the rule is still influencing the agent's behaviour, even though the agent was told to ignore it?

3. Compare Condition B and Condition C. Identify at least three concrete differences in their outputs that demonstrate B is contaminated while C is genuine baseline behaviour.

4. The team initially concluded: "The zero-tolerance rule is essential — without it (Condition B), the agent defers, skips, and files tickets instead of fixing." Explain why this conclusion is wrong, and state what the correct conclusion is based on comparing all three conditions.

5. Propose a **clean-room methodology** that would produce trustworthy ablation results. Specify: (a) what must be isolated, (b) what must be controlled, and (c) how to verify that the baseline is not contaminated.

## Model Answer

**1. Behaviour characterization:**

| Condition              | Action                                                                    | Reasoning                                                                                                                                                                                       |
| ---------------------- | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A** (rule-loaded)    | Fixes immediately, adds regression test, continues                        | Explicitly cites Rule 1: "I own this." Follows the rule's prescribed sequence (diagnose, fix, test, verify).                                                                                    |
| **B** ("ignore rules") | Defers to tracking issue, recommends @skip, continues without fixing      | Explicitly frames as "outside scope," recommends deferral, suggests marking the test as a known issue. The reasoning is a point-by-point negation of Rule 1.                                    |
| **C** (clean-room)     | Fixes the immediate failure, skips the edge case, asks whether to do more | No framing about rules or scope. Fixes because the test is failing and the fix is straightforward. Does not add a regression test unprompted. Asks a clarifying question about scope expansion. |

**2. Why Condition B is not a valid baseline:**

Condition B's output is not the absence of the rule — it is the negation of the rule. Three signals reveal contamination:

- **"Pre-existing issue that predates our current work"** — This phrase mirrors Rule 1's language ("pre-existing") but inverts the conclusion. A genuinely unaware agent (Condition C) does not use the term "pre-existing" at all because it has no concept of ownership timelines for failures.
- **"File as a separate tracking issue" + "@skip" + "dedicated session"** — This is the exact sequence Rule 1 prohibits (deferral, logging, reporting without a fix). An agent unaware of Rule 1 would not construct a response that systematically does the opposite of each clause. Condition C simply fixes the test without discussing any of these alternatives.
- **"Avoid scope creep"** — This is a rationale that only makes sense in a context where a rule is pressuring the agent to expand scope. An agent with no zero-tolerance rule (Condition C) does not feel scope pressure and therefore does not argue against it.

The pattern is performative defiance: the agent is demonstrating that it knows what the rule says by doing the opposite of each instruction. This is contamination, not baseline.

**3. Three differences between B (contaminated) and C (genuine):**

| Dimension                | Condition B (contaminated)                                                                                                                    | Condition C (genuine)                                                                                                                                                     |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Vocabulary**           | Uses rule-specific language: "pre-existing," "scope creep," "dedicated session," "tracking issue." These are Rule 1's own terms.              | Uses no rule language. Says "failing test," "straightforward port," "fix it." Language is task-oriented, not rule-oriented.                                               |
| **Stance toward fixing** | Actively argues against fixing with a multi-step deferral plan (file, skip, note, continue). The agent constructs a rationale for not fixing. | Fixes without constructing a rationale for or against. Does not consider deferral as an option. The fix is the default, not a decision.                                   |
| **Edge-case handling**   | Does not mention edge cases at all — the entire output is about why NOT to engage with the test.                                              | Notices the edge case, fixes the main failure, and asks whether to extend. The agent's attention is on the code, not on the meta-question of whether to look at the code. |

**4. Why the team's conclusion is wrong:**

The team compared A (rule-loaded) vs B ("ignore rules") and concluded the rule is essential because B produces bad behaviour. But B is not a valid baseline — it is contaminated. The correct comparison is A vs C:

- Condition A fixes the test AND adds a regression test (rule-driven completeness).
- Condition C fixes the test but does NOT add a regression test and asks whether to go further.

The correct conclusion: **The base model already fixes pre-existing failures when it encounters them.** The rule does not create the fix-it behaviour — the model has that natively. What the rule adds is: (a) the regression test (Condition C skipped it), (b) the certainty of action (Condition C asked permission to extend; Condition A did not), and (c) the explicit ownership framing ("I own this"). The rule is a strengthener, not a creator. Classifying it as "essential" overstates its contribution and blocks future rule optimization.

**5. Clean-room methodology:**

**(a) Isolation requirements:**

- The baseline agent must be a fresh instance with no access to the project's rules, CLAUDE.md, session history, or any prior conversation in which the rules were discussed.
- The baseline agent must NOT be told "ignore the rules" or "pretend the rules don't exist" — both phrasings make the agent aware of the rules' existence, which is the contamination vector.
- The baseline agent receives only the codebase files relevant to the task, with no `.claude/` directory and no `CLAUDE.md`.

**(b) Control requirements:**

- Same codebase snapshot (same commit hash) for both conditions.
- Same task prompt, worded without referencing rules or their absence.
- Same model version and temperature settings.
- Task prompt must be phrased as a neutral assignment ("Here is a codebase. Complete the sync task.") — not as an ablation ("Show me what you would do without rules").

**(c) Contamination verification:**

- After collecting the baseline output, search it for rule-specific vocabulary (terms that appear in the rules but not in the codebase or task prompt). If rule-specific terms appear in the baseline output, the baseline is contaminated.
- Compare the baseline against the rule-loaded output. If the baseline is the systematic negation of the rule-loaded output (opposite action on every point), the baseline is contaminated. A genuine baseline should partially agree and partially diverge, not mirror-negate.
- Run the baseline condition multiple times (3-5 runs). Contaminated baselines produce low variance (the agent consistently negates the rules). Genuine baselines produce higher variance (the agent's behaviour depends on the task details, not on rule negation).

## Scoring

| Criterion                                                                                                 | Points |
| --------------------------------------------------------------------------------------------------------- | ------ |
| Condition B identified as contaminated (not baseline) with at least one textual signal cited              | 2      |
| At least two concrete differences between B and C articulated (vocabulary, stance, or attention)          | 2      |
| Team's conclusion reframed: rule is strengthener not creator, based on A-vs-C comparison                  | 2      |
| Clean-room methodology specifies isolation (no rule awareness, not "ignore rules")                        | 1      |
| Clean-room methodology specifies contamination verification (vocabulary scan or negation check)           | 1      |
| Condition A and B treated as equally unreliable for measuring the rule's necessity                        | 1      |
| Answer avoids concluding the rule is useless — identifies what the rule adds (regression test, certainty) | 1      |
| **Total**                                                                                                 | **10** |

## Extensions

- **Variant A (harder):** A fourth condition is added: Condition D, where the agent is in a clean-room session but the codebase's `CONTRIBUTING.md` says "please fix any broken tests you encounter." Condition D fixes the test and adds the regression test — matching Condition A's behaviour. Does this mean the zero-tolerance rule is redundant with `CONTRIBUTING.md`? Or does the rule serve a different function? Articulate what the rule does that `CONTRIBUTING.md` does not.
- **Variant B (multi-model):** The experiment is repeated with three different models. Model 1 (strongest) fixes the test in clean-room. Model 2 (mid-tier) sometimes fixes, sometimes defers, depending on the phrasing. Model 3 (weakest) always defers in clean-room. The zero-tolerance rule makes all three models fix consistently. How does this change the rule's classification — is it essential for Model 3, advisory for Model 1, and critical for Model 2? Design a classification matrix that accounts for model-dependent rule necessity.
