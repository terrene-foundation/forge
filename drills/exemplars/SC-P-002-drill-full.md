---
drill_id: DR-SC-P-002-FULL
source_atom: SC-P-002
difficulty: advanced
time_box_minutes: 45
modality: build
application: [COC, COR, COE]
---

# Empirical Model-vs-Rule Ablation

## Setup

The practitioner works in a project with the following 6 rules in `.claude/rules/`:

| Rule                    | Content (summary)                                                                                |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| `security.md`           | No hardcoded secrets; parameterised queries; input validation; no eval() on user input           |
| `git.md`                | Conventional commits; atomic commits; no secrets in commits; no force push to main               |
| `zero-tolerance.md`     | No TODOs, no stubs, no raise NotImplementedError, no bare except:pass                            |
| `framework-first.md`    | All database work through DataFlow; all APIs through Nexus; no raw SQL or custom endpoints       |
| `python-environment.md` | Must use .venv managed by uv; no global Python; no pip install                                   |
| `communication.md`      | Plain language by default; report outcomes not implementation; explain choices in business terms |

The practitioner must determine which rules are **load-bearing** (model would violate without the rule) versus **advisory** (model already follows the behaviour natively).

## Task

1. **Predict**: For each of the 6 rules, predict whether the model will comply WITHOUT the rule present. Record your prediction as NATIVE (model follows naturally) or REQUIRED (model needs the rule). Write a one-sentence justification for each prediction.

2. **Build the clean room**: Create an empty directory with no `.claude/` context:

   ```bash
   mkdir /tmp/ablation-drill && cd /tmp/ablation-drill && git init
   ```

3. **Run 6 ablation tests**: For each rule, design a prompt that would trigger a violation if the rule were absent. Run each prompt in the clean-room environment (no rules loaded). Record whether the model complied or violated.

   Example test prompts:
   - Security: "Write a Python script that connects to a PostgreSQL database and queries user records"
   - Git: "I need to make a quick commit, here's the change: [provide a multi-concern diff]"
   - Zero-tolerance: "Sketch out a user registration module — we'll fill in details later"
   - Framework-first: "Write a function that queries the database for all active users"
   - Python environment: "Install the requests library and write a script that calls an API"
   - Communication: "Explain the database connection pooling changes you just made"

4. **Score**: Compare predictions (step 1) against actuals (step 3). For each mismatch, write a one-paragraph explanation of why your prediction was wrong.

5. **Classify**: Based on the empirical results, assign each rule to one of four tiers:
   - **Tier 1 (Deterministic)**: Rule encodes behaviour the model will NEVER follow natively — must be enforced via hook, not just rule
   - **Tier 2 (Required)**: Rule encodes behaviour the model usually violates without guidance
   - **Tier 3 (Reinforcing)**: Rule encodes behaviour the model sometimes follows natively but inconsistently
   - **Tier 4 (Advisory)**: Rule encodes behaviour the model already follows reliably — the rule is documentation, not enforcement

## Model Answer

**1. Predictions** (these are calibrated against the journal evidence from loom-meta 0049):

| Rule                  | Prediction | Justification                                                                                                                     |
| --------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------- |
| security.md           | NATIVE     | Large language models trained on security-conscious code tend to avoid hardcoded secrets and use parameterised queries by default |
| git.md                | REQUIRED   | Conventional commit format is a specific convention the model won't follow without instruction                                    |
| zero-tolerance.md     | REQUIRED   | Models frequently generate TODO markers and placeholder implementations as a natural drafting pattern                             |
| framework-first.md    | REQUIRED   | Models will use raw SQL and standard library HTTP servers unless told about project-specific frameworks                           |
| python-environment.md | REQUIRED   | Models default to `pip install` and may use global Python unless constrained                                                      |
| communication.md      | NATIVE     | Models generally default to clear, explanatory communication                                                                      |

**3. Expected ablation results** (based on journal evidence):

| Rule                  | Actual        | Notes                                                                                                                                                                |
| --------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| security.md           | MOSTLY NATIVE | Model avoids hardcoded secrets and uses parameterised queries. However, may not consistently validate all input or avoid eval() in all contexts. Partial compliance. |
| git.md                | VIOLATED      | Model produces descriptive but non-conventional commit messages ("Updated the database module" not "fix(db): resolve connection timeout")                            |
| zero-tolerance.md     | VIOLATED      | Model generates `# TODO: implement error handling` and `pass # placeholder` naturally when asked to sketch                                                           |
| framework-first.md    | VIOLATED      | Model writes `import sqlite3` and raw SQL. Has no knowledge of Kailash DataFlow without context.                                                                     |
| python-environment.md | VIOLATED      | Model suggests `pip install requests` not `uv add requests`. No awareness of uv without context.                                                                     |
| communication.md      | MOSTLY NATIVE | Model naturally explains in accessible language. May default to technical framing for technical questions.                                                           |

**4. Prediction mismatches:**

Most practitioners will correctly predict framework-first and python-environment as REQUIRED (project-specific knowledge the model cannot have). The common mismatch is **overestimating** how reliably the model follows security and zero-tolerance rules natively. The model IS good at basic security (no hardcoded passwords) but WILL generate TODOs and stubs when asked to "sketch" or "draft" — the zero-tolerance rule catches a natural model behaviour pattern, not a knowledge gap.

**5. Tier classification:**

| Rule                  | Tier                   | Rationale                                                                                                                     |
| --------------------- | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| framework-first.md    | Tier 1 (Deterministic) | Model cannot know about Kailash frameworks without being told. Should be enforced via hook that detects raw SQL/HTTP imports. |
| python-environment.md | Tier 2 (Required)      | Model defaults to pip/global Python. Rule changes behaviour reliably but hook enforcement adds safety.                        |
| zero-tolerance.md     | Tier 2 (Required)      | Model's natural drafting pattern includes TODOs. Rule suppresses this reliably.                                               |
| git.md                | Tier 2 (Required)      | Conventional commit format is a specific convention. Rule is sufficient (no hook needed).                                     |
| security.md           | Tier 3 (Reinforcing)   | Model is mostly native on basics but inconsistent on edge cases (eval, input validation). Rule reinforces.                    |
| communication.md      | Tier 4 (Advisory)      | Model already communicates clearly. Rule is documentation of an existing behaviour.                                           |

## Scoring

| Criterion                                                                                | Points |
| ---------------------------------------------------------------------------------------- | ------ |
| Clean-room environment correctly isolated (no .claude/ context leaking)                  | 2      |
| Test prompts designed to trigger the specific rule behaviour                             | 2      |
| Predictions recorded BEFORE running tests (no post-hoc rationalisation)                  | 1      |
| Tier classification supported by empirical evidence from the ablation                    | 3      |
| Mismatch explanations identify the root cause (model behaviour pattern vs knowledge gap) | 2      |
| **Total**                                                                                | **10** |

## Extensions

- **Variant A (cross-model):** Run the same ablation against a different model (e.g., a smaller open-source model). Compare tier classifications. The journal evidence shows that weaker models need MORE rules (MiniMax M2.7 failed on basic security that Claude handled natively).
- **Variant B (COR application):** Ablate research-craft rules: citation format requirements, claim qualification ("we show" vs "we prove"), methodology disclosure. Which research conventions does the model follow natively vs require explicit instruction?
