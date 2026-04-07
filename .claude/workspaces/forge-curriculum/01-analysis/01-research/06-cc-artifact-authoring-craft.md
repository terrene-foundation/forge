---
source: claude-code-architect
date: 2026-04-07
session: forge-curriculum reframe (craft school)
destination: atelier/co-codegen (universal CC artifact patterns) + lyceum/programs/forge (curriculum that drills practitioners on them)
status: pre-rigor-bar — do not use as curriculum source
---

# CC Artifact Authoring Craft — Distilled

## 1. The Four Quality Dimensions

Source: `loom/.claude/commands/cc-audit.md` lines 20-25; `atelier/.claude/commands/cc-audit.md` lines 18-25.

| Dimension            | Concrete measure                                                                            | Healthy signal                                                                       | Failure mode if ignored                                                                           | Evidence file                                                                                                                                |
| -------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Competency**       | Instructions precise enough that the artifact completes its job without clarification turns | Numbered "Critical Rules" + quantified targets ("not 'fast' but '<100ms'")           | Artifact "knows about" a domain but cannot finish a task — re-asks the user every session         | `loom/.claude/agents/analysis/analyst.md` lines 13-19 (six numbered critical rules)                                                          |
| **Completeness**     | Edge cases enumerated; handoffs to other artifacts named; cross-refs resolve                | "Related Agents" + "Skill References" sections; "Common Gotchas" list                | Dead handoffs ("see X" where X was deleted); silent gaps that surface as hallucination            | `loom/.claude/agents/frameworks/dataflow-specialist.md` lines 59-67 (7 named gotchas) and lines 92-102 (Related Agents + Full Documentation) |
| **Effectiveness**    | Output format specified verbatim; behavior reliably reproducible across sessions            | Explicit `## Output Format` block with template                                      | Each session formats output differently, breaking downstream parsers and review workflows         | `loom/.claude/agents/quality/reviewer.md` lines 73-93 (literal Markdown output template)                                                     |
| **Token Efficiency** | Path-scoped, no CLAUDE.md duplication, descriptions short, ≤ line caps                      | Frontmatter `paths:` present on domain rules; ≤ 400 line agents; ≤ 150 line commands | ~1,300 lines/turn wasted from unscoped rules — measured 48% per-turn baseline reduction available | `loom/journal/0003-DISCOVERY-cc-artifact-audit.md` lines 60-74 (H1 finding)                                                                  |

## 2. Hard Limits and Why They Exist

All limits sourced from `loom/.claude/rules/cc-artifacts.md` and `atelier/.claude/rules/cc-artifacts.md`.

| #   | Limit                                                                   | Why (failure mode prevented)                                                                                                              | Evidence                                     |
| --- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| 1   | Agent description ≤ 120 chars, must contain "Use when/for…" trigger     | Description loads on every selection decision; long ones tax every turn (worst found: kaizen-specialist at 432 chars)                     | cc-artifacts.md L12-24; journal/0003 C1      |
| 2   | Agent file ≤ 400 lines                                                  | Whole agent loads on delegation; 600-line agents crowd out the actual task. Worst: kailash-rs kaizen-specialist 664 lines                 | cc-artifacts.md L171-186; journal/0003 C2    |
| 3   | Command file ≤ 150 lines                                                | Commands inject as user messages and compete with the actual user intent for token budget                                                 | cc-artifacts.md L87-91                       |
| 4   | CLAUDE.md ≤ 200 lines, 3-5 directives, navigation only                  | CLAUDE.md loads on every turn; restating rules doubles cost (worst: kailash-rs CLAUDE.md 679 lines with embedded crate docs)              | cc-artifacts.md L93-123; journal/0003 C5     |
| 5   | Every MUST/MUST NOT rule has a `**Why:**` line                          | Without rationale, Claude obeys the letter and breaks the spirit in edge cases                                                            | cc-artifacts.md L81-85                       |
| 6   | Every MUST rule has a DO / DO NOT code example                          | Without examples Claude reinterprets the rule each session; examples anchor consistent behavior                                           | cc-artifacts.md L52-77                       |
| 7   | Domain rules use YAML frontmatter `paths:` (not `globs:`)               | `globs:` is not a recognized frontmatter key — the rule then loads on every turn instead of being scoped                                  | cc-artifacts.md L125-143; journal/0003 H2    |
| 8   | SKILL.md answers 80% of routine questions without sub-file reads        | Otherwise basic answers cost 5 unnecessary tool calls                                                                                     | cc-artifacts.md L32-50                       |
| 9   | Skills/rules MUST NOT duplicate CLAUDE.md content                       | CLAUDE.md is always loaded — duplication doubles tokens for zero benefit                                                                  | cc-artifacts.md L187-191                     |
| 10  | Hooks must include a `setTimeout` fallback returning `{continue: true}` | A hanging hook blocks the entire CC session indefinitely (6 hooks were missing this in all 4 BUILD/USE repos)                             | cc-artifacts.md L151-167; journal/0003 H3    |
| 11  | Hooks check structure only — no semantic regex                          | Regex semantic analysis is slow + brittle, produces spurious blocks                                                                       | cc-artifacts.md L210-216                     |
| 12  | No dangling cross-references after extraction (grep before delete)      | Dangling refs cause file-not-found errors and force Claude to fabricate replacements (RT3-1 bug: 3 agent files referenced deleted skills) | cc-artifacts.md L237-250; journal/0011 RT3-1 |

## 3. Recurring Micro-Patterns (the moves that make artifacts work)

| Pattern                              | What it looks like                                        | Where it works                 | Evidence file                                                                                           |
| ------------------------------------ | --------------------------------------------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------- |
| **Trigger-phrase description**       | `description: "X specialist. Use for A, B, C."`           | All loom specialists           | `loom/.claude/agents/frameworks/dataflow-specialist.md` line 3                                          |
| **Path-scoping frontmatter**         | `paths: ["**/db/**", "migrations/**"]` at top of rule     | Domain rules                   | `loom/.claude/rules/cc-artifacts.md` L2-8; `loom/.claude/rules/journal.md` L2-5                         |
| **DO / DO NOT pair**                 | Two adjacent code blocks, never just one                  | Every MUST rule                | `loom/.claude/rules/cc-artifacts.md` L16-22; `forge/.claude/rules/security.md` (DO/DO NOT for SQL)      |
| **`**Why:**` one-liner**             | Single italicized line under each rule                    | Every rule across loom/atelier | `forge/.claude/rules/zero-tolerance.md` (every rule); `loom/.claude/rules/cc-artifacts.md` (every rule) |
| **Skills Quick Reference table**     | Agent points "question → file" before any narrative       | Specialist agents              | `loom/.claude/agents/frameworks/dataflow-specialist.md` L20-45                                          |
| **Output Format block**              | Verbatim markdown skeleton at agent's end                 | Quality + analysis agents      | `loom/.claude/agents/quality/reviewer.md` L73-93; `loom/.claude/agents/analysis/analyst.md` L96-122     |
| **Related Agents handoff list**      | Bulleted "X — escalate Y" near the end                    | Every loom agent               | `loom/.claude/agents/quality/reviewer.md` L101-106                                                      |
| **Critical Rules — numbered, terse** | 5-7 numbered imperatives at the top, no prose             | Analysis/specialist agents     | `loom/.claude/agents/analysis/analyst.md` L13-19                                                        |
| **Quick-Reference matrix table**     | Layer/Need/API matrix near top of artifact                | Skills + specialists           | `loom/.claude/skills/30-claude-code-patterns/SKILL.md` L17-23 (artifact-type matrix)                    |
| **Progressive disclosure index**     | SKILL.md = quick ref + matrix; sub-files linked by topic  | Numbered skill dirs            | `loom/.claude/skills/02-dataflow/SKILL.md` L74-80 (Reference Documentation links)                       |
| **MUST / MUST NOT split**            | Two h2 sections, both populated, both with examples + Why | All quality rules              | `loom/.claude/rules/cc-artifacts.md` L11-250                                                            |
| **Risk/Issue priority table**        | Critical / Major / Minor with criteria + action columns   | Analysis + reviewer agents     | `loom/.claude/agents/analysis/analyst.md` L33-38; `loom/.claude/agents/quality/reviewer.md` L66-70      |

## 4. Anti-Patterns (named so they can be hunted)

| Anti-pattern                   | What it is                                                                                                                                 | Evidence                                                                                                                                     |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Scope leak**                 | Domain rule with no `paths:` frontmatter — loads every turn                                                                                | journal/0003 H1: 7 rules totaling ~1,300 lines/turn loading globally; 48% baseline reduction available                                       |
| **Knowledge dump in agent**    | Agent file embeds reference material instead of linking a skill                                                                            | journal/0003 C2: kaizen-specialist 664 lines, nexus-specialist 508 lines; cc-artifacts.md MUST NOT 1                                         |
| **CLAUDE.md restatement**      | CLAUDE.md repeats rule content already in `.claude/rules/`                                                                                 | journal/0003 C5: kailash-rs CLAUDE.md 679 lines (4× the 200 max) with embedded docs                                                          |
| **Wrong frontmatter key**      | `globs:` instead of `paths:` — silent failure, rule loads globally                                                                         | journal/0003 H2: present in cc-artifacts.md across all 4 repos                                                                               |
| **Prose-only constraint**      | MUST rule with no DO/DO NOT example — Claude reinterprets each session                                                                     | journal/0011 RT4-1: autonomous-execution.md MUST 2 lacked example until R1                                                                   |
| **Why-less rule**              | MUST/MUST NOT without `**Why:**` line — letter obeyed, spirit broken                                                                       | journal/0011 RT4-2: independence.md MUST NOT 1-3 lacked Why until R1                                                                         |
| **Dead cross-reference**       | Agent points to a skill/file that has been deleted                                                                                         | journal/0011 RT3-1: 3 coc-claude-rs agents pointed to deleted 10-governance, 32-kaizen-agents                                                |
| **Hanging hook**               | Hook with no `setTimeout` fallback — can block whole session                                                                               | journal/0003 H3: 6 hooks missing timeout in all 4 repos (auto-format, detect-package-manager, pre-compact, session-end, session-start, stop) |
| **Semantic-regex hook**        | Hook attempting meaning analysis via regex — false positives block work                                                                    | cc-artifacts.md MUST NOT 4                                                                                                                   |
| **BUILD-in-USE contamination** | Repo-specific artifact (cross-sdk-inspection, frontend agents in BUILD repos) shipped where it has no referent                             | cc-artifacts.md MUST NOT 5; journal/0003 "Currently WRONG" table                                                                             |
| **Long agent description**     | Description > 120 chars steals tokens on every selection                                                                                   | journal/0003 C1: kaizen-specialist 432, coc-expert 332, care-expert 327                                                                      |
| **Orphaned-skill panic**       | Treating numbered skill dirs as "orphans" because nothing imports them — they are discovered by SKILL.md trigger descriptions, not by name | journal/0011 "Correction: TODO-3.1 — INVALID"                                                                                                |

---

## Evidence files (absolute paths)

- `/Users/esperie/repos/atelier/.claude/commands/cc-audit.md`
- `/Users/esperie/repos/loom/.claude/commands/cc-audit.md`
- `/Users/esperie/repos/atelier/.claude/rules/cc-artifacts.md`
- `/Users/esperie/repos/loom/.claude/rules/cc-artifacts.md`
- `/Users/esperie/repos/loom/.claude/rules/journal.md`
- `/Users/esperie/repos/loom/.claude/rules/agents.md`
- `/Users/esperie/repos/loom/.claude/rules/communication.md`
- `/Users/esperie/repos/loom/.claude/agents/frameworks/dataflow-specialist.md`
- `/Users/esperie/repos/loom/.claude/agents/quality/reviewer.md`
- `/Users/esperie/repos/loom/.claude/agents/analysis/analyst.md`
- `/Users/esperie/repos/loom/.claude/skills/30-claude-code-patterns/SKILL.md`
- `/Users/esperie/repos/loom/.claude/skills/02-dataflow/SKILL.md`
- `/Users/esperie/repos/loom/journal/0003-DISCOVERY-cc-artifact-audit.md`
- `/Users/esperie/repos/loom/journal/0011-RISK-redteam-r1-findings.md`
- `/Users/esperie/repos/loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md`
