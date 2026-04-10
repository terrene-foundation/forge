---
drill_id: DR-SC-A-009-FULL
source_atom: SC-A-009
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Restructure a Flat CC Artifact Set into Progressive Disclosure Levels

## Setup

The practitioner receives a flat CC artifact set from a mid-sized project. All rules and agents are loaded globally on every turn — there is no `paths:` scoping, no skill extraction, and CLAUDE.md restates several rules verbatim. The practitioner also receives a token-count reference (1 line of markdown ~ 4 tokens) and the project's directory structure.

**Project directory structure:**

```
myproject/
├── .claude/
│   ├── CLAUDE.md              (85 lines)
│   ├── agents/
│   │   ├── backend-dev.md     (480 lines)
│   │   └── frontend-dev.md    (320 lines)
│   └── rules/
│       ├── api-conventions.md      (60 lines)
│       ├── auth-security.md        (45 lines)
│       ├── code-style.md           (30 lines)
│       ├── communication.md        (25 lines)
│       ├── database-patterns.md    (55 lines)
│       ├── deployment-checklist.md (40 lines)
│       ├── error-handling.md       (35 lines)
│       ├── git-workflow.md         (20 lines)
│       ├── graphql-schema.md       (70 lines)
│       ├── naming-conventions.md   (25 lines)
│       ├── react-patterns.md       (65 lines)
│       ├── rest-api-design.md      (50 lines)
│       ├── state-management.md     (55 lines)
│       ├── testing-policy.md       (40 lines)
│       └── zero-tolerance.md       (30 lines)
├── src/
│   ├── api/          (REST + GraphQL endpoints)
│   ├── auth/         (authentication module)
│   ├── components/   (React frontend)
│   ├── db/           (database models and migrations)
│   ├── deploy/       (deployment scripts)
│   └── utils/        (shared utilities)
└── tests/
```

**CLAUDE.md contents (85 lines, excerpt of key sections):**

```markdown
# MyProject

## Absolute Directives

1. All API endpoints must use REST conventions (see rules/api-conventions.md)
2. Authentication uses JWT with refresh tokens
3. Database access through ORM only — no raw SQL
4. All code must follow the naming conventions in rules/naming-conventions.md
5. Zero tolerance for stubs, placeholders, or TODO markers

## Code Style

- Use 4-space indentation
- Maximum line length: 100 characters
- Prefer const over let in JavaScript

## Error Handling

- All API endpoints must return structured error responses
- Never expose stack traces to clients
- Log all errors with correlation IDs

## Deployment

- Always run the deployment checklist before releasing
- Staging must pass all tests before production deploy
```

**Key observations the practitioner should notice:**

- CLAUDE.md §Code Style restates `rules/code-style.md` verbatim
- CLAUDE.md §Error Handling restates `rules/error-handling.md` verbatim
- CLAUDE.md §Deployment restates `rules/deployment-checklist.md` content
- `backend-dev.md` (480 lines) embeds detailed database patterns, API design guidance, and testing instructions that duplicate the corresponding rule files
- `frontend-dev.md` (320 lines) embeds React component patterns and state management guidance
- None of the 15 rule files have `paths:` frontmatter — all load globally regardless of which directory the user is working in
- `graphql-schema.md` loads when editing `src/components/` (irrelevant)
- `database-patterns.md` loads when editing `src/deploy/` (irrelevant)
- `react-patterns.md` loads when editing `src/db/` (irrelevant)

## Task

1. Classify each artifact (CLAUDE.md, 2 agents, 15 rules) into one of four progressive disclosure levels:
   - **Level 1 — Master Directive:** Always loaded, every session. Only identity + absolute directives. Target: under 40 lines.
   - **Level 2 — Index:** Agent and skill stubs loaded on demand by CC's discovery mechanism. Target: agent definitions under 150 lines each.
   - **Level 3 — Topic:** Individual rules and extracted skills loaded when relevant (via `paths:` scoping or skill invocation). This is where most content lives.
   - **Level 4 — Deep Reference:** Guides, specs, and ADRs loaded only when explicitly requested. Rarely needed during normal work.

2. For every rule classified as Level 3, write the `paths:` frontmatter that scopes it to the correct source directories. Use the project directory structure to determine which directories each rule is relevant to.

3. Identify every instance where CLAUDE.md restates content from a rule file. For each, state: which CLAUDE.md section restates which rule, and whether the restatement should be (a) removed entirely (the rule file handles it) or (b) compressed to a one-line pointer.

4. For each agent over 150 lines, identify what content should be extracted into skill files at Level 3. Write the stub agent definition (Level 2) and list the skill files it would reference.

5. Calculate the before and after always-loaded token count. Always-loaded = Level 1 (CLAUDE.md) + Level 2 (agent stubs) + any rules that remain unscoped (no `paths:` frontmatter).

## Model Answer

**1. Classification:**

| Artifact                  | Current Lines | Level                             | Rationale                                                                                                                                 |
| ------------------------- | ------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| CLAUDE.md                 | 85            | Level 1 (after trimming to ~35)   | Master directive, but currently bloated with restated rules. After removing restatements, it holds only identity + 5 absolute directives. |
| `backend-dev.md`          | 480           | Level 2 (stub) + Level 3 (skills) | Agent definition is bloated. Extract database patterns, API design, and testing guidance into skills. Stub stays at ~120 lines.           |
| `frontend-dev.md`         | 320           | Level 2 (stub) + Level 3 (skills) | Extract React patterns and state management into skills. Stub stays at ~100 lines.                                                        |
| `api-conventions.md`      | 60            | Level 3                           | Relevant only when editing `src/api/`.                                                                                                    |
| `auth-security.md`        | 45            | Level 3                           | Relevant when editing `src/auth/` and `src/api/`.                                                                                         |
| `code-style.md`           | 30            | Level 3 (unscoped)                | Applies to all source files — keep unscoped but at Level 3 (loaded by CC for all `src/` paths).                                           |
| `communication.md`        | 25            | Level 3 (unscoped)                | Applies to all interactions — keep unscoped.                                                                                              |
| `database-patterns.md`    | 55            | Level 3                           | Relevant only when editing `src/db/`.                                                                                                     |
| `deployment-checklist.md` | 40            | Level 3                           | Relevant only when editing `src/deploy/`.                                                                                                 |
| `error-handling.md`       | 35            | Level 3                           | Relevant when editing `src/api/` and `src/auth/`.                                                                                         |
| `git-workflow.md`         | 20            | Level 3 (unscoped)                | Applies to all git operations — keep unscoped.                                                                                            |
| `graphql-schema.md`       | 70            | Level 3                           | Relevant only when editing `src/api/` (GraphQL resolvers live here).                                                                      |
| `naming-conventions.md`   | 25            | Level 3 (unscoped)                | Applies to all source files — keep unscoped.                                                                                              |
| `react-patterns.md`       | 65            | Level 3                           | Relevant only when editing `src/components/`.                                                                                             |
| `rest-api-design.md`      | 50            | Level 3                           | Relevant only when editing `src/api/`.                                                                                                    |
| `state-management.md`     | 55            | Level 3                           | Relevant only when editing `src/components/`.                                                                                             |
| `testing-policy.md`       | 40            | Level 3                           | Relevant when editing `tests/` and any `src/` directory.                                                                                  |
| `zero-tolerance.md`       | 30            | Level 3 (unscoped)                | Universal policy — keep unscoped.                                                                                                         |

**2. `paths:` frontmatter for scoped rules:**

```yaml
# api-conventions.md
---
paths:
  - src/api/**
---
# auth-security.md
---
paths:
  - src/auth/**
  - src/api/**
---
# database-patterns.md
---
paths:
  - src/db/**
---
# deployment-checklist.md
---
paths:
  - src/deploy/**
---
# error-handling.md
---
paths:
  - src/api/**
  - src/auth/**
---
# graphql-schema.md
---
paths:
  - src/api/**
---
# react-patterns.md
---
paths:
  - src/components/**
---
# rest-api-design.md
---
paths:
  - src/api/**
---
# state-management.md
---
paths:
  - src/components/**
---
# testing-policy.md
---
paths:
  - tests/**
  - src/**
---
```

Rules left unscoped (universal): `code-style.md`, `communication.md`, `git-workflow.md`, `naming-conventions.md`, `zero-tolerance.md`.

**3. CLAUDE.md restatement audit:**

| CLAUDE.md Section            | Restates                                | Action                                                                                                                                                |
| ---------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| §Code Style (3 lines)        | `rules/code-style.md` verbatim          | **Remove entirely.** The rule file loads for all `src/` work. CLAUDE.md does not need to restate indentation and line-length settings.                |
| §Error Handling (3 lines)    | `rules/error-handling.md` verbatim      | **Remove entirely.** The rule file loads when editing `src/api/` and `src/auth/`, which are the only places error handling matters.                   |
| §Deployment (2 lines)        | `rules/deployment-checklist.md` content | **Compress to pointer:** "See `rules/deployment-checklist.md` before any release." Or remove if the `paths:` scoping on deploy is sufficient.         |
| §Absolute Directives, item 1 | `rules/api-conventions.md`              | **Keep as one-line pointer.** The directive "All API endpoints must use REST conventions" is appropriate for Level 1. The details belong in the rule. |
| §Absolute Directives, item 4 | `rules/naming-conventions.md`           | **Keep as one-line pointer.** Same pattern — the directive belongs in CLAUDE.md, the details in the rule.                                             |
| §Absolute Directives, item 5 | `rules/zero-tolerance.md`               | **Keep as one-line pointer.** The zero-tolerance principle is a master directive; the enumeration of what counts as a stub belongs in the rule.       |

After removing restatements and compressing pointers, CLAUDE.md drops from 85 lines to ~35 lines.

**4. Agent extraction:**

**`backend-dev.md` (480 -> ~120 lines):**

Extracted skills:

- `skills/database-guide.md` (~80 lines) — database patterns, ORM usage, migration guidance currently embedded in the agent body
- `skills/api-design-guide.md` (~90 lines) — API endpoint design patterns, request/response structures
- `skills/backend-testing.md` (~70 lines) — backend-specific testing patterns and fixture guidance

Stub agent retains: role description, when to invoke, which skills to load, decision-making heuristics (~120 lines).

**`frontend-dev.md` (320 -> ~100 lines):**

Extracted skills:

- `skills/react-component-guide.md` (~75 lines) — component composition patterns, hook usage
- `skills/state-management-guide.md` (~65 lines) — state architecture, store patterns

Stub agent retains: role description, when to invoke, which skills to load, component vs page decision heuristics (~100 lines).

**5. Token budget calculation:**

**Before (all always-loaded):**

| Artifact                | Lines     | Tokens (~4/line) |
| ----------------------- | --------- | ---------------- |
| CLAUDE.md               | 85        | 340              |
| backend-dev.md          | 480       | 1,920            |
| frontend-dev.md         | 320       | 1,280            |
| 15 rules (all unscoped) | 645       | 2,580            |
| **Total always-loaded** | **1,530** | **~6,120**       |

**After (progressive disclosure applied):**

| Artifact                | Lines   | Tokens (~4/line) |
| ----------------------- | ------- | ---------------- |
| CLAUDE.md (trimmed)     | 35      | 140              |
| backend-dev stub        | 120     | 480              |
| frontend-dev stub       | 100     | 400              |
| 5 unscoped rules        | 130     | 520              |
| **Total always-loaded** | **385** | **~1,540**       |

**Reduction: 1,530 lines -> 385 lines (75% reduction). ~6,120 tokens -> ~1,540 tokens.**

The remaining 1,145 lines (10 scoped rules + 5 extracted skills) load only when the practitioner is working in the relevant directory. A session editing `src/components/` loads the 5 unscoped rules (130 lines) plus `react-patterns.md` (65) and `state-management.md` (55) — a total of 250 lines on top of the always-loaded set, far less than the previous 1,530.

## Scoring

| Criterion                                                                                                                    | Points |
| ---------------------------------------------------------------------------------------------------------------------------- | ------ |
| All 18 artifacts classified into the correct level (no domain rules at Level 1, no universal rules scoped to specific paths) | 2      |
| `paths:` frontmatter written for at least 8 of the 10 scopeable rules with correct directory mappings                        | 2      |
| All 3 CLAUDE.md restatements identified with correct source rule                                                             | 1      |
| At least one agent extracted into stub + skills with the stub under 150 lines                                                | 2      |
| Before token count calculated (within 20% of ~6,120)                                                                         | 1      |
| After token count calculated (within 20% of ~1,540) showing measurable reduction                                             | 1      |
| No institutional knowledge removed — all content is relocated, not deleted                                                   | 1      |
| **Total**                                                                                                                    | **10** |

## Extensions

- **Variant A (classification conflict):** Add `auth-security.md` to the scenario with a twist: it contains both a universal principle ("never store passwords in plaintext") and a module-specific pattern ("use the `src/auth/jwt.py` helper for token generation"). The practitioner must split the rule into two files — a universal principle (unscoped, Level 3) and a module-specific pattern (scoped to `src/auth/`). This tests whether the practitioner handles rules that span two levels rather than forcing the entire file into one.
- **Variant B (token ablation data):** Give the practitioner the KEEP/COMPRESS/REMOVE/DEMOTE classification data from a real token ablation experiment. For each of the 15 rules, the experiment records whether removing the rule caused the model to violate it. The practitioner must reconcile the ablation data with their progressive disclosure classification: a rule classified as REMOVE (model follows it without seeing it) can be demoted to Level 4 or deleted entirely, while a rule classified as KEEP (model violates it when removed) must stay at Level 3 or be promoted to a hook. This connects to SC-P-002 (empirical model-vs-rule ablation).
