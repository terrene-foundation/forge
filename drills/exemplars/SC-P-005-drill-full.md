---
drill_id: DR-SC-P-005-FULL
source_atom: SC-P-005
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Diagnose Friction-Gradient Inversions Where the Best API Has the Worst Discoverability

## Setup

The practitioner receives a module directory for an AI agent framework (`kaizen/`) containing three API entry points at different abstraction levels, plus the module's COC artifacts.

**Entry Point 1 — `BaseAgent` (low-level primitive):**

File: `kaizen/base_agent.py` (220 lines). Requires the developer to subclass, override `execute()`, manually register tools, manage conversation state, and handle errors. A working agent takes 60+ lines of boilerplate. Fully functional — all tests pass.

**Entry Point 2 — `Agent` (engine-level API):**

File: `kaizen/agent.py` (180 lines). Accepts a `tools=` parameter in the constructor and presents a 6-line quick-start. However, the `tools=` parameter is silently ignored — tools passed at construction are never registered in the execution loop. The agent runs without error but fabricates success responses when tool calls fail. 3 critical bugs:

1. `tools=` parameter accepted but never wired to the tool registry
2. Tool execution failures return `{"status": "ok", "result": None}` instead of raising
3. `on_error` callback is defined in the signature but never invoked

**Entry Point 3 — `Delegate` (high-level engine):**

File: `kaizen/engines/delegate.py` (90 lines). Two-line agent creation: `agent = Delegate(name="analyst", instructions="...", tools=[...])`. Tools are correctly registered, errors propagate, conversation state is managed automatically. All tests pass. Superior to both BaseAgent and Agent in every dimension except discoverability.

**COC artifacts:**

- `CLAUDE.md` (project root): Lists "kaizen" in the framework table. Says "Use `Agent` for AI agent work" in the quick-start section. Does not mention `Delegate`.
- `.claude/skills/agent-creation.md`: Documents the `Agent` API with a 6-line example. Does not mention `BaseAgent` or `Delegate`.
- `.claude/rules/framework-first.md`: Says "Use Kaizen for agent work." Does not specify which Kaizen API.
- `patterns.md` (in module root): Shows `Agent(tools=[...])` as the primary pattern. 6 lines. Does not mention `Delegate`. Mentions `BaseAgent` as "advanced usage for custom execution loops."
- `kaizen/engines/README.md`: Documents `Delegate` with full API reference. Located 2 directories deep. Not linked from any higher-level document.

## Task

1. Map the friction gradient. For each of the three entry points, record two measurements:
   - **Discovery cost**: How many documents/clicks from `CLAUDE.md` to find this API? Is it in the quick-start, in a skill file, in a subdirectory README, or undocumented at the top level?
   - **Usage quality**: Does it actually work? How many lines for a working agent? Does it handle errors correctly? Rate as HIGH / MEDIUM / LOW with a one-sentence justification.
2. Identify the inversion: which entry point has the worst discoverability-to-quality ratio? State the inversion as a single sentence: "The [best/worst] API is the [easiest/hardest] to discover."
3. Trace the root cause through COC layers. For each of the five COC layers (Intent, Context, Guardrails, Instructions, Learning), state whether that layer contributes to the inversion persisting. Identify the primary layer responsible.
4. Write a specific intervention: name the exact COC artifact (file path), the exact change (what to add, remove, or modify), and why this change corrects the gradient. The intervention must not be "write better documentation" — it must name a file and a change.
5. Explain why fixing the `Agent` bugs alone is insufficient to resolve the inversion, even after the bugs are patched.

## Model Answer

**Step 1 — Friction gradient map:**

| Entry Point | Discovery Cost                                                                                                                                                       | Usage Quality                                                                                                                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Agent`     | LOW (1 hop: CLAUDE.md → quick-start mentions it; skill file documents it; patterns.md shows it)                                                                      | LOW — silently drops tools, fabricates success on failure, never invokes on_error. A developer following the documented pattern gets an agent that appears to work but produces wrong results. |
| `BaseAgent` | MEDIUM (2 hops: patterns.md mentions it as "advanced usage"; not in CLAUDE.md quick-start or skill file)                                                             | MEDIUM — fully functional but requires 60+ lines of boilerplate. Correct but expensive.                                                                                                        |
| `Delegate`  | HIGH (3+ hops: not in CLAUDE.md, not in skill file, not in patterns.md. Only documented in `kaizen/engines/README.md`, 2 directories deep, not linked from anywhere) | HIGH — 2 lines, tools correctly wired, errors propagate, state managed. Best API in the module by every quality metric.                                                                        |

**Step 2 — The inversion:**

"The highest-quality API (`Delegate` — 2 lines, correct behaviour) is the hardest to discover (buried in a subdirectory README with no inbound links), while the lowest-quality API (`Agent` — silently broken) is the easiest to discover (first result in CLAUDE.md, skill file, and patterns.md)."

**Step 3 — COC layer analysis:**

| COC Layer                        | Contributes?       | Explanation                                                                                                                                                                                                                                                                |
| -------------------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Layer 1 — Intent                 | No                 | The intent (build agents) is clear and correct.                                                                                                                                                                                                                            |
| Layer 2 — Context (CLAUDE.md)    | **YES — PRIMARY**  | CLAUDE.md routes developers to `Agent` as the default. This is the authoritative entry point for every session. A developer following CLAUDE.md's instruction will use the broken API and never encounter Delegate.                                                        |
| Layer 3 — Guardrails (rules/)    | Yes — secondary    | `framework-first.md` says "Use Kaizen" but does not specify which Kaizen API. A rule that said "Use `Delegate` for agent creation; `Agent` is deprecated" would prevent the wrong choice.                                                                                  |
| Layer 4 — Instructions (skills/) | Yes — secondary    | `agent-creation.md` documents only the broken `Agent` API. The skill actively teaches the wrong pattern.                                                                                                                                                                   |
| Layer 5 — Learning (codify)      | Yes — root enabler | The codify phase after the `Agent` bugs were discovered never updated the COC artifacts. The bugs were filed and fixed in code, but the COC continued routing developers to the formerly-broken API. The learning layer's failure to update is why the inversion persists. |

Primary responsibility: **Layer 2 (Context)** — because CLAUDE.md is the first document read by every session, and it currently routes to the broken API. Layer 5 (Learning) is the root enabler — the codify pass that should have updated Layer 2 never happened.

**Step 4 — Specific intervention:**

Three changes, in priority order:

1. **`CLAUDE.md`** (Layer 2): In the quick-start section, change "Use `Agent` for AI agent work" to "Use `Delegate` for AI agent work — `from kaizen.engines import Delegate`. The `Agent` class is deprecated (silent tool-dropping bug, see issue #XX)." This corrects the authoritative routing.

2. **`.claude/skills/agent-creation.md`** (Layer 4): Replace the `Agent` example with a `Delegate` example. Add a "Deprecated" warning for `Agent` with a link to the issue. This prevents the skill from teaching the wrong pattern.

3. **`.claude/rules/framework-first.md`** (Layer 3): Add a specific directive: "For agent creation, use `kaizen.engines.Delegate`, NOT `kaizen.Agent`. The `Agent` class has known silent-failure bugs." This adds a guardrail that catches developers who skip the skill file.

**Step 5 — Why bug-fixing alone is insufficient:**

Fixing the three bugs in `Agent` closes the quality gap but does not close the discovery gap. After fixing, `Agent` and `Delegate` would both work correctly, but `Delegate` would still be a 2-line API while `Agent` would still require 6 lines plus understanding its parameter model. More critically, three development teams have already routed around `Agent` to `BaseAgent` or `Delegate` — the reputation damage persists even after the bugs are patched. Unless the COC artifacts are updated to route new developers to `Delegate`, new sessions will still discover `Agent` first (easiest to find), use it (it works now), and write more boilerplate than necessary (6 lines vs 2). The friction gradient flattens but does not invert to the correct orientation. The COC change is required to complete the correction.

## Scoring

| Criterion                                                                             | Points |
| ------------------------------------------------------------------------------------- | ------ |
| All 3 entry points mapped with both discovery cost and usage quality                  | 1      |
| Discovery cost measured in hops from CLAUDE.md, not just "easy/hard"                  | 1      |
| Usage quality assessment of `Agent` identifies silent failure (not just "buggy")      | 1      |
| Inversion stated as a single sentence linking best quality to worst discoverability   | 1      |
| All 5 COC layers assessed (not just "docs are bad")                                   | 1      |
| Primary layer correctly identified as Layer 2 (Context/CLAUDE.md)                     | 1      |
| Intervention names a specific file path and a specific change                         | 1      |
| Intervention targets the COC artifact, not just the code bug                          | 1      |
| Explanation of why bug-fixing alone is insufficient references the discovery gap      | 1      |
| No recommendation reduces to "write better documentation" without naming the artifact | 1      |
| **Total**                                                                             | **10** |

## Extensions

1. **Adoption audit.** Give the practitioner 5 downstream projects that use the kaizen module. For each project, the practitioner must determine which API it uses (BaseAgent, Agent, or Delegate) by reading the import statements. Tally the adoption: if 4 of 5 use BaseAgent and 0 use Delegate, the friction-gradient inversion is confirmed by independent convergence on the suboptimal path. This mirrors the journal evidence where three teams independently converged on BaseAgent.

2. **Post-fix re-measurement.** After the practitioner writes the COC intervention, present a hypothetical new session that follows the updated CLAUDE.md. The practitioner must trace the new developer's path: CLAUDE.md → Delegate → 2-line agent → working. Compare this path to the pre-fix path: CLAUDE.md → Agent → 6-line agent → silent failure → debugging → discover Delegate → rewrite. Measure the saved hops and wasted lines.

3. **Silent-failure detection drill.** Remove the explicit statement that `Agent` has bugs. Give the practitioner only the source code and the COC artifacts. The practitioner must discover the bugs through code reading alone — specifically, that `tools=` is accepted but never wired and that tool failures return success. This tests whether the practitioner treats documented APIs as testable claims (SC-P-009) rather than trusting them.
