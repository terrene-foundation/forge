---
drill_id: DR-SC-A-010-FULL
source_atom: SC-A-010
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Decide the Baked-vs-External Boundary for a Token-Heavy Artifact Set

## Setup

The practitioner receives a token budget analysis from a custom AI agent CLI called `kz`. The CLI uses an external-artifact model: all rules, agents, and skills are loaded from markdown files in a `.kz/` directory at the start of every session. The analysis was conducted after users reported that conversations became shallow and repetitive earlier than expected — compaction was firing within 8-10 turns instead of the expected 25-30.

**Token budget analysis:**

```
SYSTEM PROMPT COMPOSITION — kz v0.4.2
=======================================

Baked-in base prompt:          ~2,800 tokens  (compiled into binary)
                                               [cacheable prefix — paid once]

External artifacts loaded per session:
  .kz/CLAUDE.md                  2,200 tokens  [project identity + directives]
  .kz/rules/ (22 files total)  36,400 tokens  [all loaded globally]
  .kz/agents/ (4 files)         6,800 tokens  [agent definitions]
  .kz/skills/ (8 files)         4,600 tokens  [skill definitions]
  ─────────────────────────────────────────
  External subtotal:           50,000 tokens  [dynamic suffix — paid EVERY turn]

Total system prompt:           52,800 tokens
Context window:               200,000 tokens
System prompt overhead:           26.4%

COMPARISON — Claude Code (Anthropic's approach):
  Baked-in system prompt:      ~13,000 tokens  [all cacheable]
  External artifacts:            variable       [user's CLAUDE.md + rules only]
```

**The 22 external rules, with line counts and descriptions:**

| #   | Rule File                 | Lines | Tokens | Description                                                    |
| --- | ------------------------- | ----- | ------ | -------------------------------------------------------------- |
| 1   | `zero-tolerance.md`       | 85    | 2,100  | No stubs, no placeholders, no workarounds, no silent fallbacks |
| 2   | `communication.md`        | 70    | 1,750  | Plain language defaults, explain choices in business terms     |
| 3   | `agents.md`               | 95    | 2,400  | Specialist delegation, quality gates, parallel execution       |
| 4   | `security.md`             | 80    | 2,000  | No hardcoded secrets, parameterized queries, input validation  |
| 5   | `git-workflow.md`         | 50    | 1,250  | Conventional commits, branch naming, atomic commits            |
| 6   | `testing-policy.md`       | 90    | 2,250  | 3-tier testing, regression tests, coverage requirements        |
| 7   | `error-handling.md`       | 40    | 1,000  | Structured errors, correlation IDs, no stack trace exposure    |
| 8   | `naming-conventions.md`   | 35    | 875    | Variable naming, file naming, module naming standards          |
| 9   | `documentation.md`        | 30    | 750    | Version numbers match, no dead links, no placeholders          |
| 10  | `autonomous-execution.md` | 60    | 1,500  | No human-team framing, estimate in sessions not days           |
| 11  | `react-patterns.md`       | 110   | 2,750  | Component composition, hook patterns, state management         |
| 12  | `api-design.md`           | 95    | 2,375  | REST conventions, pagination, error responses                  |
| 13  | `database-patterns.md`    | 85    | 2,125  | ORM usage, migration patterns, query optimization              |
| 14  | `deployment.md`           | 75    | 1,875  | CI/CD pipeline, staging gates, rollback procedures             |
| 15  | `graphql-schema.md`       | 70    | 1,750  | Schema design, resolver patterns, subscription handling        |
| 16  | `auth-patterns.md`        | 65    | 1,625  | JWT handling, refresh tokens, session management               |
| 17  | `caching-strategy.md`     | 45    | 1,125  | Cache invalidation, TTL policies, cache-aside pattern          |
| 18  | `monitoring.md`           | 40    | 1,000  | Metrics collection, alerting thresholds, log aggregation       |
| 19  | `accessibility.md`        | 55    | 1,375  | WCAG compliance, ARIA patterns, keyboard navigation            |
| 20  | `i18n-patterns.md`        | 50    | 1,250  | Internationalization, locale handling, RTL support             |
| 21  | `performance.md`          | 60    | 1,500  | Bundle optimization, lazy loading, render performance          |
| 22  | `feature-flags.md`        | 35    | 875    | Flag lifecycle, rollout percentages, cleanup policy            |

**Architecture note:** The `kz` binary is compiled from Rust source. Rules in the baked-in base prompt are embedded in `system_prompt.rs` at compile time and land in the **cacheable prefix** of the prompt — after the first turn, they cost zero incremental tokens. External rules are appended to the prompt as the **dynamic suffix** — they are paid in full on every turn because any change to the suffix invalidates the prompt cache.

## Task

1. Classify each of the 22 rules using the three-axis test. For each rule, evaluate:
   - **Project variance:** Does this rule change between projects, or is it universal to every `kz` user? (High variance = external. Low variance = bake candidate.)
   - **Cache position:** Is this rule in the cacheable prefix (baked) or the dynamic suffix (external)? What is the per-turn cost difference?
   - **Editability need:** Do users need to read, modify, or override this rule? Or is it the agent's core personality that users should not see or change?

   Assign each rule a verdict: **BAKE** (move to `system_prompt.rs`), **KEEP EXTERNAL** (stays in `.kz/rules/`), or **SCOPE** (stays external but add `paths:` frontmatter to avoid loading globally).

2. Calculate the token budget under your proposed hybrid model. Report: (a) new baked-in total, (b) new external always-loaded total, (c) new external scoped total (loaded only when relevant), and (d) the effective per-turn cost compared to the current 52,800.

3. Identify the rules that sit on the boundary — cases where the three-axis test does not give a clear answer. For each boundary case, explain the tension and make a judgment call with reasoning.

4. Address the counter-argument: "Baking rules into the binary makes them invisible and uneditable. This violates CO's principle that institutional knowledge should be living documents. Everything should stay external." In 2-3 sentences, explain why this argument is partially right and partially wrong, and where the boundary falls.

## Model Answer

**1. Three-axis classification:**

| #   | Rule                      | Project Variance                      | Cache Position                   | Editability Need                             | Verdict           |
| --- | ------------------------- | ------------------------------------- | -------------------------------- | -------------------------------------------- | ----------------- |
| 1   | `zero-tolerance.md`       | Low — universal policy                | Dynamic suffix (paid every turn) | Low — users should not weaken this           | **BAKE**          |
| 2   | `communication.md`        | Low — universal style                 | Dynamic suffix                   | Low — core personality                       | **BAKE**          |
| 3   | `agents.md`               | Low — universal orchestration         | Dynamic suffix                   | Low — framework behavior                     | **BAKE**          |
| 4   | `security.md`             | Low — universal policy                | Dynamic suffix                   | Low — users should not weaken security rules | **BAKE**          |
| 5   | `git-workflow.md`         | Low — universal conventions           | Dynamic suffix                   | Low — standard practice                      | **BAKE**          |
| 6   | `testing-policy.md`       | Medium — coverage thresholds vary     | Dynamic suffix                   | Medium — teams adjust thresholds             | **BOUNDARY**      |
| 7   | `error-handling.md`       | Low — universal pattern               | Dynamic suffix                   | Low — standard practice                      | **BAKE**          |
| 8   | `naming-conventions.md`   | Medium — some teams override          | Dynamic suffix                   | Medium — style preference                    | **BOUNDARY**      |
| 9   | `documentation.md`        | Low — universal hygiene               | Dynamic suffix                   | Low — standard practice                      | **BAKE**          |
| 10  | `autonomous-execution.md` | Low — universal framing               | Dynamic suffix                   | Low — core personality                       | **BAKE**          |
| 11  | `react-patterns.md`       | High — not all projects use React     | Dynamic suffix                   | High — framework-specific                    | **SCOPE**         |
| 12  | `api-design.md`           | Medium — conventions vary             | Dynamic suffix                   | High — team-specific conventions             | **KEEP EXTERNAL** |
| 13  | `database-patterns.md`    | High — ORM and DB vary                | Dynamic suffix                   | High — project-specific                      | **SCOPE**         |
| 14  | `deployment.md`           | High — CI/CD pipelines differ         | Dynamic suffix                   | High — environment-specific                  | **SCOPE**         |
| 15  | `graphql-schema.md`       | High — not all projects use GraphQL   | Dynamic suffix                   | High — schema-specific                       | **SCOPE**         |
| 16  | `auth-patterns.md`        | Medium — auth approaches vary         | Dynamic suffix                   | High — security model specific               | **SCOPE**         |
| 17  | `caching-strategy.md`     | High — caching varies by architecture | Dynamic suffix                   | High — infrastructure-specific               | **SCOPE**         |
| 18  | `monitoring.md`           | High — monitoring stacks differ       | Dynamic suffix                   | High — infrastructure-specific               | **SCOPE**         |
| 19  | `accessibility.md`        | Medium — required for web, not CLI    | Dynamic suffix                   | Medium — compliance requirements vary        | **SCOPE**         |
| 20  | `i18n-patterns.md`        | High — many projects are English-only | Dynamic suffix                   | High — project-specific                      | **SCOPE**         |
| 21  | `performance.md`          | Medium — patterns vary by stack       | Dynamic suffix                   | High — architecture-specific                 | **SCOPE**         |
| 22  | `feature-flags.md`        | High — not all projects use flags     | Dynamic suffix                   | High — tooling-specific                      | **SCOPE**         |

**Summary:** 8 BAKE, 1 KEEP EXTERNAL, 11 SCOPE, 2 BOUNDARY.

**2. Token budget under hybrid model:**

**(a) New baked-in total:**

Current baked: 2,800 tokens. Add 8 baked rules:

| Baked Rule                   | Tokens     |
| ---------------------------- | ---------- |
| `zero-tolerance.md`          | 2,100      |
| `communication.md`           | 1,750      |
| `agents.md`                  | 2,400      |
| `security.md`                | 2,000      |
| `git-workflow.md`            | 1,250      |
| `error-handling.md`          | 1,000      |
| `documentation.md`           | 750        |
| `autonomous-execution.md`    | 1,500      |
| **Subtotal new baked rules** | **12,750** |

New baked-in total: 2,800 + 12,750 = **15,550 tokens** (cacheable — paid once, then zero per turn).

**(b) New external always-loaded total:**

| Always-Loaded External                           | Tokens     |
| ------------------------------------------------ | ---------- |
| `.kz/CLAUDE.md`                                  | 2,200      |
| `api-design.md` (keep external, unscoped)        | 2,375      |
| Agents (4 files)                                 | 6,800      |
| Skills (8 files)                                 | 4,600      |
| Boundary decisions (see §3): `testing-policy.md` | 2,250      |
| Boundary decisions: `naming-conventions.md`      | 875        |
| **Subtotal**                                     | **19,100** |

**(c) New external scoped total (loaded only when relevant):**

| Scoped External | Tokens |
| --------------- | ------ |
| 11 scoped rules | 17,250 |

These 17,250 tokens load only when the user is editing files in the relevant directory. A session editing React components loads `react-patterns.md` (2,750) and `accessibility.md` (1,375) — an additional 4,125, not the full 17,250.

**(d) Effective per-turn cost comparison:**

| Model                             | First Turn                              | Subsequent Turns                 |
| --------------------------------- | --------------------------------------- | -------------------------------- |
| Current (all external)            | 52,800                                  | 52,800 (nothing cached)          |
| Hybrid (baked + external)         | 34,650 (15,550 baked + 19,100 external) | 19,100 (baked = 0 after caching) |
| Hybrid + scoped (typical session) | 34,650 + ~4,000 scoped                  | 19,100 + ~4,000 scoped = ~23,100 |

**Reduction: 52,800/turn -> ~23,100/turn on a typical session (56% reduction).** On the first turn, baked tokens are paid once. On subsequent turns, only the external suffix is paid. Compaction that was firing at turn 8-10 should now extend to turn 20-25, matching the expected 25-30 turn target.

**3. Boundary cases:**

**`testing-policy.md` (2,250 tokens):**

- **Tension:** Testing policy is largely universal (3-tier testing, regression tests required), but coverage thresholds (80% general, 100% for security-critical) are project-specific. Some projects adjust these.
- **Judgment:** KEEP EXTERNAL (unscoped). The cost is 2,250 tokens/turn, but the editability need is real — teams that ship to regulated environments raise coverage to 100% across the board, and teams in rapid prototyping phases temporarily lower it. Baking the thresholds would require recompilation to adjust. The universal parts (regression test policy, no mocking in integration tests) could theoretically be split out and baked, but splitting one rule into two files for a 2,250-token save introduces maintenance overhead that does not pay for itself.

**`naming-conventions.md` (875 tokens):**

- **Tension:** Naming conventions feel universal (snake_case for Python, camelCase for JS), but some teams override them (a Java shop using `kz` might want PascalCase for classes). The token cost is small (875), so the per-turn savings from baking are minimal.
- **Judgment:** KEEP EXTERNAL (unscoped). At 875 tokens, the cost of keeping it external is low. The editability benefit (teams can adjust naming without recompilation) outweighs the small cache saving. If this were 3,000+ tokens, the calculus would flip.

**4. Counter-argument response:**

The argument that baking violates CO's living-documents principle is right about external rules and wrong about universal rules. CO §13 (Layer 2 — Context) requires institutional knowledge to be living documents — but not all rules are institutional knowledge in the CO sense. Rules like `zero-tolerance.md` and `communication.md` are the agent's **core personality** — they define what the agent IS, not what a specific project needs. Personality does not need to be editable by users any more than Claude's base system prompt needs to be editable. The boundary falls at project variance: rules that change between projects are institutional knowledge (living, external, editable); rules that are universal across every project are personality (compiled, cached, invisible). Baking personality is not hiding knowledge — it is recognizing that some knowledge has graduated from "per-project convention" to "platform invariant."

## Scoring

| Criterion                                                                                             | Points |
| ----------------------------------------------------------------------------------------------------- | ------ |
| At least 6 of the 8 universal rules correctly classified as BAKE with low-variance rationale          | 2      |
| At least 8 of the 11 domain-specific rules correctly classified as SCOPE with high-variance rationale | 2      |
| Three-axis test applied consistently (each rule evaluated on all three axes, not just one)            | 1      |
| Token budget calculated with before/after comparison showing measurable per-turn reduction            | 2      |
| At least one boundary case identified with explicit tension and judgment call                         | 1      |
| Counter-argument addressed with nuance (not dismissed entirely or accepted entirely)                  | 1      |
| Cacheable-prefix vs dynamic-suffix distinction understood and applied to cost analysis                | 1      |
| **Total**                                                                                             | **10** |

## Extensions

- **Variant A (multi-model envelope):** The `kz` CLI supports both Claude (200K context) and a smaller model (32K context). On the smaller model, the 52,800-token system prompt consumes 165% of the context window — it literally does not fit. The practitioner must design a tiered baking strategy: the small-model binary bakes MORE rules (reducing the external load to fit within 32K), while the large-model binary bakes fewer (keeping more external for editability). This forces the practitioner to re-run the three-axis test with a fourth axis: model-context-budget pressure. Rules that were KEEP EXTERNAL for the 200K model may flip to BAKE for the 32K model.
- **Variant B (bake-then-override architecture):** Instead of the binary bake-or-external decision, design a layered architecture where baked rules serve as defaults that external rules can override. A project that needs different naming conventions places a `naming-conventions.md` in `.kz/rules/` that takes precedence over the baked default. The practitioner must design the override semantics (full replacement? merge? conflict detection?) and calculate whether the override mechanism itself adds enough complexity to negate the token savings.
