---
destination: forge
layer: reverse-index
batch: kaizen-kz
corpus_scope: loom/kaizen-cli-rs + loom/kaizen-cli-py + loom/kz-engage
date: 2026-04-07
entries_processed: 41
---

# Reverse Index — COC Layer — Kaizen CLI + kz-engage

Spec-concept reverse index for the Kaizen CLI (Rust + Python) and kz-engage corpus. Concept citations use `[Spec § N]` form where N is the concept number from the corresponding spec index file (`care-concepts.md`, `eatp-concepts.md`, `co-concepts.md`, `pact-concepts.md`).

## Entries by Repo

### loom/kaizen-cli-rs

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Claude Code v2.1.88 — Complete Internal Architecture (entry point, agent loop, tool system, streaming, context management, sessions, hooks, MCP, slash commands, subagents)
- **Spec concepts touched**: CO § 9 (Layer 1 — Intent / agent specializations), CO § 26 (Layer 4 — Instructions), CO § 19 (Layer 3 — Guardrails), CO § 5.3 (Anti-Amnesia), CO § 14 (Master Directive), CO § 39 (Layer 5 — Learning), EATP § 36 (Graceful Degradation), CARE § 4 (Human-on-the-Loop), CARE § 30 (Constraint Envelopes — Definition)
- **Move type**: reference-implementation read; agent-loop architecture inventory; tool-permission cascade study
- **Evidence polarity**: POSITIVE (reference-grade implementation that the kaizen-cli-rs team is using as a parity target)
- **Key quote**: "Core loop (`while (true)`): 1. **Yield** `stream_request_start` event ... 5. **Tool execution**: `StreamingToolExecutor` or `runTools()` — concurrent for safe tools, serial for unsafe. Max concurrency configurable (default 10, env `CLAUDE_CODE_MAX_TOOL_USE_CONCURRENCY`)"
- **Craft skill hint**: "Reading a reference agent harness end-to-end to extract the agent-loop, tool, hook, and session contracts that a parity build must honour."
- **Notes**: Heavy Layer 1 + Layer 4 evidence. Hook lifecycle list (20+ events) is a direct CO Layer 3 / anti-amnesia evidence cluster. The five permission-flow steps map to PACT verification gradient but are _not_ called PACT in the source — pure CO Layer 3.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Claw Code — Rust reimplementation of Claude Code (6 crates, 21K LOC, 16 tools, permission cascade)
- **Spec concepts touched**: CO § 9 (Layer 1), CO § 26 (Layer 4), CO § 19 (Layer 3), CO § 23 (Three-Tier Enforcement), CO § 17 (Single Source of Truth), CARE § 30 (Constraint Envelopes), CARE § 6 (Trust Verification Bridge)
- **Move type**: comparable-product audit; clean-room reimplementation pattern study
- **Evidence polarity**: MIXED (validates the architecture but flags features Claw Code chose NOT to implement, e.g. hook execution despite parsing config)
- **Key quote**: "Claw Code chose NOT to implement hook execution despite parsing hook config — if hooks are the primary extensibility mechanism, was this a pragmatic MVP cut or a design mistake?"
- **Craft skill hint**: "Comparing a competitor's clean-room reimplementation to find which capabilities are load-bearing vs aspirational, before committing to your own scope."
- **Notes**: Permission cascade `ReadOnly → WorkspaceWrite → DangerFullAccess → Prompt` is a four-zone gradient cousin to CO § 24 (Tier 3 Transport-Level Enforcement) and PACT § 19 (Verification Gradient).

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: kaizen-cli-rs `kz` binary current capabilities — 3-layer arch, 5 tools, 13 commands, hook system, session management, permission system, config
- **Spec concepts touched**: CO § 9 (Layer 1 — Intent), CO § 26 (Layer 4 — Instructions), CO § 19 (Layer 3 — Guardrails), CO § 5.1 (Hard Enforcement), CO § 23 (Three-Tier Enforcement Model), CARE § 30 (Constraint Envelopes), CARE § 4 (Human-on-the-Loop)
- **Move type**: own-codebase capability inventory; pre-implementation baseline read
- **Evidence polarity**: MIXED (shows architectural strengths + acknowledges feature gaps)
- **Key quote**: "**SHA-256 trust model:** ... `fn is_trusted(&self, script: &HookScript) -> bool { trust.get(&script.name) == Some(&script.hash) }` ... Untrusted hooks (hash mismatch) are skipped"
- **Craft skill hint**: "Inventorying your own agent harness so the next pass can route by responsibility (commands, tools, hooks, sessions) rather than by file."
- **Notes**: SHA-256 hook trust model is a Layer 3 hard-enforcement primitive with no direct spec analogue in the index. Closest fit is CO § 5.1 / § 19. The PACT framework is mentioned ("PACT framework") as upstream-but-not-yet-wired — a cross-PACT-layer marker (PACT spec → PACT primitives gap).

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Community reverse-engineering of Claude Code — verified claims (employee gate, auto-compaction, brevity mandate, agent swarm, file read blind spot, tool result truncation, grep-not-AST, MEMORY.md + autoDream)
- **Spec concepts touched**: CO § 3.1 (Amnesia), CO § 5 (Deterministic Enforcement), CO § 14 (Master Directive), CO § 3.3 (Safety Blindness), CO § 39 (Layer 5 — Learning), CO § 40 (Observe-Capture-Evolve), CARE § 16 (Pilot/Autopilot), CARE § 67 (Observable Execution)
- **Move type**: external-source verification; claim-vs-source audit
- **Evidence polarity**: MIXED (some claims confirmed, several "PARTIALLY FALSE" or "PARTIALLY TRUE" — explicit verification table)
- **Key quote**: "**Verification status**: PARTIALLY FALSE. Actual thresholds from source: `AUTOCOMPACT_BUFFER_TOKENS = 13,000`. Auto-compaction fires at `(effective_context_window - 13K)`, which for a ~200K context model is ~187K tokens, NOT 167K."
- **Craft skill hint**: "Verifying community claims about an agent harness against the actual source before letting them shape your roadmap — not all viral analysis is right."
- **Notes**: The MEMORY.md / autoDream pattern is a direct CO Layer 5 evidence cluster. The "USER_TYPE === 'ant' employee gate" theme recurs in entries 0007 and 0025 — a cross-entry policy-flag thread that becomes the "ship what they gate" decision.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: kz CLI vs Claude Code capability gap matrix (4 tiers, 47 features, prioritized roadmap, ~14 sessions to parity)
- **Spec concepts touched**: CO § 9 (Layer 1), CO § 26 (Layer 4), CO § 19 (Layer 3), CO § 39 (Layer 5), CO § 56 (Adoption Phase 4 — Process), CO § 14 (Master Directive)
- **Move type**: gap-matrix authoring; phased capability roadmap
- **Evidence polarity**: MIXED (gap exists but plan is concrete and parallelizable)
- **Key quote**: "**Critical gaps**: Tool breadth (5 vs 30+), no sub-agent spawning, no context compaction awareness, no MCP integration, no system prompt construction, limited slash commands (13 vs 100+), no interactive permission prompting."
- **Craft skill hint**: "Authoring a tiered capability gap matrix that distinguishes blockers from polish so a parallelizable roadmap falls out naturally."
- **Notes**: ⚠ no clean PACT mapping for the gap matrix; the roadmap is COC-layer planning, not governance content.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: kailash-py Delegate and Kaizen Agents — Python autonomous agent capabilities (3-layer progressive disclosure, TAO loop, typed streaming events, GovernedSupervisor with Budget/Clearance/Accountability/Dereliction/Bypass/Vacancy/Cascade)
- **Spec concepts touched**: CO § 9 (Layer 1), CO § 26 (Layer 4), CO § 13 (Layer 2 — Context), CO § 15 (Progressive Disclosure), PACT § 14-18 (five constraint dimensions, esp. Financial), PACT § 25-30 (Five Classification Levels — PUBLIC/RESTRICTED/CONFIDENTIAL/SECRET/TOP_SECRET), CARE § 30 (Constraint Envelopes)
- **Move type**: cross-language reference read (Python sister implementation); PACT-primitives audit
- **Evidence polarity**: POSITIVE (Python implementation is feature-complete with full PACT governance wired in; Rust CLI lags)
- **Key quote**: "**ClearanceEnforcer** | Data classification: PUBLIC → RESTRICTED → CONFIDENTIAL → SECRET → TOP_SECRET ... **Default-deny philosophy**: Empty tools list by default. $1 budget by default. PUBLIC clearance by default. All execution logged."
- **Craft skill hint**: "Reading a sister-language implementation as a richer instance of the same skill so cross-runtime parity work can route by primitive rather than rewrite by file."
- **Notes**: ⚠ This is a clean PACT (primitives) hit — the GovernedSupervisor maps directly to PACT § 25-30 (Five Classification Levels) and § 19-23 (Verification Gradient). The five-level enum collapses CARE/EATP/PACT clearance vocabularies into one — worth flagging for the PACT-disambiguation rule.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: "Ship what they gate" strategy — kz prioritises capabilities that Claude Code gates from external users (default post-edit verification, compaction transparency, effort-aware system prompts, tool result transparency, file read transparency) plus 5 phased milestones
- **Spec concepts touched**: CO § 9 (Layer 1), CO § 14 (Master Directive), CO § 19 (Layer 3 — Guardrails), CO § 26 (Layer 4 — Instructions), CO § 5 (Deterministic Enforcement), CO § 4 (Human-on-the-Loop), CARE § 9 (Transparency as Foundation), CARE § 12 (Graceful Degradation)
- **Move type**: strategic decision; phased roadmap with rationale and rejected alternatives
- **Evidence polarity**: POSITIVE
- **Key quote**: "Rather than copying Claude Code feature-for-feature, we prioritize shipping the capabilities that Anthropic **gates from external users** or **leaves as implicit knowledge**: 1. **Default post-edit verification** — Claude Code gates this behind `tengu_hive_evidence` feature flag (employee-only A/B). We ship it as default."
- **Craft skill hint**: "Distilling a competitive strategy from gated-vs-default capability differences in a reference implementation."
- **Notes**: Direct CARE § 9 (Transparency) mooring — the entry explicitly contrasts "silently truncates" vs "include truncation metadata." Strong skill atom candidate for FORGE Layer 1 practitioner craft.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Performance and quality risks — prompt caching, parallel tool execution, deferred tool loading, streaming tool exec, microcompaction, post-edit verification, file edit staleness, tool result truncation transparency, context compaction awareness, curly quote normalisation, line ending preservation, JSONL session format, system prompt registry, glob permission rules
- **Spec concepts touched**: CO § 14 (Master Directive), CO § 19 (Layer 3 — Guardrails), CO § 5 (Deterministic Enforcement), CO § 5.4 (Defense in Depth), CO § 29 (Evidence-Based Completion), CARE § 9 (Transparency), CARE § 67 (Observable Execution), CARE § 6 (Trust Verification Bridge)
- **Move type**: proactive risk register; "what we must get right" technical-quality enumeration
- **Evidence polarity**: MIXED (lays out the gap and proposes recommendations)
- **Key quote**: "**Post-Edit Verification (CRITICAL — 29-30% false-claims without it)** ... Without this, Anthropic's own data shows 29-30% false-claims rate. **Risk for us**: If our agent reports 'Done!' without verification, nearly 1 in 3 completions will have errors the user discovers later."
- **Craft skill hint**: "Stress-testing an autonomous-agent harness against speed and quality failure modes that only matter at the millisecond and percentile-of-completion scale."
- **Notes**: The "stale file edit detection via readFileState timestamps" pattern is a CO Layer 4 evidence-based-completion (CO § 29) cluster. Curly quote and line ending normalisation are tool-craft polish — no clean spec mapping but high-frequency journal evidence.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0009-GAP-full-parity-analysis.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: Full parity scorecard — 36% complete, 160 features missing, three zero-percent areas (Compaction, Memory, MCP)
- **Spec concepts touched**: CO § 9 (Layer 1), CO § 13 (Layer 2 — Context), CO § 19 (Layer 3), CO § 26 (Layer 4), CO § 39 (Layer 5)
- **Move type**: parity scorecard; spec-by-spec coverage measurement
- **Evidence polarity**: NEGATIVE (zero-percent areas are blocking)
- **Key quote**: "**Critical Zero-Percent Areas** - **Compaction** — No context management for long sessions - **Memory** — No cross-session knowledge persistence - **MCP** — Stub only, no tool ecosystem integration"
- **Craft skill hint**: "Producing a per-spec percent-complete scorecard that distinguishes infrastructure-shipped from user-facing-shipped."
- **Notes**: Self-aware about scorecard manipulation: "The 35 'complete' features are mostly infrastructure (config loading, session metadata, hook framework). The 160 missing features include ALL user-facing capabilities."

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0010-RISK-redteam-implementation-plan.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Red team — stress-testing the implementation plan (10 risks: upstream dependency uncertainty, provider-agnostic vs Anthropic-specific, tool count explosion, compaction complexity, session migration, CLI UX limits, memory cold start, ML classifier feasibility, effort estimate accuracy, spec drift)
- **Spec concepts touched**: CO § 26 (Layer 4 — Instructions), CO § 56 (Adoption Phase 4 — Process), CO § 17 (Single Source of Truth), EATP § 9 (Verification Gradient — Three Levels), CARE § 4 (Human-on-the-Loop)
- **Move type**: red-team validation of an implementation plan; pre-execution risk surfacing
- **Evidence polarity**: MIXED (plan is "VIABLE with adjustments")
- **Key quote**: "**Risk 1: Upstream Dependency Uncertainty (CRITICAL)** ... Before starting implementation, audit the actual kaizen-agents API surface. Read the 93 files. Map every spec requirement to an upstream capability or identify gaps that need upstream changes."
- **Craft skill hint**: "Red-teaming a 14-session plan against upstream-dependency, scope-explosion, and effort-estimate blast radius BEFORE the first commit."
- **Notes**: Explicitly cites the autonomous execution multiplier (FORGE rule `autonomous-execution.md` Risk 9). Risk 9 (effort estimation) corrects 6-8 days → 15-20 days — direct evidence for the "first session is 2-3x, not 10x" caveat.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md

- **Type**: TRADE-OFF
- **Date**: 2026-04-01
- **Topic**: External COC artifacts vs baked-in internals — token cost explosion (146KB external rules ≈ 36K tokens/turn vs 13K for Claude Code's compiled prompts.ts)
- **Spec concepts touched**: CO § 14 (Master Directive), CO § 13 (Layer 2 — Context), CO § 15 (Progressive Disclosure), CO § 5 (Deterministic Enforcement), CO § 17 (Single Source of Truth)
- **Move type**: artifact-craft economics; token-budget-driven architecture trade-off
- **Evidence polarity**: NEGATIVE about current external-artifact model; POSITIVE about hybrid recommendation
- **Key quote**: "Our external rules alone cost **2.7x more tokens** than Claude Code's entire baked-in system prompt. And that's JUST rules — add agents, skills, guides and we're at 5-10x."
- **Craft skill hint**: "Quantifying the per-turn token cost of external CC artifacts so the trade-off between editability and context-window pressure can be argued in numbers, not preference."
- **Notes**: ⚠ HIGH-SIGNAL FOR FORGE LAYER 2 (artifact craft). This entry directly questions the COC-template philosophy of "everything as markdown" — important counter-evidence for the brokerage layer. The "Option C Hybrid" recommendation (bake core, keep project-specific external) is a serious challenge to current loom/kailash-coc-claude-rs structure.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Core vs Plugin classification — 19 CORE rules / 10 PLUGIN rules / ~20 CORE agents / ~12 PLUGIN agents / ~13 CORE skill dirs / ~25 PLUGIN skill dirs
- **Spec concepts touched**: CO § 17 (Single Source of Truth), CO § 13 (Layer 2 — Context), CO § 9 (Layer 1), CO § 19 (Layer 3), CO § 16 (Framework-First)
- **Move type**: variant classification; "does this rule apply if user is NOT using Kailash?" decision rule
- **Evidence polarity**: POSITIVE (concrete classification rule + per-file dispositions)
- **Key quote**: "**Test**: 'Does this rule apply if the user is NOT using Kailash?' YES → Core. NO → Plugin."
- **Craft skill hint**: "Classifying COC artifacts as core-vs-plugin against a single sentence-long test rule, so the variant overlay system has unambiguous source-of-truth dispositions."
- **Notes**: ⚠ HIGHEST-SIGNAL FOR FORGE LAYER 3 (brokerage craft). This is direct evidence of the variant-classification skill atom (`variants/py/` vs `variants/rs/` vs `globals/`). Borderline cases (terrene-naming, independence) raise the question "is there a 'terrene' plugin tier?" — a real classification ambiguity worth a skill atom.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0013-DECISION-todos-approved.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Comprehensive todos — 847 checkboxes across 8 files, red-teamed against 13 specs (~90% spec coverage post red team)
- **Spec concepts touched**: CO § 26 (Layer 4 — Instructions), CO § 27 (Structured Workflow), CO § 28 (Approval Gate), CO § 50 (Workflow Discipline)
- **Move type**: approved-plan handoff; phase 02 → phase 03 gate
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Spec coverage before red team**: ~55% of features had todos **After M5 gap closures**: ~90%+ coverage **Remaining ~10%**: Edge cases and internal-only features..."
- **Craft skill hint**: "Driving a todo file through a red-team coverage check until spec-coverage exceeds a numeric threshold before shipping the approval gate."
- **Notes**: Direct evidence of CO § 28 (Approval Gate) at the /todos → /implement transition. Spec coverage threshold is a CO § 50 (Quality Criteria — Workflow Discipline) instance.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0014-DISCOVERY-existing-codebase-architecture.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Existing codebase architecture audit — 21 files, ~10K LOC, per-file disposition (SOLID / NEEDS MIGRATION / EXTEND / ENHANCE / REWRITE / IMPLEMENT)
- **Spec concepts touched**: CO § 26 (Layer 4 — Instructions), CO § 16 (Framework-First)
- **Move type**: pre-implementation own-codebase audit; per-file disposition labelling
- **Evidence polarity**: POSITIVE (foundation is solid, mostly extension not rewrite)
- **Key quote**: "Pre-implementation audit of what exists. Key finding: **solid foundation, needs extension not rewrite**."
- **Craft skill hint**: "Performing a per-file disposition pass on an existing codebase before implementation so the agent stops re-deriving status mid-edit."
- **Notes**: ⚠ no clean spec mapping for "per-file disposition labelling" — but this is a recurring practitioner-craft move that should become a skill atom in the FORGE Layer 1 practitioner pile.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0015-DISCOVERY-upstream-api-audit-results.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Upstream API audit — 85% exists in kailash-rs, 11% partial, 4% missing; "we're wiring, not building"
- **Spec concepts touched**: CO § 16 (Framework-First), CO § 17 (Single Source of Truth), CO § 9 (Layer 1), CO § 26 (Layer 4)
- **Move type**: upstream-API audit; build-vs-wire decision rule
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Verdict: 85% EXISTS. We're wiring, not building.** | Status | Count | % | Impact | | EXISTS (Full) | 23 | 85% | Use directly | | PARTIAL | 3 | 11% | Extend upstream | | MISSING | 1 | 4% | Build in kz |"
- **Craft skill hint**: "Auditing an upstream dependency surface against your spec to discover whether you're a wiring task or a build task — before you commit to either."
- **Notes**: Strong CO § 16 (Framework-First / composition over creation) evidence — the agent literally asks "is this in the upstream?" before authoring new code. Listing the upstream changes needed (`LlmRequest.cache_hints`, `thinking_budget`, `ToolResult.metadata`) as additive backward-compatible changes is a brokerage move (cross-repo coordination).

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0016-GAP-session-wrapup-implementation-start.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: Session wrapup at end of analysis phase — handoff state for next session
- **Spec concepts touched**: CO § 39 (Layer 5 — Learning), CO § 35 (Phase — Execute), CO § 26 (Layer 4)
- **Move type**: session boundary handoff; "next session: start with X" framing
- **Evidence polarity**: MIXED
- **Key quote**: "## Next Session: Start with T0.2 + T0.3 in parallel ... Recommendation: Keep `AgentLoop` as the kz-specific wrapper (handles commands, hooks, permissions, system prompt) and use `TaodRunner` internally for the actual LLM loop."
- **Craft skill hint**: "Writing a session-wrapup that resumes a multi-session implementation cleanly — what is done, what is next, what is blocked."
- **Notes**: Direct evidence of the FORGE rule `wrapup` command and Layer 5 institutional knowledge capture across sessions. Architecture decision (AgentLoop wraps TaodRunner) is recorded mid-wrapup — not pure handoff.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: M0 Foundation complete — SystemPromptBuilder (1,202 lines), Tool trait + KzToolRegistry + bridge (1,070), 7 core tools (3,362), 27 deferred tools + ToolSearch (896), ToolExecutor with concurrent batching (847); 7,876 new lines; 454 tests
- **Spec concepts touched**: CO § 14 (Master Directive), CO § 19 (Layer 3), CO § 26 (Layer 4), CO § 9 (Layer 1), CO § 16 (Framework-First)
- **Move type**: milestone-complete decision; architecture-decisions-from-implementation post-mortem
- **Evidence polarity**: POSITIVE
- **Key quote**: "### 1. SystemPromptBuilder — static/dynamic split with cache boundary ... Static prefix (7 sections) is cacheable across sessions. Dynamic suffix (6 sections) recomputes per turn. `__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__` marker enables API-layer differential caching."
- **Craft skill hint**: "Recording milestone-complete architecture decisions as post-implementation discoveries, not as forward-looking plans, so the codebase decisions stay verifiable from the journal."
- **Notes**: SystemPromptBuilder static-vs-dynamic split is a Layer 2 (Context) implementation pattern that has no direct spec concept but is high-leverage for FORGE Layer 2 artifact-craft skill atoms. Effort-level-aware output style ("Low=terse, Medium=balanced, High=flowing prose") is a CARE § 4 / CO § 4 (Human-on-the-Loop) refinement evidence cluster.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md

- **Type**: DECISION
- **Date**: 2026-04-02
- **Topic**: M1 (core loop) + M2 (context management) implementation complete — token budget (~280), permissions (~760, glob matching, six modes), config schema (~450), compaction (~580), JSONL sessions (~620), hooks (27 events, ~400 added)
- **Spec concepts touched**: CO § 19 (Layer 3 — Guardrails), CO § 23 (Three-Tier Enforcement), CO § 26 (Layer 4), CO § 39 (Layer 5), CO § 14 (Master Directive), CARE § 30 (Constraint Envelopes — Definition), CARE § 6 (Trust Verification Bridge)
- **Move type**: milestone-complete decision; post-implementation contract record
- **Evidence polarity**: POSITIVE
- **Key quote**: "1. **Permission engine uses glob matching** via globset crate for `Bash(npm *)` patterns. Deny rules always override allow. Six modes with cycle support."
- **Craft skill hint**: "Wiring a six-mode permission engine that resolves deny > allow > ask > prompt with glob patterns instead of exact tool names."
- **Notes**: Glob-pattern permission rules (`Bash(npm *)`) are CO Layer 3 (Guardrails) but not explicitly mapped to any single spec concept — closest is CO § 24 (Tier 3 Transport-Level Enforcement) and CARE § 30 (Constraint Envelopes). The 27-event hook expansion is a Layer 3 hard-enforcement scaling evidence cluster.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0019-DECISION-m3-memory-system-implementation.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: M3 Memory System (T3.2) — `src/memory/` with 6 submodules (types, entrypoint, scan, age, save, team), 75 new tests, 2,260 lines, six-attack-vector path hardening
- **Spec concepts touched**: CO § 39 (Layer 5 — Learning), CO § 40 (Observe-Capture-Evolve), CO § 41 (Confidence Thresholds), CO § 42 (Knowledge Growth)
- **Move type**: memory-subsystem implementation; security-hardened path validation
- **Evidence polarity**: POSITIVE
- **Key quote**: "2. **Team memory path hardening** — Six attack vectors covered: null bytes, URL-encoded traversal, Unicode NFKC normalization (fullwidth `.` and `/`), backslashes, absolute paths, symlink escape. Two-pass validation (string-level then filesystem-level)."
- **Craft skill hint**: "Implementing a two-pass path-validation gate that covers string-level encoding attacks AND filesystem-level symlink escapes for any file-write tool."
- **Notes**: Six-attack-vector path hardening is a security skill atom — fits Layer 3 (Guardrails) but no spec concept covers Unicode-NFKC fullwidth attacks. Defer-by-design for findRelevantMemories and autoDream is a CO Layer 5 boundary marker.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0020-RISK-redteam-agent-loop-wiring.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Red team validation of async agent loop wiring — 4 parallel agents (spec audit, security review, test coverage, code quality); 5 fixes applied (is_read_only hardcoded false, hook arg rewriting silently ignored, execute_batch bypass, untested branches, suppression)
- **Spec concepts touched**: CO § 19 (Layer 3 — Guardrails), CO § 49 (Quality Criteria — Enforcement Reliability), CO § 5 (Deterministic Enforcement), CARE § 6 (Trust Verification Bridge), EATP § 36 (Graceful Degradation)
- **Move type**: red-team validation; security review at gate
- **Evidence polarity**: MIXED (5 fixed, 5 noted not yet fixed)
- **Key quote**: "### Security (H2): Hook argument rewriting silently ignored ... `HookOutcome::Allow(Some(modified_args))` was discarded — the original dangerous arguments were executed. Fixed by applying modified args via a `modified_call` before execution."
- **Craft skill hint**: "Running four parallel red-team agents (spec / security / test / quality) at a gate, then triaging into fix-now-vs-note-not-fixed."
- **Notes**: Direct CO § 49 (Enforcement Reliability) evidence — "intentionally attempt to violate critical rules and verify hard enforcement catches it." H2 (silent hook arg discard) is a defense-in-depth failure caught by the red team — strong evidence for the FORGE rule `agents.md` quality-gate cadence.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0021-DECISION-async-agent-loop-wiring.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: LlmClient async trait replaces sync callback; ToolExecutor made async with `is_read_only`; PermissionEngine wired as Optional; hook modified args applied
- **Spec concepts touched**: CO § 26 (Layer 4 — Instructions), CO § 19 (Layer 3 — Guardrails), CARE § 6 (Trust Verification Bridge), CARE § 30 (Constraint Envelopes)
- **Move type**: async refactor; trait-design decision
- **Evidence polarity**: POSITIVE
- **Key quote**: "3. **PermissionEngine wired as Optional** field on AgentLoop. Checked in `execute_tool()` BEFORE hooks. All 3 variants handled: Allow proceeds, Deny returns error, NeedPrompt denies in non-interactive mode."
- **Craft skill hint**: "Designing a trait split where permission enforcement is checked BEFORE hook execution and BEFORE tool dispatch, with NeedPrompt → Deny in non-interactive mode."
- **Notes**: PermissionEngine-Optional pattern is a test-vs-production trade-off — entry point in main.rs must enforce construction. Documents a known fail-open risk that requires structural follow-up.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0022-DECISION-entry-point-wiring-and-subagents.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Wire SystemPromptBuilder, MCP bridge, sub-agent protocol into production entry point — main.rs becomes full production wiring point
- **Spec concepts touched**: CO § 9 (Layer 1 — Intent), CO § 14 (Master Directive), CO § 26 (Layer 4 — Instructions), CO § 19 (Layer 3)
- **Move type**: subsystem-to-entry-point wiring; legacy-vs-production path coexistence
- **Evidence polarity**: POSITIVE
- **Key quote**: "1. **SystemPromptBuilder** → main.rs renders the full system prompt (static+dynamic sections, CLAUDE.md walk-up, env detection) and passes it to AgentLoopConfig."
- **Craft skill hint**: "Wiring four major subsystems into a production entry point in a single pass while keeping legacy test paths alive — coexistence over rewrite."
- **Notes**: CLAUDE.md walk-up discovery is a CO § 14 (Master Directive) implementation pattern. Tokio task-local for AgentContext is a sub-agent isolation pattern (CO § 9 evidence).

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md

- **Type**: RISK
- **Date**: 2026-04-02
- **Topic**: Red team round 1 — LLM client + agent loop findings (5 critical fixed: double `[ERROR]` prefix, `raw_messages` unbounded growth, SSE error events ignored, default model wrong, UTF-8 boundary splits; 6 security findings noted: exec policy bypass, scripting interpreters, session perms, API key zeroization, error body forwarding, hook trust file atomicity; 6 PARTIAL subsystems unwired)
- **Spec concepts touched**: CO § 19 (Layer 3 — Guardrails), CO § 49 (Quality Criteria — Enforcement Reliability), CO § 5 (Deterministic Enforcement), CARE § 30 (Constraint Envelopes), CARE § 67 (Observable Execution), CARE § 9 (Transparency)
- **Move type**: red-team round 1; multi-agent security audit; partial-vs-wired distinction
- **Evidence polarity**: MIXED (5 critical fixed; 6 noted; 6 subsystems flagged unwired)
- **Key quote**: "**`raw_messages` grew unbounded** — No compaction applied to the structured message buffer. Long sessions would exhaust the API context window. Fixed by pruning `raw_messages` when `ConversationHistory::compact()` fires..."
- **Craft skill hint**: "Catching 'implemented but not wired' subsystems via a red-team that explicitly distinguishes COMPLETE / PARTIAL / WIRED states."
- **Notes**: ⚠ HIGH-SIGNAL — "PARTIAL (implemented but not wired)" is a recurring failure mode that deserves a dedicated FORGE skill atom. The exec policy `/usr/bin/sudo` bypass is a CO § 19 / Layer 3 evasion pattern.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md

- **Type**: DISCOVERY
- **Date**: 2026-04-03
- **Topic**: 90 CC feature flags identified — ant-only features as the value proposition; 5 Tier-A categories (permission intelligence, agent autonomy, context management, background sessions, memory)
- **Spec concepts touched**: CO § 9 (Layer 1 — Intent), CO § 19 (Layer 3), CO § 39 (Layer 5), CARE § 4 (Human-on-the-Loop), CARE § 9 (Transparency)
- **Move type**: feature-flag inventory; value-proposition reframing
- **Evidence polarity**: MIXED
- **Key quote**: "The user explicitly stated: 'I want to implement ALL the ant-only internal ones, that's where the value is.' This reframes the project from 'CC parity' to 'CC superset' — shipping employee-grade features as the default experience."
- **Craft skill hint**: "Inventorying compile-time feature flags in a closed-source binary to identify the gated-vs-default split that defines the competitive opportunity."
- **Notes**: Direct connection to FORGE rule `autonomous-execution.md` — gated features represent the actual employee-grade autonomous execution baseline. The reframing from "parity" to "superset" is a strategic deliberation move worth a skill atom.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md

- **Type**: DECISION
- **Date**: 2026-04-03
- **Topic**: Ship ALL 90 CC ant-only feature flags as kz defaults; PACT governance replaces feature-flag gating as safety boundary
- **Spec concepts touched**: CO § 4 (Human-on-the-Loop), CO § 19 (Layer 3), PACT § 1 (Human-on-the-Loop Operationalization), PACT § 19 (Verification Gradient), CARE § 4 (Human-on-the-Loop), CARE § 9 (Transparency)
- **Move type**: human-overrides-agent-tiering decision; strategic scope expansion
- **Evidence polarity**: POSITIVE
- **Key quote**: "User directive: 'I want to implement ALL the ant-only internal ones, that's where the value is.' The tiering (A/B/C) was an agent prioritization — the human corrected it. ... PACT governance replaces CC's feature-flag gating as the safety boundary"
- **Craft skill hint**: "Recording a human override of an agent's tiered prioritisation as a journal DECISION so the rationale survives the next session's instinct to re-tier."
- **Notes**: ⚠ HIGH-SIGNAL FOR PACT-DISAMBIGUATION. "PACT governance replaces ... safety boundary" is the PACT (primitives or platform) reading, not PACT (spec). Worth flagging for the FORGE rule `forge-scope.md` Section 2 — exactly the kind of unqualified PACT reference the rule is trying to prevent.

#### Entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0026-DISCOVERY-oauth-auth-architecture-gap.md

- **Type**: DISCOVERY
- **Date**: 2026-04-04
- **Topic**: OAuth auth rejected by Anthropic public API — CC uses internal routing; kz reads CC OAuth credentials but Bearer auth fails E2E
- **Spec concepts touched**: EATP § 14 (Trust Lineage Chain Overview), EATP § 15 (Element 1 — Genesis Record), EATP § 28 (Operation 1 — ESTABLISH), CARE § 19 (Trust is Human)
- **Move type**: external-API parity blocker discovery
- **Evidence polarity**: NEGATIVE
- **Key quote**: "CC OAuth tokens (`sk-ant-oat01-*`) are sent as `Authorization: Bearer` via the Anthropic SDK's `authToken` parameter. The public `api.anthropic.com/v1/messages` endpoint returns 'OAuth authentication is currently not supported' for Bearer auth. CC works because the SDK or server routes OAuth-authenticated requests differently..."
- **Craft skill hint**: "Reading authentication chains from a closed-source agent to detect which trust establishment paths require server-side allowlisting vs raw API keys."
- **Notes**: ⚠ out of scope — OAuth blocker is a vendor-API authentication issue, not CO/COC craft. Marginal EATP § 14 mooring (trust establishment) but not a strong FORGE skill atom. Note for completeness, do not promote.

### loom/kaizen-cli-py

#### Entry: loom/kaizen-cli-py/workspaces/kz-foundation/journal/0001-GAP-test-coverage-gaps.md

- **Type**: GAP
- **Date**: 2026-03-29
- **Topic**: Red team — 6 of 9 src/kz/ modules have zero test coverage (~1,800 lines); critical untested security paths (SSRF, sandbox, governance enforcement chain); structural issues (no regression dir, custom test runner)
- **Spec concepts touched**: CO § 49 (Quality Criteria — Enforcement Reliability), CO § 19 (Layer 3 — Guardrails), CO § 5 (Deterministic Enforcement)
- **Move type**: test-coverage red team; security-path prioritisation
- **Evidence polarity**: NEGATIVE
- **Key quote**: "## CRITICAL untested security paths - `web_tools.py` `_check_ssrf()` — SSRF protection blocking private IPs, metadata services - `sandbox.py` — Seatbelt/bubblewrap profile generation, SBPL path sanitization - `app.py` `_wrap_executor()` — 6-step governance enforcement chain (budget, permissions, hooks)"
- **Craft skill hint**: "Triaging untested code by severity-of-failure-if-broken — security paths first, governance chain second, helpers last."
- **Notes**: Direct CO § 49 evidence: "Intentionally attempt to violate critical rules and verify hard enforcement catches it." 6-step governance enforcement chain (budget, permissions, hooks) is the PACT (primitives) implementation surface in kaizen-cli-py.

#### Entry: loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md

- **Type**: RISK
- **Date**: 2026-03-29
- **Topic**: Security findings round 1 — 0 CRITICAL / 5 HIGH / 6 MEDIUM (H1 hardcoded models, H2 NaN/Inf bypass, H3 unbounded checkpoint dict, H4 symlink follow on restore, H5 SSRF on search redirects)
- **Spec concepts touched**: CO § 19 (Layer 3 — Guardrails), CO § 5 (Deterministic Enforcement), CO § 5.4 (Defense in Depth), CO § 49 (Enforcement Reliability)
- **Move type**: security-reviewer red team round 1
- **Evidence polarity**: MIXED ("Strong security posture overall" but 5 HIGH findings)
- **Key quote**: "1. **H1**: Hardcoded `\"gpt-5-chat-latest\"` in 4 locations (app.py x3, orchestrate.py x1). Violates env-models.md Rule 2 (BLOCKING). Should use env-based resolution."
- **Craft skill hint**: "Catching hardcoded model names across 4 source locations and tracing them to a Layer 3 rule violation that BLOCKS rather than warns."
- **Notes**: Direct CO § 5.4 (Defense in Depth — 5+ enforcement mechanisms for highest-risk rules) evidence — env-models.md is a 5-layer defense rule. NaN/Inf budget bypass (H2) is a numeric-edge-case skill atom for Layer 3 input validation.

#### Entry: loom/kaizen-cli-py/workspaces/kz-foundation/journal/0003-RISK-checkpoint-path-traversal.md

- **Type**: RISK
- **Date**: 2026-03-29
- **Topic**: Checkpoint index deserialization enabled arbitrary path rmtree — `_CheckpointRecord.from_dict()` trusted `copy_dir` from index; `delete()` called `shutil.rmtree(copy_dir)` unchecked. CRITICAL severity, fixed same session.
- **Spec concepts touched**: CO § 19 (Layer 3 — Guardrails), CO § 5.1 (Hard Enforcement), CO § 49 (Enforcement Reliability), CARE § 6 (Trust Verification Bridge)
- **Move type**: critical-security-fix; root-cause + multi-layer-mitigation pattern
- **Evidence polarity**: NEGATIVE found / POSITIVE fixed
- **Key quote**: "Red team Round 3 found that `_CheckpointRecord.from_dict()` trusted `copy_dir` from the persisted index without validation. The `delete()` method then called `shutil.rmtree(copy_dir)` directly. A crafted or corrupted index.json could contain `\"copy_dir\": \"/\"` or `\"copy_dir\": \"/home/user\"`, causing arbitrary directory deletion."
- **Craft skill hint**: "Treating any user-writable serialized state (index.json, checkpoint files) as untrusted input that must be re-validated before any filesystem-modifying call."
- **Notes**: Same-session fix with 4-layer mitigation (regex validation, containment check, symlink check on index, symlink check on copy_dir) — direct CO § 5.4 (Defense in Depth) evidence in a single fix. Excellent FORGE Layer 3 skill atom material.

### loom/kz-engage

#### Entry: loom/kz-engage/workspaces/herald/journal/0001-DECISION-sqlite-numpy-over-chromadb.md

- **Type**: DECISION
- **Date**: 2026-03-28
- **Topic**: Vector store selection — SQLite + numpy brute-force cosine similarity over ChromaDB / pgvector / FAISS / Qdrant for ~4K chunks
- **Spec concepts touched**: CO § 16 (Framework-First / composition over creation), CO § 17 (Single Source of Truth)
- **Move type**: dependency-minimisation decision; scale-appropriate technology selection
- **Evidence polarity**: POSITIVE
- **Key quote**: "At ~4,000 chunks (from ~400 source documents), brute-force cosine similarity via numpy completes in <10ms. No approximate nearest-neighbor index is needed at this scale."
- **Craft skill hint**: "Choosing the smallest dependency that meets latency budget at the actual data scale, instead of the most popular library."
- **Notes**: ⚠ marginal CO mapping — this is a software-engineering decision more than COC craft. Framework-first connection (use stdlib `sqlite3` not new dependency) is the only clean spec hook. Defer for FORGE candidates unless an Layer-2 framework-selection skill atom needs evidence.

#### Entry: loom/kz-engage/workspaces/herald/journal/0002-DECISION-single-agent-not-multi-agent.md

- **Type**: DECISION
- **Date**: 2026-03-28
- **Topic**: Single HeraldDelegate BaseAgent with retrieval as deterministic pre-processing, NOT multi-agent pipeline
- **Spec concepts touched**: CO § 9 (Layer 1 — Intent), CO § 11 (Routing Rules)
- **Move type**: agent-architecture decision; latency-budget-driven design
- **Evidence polarity**: POSITIVE
- **Key quote**: "Why retrieval is pre-processing, not an LLM tool: ... Retrieval is a deterministic data operation (given query, fetch top-k chunks) — this is structural plumbing, not agent reasoning - Consistent with the agent-reasoning rule: 'Tools are dumb data endpoints. The LLM does ALL reasoning.'"
- **Craft skill hint**: "Distinguishing 'agent reasoning' from 'structural plumbing' so the LLM-first rule is applied to decisions, not data fetches."
- **Notes**: Direct evidence of FORGE rule `agent-reasoning.md` (LLM-first principle). Strong CO § 9 (Layer 1) skill atom — when to NOT add an agent. Counter-pattern to "more agents is more sophisticated."

#### Entry: loom/kz-engage/workspaces/herald/journal/0003-DISCOVERY-concept-aware-chunking-essential.md

- **Type**: DISCOVERY
- **Date**: 2026-03-28
- **Topic**: Concept-aware chunking required for CO/COC corpus — naive section chunking fragments answers; need synthetic relationship/disambiguation/FAQ chunks tagged by tier (0-6) and concept dependencies
- **Spec concepts touched**: CO § 13 (Layer 2 — Context), CO § 15 (Progressive Disclosure), CO § 17 (Single Source of Truth)
- **Move type**: knowledge-pipeline design; chunking-strategy decision
- **Evidence polarity**: POSITIVE
- **Key quote**: "1. A single spec section may contain multiple concepts (Layer 3 contains anti-amnesia, defense in depth, hard/soft enforcement, CARE connection, EATP connection) 2. Relationships between concepts are implicit — never stated as retrievable sentences"
- **Craft skill hint**: "Designing concept-aware chunks with explicit synthetic relationship/disambiguation/FAQ chunks to surface implicit cross-concept connections that embedding similarity misses."
- **Notes**: Direct evidence of CO Layer 2 (Context) tier hierarchy in practice. The 7-tier dependency graph is a Layer 2 organizational pattern that maps to FORGE's own progressive disclosure obligation.

#### Entry: loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md

- **Type**: DISCOVERY
- **Date**: 2026-03-28
- **Topic**: Herald isn't just a product — it's a proof of concept for the Foundation's own specifications (PACT-governed while explaining PACT, CARE-bounded while explaining CARE, etc.)
- **Spec concepts touched**: PACT § 1 (Human-on-the-Loop Operationalization), CARE § 4 (Human-on-the-Loop), CARE § 9 (Transparency), CO § 4 (Human-on-the-Loop), EATP § 14 (Trust Lineage Chain Overview)
- **Move type**: positioning insight; self-demonstrating-product framing
- **Evidence polarity**: POSITIVE
- **Key quote**: "Herald isn't just a product — it's a **proof of concept for the Foundation's own specifications**: - Herald explains PACT governance while itself being governed by a PACT envelope - Herald explains CARE's Human-on-the-Loop while operating within those constraints"
- **Craft skill hint**: "Designing a product so its operating envelope is a live demonstration of the same spec it teaches — the bot demonstrates what it teaches."
- **Notes**: ⚠ HIGH-SIGNAL. The PACT references here mix PACT (spec) and PACT (primitives / envelope-as-runtime) without qualification — direct evidence for the FORGE rule `forge-scope.md` Section 2 PACT-disambiguation requirement. Cross-spec mooring across all four standards.

#### Entry: loom/kz-engage/workspaces/herald/journal/0005-CONNECTION-co-progressive-disclosure-in-herald-responses.md

- **Type**: CONNECTION
- **Date**: 2026-03-28
- **Topic**: CO's progressive disclosure principle (Layer 2) applies to Herald's own responses — Tier 0 explanation first, deeper on demand, never dump advanced concepts on beginner question
- **Spec concepts touched**: CO § 13 (Layer 2 — Context), CO § 15 (Progressive Disclosure), CO § 64 (COC Skill Hierarchy)
- **Move type**: spec-to-product mapping; principle-as-UX pattern
- **Evidence polarity**: POSITIVE
- **Key quote**: "CO's Layer 2 defines 'progressive disclosure' — load minimum context by default, pull deeper reference on demand. Herald's response strategy mirrors this exactly: - Start with Tier 0 explanation (Brilliant New Hire analogy) - Offer to go deeper ('Would you like to know about the five layers?') ... This is not just good UX — it's Herald demonstrating CO's own principles while explaining them."
- **Craft skill hint**: "Applying a CO principle to the agent's own response strategy so the UX itself is the demonstration of the spec."
- **Notes**: Direct CO § 15 (Progressive Disclosure) evidence with explicit spec-to-product mapping. Excellent skill atom material for FORGE — "applying CO principles to the products that teach CO."

#### Entry: loom/kz-engage/workspaces/herald/journal/0006-DECISION-adapters-as-nexus-api-clients.md

- **Type**: DECISION
- **Date**: 2026-03-28
- **Topic**: Platform adapters (Telegram, Discord, Slack) call Nexus via HTTP, NOT as Nexus channels — clean separation, zero-code-change migration path from single-process to multi-service
- **Spec concepts touched**: CO § 16 (Framework-First), CO § 17 (Single Source of Truth)
- **Move type**: deployment-architecture decision; migration-path preservation
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Key insight**: Even in a single-process MVP, the adapter talks to Nexus over localhost HTTP (~1ms overhead). This overhead is invisible compared to LLM latency (500-2000ms), and it means **zero code changes** when splitting into separate services later."
- **Craft skill hint**: "Picking a deployment topology that costs nothing now but preserves the migration path to multi-service without rewrites."
- **Notes**: ⚠ marginal CO mapping — software architecture decision more than COC craft. Defer.

#### Entry: loom/kz-engage/workspaces/herald/journal/0007-DECISION-milestone-ordering.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: 8 milestones (M0-M7) with 35 todos, ordered by dependency; M1, M2, M3, M5 parallelizable after M0
- **Spec concepts touched**: CO § 26 (Layer 4 — Instructions), CO § 27 (Structured Workflow), CO § 56 (Adoption Phase 4 — Process)
- **Move type**: milestone planning; parallelization-opportunity identification
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Key parallelization opportunities** M1, M2, M3, M5 can all progress concurrently after M0 completes. This is the highest-leverage parallelization in the project. In the autonomous execution model, these four milestones can run as parallel agent sessions."
- **Craft skill hint**: "Reading a milestone graph for parallelization opportunities — identifying which milestones can run as concurrent autonomous sessions."
- **Notes**: Direct evidence of FORGE rule `autonomous-execution.md` 10x throughput multiplier (parallel agent execution 3-5x).

#### Entry: loom/kz-engage/workspaces/herald/journal/0008-DISCOVERY-sdk-api-corrections.md

- **Type**: DISCOVERY
- **Date**: 2026-03-29
- **Topic**: 10 SDK API corrections from Kaizen + Nexus specialist reviews — `run_async()` requires `use_async_llm=True`, `read_from_memory()` doesn't exist, `Nexus.start()` is blocking, `session_backend` constructor params don't exist, etc.
- **Spec concepts touched**: CO § 9 (Layer 1 — Intent / specialists), CO § 16 (Framework-First), CO § 17 (Single Source of Truth)
- **Move type**: specialist-review API audit; aspirational-vs-actual API discovery
- **Evidence polarity**: NEGATIVE found / POSITIVE corrected
- **Key quote**: "Always validate SDK API assumptions against actual source before implementation. Plans based on documentation/skills may reference aspirational APIs that don't match the current SDK version."
- **Craft skill hint**: "Routing every SDK assumption through a framework specialist read-against-source pass before committing to the API surface in todos."
- **Notes**: Direct evidence of FORGE rule `agents.md` Specialist Delegation MUST rule — "MUST consult the relevant specialist." This is the failure mode that justifies the rule.

#### Entry: loom/kz-engage/workspaces/herald/journal/0009-DISCOVERY-redteam-convergence-findings.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Red team convergence — two independent agents converged on 3 of 5 critical issues (rate limiter unbounded dicts, SQLite audit thread safety, health endpoint DoS); 2 non-overlapping (null byte passthrough, model fallback race)
- **Spec concepts touched**: CO § 49 (Quality Criteria — Enforcement Reliability), CO § 19 (Layer 3 — Guardrails), CO § 5.4 (Defense in Depth)
- **Move type**: red-team convergence audit; multi-perspective validation
- **Evidence polarity**: POSITIVE (convergence validates approach)
- **Key quote**: "Convergence on 3/5 critical findings validates the multi-agent red team approach. The 2 non-overlapping findings justify running both perspectives."
- **Craft skill hint**: "Running independent red-team agents against the same artifact and using their convergence rate to validate the audit method itself."
- **Notes**: Strong CO § 5.4 (Defense in Depth) meta-evidence — running multiple independent perspectives is itself a form of defense in depth at the audit layer. Skill atom: "convergence-as-validation" pattern.

#### Entry: loom/kz-engage/workspaces/herald/journal/0010-DISCOVERY-nexus-dedup-fingerprint-bug.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Nexus DurableWorkflowServer dedup fingerprints all POSTs identically — Starlette body stream consumed by earlier middleware, `except Exception: pass` swallows the failure, every chat message gets the cached first response. Fixed via `Nexus(enable_durability=False)`.
- **Spec concepts touched**: CO § 5.1 (Hard Enforcement), CO § 19 (Layer 3 — Guardrails), CO § 49 (Enforcement Reliability)
- **Move type**: upstream-bug discovery; cross-SDK alignment filing (kailash-py + kailash-rs)
- **Evidence polarity**: NEGATIVE
- **Key quote**: "When `body=None`, `RequestFingerprinter.create_fingerprint()` hashes only `method + path + query_params`. Every POST to the same endpoint gets an identical fingerprint. The `RequestDeduplicator` then returns cached responses from the first request for all subsequent requests."
- **Craft skill hint**: "Tracing a 'bot returns same answer to every question' user-visible bug to a silent `except Exception: pass` in middleware fingerprinting."
- **Notes**: Direct violation of FORGE rule `zero-tolerance.md` Rule 3 (No Silent Fallbacks or Error Hiding). Filed cross-SDK (kailash-py#175 + kailash-rs#110) — direct evidence of FORGE rule `zero-tolerance.md` Rule 4 (No Workarounds for Core SDK Issues — file an issue).

#### Entry: loom/kz-engage/workspaces/herald/journal/0011-DISCOVERY-retrieval-tier-bias-against-constitution.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Tier boost bias — "explain anti-capture provisions" retrieved 8 generic spec overview pages (Tier 1 at 1.3x) instead of `governance/constitution/safeguards.mdx` (Tier 2 at 1.0x); 1.6x scoring gap meant the right content never surfaced
- **Spec concepts touched**: CO § 13 (Layer 2 — Context), CO § 15 (Progressive Disclosure), CO § 17 (Single Source of Truth)
- **Move type**: retrieval-quality discovery; tier-classification correction
- **Evidence polarity**: NEGATIVE found / POSITIVE corrected
- **Key quote**: "Tier boosting is a blunt instrument. The real problem is that pure RAG retrieval doesn't understand concept relationships. 'Anti-capture' connects to 'entrenched provisions,' 'Dual Plane Model,' 'monotonic tightening' — but embedding similarity treats these as separate topics."
- **Craft skill hint**: "Diagnosing why a knowledge base returned generic content instead of the specific answer by tracing the tier-boost score gap that buried the right page."
- **Notes**: ⚠ HIGH-SIGNAL FOR FORGE LAYER 2 (Context). The tier system is a Layer 2 organisational pattern AND its limits are visible here — embedding similarity ignores concept dependency graph. Strong evidence that progressive disclosure (CO § 15) needs structural support beyond simple tiering.

#### Entry: loom/kz-engage/workspaces/herald/journal/0012-DECISION-ontology-guided-reranking.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Ontology-guided re-ranking (Option B) over query expansion (Option A) — retrieve 20 candidates by similarity, then re-rank using concept graph distance between query concepts and candidate concepts
- **Spec concepts touched**: CO § 13 (Layer 2 — Context), CO § 15 (Progressive Disclosure), CO § 17 (Single Source of Truth), CO § 64 (COC Skill Hierarchy)
- **Move type**: retrieval-architecture decision; structural-knowledge-over-keyword move
- **Evidence polarity**: POSITIVE
- **Key quote**: "B compounds: adding concept relationships improves retrieval without re-embedding ... B is more robust to ambiguous queries — it doesn't need to guess the expansion, it evaluates the actual candidates"
- **Craft skill hint**: "Choosing structural-knowledge-driven re-ranking over query-expansion guessing because the structural approach compounds without re-embedding."
- **Notes**: Direct continuation of entry 0011. Strong CO § 13 / § 15 craft evidence. The "concept graph as first-class retrieval input" pattern is a Layer 2 organisational principle that has no direct spec concept but is high-leverage for FORGE Layer 2 artifact craft.

## Summary

### Counts by repo

| Repo               | Entries | Types                                                |
| ------------------ | ------- | ---------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| loom/kaizen-cli-rs | 26      | 12 DISCOVERY, 8 DECISION, 4 RISK, 2 GAP, 0 TRADE-OFF | (correction: 11 DISCOVERY + 9 DECISION + 4 RISK + 2 GAP + 1 TRADE-OFF = 27 — see note\*) |
| loom/kaizen-cli-py | 3       | 1 GAP, 2 RISK                                        |
| loom/kz-engage     | 12      | 5 DECISION, 6 DISCOVERY, 1 CONNECTION                |
| **TOTAL**          | **41**  | —                                                    |

\* Entry-by-entry kaizen-cli-rs type tally: 0001 D, 0002 D, 0003 D, 0004 D, 0005 G, 0006 D, 0007 DEC, 0008 R, 0009 G, 0010 R, 0011 TO, 0012 DEC, 0013 DEC, 0014 D, 0015 D, 0016 G, 0017 DEC, 0018 DEC, 0019 DEC, 0020 R, 0021 DEC, 0022 DEC, 0023 R, 0024 D, 0025 DEC, 0026 D → 11 DISCOVERY, 9 DECISION, 4 RISK, 1 TRADE-OFF, 1 GAP (wait — entries 0005, 0009, 0016 are GAP) = 11 D + 9 DEC + 4 R + 1 TO + 3 GAP — wait let me recount. Final accurate tally: D=11 (0001,0002,0003,0004,0006,0014,0015,0018-correction-actually-DEC,0024,0026...). Actual tally below.

**Final accurate type tally for kaizen-cli-rs (26 entries):**

| Number | Type      |
| ------ | --------- |
| 0001   | DISCOVERY |
| 0002   | DISCOVERY |
| 0003   | DISCOVERY |
| 0004   | DISCOVERY |
| 0005   | GAP       |
| 0006   | DISCOVERY |
| 0007   | DECISION  |
| 0008   | RISK      |
| 0009   | GAP       |
| 0010   | RISK      |
| 0011   | TRADE-OFF |
| 0012   | DECISION  |
| 0013   | DECISION  |
| 0014   | DISCOVERY |
| 0015   | DISCOVERY |
| 0016   | GAP       |
| 0017   | DECISION  |
| 0018   | DECISION  |
| 0019   | DECISION  |
| 0020   | RISK      |
| 0021   | DECISION  |
| 0022   | DECISION  |
| 0023   | RISK      |
| 0024   | DISCOVERY |
| 0025   | DECISION  |
| 0026   | DISCOVERY |

**Totals:** kaizen-cli-rs = 9 DISCOVERY + 9 DECISION + 4 RISK + 3 GAP + 1 TRADE-OFF = 26 ✓

**Cross-repo type totals (41 entries):**

| Type       | Count                                                          |
| ---------- | -------------------------------------------------------------- |
| DISCOVERY  | 9 (kaizen-cli-rs) + 0 (kaizen-cli-py) + 6 (kz-engage) = **15** |
| DECISION   | 9 + 0 + 5 = **14**                                             |
| RISK       | 4 + 2 + 0 = **6**                                              |
| GAP        | 3 + 1 + 0 = **4**                                              |
| TRADE-OFF  | 1 + 0 + 0 = **1**                                              |
| CONNECTION | 0 + 0 + 1 = **1**                                              |

### Top 5 most-cited spec concepts

1. **CO § 19 — Layer 3 (Guardrails)** — cited in entries 0001, 0002, 0003, 0007, 0008, 0009, 0012, 0018, 0020, 0023, 0024, 0025 (kaizen-cli-rs); 0001, 0002, 0003 (kaizen-cli-py); 0009, 0010 (kz-engage). ~17 citations. The dominant spec concept across the corpus — every red team, every security finding, every permission engine, every defense-in-depth example moors here.
2. **CO § 26 — Layer 4 (Instructions)** — cited in 0001, 0003, 0005, 0007, 0010, 0013, 0017, 0018, 0021, 0022 (kaizen-cli-rs); 0007 (kz-engage). ~11 citations. Workflow phasing, structured implementation milestones.
3. **CO § 9 — Layer 1 (Intent)** — cited in 0001, 0003, 0005, 0006, 0007, 0012, 0014-0017, 0022, 0024, 0025 (kaizen-cli-rs); 0002, 0008 (kz-engage). ~13 citations. Agent specialisations, sub-agent spawning, framework specialists.
4. **CO § 14 — Master Directive** — cited in 0001, 0004, 0007, 0008, 0011, 0017, 0018, 0022, 0024 (kaizen-cli-rs). ~9 citations. CLAUDE.md / SystemPromptBuilder pattern.
5. **CO § 49 — Quality Criteria — Enforcement Reliability** — cited in 0020, 0023 (kaizen-cli-rs); 0001, 0002, 0003 (kaizen-cli-py); 0009, 0010 (kz-engage). ~7 citations. Red team validation pattern recurs across all three repos.

Honourable mentions:

- **CO § 5 / 5.1 / 5.4 (Deterministic Enforcement / Hard Enforcement / Defense in Depth)** — frequent across security findings.
- **CO § 13 — Layer 2 (Context)** — heavy in kz-engage (knowledge pipeline, chunking, tiers, progressive disclosure).
- **CO § 39 — Layer 5 (Learning)** — recurring in memory system entries and session wrapups.
- **CARE § 30 — Constraint Envelopes** — cited where permission engines and PACT primitives are discussed.
- **CARE § 9 — Transparency as Foundation** — cited in "ship what they gate" decisions.

### Out-of-scope entries

- **0026 (kaizen-cli-rs) — OAuth auth architecture gap.** ⚠ out of scope for COC craft. This is an external-vendor-API authentication issue (Anthropic's Bearer auth rejection on the public endpoint). Marginal EATP § 14 (trust establishment) connection but not a FORGE skill atom — flag for completeness, do not promote.
- **0001 (kz-engage) — SQLite + numpy over ChromaDB.** ⚠ marginal — software-engineering dependency-minimisation decision; only weak CO § 16 (Framework-First) hook. Defer unless a Layer-2 framework-selection skill atom needs evidence.
- **0006 (kz-engage) — Adapters as Nexus API clients.** ⚠ marginal — deployment-topology decision; weak CO mapping. Defer.

No entries are _fully_ out of FORGE scope — the rest are all clean COC craft. Marginal entries are flagged but kept in the index for completeness.

### Contradictions and WIP markers

- **Entry 0010 (kaizen-cli-rs) Risk 9 vs FORGE rule `autonomous-execution.md`**: The red team explicitly corrects the 6-8 day estimate to 15-20 days, citing "first session is 2-3x, not 10x." This is _not_ a contradiction with the rule (the rule already carves out greenfield exceptions), but it is direct journal evidence of the carve-out being load-bearing. Worth surfacing as a skill atom: "estimating effort for greenfield work where the 10x multiplier does not apply."
- **Entry 0011 (kaizen-cli-rs) vs current COC template philosophy**: The trade-off entry argues that 146KB of external rules is 2.7x more expensive than Claude Code's compiled prompts.ts, and recommends a hybrid (bake core, keep project-specific external). This _contradicts_ the current loom/kailash-coc-claude-rs structure where everything is markdown. Important counter-evidence for the FORGE Layer 2 (artifact craft) discussion. Not yet acted on.
- **Entry 0019 (kaizen-cli-rs) WIP marker**: `findRelevantMemories` and `autoDream` are explicitly deferred to T3.3 because they require LLM side-query infrastructure. WIP, not contradiction.
- **Entry 0023 (kaizen-cli-rs) WIP markers**: 6 PARTIAL subsystems explicitly flagged as "implemented but not wired" (SessionManager/SessionStore, Memory system, Sub-agents, PrintMode, MCP). Strong recurring failure mode — deserves a dedicated FORGE skill atom on detecting unwired subsystems.
- **Entry 0025 (kaizen-cli-rs) PACT-disambiguation violation**: "PACT governance replaces CC's feature-flag gating as the safety boundary." This is exactly the unqualified PACT reference the FORGE rule `forge-scope.md` Section 2 is trying to prevent. Not a contradiction in the entry itself — it is a _direct example_ of why the disambiguation rule exists.

### kz-engage entries touching the post-0046 stale codify Step 7 issue

**None of the 12 kz-engage entries (0001–0012 in workspaces/herald/journal/) touch the post-0046 stale codify Step 7 issue.** The kz-engage entries cover knowledge pipeline (0001-0003), positioning (0004), CO progressive disclosure as UX (0005), deployment topology (0006), milestone planning (0007), SDK API validation (0008), red team convergence (0009), Nexus dedup bug (0010), retrieval tier bias (0011), and ontology re-ranking (0012). None reference codify, /codify, Step 7, or stale artifact propagation.

The closest match is **entry 0010 (Nexus dedup fingerprint bug)** which deals with a different stale-state issue (cached responses from `RequestDeduplicator` due to body-stream consumption + silent `except Exception: pass`). This is a _different_ class of stale-artifact bug — runtime request deduplication, not codify-time artifact promotion. Worth flagging in the cross-reference because the failure pattern (silent error suppression → stale state propagated to user) is the same shape, but the mechanism is unrelated to the codify Step 7 issue.

**Recommendation**: The post-0046 stale codify Step 7 issue is not visible in this batch's kz-engage corpus. Either (a) the relevant journal entries are in a different workspace inside kz-engage that wasn't in the cache, or (b) the issue manifested in artifacts (rules, agents, skills, codify proposals) rather than journal entries and would need a separate artifact-state audit to surface. Flag for the next reverse-index pass over the rest of the kz-engage corpus.

### kz-engage entries touching authority chain, variant classification, or stale artifact conditions (Layer 3 brokerage/governance craft)

- **0008 (SDK API corrections)** — Direct evidence of the FORGE rule `agents.md` specialist-delegation MUST. Not authority chain in the brokerage sense, but specialist authority is the precursor.
- **0010 (Nexus dedup fingerprint bug)** — Cross-SDK alignment filing (kailash-py#175 + kailash-rs#110). Direct evidence of the FORGE rule `zero-tolerance.md` Rule 4 (file the SDK issue, don't work around). This is _upstream brokerage_ — the move of routing a discovered bug back through the authority chain.
- **0011 + 0012 (tier bias + ontology reranking)** — Layer 2 (Context) corrections, not brokerage proper, but they document the _ongoing maintenance_ burden of tier classification — worth flagging as adjacent to the variant-classification skill atom.

None of the 12 kz-engage entries directly exercise the variant overlay system (variants/py/ vs variants/rs/ vs globals/) or the /sync mechanism. **The kz-engage corpus in this batch is dominated by application-craft (Layer 1 practitioner) and artifact-craft (Layer 2), not brokerage-craft (Layer 3).** This is consistent with kz-engage being a downstream consumer of the COC template, not an authority repo.
