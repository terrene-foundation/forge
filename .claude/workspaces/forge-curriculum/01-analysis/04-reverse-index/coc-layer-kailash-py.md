---
destination: forge
layer: reverse-index
batch: kailash-py
corpus_scope: loom/kailash-py/workspaces + loom/kailash-py/docs/adr
date: 2026-04-07
entries_processed: 98
---

# Reverse Index — COC Layer — Kailash Python SDK

Maps 85 workspace journal entries + 13 ADRs against the four-standard spec index (CARE / EATP / CO / PACT). Each entry cites its original repo-relative path, type, date, move classification, spec concept touched, evidence polarity, and a candidate skill atom hint. Concept numbers follow the enumeration in `../03-spec-index/{care,eatp,co,pact}-concepts.md`.

## Entries

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md

- **Type**: DECISION
- **Date**: 2026-04-02
- **Topic**: Ship fabric as `kailash-dataflow[fabric]`, not `kailash-fabric`
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth)
- **Move type**: framework use; package boundary
- **Evidence polarity**: POSITIVE
- **Key quote**: "Standalone reimplementation — Violates framework-first. Would reimplement caching, DB access, endpoint serving."
- **Craft skill hint**: Framework-first decision-making when evaluating "new package vs extension vs core evolution" options.
- **Notes**: Superseded by 0004 in same workspace — the decision was overturned one day later after further analysis.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0002-DISCOVERY-pipeline-first-cache-is-the-real-innovation.md

- **Type**: DISCOVERY
- **Date**: 2026-04-02
- **Topic**: Pipeline-driven cache is the only genuinely novel architectural insight
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: architectural differentiation
- **Evidence polarity**: MIXED
- **Key quote**: "Cache is updated ONLY when the full data pipeline succeeds. Pipelines drive cache freshness, not TTL."
- **Craft skill hint**: Competitor-pattern scan as forcing function for identifying real vs apparent novelty.
- **Notes**: Pure product-strategy content; no clear CO/COC craft lesson beyond "red-team your value props before committing."

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0003-GAP-write-path-undefined.md

- **Type**: GAP
- **Date**: 2026-04-02
- **Topic**: Write path (mutations) entirely unaddressed in design
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion); CO Concept 50 (Quality Criteria — Workflow Discipline)
- **Move type**: analysis completeness check
- **Evidence polarity**: NEGATIVE
- **Key quote**: "VP2 ('single data contract for frontend') is half-true — reads go through fabric, writes bypass it."
- **Craft skill hint**: Value-proposition coverage audit — checking that every claim has a design answer.
- **Notes**: Shows a design phase catching a "half-implemented claim" before it reaches users.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md

- **Type**: DECISION
- **Date**: 2026-04-02
- **Topic**: DataFlow IS the fabric — supersedes 0001
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth); CO Concept 3.2 (Convention Drift)
- **Move type**: framework identity evolution; decision reversal
- **Evidence polarity**: POSITIVE
- **Key quote**: "This supersedes `0001-DECISION-dataflow-extension-not-separate-package.md`. That decision recommended extension; this goes further — DataFlow's identity evolves."
- **Craft skill hint**: Decision reversal journaling — preserving the prior decision rather than overwriting it.
- **Notes**: Good evidence of journal immutability pattern (Rule: no overwrite, new entry references the original). Demonstrates CO Principle 7 (Knowledge Compounds) — prior entry still informs current.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0005-DISCOVERY-three-concepts-are-all-you-need.md

- **Type**: DISCOVERY
- **Date**: 2026-04-02
- **Topic**: The entire fabric is three new concepts on top of existing DataFlow
- **Spec concepts touched**: CO Concept 15 (Progressive Disclosure); CO Concept 16 (Framework-First)
- **Move type**: API surface minimization
- **Evidence polarity**: POSITIVE
- **Key quote**: "The fabric API reduces to exactly three new concepts added to existing DataFlow. Everything else is unchanged."
- **Craft skill hint**: DX red-team loop — iteratively compressing surface area until only essential additions remain.
- **Notes**: Two rounds of red-teaming convergence — the CO Phase 04 (/vet) pattern applied to API design.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0006-DISCOVERY-fabric-is-runtime-system-not-library.md

- **Type**: DISCOVERY
- **Date**: 2026-04-02
- **Topic**: The fabric is a runtime system, not a code library
- **Spec concepts touched**: CARE Concept 44 (Failure Modes — architectural resilience); CO Concept 29 (Evidence-Based Completion)
- **Move type**: mental-model correction
- **Evidence polarity**: MIXED
- **Key quote**: "This is more like starting PostgreSQL than calling a function. The developer configures it, starts it, and it runs — handling requests, managing state, processing events continuously."
- **Craft skill hint**: Mental-model red-team — catching the "library vs system" misframing before it shapes the whole design.
- **Notes**: User challenge ("how does monitoring work? how are endpoints consumed?") was the forcing function — a practitioner Dual-Plane move where user holds the trust plane and challenges the execution-plane design assumptions.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0007-DISCOVERY-fabric-is-four-needs.md

- **Type**: DISCOVERY
- **Date**: 2026-04-02
- **Topic**: The fabric solves exactly four needs — previous analysis was tech-forward, not needs-forward
- **Spec concepts touched**: CO Concept 2 (Brilliant New Hire Principle); CO Concept 48 (Quality Criteria — Coverage)
- **Move type**: reframing from tech-first to needs-first
- **Evidence polarity**: NEGATIVE-turned-POSITIVE
- **Key quote**: "Three rounds of red-teaming kept finding gaps because the analysis was technology-forward ('here are the components') instead of needs-forward ('here are the needs')."
- **Craft skill hint**: Needs-forward reframing as red-team escape — when gaps keep appearing, the frame is wrong.
- **Notes**: A diagnostic pattern: repeated red-team gaps signal a framing bug, not a component bug.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0008-RISK-default-open-endpoints-in-zero-config.md

- **Type**: RISK
- **Date**: 2026-04-02
- **Topic**: Zero-config mode exposes write endpoints without authentication
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); EATP Concept 66 (Constraint Dimensions — Communication); PACT Concept 18 (Communication Dimension)
- **Move type**: safety boundary correction
- **Evidence polarity**: NEGATIVE
- **Key quote**: "A developer using the simplest path — `await db.start()` — exposes database write operations to anyone who can reach the server."
- **Craft skill hint**: Zero-config safety audit — what the default emits and who can reach it.
- **Notes**: Classic safety-blindness pattern: "simplest path" optimization without checking exposure. Maps to CO Three Failure Modes.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0009-DECISION-trusted-code-boundary.md

- **Type**: DECISION
- **Date**: 2026-04-02
- **Topic**: Product functions are trusted code — `depends_on` controls refresh, not access
- **Spec concepts touched**: CARE Concept 30 (Constraint Envelopes); EATP Concept 40 (Enforcement Model — PDP/PEP)
- **Move type**: trust boundary declaration
- **Evidence polarity**: POSITIVE
- **Key quote**: "Restricting their access to sources would add complexity without security benefit — they could bypass it trivially by importing httpx directly."
- **Craft skill hint**: Trust-boundary vs attack-surface distinction — false-security detection in proposed controls.
- **Notes**: Explicitly names and rejects "false security." Shows how to defend a "no guard needed" decision against a security-reviewer default.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0010-DECISION-aether-first-consumer-not-reference.md

- **Type**: DECISION
- **Date**: 2026-04-02
- **Topic**: Aether is the first consumer, not the reference implementation
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion); CO Concept 72 (COC Institutional Knowledge Thesis)
- **Move type**: reference vs validation separation
- **Evidence polarity**: POSITIVE
- **Key quote**: "A reference should be minimal and self-contained. Aether has 52 stores and 264 routes — it demonstrates complexity, not clarity. But Aether is the proof that the fabric works at real scale. Both are needed."
- **Craft skill hint**: Reference-implementation scope discipline — minimalism for teaching, real scale for validation.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0011-DISCOVERY-request-to-event-driven-is-the-transformation.md

- **Type**: DISCOVERY
- **Date**: 2026-04-02
- **Topic**: The fundamental transformation is execution model, not features
- **Spec concepts touched**: CARE Concept 44 (Failure Modes); CO Concept 3 (Three Failure Modes)
- **Move type**: architectural reframing
- **Evidence polarity**: MIXED
- **Key quote**: "This is not 'adding fabric features.' This is changing DataFlow's execution model from **request-driven** (nothing runs until called) to **event-driven** (things run continuously in response to source changes)."
- **Craft skill hint**: Recognizing when "feature addition" is actually architectural mutation — cost and risk reframing.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0012-CONNECTION-competitor-patterns-inform-fabric-design.md

- **Type**: CONNECTION
- **Date**: 2026-04-02
- **Topic**: Competitor runtime architectures validate and refine fabric design choices
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: external-evidence integration
- **Evidence polarity**: POSITIVE
- **Key quote**: "Hasura's event triggers are NOT push-based internally. They poll an `event_log` table every 1 second with a configurable batch size (100 default)."
- **Craft skill hint**: Competitive architecture audit as CO Layer 2 institutional knowledge source.
- **Notes**: Pure competitive research; not tied to a specific spec concept, but shows how prior-art reading reshapes design choices.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0013-DISCOVERY-fabric-layers-precisely-mapped.md

- **Type**: DISCOVERY
- **Date**: 2026-04-03
- **Topic**: Fabric maps onto DataFlow's 10-layer stack with surgical precision
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth)
- **Move type**: integration-surface minimization
- **Evidence polarity**: POSITIVE
- **Key quote**: "7 of 10 existing layers are completely unchanged. The 3 extended layers gain additive-only changes — no existing API modified."
- **Craft skill hint**: Additive-only integration check — the "does this modify or only extend?" test.

### Entry: loom/kailash-py/workspaces/data-fabric-engine/journal/0014-DECISION-todo-structure-and-milestones.md

- **Type**: DECISION
- **Date**: 2026-04-03
- **Topic**: Todo structure — 6 milestones, 39 todos, dependency-ordered
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); CO Concept 34 (Phase — Plan)
- **Move type**: Phase 02 plan structuring
- **Evidence polarity**: POSITIVE
- **Key quote**: "Red team additions: TODO-34 through TODO-39 were added by the todo red team. They close gaps from the analysis red team findings that had no implementation path."
- **Craft skill hint**: Cross-phase gap-closure — making sure analysis findings land as concrete todos.

### Entry: loom/kailash-py/workspaces/dataflow-enhancements/journal/0001-DISCOVERY-existing-infrastructure-vs-missing-integration.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Existing infrastructure is substantial but integration is zero
- **Spec concepts touched**: CO Concept 1 (Institutional Knowledge Thesis); CO Concept 16 (Framework-First)
- **Move type**: codebase audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The implementation effort for these features is dominated by **integration plumbing**, not building new capability. The infrastructure teams (or prior sessions) built the components in isolation."
- **Craft skill hint**: "Built but not wired" audit — catching speculative infrastructure before estimates are built on top of it.

### Entry: loom/kailash-py/workspaces/dataflow-enhancements/journal/0002-RISK-eventbus-wildcard-gap-and-synchronous-dispatch.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: EventBus wildcard gap and synchronous dispatch threaten DerivedModel viability
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 29 (Evidence-Based Completion)
- **Move type**: API surface silent-failure detection
- **Evidence polarity**: NEGATIVE
- **Key quote**: "If implemented as documented, `on_model_change()` would **silently receive zero events** -- no error, no warning, just silent failure."
- **Craft skill hint**: Silent-failure hunt — specification-vs-implementation mismatch detection.

### Entry: loom/kailash-py/workspaces/dataflow-enhancements/journal/0003-CONNECTION-six-features-share-engine-bottleneck-and-cross-workspace-dependencies.md

- **Type**: CONNECTION
- **Date**: 2026-04-01
- **Topic**: Six features converge on engine.py and connect to nexus-transport-refactor and mcp-platform-server
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); CO Concept 17 (Single Source of Truth)
- **Move type**: cross-workspace dependency mapping
- **Evidence polarity**: MIXED
- **Key quote**: "All features converge on engine.py (6,400 lines). Every feature modifies `__init__`, `model()`, or adds properties there."
- **Craft skill hint**: Monolith-bottleneck detection when planning parallel work.

### Entry: loom/kailash-py/workspaces/dataflow-enhancements/journal/0004-RISK-sync-async-impedance-mismatch-in-cache-layer.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Sync/async impedance mismatch between CacheInvalidator, RedisCacheManager, and InMemoryCache
- **Spec concepts touched**: CO Concept 3.2 (Convention Drift); CO Concept 17 (Single Source of Truth)
- **Move type**: type-boundary audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "If `CacheInvalidator` receives an `InMemoryCache` instance, calling `self.cache_manager.delete(key)` returns a coroutine object, not an `int`. The delete silently does nothing."
- **Craft skill hint**: Protocol/ABC extraction as a fix for silent async-boundary bugs.
- **Notes**: Classic sync/async boundary drift. Should be a case study for protocol-first thinking.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md

- **Type**: DISCOVERY
- **Date**: (undated in frontmatter; session context mid-sprint)
- **Topic**: PACT engine has 4 security invariant violations
- **Spec concepts touched**: CARE Concept 6 (Trust Verification Bridge); PACT Concept 10 (Monotonic Tightening Invariant); PACT Concept 19 (Verification Gradient); EATP Concept 40 (Enforcement Model)
- **Move type**: spec-conformance audit of governance primitives
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Single-gate governance (#234) — verify_action() called once at submit, not per-node. Envelope constraints (blocked_actions, temporal, compartments) bypassed after gate."
- **Craft skill hint**: Invariant-violation hunt in governance code — "where is verify_action() actually called?" audit.
- **Notes**: NO frontmatter (violates `rules/journal.md`). Flags: NaN budget evasion (`NaN > 0 is False`) is a classic floating-point edge case.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0002-DISCOVERY-existing-red-tests.md

- **Type**: DISCOVERY
- **Date**: (undated)
- **Topic**: SSE streaming tests already exist in RED phase
- **Spec concepts touched**: CO Concept 16 (Framework-First)
- **Move type**: pre-existing-test discovery
- **Evidence polarity**: POSITIVE
- **Key quote**: "PR 3A should make these pass, not write new tests."
- **Craft skill hint**: TDD-state audit before scoping — check what tests already exist.
- **Notes**: NO frontmatter.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0003-CONNECTION-pact-kaizen-cross-package.md

- **Type**: CONNECTION
- **Date**: (undated)
- **Topic**: Per-node governance (#234) spans kailash-pact + kailash-kaizen
- **Spec concepts touched**: PACT Concept 40 (EATP Integration); EATP Concept 16 (Delegation Record)
- **Move type**: cross-package coordination risk mapping
- **Evidence polarity**: MIXED
- **Key quote**: "This is the highest-risk change in the plan — cross-package coordination with independent release cycles."
- **Craft skill hint**: Cross-package governance primitive wiring — where protocols live vs where enforcement happens.
- **Notes**: NO frontmatter.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0004-GAP-fabric-engine-operational-status.md

- **Type**: GAP
- **Date**: (undated)
- **Topic**: Fabric engine operational status unknown — PR 4C assumes it works
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion)
- **Move type**: assumption surfacing
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Must verify before Session 3. If not operational, either defer #244 or include Fabric Engine completion in scope."
- **Craft skill hint**: Prerequisite-verification checklist before scope commitment.
- **Notes**: NO frontmatter. Shows a phase-gate discipline — naming an unverified assumption before it becomes a blocker.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0005-DISCOVERY-fabric-integration-tested.md

- **Type**: DISCOVERY
- **Date**: (undated)
- **Topic**: Fabric engine confirmed operational — with gaps
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion); CO Concept 50 (Quality Criteria — Workflow Discipline)
- **Move type**: prerequisite verification closure
- **Evidence polarity**: POSITIVE
- **Key quote**: "Treasury Fabric integration testing (42 loans, 22 active, 7 currencies, real production data) confirmed: Materialized products work correctly; Virtual products broken (#245)"
- **Craft skill hint**: Real-data validation as the evidence bar for "operational" claims.
- **Notes**: NO frontmatter. Closes the loop on 0004's assumption gap.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0006-CONNECTION-fabric-mcp-consumer-synergy.md

- **Type**: CONNECTION
- **Date**: (undated)
- **Topic**: MCP tool generation (#250) + consumer adapters (#244) are complementary
- **Spec concepts touched**: EATP Concept 49 (MCP + EATP Integration); CO Concept 69 (COC Slash Commands)
- **Move type**: API design synergy identification
- **Evidence polarity**: POSITIVE
- **Key quote**: "Together they enable: one `@db.product()` definition → multiple REST views + auto-generated MCP tools. This is the 'write once, serve everywhere' pattern."
- **Craft skill hint**: "Do these two features compose?" scan before independent scheduling.
- **Notes**: NO frontmatter.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0007-DECISION-redteam-todo-fixes.md

- **Type**: DECISION
- **Date**: (undated)
- **Topic**: Red team todo fixes applied — 5 issues resolved pre-implementation
- **Spec concepts touched**: CO Concept 36 (Phase — Vet); EATP Concept 4 (Constraint Envelope Schema — Five-Dimensional); PACT Concept 18 (Communication Dimension)
- **Move type**: todo red-team loop closure
- **Evidence polarity**: POSITIVE
- **Key quote**: "MISSING Communication dimension in PR 1C envelope adapter — added mapping for `allowed_channels` and `notification_policy` from Communication constraint dimension. All 5 canonical PACT dimensions now covered."
- **Craft skill hint**: Five-dimension completeness check — always verify all 5 EATP/PACT constraint dimensions are present.
- **Notes**: NO frontmatter. Directly maps to canonical five-dimension spec concept.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0008-DISCOVERY-session1-parallel-execution.md

- **Type**: DISCOVERY
- **Date**: (undated)
- **Topic**: Session 1 parallel execution — 3/5 PRs complete via isolated worktrees
- **Spec concepts touched**: CO Concept 71 (COC Observation-Instinct-Evolution Pipeline); `rules/autonomous-execution.md` (10x multiplier)
- **Move type**: parallel agent orchestration
- **Evidence polarity**: POSITIVE
- **Key quote**: "Isolated git worktrees prevent file conflicts between parallel agents. Each agent: (1) Gets its own branch and file copy (2) Can run tests independently (3) Commits without interfering with other agents."
- **Craft skill hint**: Worktree-per-agent pattern for safe parallel autonomous execution.
- **Notes**: NO frontmatter. "Key learning: agents need PYTHONPATH overrides" is a concrete autonomous-execution operational detail.

### Entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0009-DECISION-codification-scope.md

- **Type**: DECISION
- **Date**: (undated)
- **Topic**: Codification scope for 23-issue sprint — 3 skills + 6 proposals
- **Spec concepts touched**: CO Concept 37 (Phase — Codify); CO Concept 40 (Observe-Capture-Evolve Pipeline); CO Concept 64 (COC Skill Hierarchy)
- **Move type**: post-sprint codification discipline
- **Evidence polarity**: POSITIVE
- **Key quote**: "The 23 issues span 5 packages but cluster into 3 knowledge domains... PACT governance (9 issues → 1 skill), DataFlow quality (3 issues → 1 skill), Fabric engine (8 issues → 1 skill)"
- **Craft skill hint**: Knowledge-cluster sizing — avoiding one-skill-per-issue bloat.
- **Notes**: NO frontmatter. Surfaces the `coc` vs `coc-py` tier decision — explicit artifact authority chain (`rules/artifact-flow.md`) awareness.

### Entry: loom/kailash-py/workspaces/kailash/journal/0001-DISCOVERY-delegate-streaming-architecture-gap.md

- **Type**: DISCOVERY
- **Date**: 2026-03-29
- **Topic**: Delegate streaming has a type-boundary gap preventing tool event emission
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion); CO Concept 3.3 (Safety Blindness)
- **Move type**: type-boundary architectural audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Layer 2 (`run_turn`): Typed as `AsyncGenerator[str, None]` — can only yield text, discards tool events as a boolean flag"
- **Craft skill hint**: Layer-by-layer type trace when a documented event "never fires."

### Entry: loom/kailash-py/workspaces/kailash/journal/0002-DECISION-emit-from-execute-not-stream.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Emit tool events from `_execute_tool_calls()`, not streaming detection
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 29 (Evidence-Based Completion)
- **Move type**: emission-point semantic correction
- **Evidence polarity**: POSITIVE
- **Key quote**: "Semantically correct: 'tool started' means 'execution started', not 'model started generating JSON'"
- **Craft skill hint**: Choosing the correct event emission point by semantic fit, not proximity to detection site.

### Entry: loom/kailash-py/workspaces/kailash/journal/0003-DECISION-codify-error-sanitization-pattern.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Codified error sanitization as institutional security pattern
- **Spec concepts touched**: CO Concept 37 (Phase — Codify); CO Concept 40 (Observe-Capture-Evolve); CARE Concept 9 (Principle 3 — Transparency as Foundation)
- **Move type**: pattern codification to prevent regression
- **Evidence polarity**: POSITIVE
- **Key quote**: "The red team (R2) found `str(exc)` leaking internal details in 4 locations... After fixing all 4, the pattern should be codified so future development follows the same approach."
- **Craft skill hint**: "Codify after fix" discipline — every recurring fix becomes a skill entry.

### Entry: loom/kailash-py/workspaces/kailash/journal/0004-DISCOVERY-ci-release-failure-modes.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Three CI failure modes discovered during kaizen-agents v0.5.0 release
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CARE Concept 44 (Failure Modes)
- **Move type**: release-path pre-existing failure surfacing
- **Evidence polarity**: NEGATIVE
- **Key quote**: "All three were pre-existing latent issues that only manifested during this specific release sequence (manual release creation + non-container package tag + PR labeling)."
- **Craft skill hint**: Rare-path audit — latent CI failures only surface under unusual combinations.
- **Notes**: Classic "happy-path only tested" pattern.

### Entry: loom/kailash-py/workspaces/kailash/journal/0005-DECISION-docker-hub-distribution.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Publish full SDK image to Docker Hub
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: distribution channel addition
- **Evidence polarity**: POSITIVE
- **Key quote**: "Multi-arch: `linux/amd64` + `linux/arm64`; Non-root user: `kailash` (UID 1000)"
- **Craft skill hint**: Container distribution rollout considerations.
- **Notes**: ⚠ out of scope for COC layer — mostly release-ops detail. Minor security-posture content (non-root user) but no spec-anchored craft lesson.

### Entry: loom/kailash-py/workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Engine layer is broken across projects — COC teaches primitives, traps new users
- **Spec concepts touched**: CO Concept 1 (Institutional Knowledge Thesis); CO Concept 3.2 (Convention Drift); CO Concept 64 (COC Skill Hierarchy); CO Concept 15 (Progressive Disclosure)
- **Move type**: institutional-knowledge audit; friction-gradient inversion detection
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The friction gradient is backwards (simplest working API has highest discovery friction)... This is a Layer 5 (institutional knowledge) failure that perpetuated Layer 2 (agent) failures."
- **Craft skill hint**: Friction-gradient diagnosis — when the easiest-to-use API has the highest discovery cost, the COC is broken.
- **Notes**: Directly cites CO's five-layer model and names the exact failure mode (Layer 5 → Layer 2 cascade). High-value for skill catalog.

### Entry: loom/kailash-py/workspaces/kailash/journal/0007-RISK-agent-api-unused-but-trap.md

- **Type**: RISK
- **Date**: 2026-03-30
- **Topic**: Agent API is unused but remains a trap for new users following docs
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 14 (Master Directive); CO Concept 72 (COC Institutional Knowledge Thesis in Practice)
- **Move type**: documentation-as-trap detection
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The Agent API is documented in `patterns.md` (6 lines) and accepts tools silently. A new user following the quick-start path will: (1) Find `Agent(model='gpt-4', tools=[...])` (2) Deploy it (3) Believe it works (BUG-2 fabricates success)"
- **Craft skill hint**: "Zero adopters but still a trap" — documented broken APIs as a special class of risk.

### Entry: loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Codify three-layer model (Raw → Primitives → Engine) as COC institutional knowledge
- **Spec concepts touched**: CO Concept 1 (Institutional Knowledge Thesis); CO Concept 16 (Framework-First); CO Concept 37 (Phase — Codify); CO Concept 39 (Layer 5 — Learning); CO Concept 64 (COC Skill Hierarchy); CO Concept 67 (COC Defense in Depth)
- **Move type**: multi-artifact codification (rules + skills + specialists + commands)
- **Evidence polarity**: POSITIVE
- **Key quote**: "11 COC artifacts created or updated at the kailash/ source of truth to establish the three-layer model (Raw → Primitives → Engine) and engine-first principle as institutional knowledge."
- **Craft skill hint**: Defense-in-depth codification — the same rule lands in rules, skills, specialists, and commands for Layer 3 coverage.
- **Notes**: Explicitly enumerates artifacts touched and shows the authority chain (kailash/ source of truth → sync-manifest → downstream).

### Entry: loom/kailash-py/workspaces/kailash/journal/0009-DISCOVERY-ci-deploy-production-preexisting-failures.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: deploy-production.yml has 4 pre-existing failures blocking kailash-kaizen releases
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness)
- **Move type**: latent CI failure surfacing; zero-tolerance enforcement
- **Evidence polarity**: NEGATIVE
- **Key quote**: "These failures existed before this session but were never triggered because the last kailash-kaizen release (v2.3.1) was published via a different workflow path. The v2.3.2 release exposed all 4 simultaneously."
- **Craft skill hint**: Zero-tolerance Rule 1 in practice — "if you found it, you own it" applied to CI.
- **Notes**: Directly manifests `rules/zero-tolerance.md` Rule 1 — pre-existing failures fixed immediately, not deferred.

### Entry: loom/kailash-py/workspaces/kailash/journal/0010-DISCOVERY-pypi-tag-push-race-condition.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: PyPI publish workflow fails silently when multiple tags pushed simultaneously
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 29 (Evidence-Based Completion)
- **Move type**: silent-failure detection in release tooling
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Session 5 claimed 4 packages were released to PyPI, but only kailash-kaizen 2.3.2 actually published."
- **Craft skill hint**: Post-release verification — "claimed release" vs "actual PyPI version" reconciliation.
- **Notes**: Perfect example of CO Concept 29 (Evidence-Based Completion) — a claimed completion not backed by evidence.

### Entry: loom/kailash-py/workspaces/kailash/journal/0011-DISCOVERY-pact-internal-only-enforcement-bug.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: PACT `internal_only` enforcement blocked all actions without explicit `is_external=False`
- **Spec concepts touched**: PACT Concept 18 (Communication Dimension); EATP Concept 17 (Element 3 — Constraint Envelope); CARE Concept 6 (Trust Verification Bridge)
- **Move type**: default-value semantic correction in governance primitive
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The semantic difference: 'block unless proven internal' vs 'block when proven external'. The former requires every caller to explicitly declare `is_external=False` for every action, which is unreasonable."
- **Craft skill hint**: Fail-closed vs fail-open defaults in envelope enforcement — when each is appropriate.

### Entry: loom/kailash-py/workspaces/kailash/journal/0012-DECISION-codify-session7-ci-fixes.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Codification of session 7 CI fixes + 4 issue resolutions
- **Spec concepts touched**: CO Concept 37 (Phase — Codify); CO Concept 40 (Observe-Capture-Evolve); CO Concept 64 (COC Skill Hierarchy)
- **Move type**: selective codification discipline
- **Evidence polarity**: POSITIVE
- **Key quote**: "What Was NOT Codified (and why) — SQLite read-back in Express.create(): Implementation detail, not a pattern."
- **Craft skill hint**: Codification triage — distinguishing recurring patterns from one-off bugs.

### Entry: loom/kailash-py/workspaces/kailash/journal/0013-DISCOVERY-cross-sdk-pseudo-posture-shared-bug.md

- **Type**: DISCOVERY
- **Date**: 2026-03-31
- **Topic**: Cross-SDK analysis confirms "pseudo" posture bug shared between py and rs
- **Spec concepts touched**: EATP Concept 65 (Trust Postures — Five-Level); EATP Concept 8 (Five Trust Postures); PACT Concept 36 (Posture-Clearance Interaction)
- **Move type**: cross-SDK spec conformance audit
- **Evidence polarity**: MIXED
- **Key quote**: "`TrustPosture('pseudo')` raised ValueError because the enum value was `'pseudo_agent'`, not `'pseudo'`. All other CARE-spec L1-L5 names parsed correctly."
- **Craft skill hint**: Enum-alias spec-name drift between code and spec; cross-SDK conformance validation.
- **Notes**: Direct CARE/EATP spec concept — posture name mismatch in code vs spec.

### Entry: loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md

- **Type**: DISCOVERY
- **Date**: 2026-03-31
- **Topic**: PACT module has 4 spec-conformance gaps — Python trailing Rust on all 4
- **Spec concepts touched**: PACT Concept 40 (EATP Integration — Governance-Protocol Mapping); PACT Concept 10 (Monotonic Tightening Invariant); PACT Concept 19 (Verification Gradient); PACT Concept 44 (Organizational Compilation); PACT Concept 35 (Bridges); PACT Concept 51 (Vacant Roles); EATP Concept 15 (Genesis Record); EATP Concept 16 (Delegation Record); EATP Concept 18 (Capability Attestation)
- **Move type**: spec-conformance gap audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "PACT builds its own audit chain (`AuditAnchor`/`AuditChain` in `audit.py`) but never emits the EATP record types (`GenesisRecord`, `DelegationRecord`, `CapabilityAttestation`) from `kailash.trust.chain`. Zero imports from `kailash.trust.chain` exist in the PACT module. Spec Section 5.7 calls this mapping 'normative.'"
- **Craft skill hint**: Spec-to-code conformance audit — "read every 'MUST' in the spec and `grep` for its implementation."
- **Notes**: High-value entry. Directly cites PACT Section 5.7 and maps 4 gaps to specific PACT and EATP concepts. The dimensions missing from `validate_tightening()` (Temporal, Data Access, Communication) are directly the canonical five dimensions.

### Entry: loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md

- **Type**: CONNECTION
- **Date**: 2026-03-31
- **Topic**: PACT and EATP trust chain are adjacent but never connected
- **Spec concepts touched**: PACT Concept 40 (EATP Integration); EATP Concept 14 (Trust Lineage Chain); EATP Concept 15 (Genesis Record); EATP Concept 16 (Delegation Record); EATP Concept 18 (Capability Attestation); PACT Concept 41 (Genesis Record — Organization-Level); PACT Concept 42 (Delegation Record — EATP Backing)
- **Move type**: cross-module integration gap naming
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The modules are structurally adjacent but functionally isolated. PACT manages organizational governance (D/T/R, envelopes, bridges) and emits its own audit trail. EATP manages trust lineage (genesis, capabilities, delegations) with cryptographic verification. They solve complementary problems but don't talk to each other."
- **Craft skill hint**: Adjacent-but-disconnected pattern — when two modules solve complementary spec concepts without the required connection.
- **Notes**: Another high-value entry. Names the exact spec concepts that should bridge the modules.

### Entry: loom/kailash-py/workspaces/kailash/journal/0016-DECISION-pact-sprint-s12-todo-structure.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Sprint S12 todo structure — 14 todos across 5 milestones for PACT conformance
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); CO Concept 34 (Phase — Plan); CO Concept 28 (Approval Gate)
- **Move type**: Phase 02 dependency-ordered planning
- **Evidence polarity**: POSITIVE
- **Key quote**: "Dependency-ordered, not severity-ordered: M3 (MEDIUM) waits for M2 (HIGH) because of the vacant head dependency, not because of severity ranking"
- **Craft skill hint**: Dependency-graph sequencing vs severity sequencing — when each applies.

### Entry: loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: Red team found 3 critical + 5 high issues in PACT sprint S12 todos
- **Spec concepts touched**: CO Concept 36 (Phase — Vet); EATP Concept 16 (Delegation Record); PACT Concept 51 (Vacant Roles); PACT Concept 35 (Bridges)
- **Move type**: pre-implementation red team
- **Evidence polarity**: POSITIVE
- **Key quote**: "C1: TrustStore API mismatch — The todos assumed synchronous, per-record emission to `TrustStore`. But `TrustStore` is async and stores complete `TrustLineageChain` objects, not individual records."
- **Craft skill hint**: Todo-level API-contract verification — catching signature mismatches before implementation begins.
- **Notes**: Shows concrete CO Phase 02 → Phase 04 loop — red-team of todos before /implement.

### Entry: loom/kailash-py/workspaces/kailash/journal/0018-DECISION-five-workspace-parallel-implementation.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Five workspaces analyzed, planned, implemented, and red-teamed in a single session
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); `rules/autonomous-execution.md` (10x multiplier); CO Concept 31 (Standard Six-Phase Workflow)
- **Move type**: massive parallel autonomous execution
- **Evidence polarity**: POSITIVE
- **Key quote**: "The COC autonomous execution model (10x multiplier) enabled parallel analysis, planning, implementation, and red-teaming across all 5 workspaces simultaneously. Cross-workspace dependencies were mapped in Phase 0"
- **Craft skill hint**: Multi-workspace Phase 0 dependency mapping as a precondition for parallel execution.
- **Notes**: Evidence for the autonomous-execution framing. 5292 total tests with 22 pre-existing failures — "zero regressions" from new work.

### Entry: loom/kailash-py/workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Nexus creates 3 independent AsyncLocalRuntime instances — killed Docker Desktop PG
- **Spec concepts touched**: CARE Concept 44 (Failure Modes); CO Concept 17 (Single Source of Truth); CO Concept 16 (Framework-First); CO Concept 19 (Layer 3 — Guardrails)
- **Move type**: resource duplication audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "a simple `DataFlow(auto_migrate=True) + Nexus()` opened 108-118 connections in <2 seconds — killing Docker Desktop PostgreSQL (max_connections=100)."
- **Craft skill hint**: Shared-resource violation audit — when "separate runtime per component" leaks pool budgets.
- **Notes**: Explicitly calls out that `rules/dataflow-pool.md` Rule 6 existed but didn't catch it because the violator was in `kailash.servers` (core SDK), not `kailash-dataflow`. Classic Layer 3 scope-hole.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0001-DISCOVERY-trl-thin-wrapper.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: AlignmentPipeline is a thin TRL wrapper; value lives in lifecycle management
- **Spec concepts touched**: CO Concept 16 (Framework-First)
- **Move type**: value-location audit
- **Evidence polarity**: MIXED
- **Key quote**: "AlignmentPipeline adds **zero training logic** beyond what TRL provides natively... If kailash-align only did training, it should not exist as a package. The lifecycle management justifies it."
- **Craft skill hint**: "Where does our value actually live?" audit — distinguishing wrapper from value.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0002-RISK-gguf-conversion-fragility.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: GGUF conversion is fragile and can fail silently
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 29 (Evidence-Based Completion)
- **Move type**: external-dependency silent-failure audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Critically, failures can be **silent** -- producing a valid-looking GGUF file that crashes only at inference time."
- **Craft skill hint**: Post-conversion validation as a mandatory step when the conversion tool has silent failure modes.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0003-DISCOVERY-llama-cpp-python-resolves-serving-risks.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: llama-cpp-python resolves three serving pipeline risks simultaneously
- **Spec concepts touched**: CO Concept 16 (Framework-First)
- **Move type**: dependency-swap risk reduction
- **Evidence polarity**: POSITIVE
- **Key quote**: "Three Problems, One Solution"
- **Craft skill hint**: Red-team round convergence via dependency substitution — R1 finds the risk, R2 finds the solution.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0004-DECISION-defer-agents-v1.1.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Defer all 4 alignment agents to v1.1
- **Spec concepts touched**: `rules/agent-reasoning.md`; CO Concept 10 (Agent Definition)
- **Move type**: agent-reasoning rule application
- **Evidence polarity**: POSITIVE
- **Key quote**: "The SFT-vs-DPO recommendation is a 4-row lookup table, not a reasoning task... An LLM call to make this recommendation adds latency, cost, and no intelligence. This risks violating the agent-reasoning rule."
- **Craft skill hint**: Agent-vs-script diagnosis — recognizing when a proposed agent is deterministic logic wearing an LLM costume.
- **Notes**: Very direct application of the agent-reasoning rule to scope decisions.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0005-DISCOVERY-delegate-already-supports-local-models.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Kaizen Delegate already has full Ollama and vLLM support via existing adapters
- **Spec concepts touched**: CO Concept 16 (Framework-First)
- **Move type**: existing-capability discovery before building
- **Evidence polarity**: POSITIVE
- **Key quote**: "KaizenModelBridge is **simpler than expected**. It does not need to create new adapters or modify the Delegate system."
- **Craft skill hint**: Framework-first preflight check — "does this already exist?" before scoping new components.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0006-TRADE-OFF-lm-eval-optional-dependency.md

- **Type**: TRADE-OFF
- **Date**: 2026-04-01
- **Topic**: lm-eval as optional dependency trades install simplicity for evaluation UX
- **Spec concepts touched**: CO Concept 15 (Progressive Disclosure)
- **Move type**: dependency scoping trade-off
- **Evidence polarity**: MIXED
- **Key quote**: "Training machines (with GPUs, VRAM constraints) may not need the evaluation harness. Evaluation machines may run benchmarks without needing training infrastructure. The extra system allows these different deployment profiles."
- **Craft skill hint**: Dependency tier decisions based on deployment profile variation.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0007-DISCOVERY-trl-dependency-chain-fragility.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: 5-level dependency chain torch-transformers-trl-peft-accelerate has specific known fragilities
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 17 (Single Source of Truth)
- **Move type**: version-pin risk audit
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Version pinning is not optional -- it is a correctness requirement. Loose pins (`>=0.8`) are functionally bugs."
- **Craft skill hint**: Version-range audit — upper bounds are as load-bearing as lower bounds in ML stacks.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0008-RISK-compound-timeline-slippage.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Session estimate slippage compounds with kailash-ml dependency
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); CO Concept 34 (Phase — Plan)
- **Move type**: cross-workspace estimation compound risk
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Presenting this estimate without the kailash-ml prerequisite context understates the actual calendar commitment by 50-100%."
- **Craft skill hint**: Dependency-chain estimation — compound effects across sequential workspaces.
- **Notes**: References autonomous-execution model explicitly — "parallel agent instances" question at end.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0009-GAP-model-registry-extension-contract.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: No defined extension contract for ModelRegistry that AdapterRegistry needs
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth); CO Concept 29 (Evidence-Based Completion)
- **Move type**: interface contract gap surfacing
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The risk is not that the contract is wrong -- it is that it does not exist at all."
- **Craft skill hint**: Named-but-unspecified interface gap detection — "assume extends X" without reading X.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0010-DISCOVERY-dpo-loss-type-unlocks-14-variants.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: DPO `loss_type` parameter unlocks 14 alignment variants with zero code change
- **Spec concepts touched**: CO Concept 16 (Framework-First)
- **Move type**: configuration-level capability discovery
- **Evidence polarity**: POSITIVE
- **Key quote**: "IPO, SimPO, NCA, CPO, BCO, and 9 other methods are available TODAY through a single config field addition — no new trainer code, no new data formats, no new pipeline logic."
- **Craft skill hint**: Config-level capability surface audit — reading the underlying library's options before scoping a wrapper.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0011-DECISION-rl-for-llms-in-align-classical-in-ml.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: RL-for-LLMs belongs in kailash-align; classical RL in kailash-ml[rl]
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth)
- **Move type**: package boundary by dependency profile
- **Evidence polarity**: POSITIVE
- **Key quote**: "Putting GRPO in kailash-ml[rl] would force LLM users to install gymnasium; putting SB3 PPO in kailash-align would force alignment users to install stable-baselines3"
- **Craft skill hint**: Package boundary drawn by transitive-dependency cost.
- **Notes**: Reversed by 0016 — "start as extra, extract later" deemed a false economy.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0012-GAP-kailash-rs-zero-alignment-capability.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: kailash-rs has zero alignment/RL capability
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: cross-SDK capability gap
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Users who fine-tune models in Python (kailash-align) cannot serve them in Rust production services without bypassing the Kailash ecosystem entirely"
- **Craft skill hint**: Cross-SDK capability parity auditing.
- **Notes**: ⚠ out of scope for COC craft — mostly strategic scope planning.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0013-RISK-superficial-kailash-rs-analysis.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Previous kailash-rs analysis was superficial — missed 179-todo native Rust ML roadmap
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion); CO Concept 36 (Phase — Vet)
- **Move type**: research quality self-critique
- **Evidence polarity**: NEGATIVE
- **Key quote**: "The analysis read README files instead of source code, and missed the `workspaces/kailash-ml-crate/` workspace entirely on first pass."
- **Craft skill hint**: "Grep-vs-read" diagnostic — recognizing when research is keyword-matching rather than reading.
- **Notes**: High-value self-critique naming the exact failure mode (grep instead of read, missed workspace briefs).

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0014-CONNECTION-cross-sdk-ml-strategy.md

- **Type**: CONNECTION
- **Date**: 2026-04-01
- **Topic**: Cross-SDK ML strategy — Python wraps, Rust implements natively
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: multi-repo strategic positioning
- **Evidence polarity**: MIXED
- **Key quote**: "Python SDK wraps: sklearn, lightgbm, TRL, stable-baselines3 — leverage existing ecosystem; Rust SDK implements: Native algorithms, faer linear algebra"
- **Craft skill hint**: Cross-SDK strategy table as institutional knowledge.
- **Notes**: ⚠ out of scope for COC craft layer — strategic positioning, not craft.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0015-DECISION-todo-expansion-scope.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Todo expansion from 13 to 27 — full alignment landscape + classical RL
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); CO Concept 34 (Phase — Plan)
- **Move type**: user-directed scope expansion during Phase 02
- **Evidence polarity**: MIXED
- **Key quote**: "The user explicitly called out the previous scope as too narrow — 'alignment is not just RLHF, GRPO etc too'"
- **Craft skill hint**: User-as-approval-gate intervention during planning — when to expand vs defend scope.
- **Notes**: Direct human intervention at CO Phase 02 approval gate.

### Entry: loom/kailash-py/workspaces/kailash-align/journal/0016-DECISION-kailash-rl-separate-package.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: kailash-rl is a separate package, not kailash-ml[rl] (reverses 0011)
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth); CARE Concept 13 (Principle 7 — Evolutionary Trust)
- **Move type**: explicit decision reversal under user directive
- **Evidence polarity**: POSITIVE
- **Key quote**: "'Start as extra, extract later' is a false economy — the extraction cost is real, the architectural clarity of separation is immediate, and COC autonomous execution makes the implementation cost irrelevant."
- **Craft skill hint**: Autonomous-execution-aware scoping — when implementation cost changes decision calculus.
- **Notes**: Journal author: `human` — "Don't waste time separating later. Always go for the best optimal outcome regardless of costs." Direct programme-owner intervention.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0001-DISCOVERY-no-existing-ml-code.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: kailash-py contains zero ML code — genuinely greenfield
- **Spec concepts touched**: CO Concept 1 (Institutional Knowledge Thesis)
- **Move type**: greenfield baseline audit
- **Evidence polarity**: MIXED
- **Key quote**: "kailash-ml has no legacy code to work around or integrate with"
- **Craft skill hint**: Greenfield confirmation audit — disambiguating "new package" from "new in this package."

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0002-RISK-scope-9-engines-6-agents.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: 9 engines + 6 agents may exceed quality threshold for v1
- **Spec concepts touched**: CO Concept 48 (Quality Criteria — Coverage); CO Concept 49 (Quality Criteria — Enforcement Reliability)
- **Move type**: scope-vs-quality probabilistic analysis
- **Evidence polarity**: NEGATIVE
- **Key quote**: "If each engine has a 90% probability of shipping production-quality, the probability of ALL 9 being production-quality is 0.9^9 = 39%."
- **Craft skill hint**: Quality-probability math for scope decisions.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0003-TRADE-OFF-polars-vs-pandas.md

- **Type**: TRADE-OFF
- **Date**: 2026-04-01
- **Topic**: polars-only with interop vs pandas-native
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth)
- **Move type**: data-representation trade-off analysis
- **Evidence polarity**: MIXED
- **Key quote**: "The 'invisible pandas dependency' means polars-only is a user-facing claim, not a technical reality."
- **Craft skill hint**: Honest-claim audit — surfacing the "invisible" dependencies in a "none of that" claim.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0004-DECISION-quality-tiering-for-v1-engines.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Quality tiering strategy for 9 engines in v1 (P0/P1/P2)
- **Spec concepts touched**: CO Concept 48 (Quality Criteria — Coverage); CO Concept 49 (Quality Criteria — Enforcement Reliability)
- **Move type**: scope preservation via tiered guarantees
- **Evidence polarity**: POSITIVE
- **Key quote**: "Ship all 9, but set explicit quality expectations. P2 engines get an `@experimental` decorator and documentation warning."
- **Craft skill hint**: `@experimental` tiering as alternative to scope cut.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0005-DISCOVERY-dataflow-express-limits-for-ml.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: DataFlow Express API has limitations for ML-specific queries
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 19 (Layer 3 — Guardrails)
- **Move type**: engine-layer scope probe
- **Evidence polarity**: MIXED
- **Key quote**: "FeatureStore will be a mixed-layer engine: Express for metadata, ConnectionManager for feature data"
- **Craft skill hint**: Mixed-layer engine pattern — when to drop from engine to primitives.
- **Notes**: Explicitly references `rules/framework-first.md` — mixed-layer is permitted for "complex multi-step workflows requiring explicit node wiring."

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0006-DISCOVERY-guardrail4-baseline-doubles-compute.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Agent Guardrail 4 baseline comparison doubles compute cost silently
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 29 (Evidence-Based Completion); `rules/agent-reasoning.md`
- **Move type**: guardrail cost analysis
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Guardrail 4 doubles that to 60 minutes. The user pays the full algorithmic baseline cost even when using agents."
- **Craft skill hint**: Guardrail-cost audit — does the safety mechanism double compute in the happy path?
- **Notes**: Also names Guardrail 1 epistemic weakness — "LLM self-assessed confidence is not a calibrated probability."

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0007-RISK-integration-testing-combinatorial-explosion.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Integration testing matrix is combinatorial
- **Spec concepts touched**: CO Concept 50 (Quality Criteria — Workflow Discipline); `rules/testing.md`
- **Move type**: test-matrix sizing
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Without a prioritization strategy, the test suite will either be incomplete (missing critical combinations) or prohibitively slow (testing everything)."
- **Craft skill hint**: Critical-path vs full-matrix tiering for test suites.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0008-GAP-protocol-method-signatures-undefined.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: kailash-ml-protocols method signatures are undefined despite being a permanent interface
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth); CO Concept 29 (Evidence-Based Completion); CO Concept 14 (Master Directive)
- **Move type**: permanent-interface specification gap
- **Evidence polarity**: NEGATIVE
- **Key quote**: "protocol interfaces are effectively permanent... 'Protocol methods cannot be removed without breaking implementations.'... A poorly designed protocol locks three packages into a bad interface."
- **Craft skill hint**: "Permanent interface" gate — signatures must be frozen before /implement starts on protocol-dependent packages.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0009-RISK-protocol-surface-and-cross-workspace-contracts.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Protocol surface and cross-workspace contracts underspecified
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth); CO Concept 1 (Institutional Knowledge Thesis); CO Concept 16 (Framework-First)
- **Move type**: protocol conservatism + hidden contract naming
- **Evidence polarity**: MIXED
- **Key quote**: "The protocols package breaks the circular dependency between kailash-ml and kailash-kaizen, but it does NOT protect the kailash-ml -> kailash-align contract. That contract is raw class inheritance"
- **Craft skill hint**: "Hidden contract" detection — inheritance-based coupling that bypasses the declared protocol.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0010-DECISION-rust-engine-replaces-sklearn-wrapping.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: kailash-ml Python will use Rust engine instead of wrapping scikit-learn
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth)
- **Move type**: long-horizon implementation swap
- **Evidence polarity**: POSITIVE
- **Key quote**: "This decision does NOT block current kailash-ml Python work — the current sklearn-wrapping plan can proceed as a TEMPORARY implementation that will be replaced by Rust bindings when ready. However, avoid deep investment in Python-specific ML algorithm code that the Rust engine will supersede."
- **Craft skill hint**: "Temporary implementation" scoping — what to avoid deep investment in when a replacement is committed.
- **Notes**: Journal author: `human` — direct founder intervention reshaping the plan.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0011-RISK-sql-type-injection-in-feature-sql.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: SQL type injection via unvalidated `sql_type` parameter
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); `rules/security.md`; EATP Concept 66 (Constraint Dimensions — Data Access); PACT Concept 17 (Data Access Dimension)
- **Move type**: security audit in red team phase
- **Evidence polarity**: NEGATIVE
- **Key quote**: "While column names were validated via `_validate_identifier()`, the SQL type strings had zero validation. A crafted `sql_type` like `'TEXT); DROP TABLE users; --'` would create a SQL injection in the CREATE TABLE statement."
- **Craft skill hint**: "Validate-at-interpolation" defense-in-depth — don't trust the caller to validate.

### Entry: loom/kailash-py/workspaces/kailash-ml/journal/0012-DECISION-shared-module-deduplication.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Extracted shared constants into `_shared.py` to eliminate cross-engine duplication
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth)
- **Move type**: DRY enforcement for security allowlists
- **Evidence polarity**: POSITIVE
- **Key quote**: "The security allowlist duplication was particularly dangerous — adding a new allowed model prefix (e.g., `catboost.`) would require updating both files independently, risking divergence."
- **Craft skill hint**: Allowlist-drift prevention via single source of truth.

### Entry: loom/kailash-py/workspaces/mcp-platform-server/journal/0001-DISCOVERY-module-path-conflict.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Existing `kailash.mcp` module conflicts with platform server placement
- **Spec concepts touched**: CO Concept 1 (Institutional Knowledge Thesis); CO Concept 29 (Evidence-Based Completion)
- **Move type**: pre-implementation audit catches blocking conflict
- **Evidence polarity**: NEGATIVE
- **Key quote**: "If we had discovered this conflict during implementation rather than analysis, it would have caused a refactoring detour mid-session."
- **Craft skill hint**: "Verify the file exists" preflight before committing to a path claim in the brief.

### Entry: loom/kailash-py/workspaces/mcp-platform-server/journal/0002-DISCOVERY-static-introspection-requirement.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: MCP server must use AST-based static introspection, not runtime registries
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 1 (Institutional Knowledge Thesis); EATP Concept 49 (MCP + EATP Integration)
- **Move type**: runtime-model correction
- **Evidence polarity**: MIXED
- **Key quote**: "The MCP server process has no DataFlow database URL, no Nexus app, no running application. It has only the `project_root` directory and the installed packages."
- **Craft skill hint**: Process-boundary constraint mapping — when the subprocess can't access in-process state.

### Entry: loom/kailash-py/workspaces/mcp-platform-server/journal/0003-RISK-nexus-workspace-overlap.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Overlap with nexus-transport-refactor workspace on MCP code deletion
- **Spec concepts touched**: CO Concept 27 (Structured Workflow)
- **Move type**: cross-workspace file-conflict mapping
- **Evidence polarity**: NEGATIVE
- **Key quote**: "If both workspaces modify or delete the same Nexus MCP files: Git merge conflicts are guaranteed"
- **Craft skill hint**: Cross-workspace file-set overlap audit before parallel scheduling.

### Entry: loom/kailash-py/workspaces/mcp-platform-server/journal/0004-RISK-ast-scanning-completeness-gap.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: AST-based scanning cannot guarantee complete introspection results
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 9 (Principle 3 — Transparency as Foundation)
- **Move type**: partial-coverage disclosure pattern
- **Evidence polarity**: NEGATIVE
- **Key quote**: "If the platform server returns 8 of 10 models to Claude Code, the LLM trusts these as complete and generates code that does not account for the 2 missing models. This is more dangerous than returning 0 models"
- **Craft skill hint**: `scan_info` metadata pattern — always disclose detection boundaries so consumers can weigh trust.
- **Notes**: Maps to CARE Principle 3 (Transparency) — "partial results create false confidence" unless limitations are explicit.

### Entry: loom/kailash-py/workspaces/nexus-transport-refactor/journal/0001-DISCOVERY-core-py-monolith-verified-coupling-map.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: core.py monolith is exactly 2,436 lines with 11+ verified FastAPI coupling points
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth); CO Concept 16 (Framework-First); CO Concept 29 (Evidence-Based Completion)
- **Move type**: line-accurate coupling audit
- **Evidence polarity**: MIXED
- **Key quote**: "The audit discovered 8 additional coupling points the brief did not mention"
- **Craft skill hint**: Brief-verification audit — counting claimed coupling points and finding the unreported ones.

### Entry: loom/kailash-py/workspaces/nexus-transport-refactor/journal/0002-RISK-b0b-transport-extraction-threatens-external-plugins-and-middleware-ordering.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: B0b transport extraction risks breaking external plugins and middleware ordering
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); CO Concept 29 (Evidence-Based Completion); CARE Concept 44 (Failure Modes)
- **Move type**: public-surface breakage risk mapping
- **Evidence polarity**: NEGATIVE
- **Key quote**: "External plugins are opaque -- they cannot be audited... middleware ordering bugs are notoriously difficult to diagnose because they manifest as subtle behavioral differences rather than errors."
- **Craft skill hint**: Opaque-consumer risk accounting in refactors.

### Entry: loom/kailash-py/workspaces/nexus-transport-refactor/journal/0003-CONNECTION-nexus-refactor-enables-mcp-consolidation.md

- **Type**: CONNECTION
- **Date**: 2026-04-01
- **Topic**: Nexus transport abstraction is prerequisite for MCP platform server consolidation
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); CO Concept 16 (Framework-First)
- **Move type**: cross-workspace sequencing rationale
- **Evidence polarity**: POSITIVE
- **Key quote**: "The `HandlerRegistry` from B0a is the key enabler: handlers register once and are exposed through all transports. Without the registry abstraction, each transport must independently discover and register handlers"
- **Craft skill hint**: Prerequisite-first sequencing — when one workspace must complete before another can begin.

### Entry: loom/kailash-py/workspaces/nexus-transport-refactor/journal/0004-DISCOVERY-b0a-atomicity-and-cross-workspace-safety.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: B0a is atomically decomposable into 4 independent commits with clean rollback
- **Spec concepts touched**: CO Concept 27 (Structured Workflow); `rules/git.md` (atomic commits); CARE Concept 44 (Failure Modes)
- **Move type**: atomic-commit decomposition for safe refactoring
- **Evidence polarity**: POSITIVE
- **Key quote**: "The B0a scope, which initially appeared as a single large refactoring session, breaks cleanly into four commits with no circular dependencies"
- **Craft skill hint**: Refactor-level commit decomposition — independent rollback units.

### Entry: loom/kailash-py/workspaces/pact-309-308/journal/0001-DISCOVERY-fsm-grant-revoke-conflict.md

- **Type**: DISCOVERY
- **Date**: 2026-04-06
- **Topic**: FSM validation in `grant_clearance()` conflicts with revoke->re-grant flow
- **Spec concepts touched**: PACT Concept 38 (Pre-Clearance Workflow); PACT Concept 31 (Clearance Assignment Authority); CARE Concept 43 (State Machines)
- **Move type**: FSM transition conflict resolution
- **Evidence polarity**: POSITIVE
- **Key quote**: "REVOKED is a terminal state with no outgoing transitions, so re-granting a revoked role is rejected... `grant_clearance()` only validates FSM transitions for 'living' states (PENDING, ACTIVE, SUSPENDED)."
- **Craft skill hint**: Terminal-state vs overwrite semantics in clearance lifecycle FSMs.
- **Notes**: Sparse frontmatter (no author, no session_turn).

### Entry: loom/kailash-py/workspaces/pact-309-308/journal/0002-DISCOVERY-suspended-reinstatement-path.md

- **Type**: DISCOVERY
- **Date**: 2026-04-06
- **Topic**: SUSPENDED->ACTIVE reinstatement is essential
- **Spec concepts touched**: PACT Concept 38 (Pre-Clearance Workflow); CARE Concept 43 (State Machines); EATP Concept 37 (Posture Transition Triggers)
- **Move type**: FSM transition set correction
- **Evidence polarity**: POSITIVE
- **Key quote**: "The FSM must include `SUSPENDED -> ACTIVE` for direct reinstatement... Only REVOKED is truly terminal."
- **Craft skill hint**: SUSPENDED vs REVOKED semantic distinction in clearance lifecycle.
- **Notes**: Sparse frontmatter.

### Entry: loom/kailash-py/workspaces/pact-309-308/journal/0003-DISCOVERY-308-already-at-l1.md

- **Type**: DISCOVERY
- **Date**: 2026-04-06
- **Topic**: All #308 governance helpers already at L1 in kailash-py
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion)
- **Move type**: issue-premise verification
- **Evidence polarity**: POSITIVE
- **Key quote**: "The issue's premise that they need to move from L3 to L1 is incorrect for Python."
- **Craft skill hint**: Verify-the-premise audit — cross-SDK issues may misstate the Python state.
- **Notes**: Sparse frontmatter. Minor entry — shows "no work required" is a valid analysis outcome.

### Entry: loom/kailash-py/docs/adr/0017-llm-provider-architecture.md

- **Type**: ADR
- **Date**: undated (LLMAgentNode / OpenAI-era)
- **Topic**: LLM provider architecture — abstract provider with registry
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth); CO Concept 11 (Routing Rules)
- **Move type**: extensibility via base class + registry
- **Evidence polarity**: POSITIVE
- **Key quote**: "Dependency Isolation: Provider-specific dependencies are isolated"
- **Craft skill hint**: Provider-registry pattern as extension mechanism.
- **Notes**: ADR number `0017` is reused by a second ADR (multi-workflow) — duplicate numbering flag.

### Entry: loom/kailash-py/docs/adr/0017-multi-workflow-api-architecture.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Multi-workflow API gateway architecture
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 27 (Structured Workflow)
- **Move type**: unified gateway over per-workflow servers
- **Evidence polarity**: POSITIVE
- **Key quote**: "Single entry point for all workflows; Centralized management and monitoring"
- **Craft skill hint**: Gateway pattern for multi-workflow deployment.
- **Notes**: ADR number `0017` duplicates LLM provider ADR — numbering collision flag.

### Entry: loom/kailash-py/docs/adr/0018-http-rest-client-architecture.md

- **Type**: ADR
- **Date**: undated
- **Topic**: HTTP and REST client architecture — layered urllib-based
- **Spec concepts touched**: CO Concept 16 (Framework-First); CO Concept 17 (Single Source of Truth)
- **Move type**: layered dependency-minimal client
- **Evidence polarity**: POSITIVE
- **Key quote**: "Minimal Dependencies: Use standard library (urllib) instead of external libraries"
- **Craft skill hint**: Dependency-minimization decision when adding enterprise clients.

### Entry: loom/kailash-py/docs/adr/0051-tier3-advanced-search-filters.md

- **Type**: ADR
- **Date**: undated (Kailash Studio tier-3)
- **Topic**: Advanced search filters for workflow discovery
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: product feature architecture
- **Evidence polarity**: POSITIVE
- **Key quote**: "Query Builder Pattern... Centralized query logic"
- **Craft skill hint**: Query builder pattern as enforcement boundary.
- **Notes**: ⚠ out of scope — Kailash Studio product UI, not CO/COC craft.

### Entry: loom/kailash-py/docs/adr/0051-version-history-ui-component.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Version history UI component architecture
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: UI progressive disclosure pattern
- **Evidence polarity**: POSITIVE
- **Key quote**: "Drawer-based progressive disclosure"
- **Craft skill hint**: Progressive disclosure in UI (related to CO Concept 15 but applied to UX not knowledge).
- **Notes**: ⚠ out of scope — UI product work. ADR number `0051` collides with two others.

### Entry: loom/kailash-py/docs/adr/0051-vscode-typescript-bridge.md

- **Type**: ADR
- **Date**: undated
- **Topic**: VS Code TypeScript bridge via LSP
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: subprocess IPC architecture
- **Evidence polarity**: POSITIVE
- **Key quote**: "Process Isolation: Python server runs in separate process, crash won't affect VS Code"
- **Craft skill hint**: Process-isolation rationale for crash containment.
- **Notes**: ⚠ out of scope — editor tooling, not CO/COC craft. Third ADR `0051` — numbering chaos.

### Entry: loom/kailash-py/docs/adr/0052-tier3-collaboration-presence.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Real-time collaboration presence indicators
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: multi-user presence UI
- **Evidence polarity**: POSITIVE
- **Key quote**: "Throttled Cursor Updates (60fps)"
- **Craft skill hint**: Throttling pattern for real-time updates.
- **Notes**: ⚠ out of scope — Kailash Studio product UI.

### Entry: loom/kailash-py/docs/adr/0052-ui-enhancement-prioritization.md

- **Type**: ADR
- **Date**: 2025-10-05
- **Topic**: UI enhancement prioritization strategy (three-tier, enterprise-first)
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: ROI-based feature tiering
- **Evidence polarity**: POSITIVE
- **Key quote**: "Enterprise-First Prioritization Strategy"
- **Craft skill hint**: Tiered prioritization by ROI.
- **Notes**: ⚠ out of scope — product strategy doc. Second `0052` — numbering collision.

### Entry: loom/kailash-py/docs/adr/0053-tier3-load-testing-scripts.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Load testing script generation for multiple frameworks
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: multi-framework code generation
- **Evidence polarity**: POSITIVE
- **Key quote**: "Multi-Framework Support... User Choice: Different teams prefer different frameworks"
- **Craft skill hint**: Multi-framework template generation.
- **Notes**: ⚠ out of scope — Kailash Studio product.

### Entry: loom/kailash-py/docs/adr/0054-tier3-community-marketplace.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Community marketplace for workflow sharing
- **Spec concepts touched**: ⚠ no clean mapping
- **Move type**: marketplace product architecture
- **Evidence polarity**: POSITIVE
- **Key quote**: "Marketplace Economy: Potential future monetization"
- **Craft skill hint**: Content-moderation + curation pattern.
- **Notes**: ⚠ out of scope — product feature.

### Entry: loom/kailash-py/docs/adr/0055-kailash-studio-dual-persona-foundation.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Kailash Studio dual-persona shared foundation (no-code + code-first)
- **Spec concepts touched**: CO Concept 1 (Institutional Knowledge Thesis); CO Concept 2 (Brilliant New Hire Principle); CO Concept 16 (Framework-First)
- **Move type**: intermediate representation to unify personas
- **Evidence polarity**: POSITIVE
- **Key quote**: "Canvas JSON is the ONLY workflow representation, forcing developers into a no-code workflow that is unnatural for their skillset."
- **Craft skill hint**: Universal intermediate representation to avoid persona exclusion.
- **Notes**: Partially in scope — the "persona exclusion" framing parallels CO's "brilliant new hire" but it's product design not COC craft. Borderline.

### Entry: loom/kailash-py/docs/adr/0056-phase-1a-blocker-resolution.md

- **Type**: ADR
- **Date**: undated
- **Topic**: Phase 1A blocker resolution — scenario execution vs unit tests
- **Spec concepts touched**: CO Concept 29 (Evidence-Based Completion); CO Concept 50 (Quality Criteria — Workflow Discipline); CO Concept 36 (Phase — Vet)
- **Move type**: completion criteria correction
- **Evidence polarity**: NEGATIVE-turned-corrective
- **Key quote**: "Test-First Development (TDD) focused on unit tests, not E2E scenarios... Intermediate review validated code quality, not scenario completion"
- **Craft skill hint**: Scenario-vs-unit-test completion audit — "tests pass" != "user can complete the workflow."
- **Notes**: Direct application of CO Concept 29 (Evidence-Based Completion) — a "complete" claim not backed by scenario evidence.

### Entry: loom/kailash-py/docs/adr/0057-async-sql-pytest-event-loop-pool-key-fix.md

- **Type**: ADR
- **Date**: undated
- **Topic**: AsyncSQLDatabaseNode pytest event loop pool key fix
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness); `rules/testing.md`
- **Move type**: test-environment detection as adaptive behavior
- **Evidence polarity**: POSITIVE
- **Key quote**: "Testing: Excludes event loop ID for test-to-test reuse"
- **Craft skill hint**: Test-env detection as a legitimate mode-switching pattern (when production state is incompatible with test framework behavior).
- **Notes**: Borderline — a test-specific fix rather than core CO/COC craft lesson.

## Summary

**Entries processed:** 98 (85 workspace journal entries + 13 ADRs)

**Type distribution (workspace entries):**

- DISCOVERY: 34
- DECISION: 23
- RISK: 13
- GAP: 6
- CONNECTION: 8
- TRADE-OFF: 2

**Evidence polarity distribution:**

- NEGATIVE (bug / gap / anti-pattern evidence): 31
- POSITIVE (pattern / correction / success evidence): 38
- MIXED: 29

**Top 5 most-cited spec concepts (batch):**

1. **CO Concept 16 (Framework-First)** — 20 entries
2. **CO Concept 17 (Single Source of Truth)** — 16 entries
3. **CO Concept 29 (Evidence-Based Completion)** — 15 entries
4. **CO Concept 3.3 (Safety Blindness)** — 12 entries
5. **CO Concept 1 (Institutional Knowledge Thesis)** — 7 entries

Secondary cluster: **CO Concept 27 (Structured Workflow)** — 7 entries; **CO Concept 37 (Phase — Codify)** — 4 entries; **PACT Concept 10 (Monotonic Tightening Invariant)** — 2 entries; **PACT Concept 40 (EATP Integration)** — 3 entries; **PACT Concept 18 (Communication Dimension)** — 3 entries.

**Concepts notably absent from this batch:**

- CARE first-principle concepts 7-14 (autonomy, choice, transparency, continuous operation, accountability, degradation, evolutionary trust, purpose alignment) — only touched indirectly. Batch is engineering-heavy, not philosophy-heavy.
- CARE Mirror Thesis (Concepts 3, 22), Knowledge Ledger (35, 36), Workspaces (37, 38), Uncertainty (39, 40, 41), Human competencies (23-28, 45-52) — zero direct citations.
- EATP reasoning traces, confidentiality levels, interop formats (Concepts 20-23, 73-88) — zero direct citations.
- PACT D/T/R Grammar (Concepts 4-9), Knowledge Clearance (Concepts 25-37) — only `Communication Dimension` and `Monotonic Tightening` invoked.

The batch is dominated by **COC Layer 2/3 concerns**: framework-first, single source of truth, safety blindness, evidence-based completion, structured workflow. This matches expectation — kailash-py workspaces are engineering journals, not governance/philosophy journals.

**Out-of-scope entries (⚠):** 11 entries flagged

- `workspaces/data-fabric-engine/journal/0002-DISCOVERY-pipeline-first-cache-is-the-real-innovation.md` (competitive strategy)
- `workspaces/data-fabric-engine/journal/0012-CONNECTION-competitor-patterns-inform-fabric-design.md` (competitive research)
- `workspaces/kailash/journal/0005-DECISION-docker-hub-distribution.md` (release-ops)
- `workspaces/kailash-align/journal/0012-GAP-kailash-rs-zero-alignment-capability.md` (cross-SDK strategy)
- `workspaces/kailash-align/journal/0014-CONNECTION-cross-sdk-ml-strategy.md` (strategy table)
- `docs/adr/0051-tier3-advanced-search-filters.md` (Kailash Studio UI)
- `docs/adr/0051-version-history-ui-component.md` (Kailash Studio UI)
- `docs/adr/0051-vscode-typescript-bridge.md` (editor tooling)
- `docs/adr/0052-tier3-collaboration-presence.md` (Kailash Studio UI)
- `docs/adr/0052-ui-enhancement-prioritization.md` (product strategy)
- `docs/adr/0053-tier3-load-testing-scripts.md` (Kailash Studio feature)
- `docs/adr/0054-tier3-community-marketplace.md` (Kailash Studio feature)

That is ~12 entries out of 98 (~12%), concentrated in Kailash Studio ADRs and cross-SDK strategy docs. Mostly product-surface work that has no CO/COC craft content beyond general engineering hygiene.

**Contradictions and WIP flags:**

- **Data-fabric workspace 0001 vs 0004**: Decision 0001 ("DataFlow extension, not new package") is superseded by 0004 ("DataFlow IS the fabric") one day later. Both preserved per journal immutability.
- **kailash-align 0011 vs 0016**: Decision 0011 placed RL under `kailash-ml[rl]`; decision 0016 reverses it and makes `kailash-rl` a first-class package. The reversal is explicit and authored by the human founder.
- **kailash-align 0013 self-critique**: Flags that earlier analysis in the same workspace (doc 11, first version) was superficial. The workspace internally documents the correction.
- **kailash/0014 and kailash/0015**: Identify that PACT and EATP modules are adjacent but not wired, with the Python SDK trailing Rust on all 4 spec-conformance gaps. These are consistent with each other, not contradictory, but they name a spec gap that was live at the time of writing.
- **pact-309-308/0003**: The issue premise (#308) is explicitly incorrect for Python. The entry closes the issue with "no Python work required" — a valid analysis outcome but unusual.

**Frontmatter quality flags:**

- **gh-issues-consolidation workspace**: 9 of 9 entries have NO YAML frontmatter (only an `issue` field for some). Violates `rules/journal.md`.
- **pact-309-308 workspace**: 3 of 3 entries have sparse frontmatter (only `type`, `date`, `issue` — no author, no session_turn, no tags).
- **ADRs**: Most are undated; ADR numbering collisions (three `0051`, two `0017`, two `0052`) indicate parallel ADR authorship without a central sequence authority.

**Numbering collisions flagged:**

- ADR `0017`: LLM provider + multi-workflow API (two distinct docs at same number)
- ADR `0051`: advanced search + version history + VS Code bridge (three distinct docs at same number)
- ADR `0052`: collaboration presence + UI enhancement prioritization (two distinct docs at same number)

**Most craft-productive entries (candidates for priority skill atom extraction):**

1. `workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md` — Friction-gradient inversion + Layer 5→Layer 2 cascade, cites CO layers directly
2. `workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md` — 11-artifact defense-in-depth codification
3. `workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md` — Spec-to-code conformance audit methodology
4. `workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md` — Adjacent-but-disconnected module pattern
5. `workspaces/kailash-align/journal/0004-DECISION-defer-agents-v1.1.md` — Direct application of agent-reasoning rule
6. `workspaces/dataflow-enhancements/journal/0002-RISK-eventbus-wildcard-gap-and-synchronous-dispatch.md` — Silent failure pattern with quantified subscriber bound
7. `workspaces/gh-issues-consolidation/journal/0008-DISCOVERY-session1-parallel-execution.md` — Worktree-per-agent pattern for autonomous parallel execution
8. `workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md` — Layer 3 scope-hole in guardrail rules (`rules/dataflow-pool.md` didn't catch the violation because it was in core SDK, not kailash-dataflow)
9. `workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md` — Pre-implementation red team of todos catching API contract mismatches
10. `workspaces/nexus-transport-refactor/journal/0004-DISCOVERY-b0a-atomicity-and-cross-workspace-safety.md` — Atomic commit decomposition for safe refactoring
