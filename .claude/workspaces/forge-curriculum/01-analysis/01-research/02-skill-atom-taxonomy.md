---
source: analyst (skill-atom taxonomy lens)
date: 2026-04-07
session: forge-curriculum reframe (craft school)
destination: lyceum/programs/forge (curriculum spine)
status: pre-rigor-bar — do not use as curriculum source
---

# Skill-Atom Taxonomy for COC Practitioner Craft

The journals describe craft, not concepts. Each atom below is grounded in at least one observable journal moment.

## (a) Skill atoms for the six user-named areas, plus a seventh derived from the journals

### 1. How to prompt

| #   | Atom                         | One-sentence description                                                                                                             | Evidence                                                  |
| --- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| 1.1 | Specify-the-isolation        | Frame the prompt so the model has _no_ unstated context to lean on (clean dir, empty git, no rules) when you want a baseline answer. | 0049 Phase 2 clean-room ablation                          |
| 1.2 | Avoid the "act vanilla" trap | Never ask an in-session agent to roleplay as a different version of itself; it overcorrects in both directions.                      | 0049 Phase 1 contamination                                |
| 1.3 | Anchor with required-tools   | State the tools the agent must have available _before_ the task, not after, to convert silent degradation into an explicit error.    | 0034 Finding 7                                            |
| 1.4 | Ask for the contradictions   | End complex prompts with "what would invalidate this?" rather than "is this right?"                                                  | 0034 "For Discussion" sections; recurring across journals |
| 1.5 | Scope by paths, not prose    | When constraining behavior, attach a path glob, not a sentence; prose constraints are silently ignored under load.                   | 0003 H1 (paths: vs prose)                                 |
| 1.6 | Compress with examples kept  | When token-trimming a prompt, drop the _explanation_ before you drop the _example_; examples carry more behavior per token.          | 0049 §"Compression risk"                                  |

### 2. How codegen behaves

| #   | Atom                          | Description                                                                                                           | Evidence                                   |
| --- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| 2.1 | Read the model's native floor | Know which behaviors a given model produces _without_ rules so you don't waste budget enforcing what it already does. | 0049 §2 Claude baseline findings           |
| 2.2 | Spot tool-discoverable facts  | Recognize when the model can recover a fact via tools (PyPI lookup, file read) vs when context must be pre-loaded.    | 0049 §5 terrene-naming                     |
| 2.3 | Detect overcorrection         | Notice when an agent's answer is too confidently in _either_ direction — both signal contamination, not correctness.  | 0049 Phase 1                               |
| 2.4 | Recognize silent fallback     | Catch the moment a model reports "0 tools available" instead of "tool X missing" — these are different failures.      | 0034 Finding 7; 0017 §2 stderr-not-context |
| 2.5 | Track the moving target       | Know when an agent's "introspection" is reading a registry that is _itself_ being refactored this session.            | 0034 Finding 3                             |
| 2.6 | Read the cross-model gap      | Know which rules a _weaker_ model needs that a stronger one doesn't (plaintext password compare, hardcoded keys).     | 0049 §3 MiniMax findings                   |

### 3. How to intervene

| #   | Atom                         | Description                                                                                                                      | Evidence                                                          |
| --- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| 3.1 | Convert exec to execFile     | Replace shell-interpreted calls with argument-list calls the moment you see user-controlled data flow into them.                 | 0050 resolution                                                   |
| 3.2 | Split the long-pole          | Break a single multi-session blocker into a fast-unblock half and a parallel-completion half.                                    | 0034 B0a/B0b split                                                |
| 3.3 | Insert a gate, not a check   | When integration is implicit, add a gating workspace, not another assertion in an existing one.                                  | 0034 Finding 2 (WS-4.5)                                           |
| 3.4 | Promote prose to frontmatter | When a rule keeps getting violated, move its scope from the body text into machine-readable frontmatter.                         | 0003 H1, H2                                                       |
| 3.5 | Demote noisy rules           | Move a rule from always-loaded to on-demand once data shows it doesn't change behavior.                                          | 0049 "DEMOTE to on-demand"                                        |
| 3.6 | Strip the upstream path      | When a downstream repo is producing artifacts that have no upstream destination, gate the producing step at the repo-type level. | 0046                                                              |
| 3.7 | Fix-in-the-run               | When you find a pre-existing failure mid-task, fix it now with a regression test rather than file it for later.                  | rules/zero-tolerance.md Rule 1; recurring posture across journals |

### 4. When to intervene

| #   | Atom                                            | Description                                                                                                                | Evidence                                 |
| --- | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| 4.1 | At the contamination signal                     | Intervene the moment an agent's answer is "too vanilla" or "too compliant"; do not let the run continue.                   | 0049 Phase 1                             |
| 4.2 | Before the critical-path slip cascades          | Intervene on a long-pole estimate the moment it contradicts a deeper analysis, not after the slip.                         | 0034 Finding 1                           |
| 4.3 | Before convergence on the wrong premise         | Stop and audit when a TODO is based on a flawed framing (e.g., "204 orphan skills" that aren't orphans).                   | 0011 §TODO-3.1 correction                |
| 4.4 | When metrics look healthy but outputs are noise | Intervene when 4,579 observations produce 8 duplicate instincts — the pipeline is structurally fine and functionally dead. | 0004                                     |
| 4.5 | When a "fallback" is becoming permanent         | Intervene the moment a temporary workaround starts accumulating dependents.                                                | 0034 Finding 6                           |
| 4.6 | At the silent-divergence threshold              | Intervene when artifacts have drifted past ~15% shared — manual reconciliation no longer scales.                           | 0014 (85% drift); 0020 (skill numbering) |

### 5. How to pay attention

| #   | Atom                              | Description                                                                                                        | Evidence                           |
| --- | --------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ---------------------------------- |
| 5.1 | Inventory before judgment         | Count files, lines, and rule scopes across repos before claiming anything is "wrong."                              | 0003 inventory table               |
| 5.2 | Diff the deep against the shallow | Read the deep analysis and the summary side-by-side; compression hides the cost.                                   | 0034 Finding 1 (7 sessions vs 2-3) |
| 5.3 | Watch the seams, not the surfaces | Test where two domains meet (DataFlow ↔ Nexus, Kaizen ↔ MCP), not inside either.                                   | 0034 Finding 2                     |
| 5.4 | Read what stderr _does_           | Know which channels reach the model and which only reach the terminal — they look identical but behave oppositely. | 0017 §2                            |
| 5.5 | Read for absence                  | Look for what is _not_ in a manifest, not just what is — orphans are invisible by definition.                      | 0028 Finding 1                     |
| 5.6 | Trace the authority chain         | When an artifact appears in the wrong place, trace upstream until you find the missing repo-type guard.            | 0046                               |

### 6. When to pay attention

| #   | Atom                               | Description                                                                                                                            | Evidence                      |
| --- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------- |
| 6.1 | At every cold start                | Pay attention on session-start because that's where continuity silently breaks.                                                        | 0017                          |
| 6.2 | When numbers contradict each other | Pay attention when two parts of the same plan give different estimates for the same item.                                              | 0034 Cross-Reference Audit    |
| 6.3 | When confidence is hardcoded       | Pay attention when every record reports the same confidence (0.9) — that's a missing computation, not consensus.                       | 0004                          |
| 6.4 | Right after a refactor lands       | Pay attention to introspecting tools immediately after a registry restructure — they go stale within the same session.                 | 0034 Finding 3                |
| 6.5 | When a rule was synced unchanged   | Pay attention the first time a BUILD-repo rule appears in a USE repo; the wording leaks.                                               | 0003 §"Currently WRONG"; 0046 |
| 6.6 | At the second occurrence           | Pay attention the second time you see the same drift class (skill numbering, manifest orphans) — once is incident, twice is structure. | 0014, 0020, 0028              |

## (b) The seventh area — derived from the journals, not in the user's original list

The six cover the _cognitive_ loop (perceive → decide → act on the model). The journals reveal at least one craft area the six don't capture: **how to keep institutional memory across sessions** — a _tooling/environment_ craft, distinct from prompting or watching.

### 7. How to keep institutional memory

| #   | Atom                               | Description                                                                                             | Evidence                           |
| --- | ---------------------------------- | ------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| 7.1 | Inject through the right channel   | Know that SessionStart stderr does not reach the model; only UserPromptSubmit does.                     | 0017 §2-3                          |
| 7.2 | Search both root and workspace     | Find session notes in both the repo root and workspaces/, not whichever the active workspace points to. | 0017 fix                           |
| 7.3 | Write notes addressed to next-you  | Write session notes as instructions the next session must follow, not as a log of what happened.        | 0017 impact                        |
| 7.4 | Version the institutional artifact | Stamp `.claude/VERSION` so drift becomes detectable, not invisible.                                     | 0028; CLAUDE.md "Version tracking" |
| 7.5 | Validate before sync, not after    | Run manifest parity checks pre-sync; post-sync detection is too late.                                   | 0028 residual risk                 |

## (c) Concept vs skill atom — the distinction

| Concept (describes)                                                           | Skill atom (does)                                                                                                                                                              |
| ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Constraint Envelope** — the bounded set of operations an agent may perform. | **Collapse the envelope mid-session** — when an agent starts proposing out-of-scope work, narrow the allowed file paths in the active rule before the next prompt. (3.4 + 4.1) |
| **Contamination** — residual context bleeding between agent calls.            | **Run the clean-room** — spawn `claude -p` in `/tmp/empty-dir/` to get a true baseline answer. (1.1, 0049 Phase 2)                                                             |
| **Authority chain** — direction of artifact flow from BUILD to USE.           | **Strip the upstream path on detection** — add a runtime repo-type check to a command so downstream invocations skip the upstream-only step. (3.6, 0046)                       |

A concept tells you what something _is_. A skill atom tells you what to _do_, in what _moment_, on what _signal_, with what _expected effect_. Skill atoms are drillable; concepts are not.

## Source files referenced

- `/Users/esperie/repos/loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md`
- `/Users/esperie/repos/loom/journal/0050-DISCOVERY-session-start-command-injection.md`
- `/Users/esperie/repos/loom/journal/0017-DISCOVERY-session-notes-broken.md`
- `/Users/esperie/repos/loom/journal/0014-DISCOVERY-drift-audit.md`
- `/Users/esperie/repos/loom/journal/0011-RISK-redteam-r1-findings.md`
- `/Users/esperie/repos/loom/journal/0034-RISK-stack-gaps-redteam-attack.md`
- `/Users/esperie/repos/loom/journal/0046-DECISION-downstream-proposal-guard.md`
- `/Users/esperie/repos/loom/journal/0003-DISCOVERY-cc-artifact-audit.md`
- `/Users/esperie/repos/loom/journal/0004-RISK-learning-system-broken.md`
- `/Users/esperie/repos/loom/journal/0028-RISK-manifest-integrity-gaps.md`
- `/Users/esperie/repos/loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md`
