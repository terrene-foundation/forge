---
drill_id: DR-SC-P-012-FULL
source_atom: SC-P-012
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Decide Whether to Demote or Strengthen a Rule When Ablation Shows Partial Redundancy

## Setup

The practitioner receives ablation data from a clean-room experiment run against a Claude model with no `.claude/rules/` loaded. Five rules were tested by prompting the model with tasks designed to trigger each rule's concern, then measuring compliance without the rule present. Each rule entry includes: the rule text, the model's compliance rate without the rule, the observed failure mode when the rule was absent, and one example of the failure.

**Rule 1 — `security.md` § "No hardcoded API keys"**

- Compliance without rule: 88%
- Failure mode: Model occasionally embeds placeholder API keys that look real (`sk-proj-abc123...`) in code examples. In 12% of cases, the generated code contains a string that matches the pattern of a production credential.
- Example failure: `openai_client = OpenAI(api_key="sk-proj-4f8a2b...")`

**Rule 2 — `communication.md` § "Report outcomes, not implementation"**

- Compliance without rule: 94%
- Failure mode: When summarising complex refactoring work, the model occasionally lists file paths and function names instead of describing what changed for the user. Recoverable by asking "what does this mean for me?"
- Example failure: "Modified `src/auth/middleware.rs` lines 42-89 to refactor the validation pipeline and updated `tests/auth_test.rs` with 3 new test cases."

**Rule 3 — `zero-tolerance.md` § "No TODO/FIXME markers"**

- Compliance without rule: 71%
- Failure mode: Model naturally generates `# TODO: handle edge case` and `// FIXME: error handling` when producing draft or skeleton code. These markers persist into committed code if no linter catches them.
- Example failure: `def process_payment(amount): # TODO: validate currency`

**Rule 4 — `framework-first.md` § "All database work through DataFlow"**

- Compliance without rule: 3%
- Failure mode: Model defaults to `import sqlite3` or `import psycopg2` and writes raw SQL. It has no knowledge of the Kailash DataFlow framework without explicit context. 97% of database-related prompts produce raw SQL.
- Example failure: `conn = sqlite3.connect("app.db"); cursor.execute("SELECT * FROM users WHERE id = ?", (uid,))`

**Rule 5 — `git.md` § "Conventional commits"**

- Compliance without rule: 22%
- Failure mode: Model produces descriptive but non-conventional commit messages. Format varies between runs: sometimes sentence-case, sometimes imperative, rarely prefixed with `type(scope):`.
- Example failure: `git commit -m "Updated the database connection pooling logic to handle timeouts better"`

## Task

1. For each of the 5 rules, state your decision: **demote** (move to on-demand, compress, or remove) or **strengthen** (add hard enforcement beyond the rule text).
2. For each decision, write a one-sentence rationale that cites the **failure cost**, not the compliance percentage.
3. For each rule you demote, state the **monitoring mechanism** — what observable signal would tell you the model has regressed and the rule needs to re-enter always-in-context?
4. For each rule you strengthen, state the **enforcement mechanism** — what hook, linter, or pre-commit check would catch violations that the rule alone does not prevent?

## Model Answer

**Rule 1 — "No hardcoded API keys" — STRENGTHEN**

Rationale: A single hardcoded credential that reaches a commit is a security incident requiring key rotation across all environments — the failure cost is operational emergency, not style drift.

Enforcement mechanism: Add a pre-commit hook running `detect-secrets` (or `gitleaks`) that scans staged files for patterns matching API key formats (`sk-`, `ghp_`, `AKIA`, etc.). The hook rejects the commit before it enters history. The rule stays in the always-in-context set as a first layer, the hook acts as the second layer. Defense-in-depth: even if the model ignores the rule (12% of the time), the hook catches the output before it causes damage.

---

**Rule 2 — "Report outcomes, not implementation" — DEMOTE**

Rationale: When the model lapses into listing file paths instead of outcomes, the user can ask a follow-up question and get a corrected summary within seconds — the failure cost is one extra conversational turn, not a downstream error.

Monitoring mechanism: If session transcripts show the user asking "what does this mean?" or "can you explain that in plain language?" more than twice per session, the rule re-enters the always-in-context set. Track the frequency of clarification-request patterns in session logs.

Demotion form: Compress from a multi-paragraph rule with examples to a single line in a "communication defaults" skill file: "Summarise changes as outcomes for the user, not as file paths."

---

**Rule 3 — "No TODO/FIXME markers" — STRENGTHEN**

Rationale: TODO markers that persist into committed code create invisible technical debt — each marker is a promise of future work that is never tracked, never prioritised, and never completed. The failure cost is cumulative code quality erosion, not a single incident.

Enforcement mechanism: Add a pre-commit hook that greps staged files for `TODO`, `FIXME`, `HACK`, `STUB`, `XXX` markers and rejects the commit with a message naming each marker and its file:line. Exception list for test files (`test_*`, `*_test.*`). The 71% violation rate means the rule alone is insufficient — the model's natural drafting pattern includes TODOs, so the rule fights a strong default. The hook makes the enforcement deterministic regardless of model behaviour.

---

**Rule 4 — "All database work through DataFlow" — STRENGTHEN**

Rationale: Raw SQL bypasses DataFlow's access controls, audit logging, and dialect portability — the failure cost is ungoverned database access that is invisible to the governance layer and non-portable across database backends.

Enforcement mechanism: Add a pre-commit hook (or a CI lint step) that scans for imports of `sqlite3`, `psycopg2`, `pymysql`, `sqlalchemy.create_engine`, and raw SQL string patterns (`SELECT`, `INSERT`, `UPDATE`, `DELETE` in string literals). Reject any file outside `tests/` that matches. The 3% compliance rate means the rule is not merely supplementary — it is the ONLY signal the model has that DataFlow exists. The rule must remain always-in-context AND the hook must catch the 97% of cases where the model defaults to raw SQL despite the rule.

Note: This is a Tier 1 (Deterministic) rule per the ablation tier model. The model CANNOT comply without the rule because it has no knowledge of DataFlow without context. Strengthening is the only valid response — demotion would guarantee 97% violation.

---

**Rule 5 — "Conventional commits" — KEEP (neither demote nor strengthen)**

Rationale: Non-conventional commits break automated changelog generation and make `git log --oneline` useless for release notes — the failure cost is degraded release automation, which accumulates silently until a release manager discovers the changelog is empty.

This is a borderline case. The compliance rate (22%) suggests the rule is load-bearing — the model does not naturally produce `type(scope): description` format. However, the failure mode is not catastrophic (commits can be rebased or squashed later) and a pre-commit hook for commit message format is fragile (it blocks the developer, not the model). The correct response is to KEEP the rule at its current enforcement level: always-in-context, no additional hook. If a future model version demonstrates higher native compliance, revisit.

Alternative valid answer: STRENGTHEN with a `commit-msg` git hook that validates the `type(scope): description` format and rejects non-conforming messages. This is defensible if the practitioner's rationale cites the release automation dependency.

## Scoring

| Criterion                                                                                                                         | Points |
| --------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Rule 1 classified as strengthen (not demote) with security-cost rationale                                                         | 2      |
| Rule 2 classified as demote with low-cost/recoverable rationale                                                                   | 1      |
| Rule 3 classified as strengthen with cumulative-debt rationale                                                                    | 1      |
| Rule 4 classified as strengthen with governance/portability rationale; identified as Tier 1 (model cannot comply without context) | 2      |
| Rule 5 decision is justified by failure cost, not by compliance percentage alone                                                  | 1      |
| Monitoring mechanisms for demoted rules are observable and specific (not "review periodically")                                   | 1      |
| Enforcement mechanisms for strengthened rules name a concrete tool or hook (not "add enforcement")                                | 1      |
| No rule classified solely by threshold ("anything above 90% is demotable") — all rationales reference failure cost                | 1      |
| **Total**                                                                                                                         | **10** |

## Extensions

1. **Model-version regression variant.** After completing the exercise, the practitioner learns that the team is switching from Claude to a smaller open-source model with lower baseline compliance. Re-classify all 5 rules assuming the new model's compliance rates are 15-30 percentage points lower across the board. The practitioner must decide whether any demoted rules need to be re-promoted and whether the strengthening mechanisms are model-agnostic (hooks and linters are; prompt-level rules are not).

2. **Cost quantification variant.** For each of the 5 rules, estimate the cost of a single failure in concrete units: hours to remediate a leaked credential (Rule 1), turns to recover a summary (Rule 2), sessions before TODO debt blocks a feature (Rule 3), hours to port raw SQL across database backends (Rule 4), hours to manually reconstruct a changelog (Rule 5). Use these costs to rank the rules by expected damage (failure probability x cost per failure) and verify that the demote/strengthen classification matches the ranking.

3. **Ablation design critique.** The compliance percentages above were measured by running each rule's test prompt 50 times. The practitioner must critique the experimental design: Is 50 trials sufficient for a rule with 88% compliance (Rule 1)? What is the confidence interval? Could the 3% compliance for Rule 4 be an artifact of the prompt design (e.g., if the prompt mentioned "database" without mentioning DataFlow, the model had no trigger)? Design an improved ablation protocol that controls for prompt phrasing, sample size, and model temperature.
