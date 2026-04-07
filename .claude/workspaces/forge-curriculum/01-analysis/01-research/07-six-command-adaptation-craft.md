---
source: analyst (workflow adaptation lens)
date: 2026-04-07
session: forge-curriculum reframe (craft school)
destination: atelier/co-codegen (canonical 6-command substance) + lyceum/programs/forge (curriculum that teaches when/how to adapt)
status: pre-rigor-bar — do not use as curriculum source
live findings:
  - kz-engage codify Step 7 still says MANDATORY (post-0046 fix never re-synced) — flagged for loom, not for forge to fix
  - forge itself carries the full 6-command chain despite being a curriculum repo, not a code repo — the lyceum move is the planned cure
---

# The 6-Command Adaptation Craft

## 1. Invariant Substance

| # | Command | Core substance (what MUST happen, regardless of name) | In | Out |
|---|---|---|---|---|
| 01 | **/analyze** | Convert a `briefs/` intent into structured first-principles understanding: failure points, requirements, plans, user flows. Red team until convergence. No code yet. | `workspaces/<p>/briefs/` | `01-analysis/`, `02-plans/`, `03-user-flows/`, journal DISCOVERY/GAP entries |
| 02 | **/todos** | Decompose plans into every executable task (Build *and* Wire as separate todos), then **structurally stop** for human approval. The only mandatory human gate in the loop. | `02-plans/` | `todos/active/` + STOP |
| 03 | **/implement** | Re-anchor on plan text per todo, execute test-first with framework specialists, verify wiring against spec, move active→completed with a written verification record. Loop until empty. | `todos/active/` | `src/`, `apps/`, `.test-results`, `todos/completed/` |
| 04 | **/redteam** | Spec-coverage audit first (what was specified vs what was built/wired), then end-to-end + user-flow validation by adversarial agents. Converge on 0 critical/high + 100% spec coverage + 0 mock data. | `02-plans/`, `03-user-flows/`, codebase | `04-validate/`, `.spec-coverage`, RISK/GAP journals |
| 05 | **/codify** | Pull L5 learning artifacts and validated patterns into canonical agents/skills/rules. Closes the observe→evolve→codify loop. Repo-type detection determines whether output is local-only or proposal-flow. | `04-validate/`, `.claude/learning/`, `docs/` | Updated `agents/`, `skills/`, `rules/`; (BUILD only) `.claude/.proposals/latest.yaml` |
| 06 | **/release** \| **/deploy** | Ship the artifact across the BUILD/USE boundary with version atomicity, security gate, and rollback runbook. Standalone — runs after N implement/redteam cycles, not per-workspace. | `deploy/deployment-config.md`, last release tag | Tagged release, published artifact, post-release checklist |

## 2. Variants Observed

| Repo | /analyze | /todos | /implement | /redteam | /codify | /release \| /deploy | Optimizing for |
|---|---|---|---|---|---|---|---|
| **loom** (canonical) | full | full | full | full | BUILD-only Step 7 (proposal) | `/release` real; `/deploy` is a stub redirector | Source of truth for sync |
| **atelier** | absent | absent | absent | absent | absent | absent | CC+CO authority only — no codegen happens here, so the six phases would be cargo-cult. Keeps `wrapup`, `journal`, `sync-to-coc`, `cc-audit` |
| **aegis** (BUILD-side dev) | clone of loom but uses named agents (`deep-analyst`, `requirements-analyst`, `coc-expert`) instead of loom's generic `analyst` + skill refs | clone | clone | clone | clone with Step 7 still **MANDATORY** (pre-0046) | both present; carries SDK-release scaffolding | Heavy specialist roster, pre-refactor naming |
| **aether** | clone of loom (older agent names) | clone | clone | clone | clone with Step 7 still MANDATORY | both present | Same as aegis |
| **forge** (this repo) | clone of loom | clone | clone | clone | clone with BUILD-only guard | both present but redundant for a curriculum repo | Course delivery — phases used to drive workspaces for *course-module production*, not product code |
| **kz-engage** (downstream USE) | clone | clone | clone | clone | **bug**: Step 7 still MANDATORY despite being downstream — post-0046 fix never re-synced | both | Downstream consumer — should be local-only codify |

## 3. Adaptation Judgment Calls

1. **Are the six phases applicable at all?** If the repo produces *no codegen output* (atelier — pure CC+CO authority), drop the entire 01-06 chain and keep only `wrapup`/`journal`/`sync-*`. Criterion: does the repo have a `workspaces/` dir or generate code?
2. **Is the `analyst` a generic role or a named specialist?** Loom uses generic `analyst` + skill refs (`co-reference`, `decide-framework`); aegis/aether keep named specialists (`deep-analyst`, `requirements-analyst`, `coc-expert`). Criterion: how many concurrent analysis perspectives does the domain need? Software repos collapse to one; multi-disciplinary repos retain named roles.
3. **Does /codify produce upstream proposals or stay local?** Set by repo position in the authority chain (`rules/artifact-flow.md`). BUILD repos = proposal flow; USE/downstream = local-only. Loom journal `0046-DECISION-downstream-proposal-guard.md` is the canonical fix; kz-engage's stale copy is the canonical *failure* of this judgment.
4. **/release vs /deploy split?** Two distinct commands by deliberate convention: `/release` ships the SDK from BUILD repos (PyPI, version atomicity); `/deploy` ships applications from USE repos. Each repo keeps both files, with the inapplicable one as a one-paragraph redirector. Criterion: BUILD repo? `/deploy` is the stub. USE repo? `/release` is the stub.
5. **When does /implement add a phase-specific step?** Loom adds the **Context anchoring** step (re-read plan + journal + todo body) and **spec-verify before close** because of the discovery in `0017-DISCOVERY-session-notes-broken.md` — without explicit re-read, agents implement from amnesia. Any repo whose work spans multiple sessions inherits this; one-shot scripting repos can drop it.
6. **Does /redteam need a parity check?** Step 6 ("if parity with an existing system is required") is conditional in loom and carried verbatim by aegis/aether/forge. Criterion: is there a legacy system being replaced? If no, drop the section to keep the command under the 150-line cc-artifacts limit.
7. **What journal entry types does each phase emit?** Each command's terminal `### Journal` block declares which types are mandatory: analyze→DISCOVERY/GAP/CONNECTION; todos→DECISION/TRADE-OFF/RISK; implement→DECISION/DISCOVERY/RISK; redteam→RISK/GAP/CONNECTION; codify→DECISION/CONNECTION/TRADE-OFF. Adapting a repo means choosing whether to enforce these or relax them. Forge inherited all five sets unchanged.
8. **Approval gate placement.** Loom has exactly one structural gate (`/todos`). Aegis/aether/forge inherit it. A repo could insert a second gate (e.g., before /release in BUILD) but no surveyed repo does — `/release` Step 0 instead does an in-line "STOP and wait for human approval" inside the command itself. Criterion: is the gate a phase boundary or an in-command stop?

## 4. Fault Lines (where adaptation goes wrong)

- **Drift through neglect** — `0014-DISCOVERY-drift-audit.md`: only 15% of artifacts were truly shared between py and rs templates; 66% of skills had drifted because BUILD repos synced directly to templates, bypassing alignment. Adaptation without a sync-manifest produces silent divergence.
- **Stale guards** — `0046-DECISION-downstream-proposal-guard.md` fixed the missing BUILD-only guard in `/codify` Step 7. Kz-engage's `commands/codify.md` (lines 69, 132) still says "MANDATORY" with no detection logic — a downstream repo will create a `.proposals/latest.yaml` that has nowhere to go. The adaptation craft requires re-syncing on every upstream phase change.
- **Continuity invisibility** — `0017-DISCOVERY-session-notes-broken.md`: the anti-amnesia mechanism was silently broken because session notes lived at repo root but `detectActiveWorkspace()` only looked under `workspaces/`. Phases assume continuity that the hooks may not actually deliver — adapting `/implement` without verifying its session-notes injection path is the canonical "phase looks right, runs blind" failure.
- **Identity drift** — `0001-DECISION-co-refocusing.md`: loom itself was originally framed as a coding environment until it was repositioned as artifact management. The phases were preserved (for sync) but the *repo's own* `/analyze`-`/release` chain became inapplicable. Atelier represents the correct destination of this judgment — phases removed entirely. **Forge has not yet made this call**: it carries all six commands despite being a curriculum repo where most "implementation" is course content production. The lyceum move is the planned cure.

## Files cited

- `/Users/esperie/repos/loom/.claude/commands/{analyze,todos,implement,redteam,codify,release,deploy}.md`
- `/Users/esperie/repos/atelier/.claude/commands/` (only 7 files, no phase commands)
- `/Users/esperie/repos/dev/aegis/.claude/commands/{analyze,codify}.md`
- `/Users/esperie/repos/dev/aether/.claude/commands/{analyze,codify}.md`
- `/Users/esperie/repos/training/forge/.claude/commands/{analyze,codify}.md`
- `/Users/esperie/repos/loom/kz-engage/.claude/commands/codify.md`
- `/Users/esperie/repos/loom/journal/0001-DECISION-co-refocusing.md`
- `/Users/esperie/repos/loom/journal/0014-DISCOVERY-drift-audit.md`
- `/Users/esperie/repos/loom/journal/0017-DISCOVERY-session-notes-broken.md`
- `/Users/esperie/repos/loom/journal/0046-DECISION-downstream-proposal-guard.md`
