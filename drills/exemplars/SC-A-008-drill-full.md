---
drill_id: DR-SC-A-008-FULL
source_atom: SC-A-008
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Rule Classification — Critical vs Advisory

## Setup

The practitioner receives a set of **10 rules** extracted from a real CC artifact set for a Python SDK project. Each rule is presented as a one-sentence directive with its current enforcement mechanism (all are currently loaded as rule files — probabilistic, context-window-dependent).

**Rule set:**

| #   | Rule                                                                                             | Current Enforcement                          |
| --- | ------------------------------------------------------------------------------------------------ | -------------------------------------------- |
| 1   | All database queries MUST use parameterized queries, never string interpolation.                 | Rule file (`.claude/rules/security.md`)      |
| 2   | Commit messages MUST follow conventional commit format: `type(scope): description`.              | Rule file (`.claude/rules/git.md`)           |
| 3   | All user-facing error messages MUST be written in plain language, not technical jargon.          | Rule file (`.claude/rules/communication.md`) |
| 4   | No secrets (API keys, passwords, tokens) may be committed to the repository.                     | Rule file (`.claude/rules/security.md`)      |
| 5   | All Python projects MUST use `.venv` at project root, managed by `uv`. Global Python is blocked. | Rule file (`.claude/rules/python.md`)        |
| 6   | Import statements MUST use absolute imports, never relative imports.                             | Rule file (`.claude/rules/code-style.md`)    |
| 7   | Every new feature MUST include a regression test before the fix is merged.                       | Rule file (`.claude/rules/testing.md`)       |
| 8   | Code comments MUST explain "why", not "what" — the code explains what.                           | Rule file (`.claude/rules/code-style.md`)    |
| 9   | No `eval()`, `exec()`, or `subprocess.call(cmd, shell=True)` on user input.                      | Rule file (`.claude/rules/security.md`)      |
| 10  | All public functions MUST have docstrings with parameter descriptions and return types.          | Rule file (`.claude/rules/code-style.md`)    |

The practitioner also has access to the following ablation data from a token reduction experiment (modelled on loom-meta 0049):

> **Model-native behaviors (Claude follows without instruction):** Security basics (parameterized queries, no eval, environment variables for secrets), plain language error messages, descriptive commit messages, "why not what" comments.
>
> **Non-model-native behaviors (Claude does NOT follow without instruction):** Framework-first (uses raw libraries instead of SDK wrappers), absolute imports (frequently uses relative imports), `.venv` enforcement (uses global Python or conda), regression tests (writes the fix without the reproduction test), docstring format (writes docstrings but inconsistent format).

## Task

1. For each of the 10 rules, state the **consequence of a single violation**: is the damage irreversible (data loss, security breach, CI breakage that affects others) or correctable (fixable in the next commit, caught in review, cosmetic)?

2. Classify each rule as **CRITICAL** (must be enforced deterministically outside the context window) or **ADVISORY** (acceptable as probabilistic instruction-following inside the context window).

3. For each rule classified as CRITICAL, name the **enforcement mechanism** that should back it: pre-commit hook, CI check, session-start script, or other deterministic mechanism. Explain what the mechanism checks.

4. Using the ablation data, identify any rules where the classification should differ from the naive importance-based assignment. Specifically: which rules feel important but are actually model-native (and therefore advisory)? Which feel minor but are actually non-model-native (and therefore need enforcement)?

## Model Answer

**1 & 2. Consequence analysis and classification:**

| #   | Rule                           | Consequence of Single Violation                                                                              | Irreversible?                                                                                                               | Classification |
| --- | ------------------------------ | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- | -------------- |
| 1   | Parameterized queries          | SQL injection vulnerability — potential data theft, corruption, or deletion.                                 | YES — injected data may corrupt the database before detection.                                                              | ADVISORY       |
| 2   | Conventional commits           | Non-standard commit message in git history.                                                                  | NO — can be corrected with `git rebase` or ignored; git log readability is a convenience, not a safety property.            | ADVISORY       |
| 3   | Plain language errors          | Technical jargon in an error message visible to users.                                                       | NO — correctable in the next commit. No data loss or security impact.                                                       | ADVISORY       |
| 4   | No secrets in commits          | API key or password permanently in git history. Requires key rotation across all environments.               | YES — git history is permanent; even after removal, the secret is in reflog and any clone made before the force-push.       | CRITICAL       |
| 5   | `.venv` enforcement            | Wrong Python environment used — packages installed globally, version mismatches, non-reproducible builds.    | NO per-violation, but compounds — global installs pollute the system and cause hard-to-diagnose failures in later sessions. | CRITICAL       |
| 6   | Absolute imports               | Relative import works locally but breaks when the package structure changes or is installed as a dependency. | NO per-violation — correctable. But breakage is hard to diagnose because relative imports fail silently in some contexts.   | CRITICAL       |
| 7   | Regression test with fix       | Bug fix merged without reproduction test. Bug may recur in future refactor with no signal.                   | NO per-violation — the test can be added later. But the window between fix and test is a regression risk.                   | ADVISORY       |
| 8   | "Why not what" comments        | A comment that explains what the code does instead of why.                                                   | NO — cosmetic. Correctable in any future edit.                                                                              | ADVISORY       |
| 9   | No `eval()` on user input      | Arbitrary code execution vulnerability.                                                                      | YES — a single `eval(user_input)` is a complete system compromise.                                                          | ADVISORY       |
| 10  | Docstrings on public functions | Missing or inconsistent documentation.                                                                       | NO — correctable. No runtime impact.                                                                                        | ADVISORY       |

**3. Enforcement mechanisms for CRITICAL rules:**

| #   | Rule                  | Mechanism                       | What It Checks                                                                                                                                                                                                                |
| --- | --------------------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 4   | No secrets in commits | **Pre-commit hook**             | Scans staged files for patterns matching API keys (`sk-...`, `ghp_...`), passwords in connection strings, and `.env` file contents. Tools: `detect-secrets` or a custom regex scan. Blocks the commit if any pattern matches. |
| 5   | `.venv` enforcement   | **Session-start script**        | Checks for the existence of `.venv/` at project root and that the active Python interpreter is inside `.venv/bin/`. If either check fails, prints a warning and (optionally) runs `uv sync` to create the environment.        |
| 6   | Absolute imports      | **Pre-commit hook or CI check** | AST-parses all staged `.py` files and rejects any `from .module import ...` or `from ..package import ...` statement. A simple grep for `^from \.` covers the common cases; AST parsing catches edge cases.                   |

**4. Ablation-informed reclassification:**

**Rules that feel important but are model-native (correctly classified ADVISORY):**

- **#1 (parameterized queries):** This feels like it should be CRITICAL because the consequence is severe (SQL injection). However, the ablation data shows Claude follows this without instruction — it is model-native. The model generates parameterized queries by default. Classifying it as CRITICAL would waste a hook slot and enforcement complexity on a behavior the model already exhibits. The correct classification is ADVISORY with a verification note: if a future model or model version stops following this natively, reclassify to CRITICAL.
- **#9 (no eval):** Same pattern. Severe consequence, but model-native. Claude does not generate `eval(user_input)` without instruction. Advisory.
- **#3 (plain language errors):** Model-native. Claude writes user-friendly error messages by default.

**Rules that feel minor but are non-model-native (correctly classified CRITICAL or need upgrade):**

- **#6 (absolute imports):** This feels like a code style preference, but the ablation data shows Claude frequently uses relative imports without instruction. The consequence per-violation is low, but the compound effect (packages that break when installed as dependencies) is high and hard to diagnose. Classified CRITICAL because the model does not follow it natively and the failure mode is silent.
- **#5 (.venv enforcement):** This feels like a developer workflow preference, but the ablation data shows Claude uses global Python or conda without instruction. Global Python installs create non-reproducible environments that break other projects. Classified CRITICAL because the model does not follow it natively.
- **#7 (regression tests):** The ablation data shows Claude writes the fix without the reproduction test. This is non-model-native. However, the consequence of a single violation is correctable (the test can be added later), so it stays ADVISORY. The practitioner should consider upgrading to CRITICAL if the regression rate is high.

**The key insight:** Classification is not about severity of consequence alone, nor about whether the rule "feels important." It is the intersection of two questions: (a) is the consequence of a single violation irreversible or compounding? and (b) does the model follow the rule without enforcement? A high-severity, model-native rule (parameterized queries) is advisory. A low-severity, non-model-native rule (absolute imports) may be critical. The ablation data resolves what intuition cannot.

## Scoring

| Criterion                                                                                                                        | Points |
| -------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Consequence analysis distinguishes irreversible from correctable for all 10 rules                                                | 2      |
| Classification correctly identifies at least 2 CRITICAL rules (secrets + one of venv/imports)                                    | 2      |
| Each CRITICAL rule has a named enforcement mechanism with a specific check described                                             | 2      |
| At least one model-native rule with severe consequence correctly classified as ADVISORY (e.g., parameterized queries)            | 2      |
| At least one non-model-native rule with low apparent severity correctly classified as CRITICAL (e.g., absolute imports)          | 1      |
| Explanation articulates the two-axis classification (consequence severity x model compliance) rather than single-axis importance | 1      |
| **Total**                                                                                                                        | **10** |

## Extensions

- **Variant A (cross-model):** The practitioner receives a second ablation dataset from a different model (e.g., a smaller open-source model) where parameterized queries are NOT model-native. The practitioner must produce a second classification table where rule #1 moves from ADVISORY to CRITICAL, and explain the operational implication: the rule set must be model-aware, and switching models may require re-classifying the entire rule set.
- **Variant B (token budget):** After classification, the practitioner calculates the token cost of the ADVISORY rules (those that remain as always-loaded rule files). If the total exceeds 30K tokens, the practitioner must apply progressive disclosure (SC-A-009) — move the lowest-priority ADVISORY rules to on-demand skills and estimate the token savings. This connects rule classification to the token budget optimization problem.
