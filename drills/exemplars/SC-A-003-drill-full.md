---
drill_id: DR-SC-A-003-FULL
source_atom: SC-A-003
difficulty: intermediate
time_box_minutes: 30
modality: drill
application: [COC]
---

# Defense-in-Depth Codification Across Multiple Artifact Locations

## Setup

The practitioner has just completed a red team review that surfaced a critical finding: **raw SQL string interpolation has been used in 3 places across the codebase despite an existing rule prohibiting it.** The existing rule file (`.claude/rules/security.md`) contains the prohibition:

```markdown
## Parameterized Queries

All database queries MUST use parameterized queries or ORM.

❌ f"SELECT \* FROM users WHERE id = {user_id}"
❌ "DELETE FROM users WHERE name = '" + name + "'"

✅ "SELECT _ FROM users WHERE id = %s", (user_id,)
✅ cursor.execute("SELECT _ FROM users WHERE id = ?", (user_id,))
✅ User.query.filter_by(id=user_id) # ORM
```

The 3 violations were introduced across 2 sessions by an agent that did not load the security rule file (it was out of the file's declared scope). The violations passed code review because the reviewer agent's checklist did not include a SQL injection check. No pre-commit hook or CI check exists for string-interpolated SQL.

The practitioner must now codify this rule with defense-in-depth so that a single missing artifact can no longer silently bypass enforcement.

## Task

1. List at least **5 distinct artifact types** where this rule could be codified. For each, name the specific artifact type (rule, hook, agent, skill, command, CI check, etc.) and describe its unique enforcement surface — what it catches that the others cannot.

2. For each artifact type, describe the **specific failure mode** if that artifact alone is missing from the stack. The failure mode must be concrete (not "things could go wrong" but "the agent generates raw SQL because the rule file is out of scope and no hook blocks the commit").

3. Identify which artifact types provide **deterministic** enforcement (the violation is mechanically blocked) versus **probabilistic** enforcement (the violation is caught only if the AI reads and follows the instruction). Explain why the deterministic/probabilistic distinction matters for this rule.

4. Write a **codification plan** that lists the artifacts in implementation order (which to create first and why) and estimates the effort for each.

5. Determine the **minimum viable defense-in-depth** — the smallest subset of artifact types that provides both deterministic and probabilistic coverage. Explain why fewer than this minimum is insufficient and why more is still valuable.

## Model Answer

**1. Five artifact types and their enforcement surfaces:**

| #   | Artifact Type                                                       | Enforcement Surface                                                                                         | What It Catches That Others Cannot                                                                                                                                                                                                                                    |
| --- | ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Rule file** (`.claude/rules/security.md`)                         | Constraint text loaded into the agent's context window. Provides the instruction.                           | Catches violations during code generation when the agent is actively reasoning about database queries and the rule file is in scope. Cannot catch violations when the rule is out of scope or the context window overflows.                                           |
| 2   | **Pre-commit hook** (`scripts/hooks/pre-commit`)                    | Deterministic grep/AST scan run on every staged file before commit.                                         | Catches violations that have already been written to a file and staged — regardless of whether the agent "knew" about the rule. Runs outside the context window entirely. Cannot catch violations in code that is executed but never committed (e.g., REPL sessions). |
| 3   | **Security reviewer agent** (`.claude/agents/security-reviewer.md`) | Specialist agent checklist consulted during code review.                                                    | Catches violations during review that the implementing agent missed. The reviewer's checklist is independent of the implementer's rule loading. Cannot catch violations if the review gate is skipped or the agent's review is superficial.                           |
| 4   | **Skill** (`.claude/skills/security-patterns/`)                     | Teaching material and code examples loaded on demand when the agent is implementing database functionality. | Provides the correct pattern so the agent generates parameterized queries from the start, preventing violations rather than catching them. Cannot enforce — it teaches, and the agent may still deviate.                                                              |
| 5   | **CI check** (GitHub Actions / pre-push)                            | Automated scan of the entire codebase on every push.                                                        | Catches violations that bypass both the pre-commit hook (e.g., `--no-verify` commits) and the review agent. Runs on the server, not the developer's machine. Cannot catch violations in local development before push.                                                |
| 6   | **CLAUDE.md directive**                                             | Top-level always-loaded instruction.                                                                        | Catches violations in sessions where the security rule file is not loaded due to scope mismatch. The CLAUDE.md directive is always in context. Cannot provide detailed examples (token budget) — only the constraint, not the teaching material.                      |

**2. Failure modes if each is missing:**

| #   | Missing Artifact  | Specific Failure Mode                                                                                                                                                                                                                                                           |
| --- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Rule file         | The agent has no instruction about parameterized queries when reasoning about SQL. In a session scoped to database work, the agent generates `f"SELECT * FROM users WHERE id = {user_id}"` as the natural pattern because string interpolation is the most direct path.         |
| 2   | Pre-commit hook   | The agent writes raw SQL, the code review agent misses it (checklist item absent or review superficial), and the violation is committed to the repository. The next session inherits the pattern as precedent, compounding the violation.                                       |
| 3   | Security reviewer | The implementing agent follows the rule most of the time (probabilistic compliance), but the 5% of cases where it deviates — complex join queries, dynamic table names, ORM fallback paths — go unreviewed. These are the highest-risk cases.                                   |
| 4   | Skill             | The agent knows the constraint (from the rule file) but not the correct pattern. It attempts parameterized queries but uses the wrong syntax for the database dialect (e.g., `%s` for SQLite instead of `?`), producing a different class of bug.                               |
| 5   | CI check          | A developer commits with `--no-verify`, bypassing the pre-commit hook. The violation reaches the repository. Without CI scanning, it remains in the codebase permanently.                                                                                                       |
| 6   | CLAUDE.md         | In sessions where the security rule file is out of scope (e.g., a frontend-focused session that touches a database migration), the agent has no awareness of the constraint at all. The rule file's scope declaration excludes the session, and no fallback instruction exists. |

**3. Deterministic vs probabilistic:**

| Artifact          | Enforcement Type | Why                                                                                                                                                                 |
| ----------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Rule file         | Probabilistic    | The agent must read, understand, and follow the instruction. Context window overflow, scope mismatch, or attention failure can cause the agent to violate the rule. |
| Pre-commit hook   | Deterministic    | A grep or AST scan mechanically blocks the commit. The agent cannot override the hook. The check runs outside the AI's reasoning process.                           |
| Security reviewer | Probabilistic    | The reviewer agent must notice the violation and flag it. If the reviewer's checklist is incomplete or the review is superficial, the violation passes.             |
| Skill             | Probabilistic    | The skill teaches the correct pattern, but the agent may deviate. Teaching prevents violations; it does not block them.                                             |
| CI check          | Deterministic    | The scan runs on the server and blocks the push. No AI reasoning is involved in the enforcement decision.                                                           |
| CLAUDE.md         | Probabilistic    | Same limitation as the rule file — the agent must follow the instruction.                                                                                           |

The distinction matters because **probabilistic enforcement has a non-zero failure rate that compounds across sessions**. If the rule file is the only enforcement and the agent deviates 5% of the time, then across 20 sessions, the expected number of violations is 1. Over 100 sessions, it is 5. Deterministic enforcement reduces the failure rate to zero for the covered path — the hook blocks the commit regardless of how many sessions run. A defense-in-depth stack must include at least one deterministic mechanism to bound the probabilistic tail.

**4. Codification plan (implementation order):**

| Order | Artifact                           | Effort | Why This Order                                                                                                                                                                                      |
| ----- | ---------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | Pre-commit hook                    | 15 min | Highest immediate value. Deterministically blocks any new violations from being committed while the rest of the stack is built. Stops the bleeding.                                                 |
| 2     | Security reviewer checklist update | 10 min | Second-highest value. Adds the check to the review agent so future code reviews explicitly test for SQL injection. Quick to implement — one checklist item.                                         |
| 3     | Rule file scope expansion          | 5 min  | The existing rule file is already correct — it just needs its scope broadened to cover all files that touch SQL, not just the database module.                                                      |
| 4     | Skill update                       | 20 min | Add dialect-specific parameterized query examples (SQLite `?`, PostgreSQL `%s`, ORM alternatives) so the agent generates correct code from the start. Takes longer because examples must be tested. |
| 5     | CI check                           | 30 min | Server-side redundancy for the pre-commit hook. Important but lower urgency — the hook already covers the local path.                                                                               |
| 6     | CLAUDE.md one-liner                | 5 min  | Fallback instruction for out-of-scope sessions. Quick to add.                                                                                                                                       |

**5. Minimum viable defense-in-depth:**

The minimum viable stack is **3 artifacts: one deterministic (pre-commit hook) + two probabilistic (rule file + security reviewer)**. The deterministic mechanism provides the hard floor — no violations can be committed. The rule file provides the instruction during generation — the agent writes correct code most of the time. The reviewer provides the independent check — violations that bypass the rule file's probabilistic compliance are caught at review.

Fewer than 3 is insufficient because: (a) rule file alone is probabilistic with no backstop, (b) hook alone blocks but does not teach — the agent keeps generating violations that the hook rejects, wasting cycles, and (c) reviewer alone is probabilistic and loads only at review time, not at generation time.

More than 3 is still valuable because: the skill prevents violations at the source (reducing hook rejections), the CI check catches `--no-verify` bypasses, and the CLAUDE.md directive covers out-of-scope sessions. Each additional layer reduces a different residual risk.

## Scoring

| Criterion                                                                                               | Points |
| ------------------------------------------------------------------------------------------------------- | ------ |
| At least 5 distinct artifact types named with unique enforcement surfaces                               | 2      |
| Failure modes are specific — each names a concrete scenario, not a generic risk                         | 2      |
| Deterministic vs probabilistic classification is correct for all artifacts                              | 2      |
| Codification plan orders by immediate value, starting with a deterministic mechanism                    | 1      |
| Minimum viable stack includes at least one deterministic and one probabilistic mechanism with rationale | 2      |
| Effort estimates are plausible and distinguish quick changes from substantial work                      | 1      |
| **Total**                                                                                               | **10** |

## Extensions

- **Variant A (negative evidence):** After completing the plan, the practitioner receives the information that the pre-commit hook has a known bypass: it does not scan files in `migrations/` because migration files were excluded from linting. The practitioner must update the defense-in-depth stack to cover the gap — either by extending the hook's scope, adding a second hook for migrations, or relying on the CI check as the deterministic backstop for migration files.
- **Variant B (cross-repo):** The rule must be enforced not just in this repository but across 5 downstream repos that sync from a template. The practitioner must identify which artifacts can be synced from the template (rules, hooks, CI checks) and which must be created locally (agent checklists, skills with repo-specific examples). This connects to the authority-chain escalation move (SC-B-002) — the template owns the global rule, and each downstream repo owns its local specialization.
