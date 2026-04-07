---
destination: forge
layer: reverse-index
batch: loom-meta
corpus_scope: loom/journal + terrene/journal
date: 2026-04-07
entries_processed: 52
entries_with_mapping: 52
entries_flagged: 0
---

# Reverse Index — COC Layer — Loom Meta Journal

This reverse index classifies 52 journal entries (50 from `loom/journal/` + 2 from `terrene/journal/`) against the four Foundation standards' canonical concept indices. Each entry is mapped to specific concepts (cited by number and name), assigned a move type, polarity, and a craft skill hint. Citations use original repo-relative paths (cache prefix stripped).

## Entries

### Entry: loom/journal/0001-DECISION-co-refocusing.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Repositioned the kailash repo identity from "COC coding environment" to "CO Artifact Management Platform" by rewriting CLAUDE.md and reclassifying 26 rules as CO-General vs COC-Specific.
- **Spec concepts touched**: CO §14 (Master Directive), CO §17 (Single Source of Truth), CO §13 (Layer 2 — Context), CO §1 (Institutional Knowledge Thesis)
- **Move type**: artifact authoring (master directive rewrite); brokerage action (rule reclassification)
- **Evidence polarity**: POSITIVE (move made — repo identity corrected, rule taxonomy clarified)
- **Key quote**: "The kailash repo manages COC artifacts across all SDK stacks but was positioned as 'Kailash COC Claude (Python)' — a coding environment identity. No coding happens in this repo."
- **Craft skill hint**: Recognising and correcting CLAUDE.md identity drift when a repo's actual function diverges from its master directive's framing.
- **Notes**: Decision predates the kailash → loom rename (entry 0026). Sets up the brokerage authority chain that 0016 finalises.

### Entry: loom/journal/0002-DECISION-cc-expert-artifacts.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Created four CC-architecture-aware artifacts (claude-code-architect agent, 30-claude-code-patterns skill, cc-artifacts rule, /cc-audit command) so that /codify sessions are not blind to CC artifact best practices.
- **Spec concepts touched**: CO §10 (Agent Definition), CO §13 (Layer 2 — Context), CO §19 (Layer 3 — Guardrails), CO §20 (Rule Classification), CO §39 (Layer 5 — Learning), COC §61 (COC Agent — Security Reviewer (Mandatory))
- **Move type**: artifact authoring (agent + skill + rule + command quartet for an audit capability)
- **Evidence polarity**: POSITIVE (move made; converged after one red-team round)
- **Key quote**: "Decided AGAINST a validate-cc-artifacts.js hook — it would violate anti-pattern #13 (hooks doing semantic analysis). The rule + agent cover the same ground with semantic understanding that a regex hook cannot match."
- **Craft skill hint**: Choosing between hook (deterministic) and rule+agent (semantic) enforcement based on whether the check requires understanding or pattern-matching.
- **Notes**: Records an explicit boundary decision between Layer 3 hard enforcement and Layer 3 soft enforcement that is later codified as anti-pattern #13.

### Entry: loom/journal/0003-DISCOVERY-cc-artifact-audit.md

- **Type**: DISCOVERY
- **Date**: 2026-03-27
- **Topic**: Systematic audit of CC artifacts across 4 repos (2 BUILD, 2 USE) revealed pervasive cc-artifacts rule violations (descriptions over 120 chars, agents over 400 lines, commands over 150 lines), 1,300 lines/turn of unscoped rule waste, and unclear BUILD-vs-USE artifact distribution.
- **Spec concepts touched**: CO §15 (Progressive Disclosure), CO §17 (Single Source of Truth), CO §19 (Layer 3 — Guardrails), CO §20 (Rule Classification), CO §49 (Quality Criteria — Enforcement Reliability), COC §63 (COC CLAUDE.md Pattern), COC §64 (COC Skill Hierarchy)
- **Move type**: governance call (compliance audit against own rules); brokerage action (BUILD-vs-USE classification)
- **Evidence polarity**: NEGATIVE (the cc-artifacts rule existed but its enforcers — the artifacts themselves — violated it 60% of the time)
- **Key quote**: "Adding `paths:` frontmatter saves ~1,300 lines/turn when not editing those files — a **48% reduction** in per-turn baseline."
- **Craft skill hint**: Auditing rule self-violation: the population of artifacts that should embody a rule are themselves the population most likely to violate it.
- **Notes**: Quantifies "convention drift" (CO §3.2) operationally: rules describe their scope in text but lack the `paths:` frontmatter that would actually enforce that scope.

### Entry: loom/journal/0004-RISK-learning-system-broken.md

- **Type**: RISK
- **Date**: 2026-03-27
- **Topic**: The CO Layer 5 learning system (observe → instinct → evolve → skill) is structurally complete across all 4 repos but functionally broken: 4,579 observations are 99% infrastructure noise, 8 generated instincts all duplicate, zero evolved skills, /codify never reads from the learning store.
- **Spec concepts touched**: CO §7 (Knowledge Compounds), CO §39 (Layer 5 — Learning), CO §40 (Observe-Capture-Evolve Pipeline), CO §41 (Confidence Thresholds), CO §51 (Quality Criteria — Learning Velocity), COC §71 (COC Observation-Instinct-Evolution Pipeline)
- **Move type**: governance call (declaring an entire CO layer non-functional); drift detection
- **Evidence polarity**: NEGATIVE (Layer 5 infrastructure exists but produces zero compounding knowledge)
- **Key quote**: "4,579 observations in kailash-py, 99% are infrastructure noise (session start/stop) ... pre-compact.js violates cc-artifacts Rule 4 (semantic analysis via regex)"
- **Craft skill hint**: Diagnosing structural-vs-functional completeness: a layer can pass an existence audit (file exists, hooks fire, schema is correct) while producing no value because its inputs target the wrong signal.
- **Notes**: Sets up the corrective decision in 0006. The contrast — "4,579 observations → 0 useful skills" — is the canonical case for distinguishing observation noise from observation signal.

### Entry: loom/journal/0005-GAP-redteam-audit-gaps.md

- **Type**: GAP
- **Date**: 2026-03-27
- **Topic**: Red team of the original CC artifact audit (entry 0003) found additional gaps the first pass missed: L5 broken, pre-compact.js semantic-analysis violation, /codify breaking the L5 feedback loop, 59% orphan skill files, two orphan agents, missing /start command in kailash-py, and the project/ skill directory containing only a migration notice.
- **Spec concepts touched**: CO §49 (Quality Criteria — Enforcement Reliability), CO §11 (Routing Rules), CO §39 (Layer 5 — Learning), CO §28 (Approval Gate), COC §66 (COC Validation Hooks)
- **Move type**: governance call (red-team gap discovery)
- **Evidence polarity**: MIXED (some gaps were genuinely missed by the first audit; one — orphan skills — turned out to be an audit-categorisation error, see 0011)
- **Key quote**: "The 59% orphan skill rate could mean skills are designed for progressive disclosure (agent reads SKILL.md, then discovers sub-files). If so, is it a problem that agents don't explicitly reference them?"
- **Craft skill hint**: Distinguishing genuine orphans from progressive-disclosure structures — a skill file unreferenced by an agent's explicit list is not necessarily orphaned if SKILL.md trigger descriptions surface it discoverably.
- **Notes**: Contradicts itself by 0011, where the orphan skill finding is retracted. Useful as an example of red-team finding that itself needs red-teaming.

### Entry: loom/journal/0006-DECISION-learning-must-work.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Declared the L5 Learning System non-negotiable (CO Principle 7 must be operational, not aspirational). Rejected "simplify" and "remove" options. Mandated rewriting observation hooks to capture decision points, real confidence scoring, working evolution, and /codify integration with evolved artifacts.
- **Spec concepts touched**: CO §7 (Knowledge Compounds), CO §39 (Layer 5 — Learning), CO §40 (Observe-Capture-Evolve Pipeline), CO §42 (Knowledge Growth), CO §47 (Knowledge Curator)
- **Move type**: governance call (binding scope decision: L5 stays mandatory)
- **Evidence polarity**: POSITIVE (move made — refused to accept Layer 5 as decorative)
- **Key quote**: "The L5 Learning System is NOT aspirational — it is a core CO principle that MUST work ... Without a working L5: Every session starts cold, Patterns discovered in one session are lost, Institutional knowledge doesn't accumulate, the 10x multiplier (autonomous-execution.md) cannot compound"
- **Craft skill hint**: Refusing to downgrade a CO principle to "aspirational" when its implementation is broken — the corrective is to fix the implementation, not relax the principle.
- **Notes**: Direct application of CO §7 (Knowledge Compounds) as a non-negotiable. Contrasts with the eventual outcome in 0049 where parts of L5 (observation hooks) get aggressively pruned because the model itself proved more capable than rules assumed.

### Entry: loom/journal/0007-GAP-co-completeness-audit.md

- **Type**: GAP
- **Date**: 2026-03-27
- **Topic**: Per-principle and per-layer compliance audit producing scores: L1 70%, L2 50%, L3 55%, L4 65%, L5 10%. Overall CO completeness 50%. Identified specific gaps for each of the 7 principles and 5 layers.
- **Spec concepts touched**: CO §1 (Institutional Knowledge Thesis), CO §2 (Brilliant New Hire), CO §3 (Three Failure Modes), CO §3.1 (Amnesia), CO §3.2 (Convention Drift), CO §3.3 (Safety Blindness), CO §4 (Human-on-the-Loop), CO §5 (Deterministic Enforcement), CO §6 (Bainbridge's Irony), CO §7 (Knowledge Compounds), CO §9 (Layer 1 — Intent), CO §13 (Layer 2 — Context), CO §19 (Layer 3 — Guardrails), CO §26 (Layer 4 — Instructions), CO §39 (Layer 5 — Learning), CO §58 (CO Conformance)
- **Move type**: governance call (per-principle compliance scoring)
- **Evidence polarity**: NEGATIVE (50% overall — half-implemented CO across 7 principles and 5 layers)
- **Key quote**: "CC artifact quality rules are PROMPT-enforced, not DETERMINISTICALLY enforced. validate-workflow.js blocks stubs but nothing blocks a 500-line agent or 300-line command. The cc-artifacts rules are convention, not enforcement. To achieve P5 fully, critical cc-artifacts limits need hook enforcement."
- **Craft skill hint**: Per-layer scoring against MUST requirements as a way to surface which layers are decorative versus operational — and to expose Hard-vs-Soft enforcement gaps within Layer 3 itself.
- **Notes**: This is the canonical reference entry for CO Conformance assessment in practice. Treats CO §58 as a checklist, not a category.

### Entry: loom/journal/0008-DECISION-coc-artifact-scoping.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Codified BUILD-vs-USE artifact scoping: BUILD repos can create/update COC artifacts anywhere in `.claude/`; USE repos can only create artifacts in `project/` subdirectories; users in USE repos must not modify global COC artifacts (those synced from BUILD).
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §13 (Layer 2 — Context), PACT §4 (Department Node Type), PACT §10 (Monotonic Tightening Invariant)
- **Move type**: brokerage action (authority chain for who can author what kind of artifact)
- **Evidence polarity**: POSITIVE (move made — separation codified)
- **Key quote**: "Global COC artifacts represent institutional knowledge maintained by the SDK team. Project artifacts represent domain-specific knowledge created by users for their projects. Separation prevents sync conflicts and preserves user customizations."
- **Craft skill hint**: Authoring a scoped namespace (`project/`) that lets downstream consumers extend a synced template without violating the upstream contract.
- **Notes**: This is the BUILD/USE authority chain in artifact form, the brokerage analogue to PACT's monotonic tightening (downstream cannot widen upstream).

### Entry: loom/journal/0009-DECISION-implementation-plan.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Implementation plan for 100% CO compliance, organised into 5 milestones (M1: L5 Learning, M2: L3 Guardrails, M3: L2 Context, M4: L1+L4, M5: Cross-cutting BUILD/USE distribution). Includes ~30 numbered TODOs with explicit observation schemas for L5 hooks.
- **Spec concepts touched**: CO §39 (Layer 5 — Learning), CO §40 (Observe-Capture-Evolve Pipeline), CO §19 (Layer 3 — Guardrails), CO §13 (Layer 2 — Context), CO §53 (Adoption Phase 1 — Audit), CO §58 (CO Conformance)
- **Move type**: artifact authoring (TODO list); governance call (milestone prioritisation)
- **Evidence polarity**: POSITIVE (move made — concrete plan with file paths and observation schemas)
- **Key quote**: "The instinct-processor expects these observation types: workflow_pattern / node_usage / error_occurrence / error_fix / framework_selection. **No hook emits any of these.** Hooks emit session_start, session_summary, stop, test_pattern, connection_pattern — types the processor ignores."
- **Craft skill hint**: Diagnosing a broken observation pipeline by tracing the schema mismatch between what hooks emit and what the processor consumes — the input format is the contract.
- **Notes**: Demonstrates the craft of writing implementable TODOs that name file paths, hook events, and observation schemas — not just outcomes.

### Entry: loom/journal/0010-DECISION-m2-m5-execution.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Progress report for Milestones 2-5 of the implementation plan: M2 (L3 Guardrails) complete, M3 (L2 Context) partial, M4 (L1+L4 Instructions) partial, M5 (Cross-cutting) substantial. Documents per-TODO actions and what's left.
- **Spec concepts touched**: CO §19 (Layer 3 — Guardrails), CO §13 (Layer 2 — Context), CO §9 (Layer 1 — Intent), CO §26 (Layer 4 — Instructions)
- **Move type**: governance call (execution status report)
- **Evidence polarity**: POSITIVE (move made — most TODOs executed within session)
- **Key quote**: "TODO-2.1: Added `paths:` frontmatter to 7 domain-specific rules in canonical. Estimated ~1,300 lines/turn savings."
- **Craft skill hint**: Reporting partial completion by named TODO with explicit file-and-line evidence for each closed item.
- **Notes**: Pure execution-status entry; useful as a template for /implement reports but light on principle citation.

### Entry: loom/journal/0011-RISK-redteam-r1-findings.md

- **Type**: RISK
- **Date**: 2026-03-27
- **Topic**: Red team R1 of the M2-M5 execution: 4 fixes made (DO/DO NOT examples added, dangling skill references corrected), 3 findings accepted as-is, 3 deferred. Most importantly, retracted the orphan-skill audit finding from entry 0005 as based on a flawed premise.
- **Spec concepts touched**: CO §15 (Progressive Disclosure), CO §19 (Layer 3 — Guardrails), CO §49 (Quality Criteria — Enforcement Reliability), CO §11 (Routing Rules)
- **Move type**: governance call (red-team convergence + retraction of prior finding)
- **Evidence polarity**: MIXED (some fixes made; one prior red-team finding retracted)
- **Key quote**: "The numbered skill directories (01-core-sdk through 30-claude-code-patterns) ARE the distilled Kailash documentation. They are discovered by Claude Code's skill system via SKILL.md trigger descriptions — agents don't need to explicitly reference them by name. These skills are NOT orphans. TODO-3.1 is deleted."
- **Craft skill hint**: Walking back a red-team finding when the underlying audit premise was wrong — and explicitly deleting the corresponding TODO so it doesn't haunt future sessions.
- **Notes**: Direct contradiction with 0005 finding #4 ("59% of skill files are orphans"). The contradiction is itself the lesson: progressive disclosure (CO §15) is invisible to a "is this file referenced by name?" audit.

### Entry: loom/journal/0012-DECISION-session-completion-state.md

- **Type**: DECISION
- **Date**: 2026-03-27
- **Topic**: Session-state snapshot: M2 complete (6/6), M3 complete (2/2 after TODO-3.1 deletion), M4 partial (2/7), M5 substantial (7/8). Background agents running on 107 over-120-char agent descriptions.
- **Spec concepts touched**: CO §28 (Approval Gate), CO §58 (CO Conformance)
- **Move type**: governance call (state checkpoint for next session)
- **Evidence polarity**: POSITIVE (move made — explicit handoff state)
- **Key quote**: "TODO-3.1 (orphan skill audit) was based on a flawed premise. Numbered skill directories ARE the distilled Kailash documentation. They are NOT orphans to audit. Deleted."
- **Craft skill hint**: Producing a clean session-handoff state document that an agent loading the journal in the next session can immediately act on without re-deriving prior decisions.
- **Notes**: Pure handoff artifact; demonstrates the craft of session-completion reporting with named "next session priorities".

### Entry: loom/journal/0013-DECISION-variant-architecture.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Implemented a variant overlay system making kailash/ the single source of truth for all CO/COC artifacts, with language-specific overlays in `variants/py/` and `variants/rs/`, governed by `sync-manifest.yaml`. /sync becomes a two-gate command (Gate 1 inbound classification → Gate 2 outbound distribution).
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §16 (Framework-First), PACT §11 (Role Envelope), PACT §10 (Monotonic Tightening Invariant)
- **Move type**: brokerage action (canonical authority designation + variant overlay system); artifact authoring (sync-manifest schema)
- **Evidence polarity**: POSITIVE (move made — replaces the old direct BUILD-to-template sync)
- **Key quote**: "BUILD repos are originators — they create knowledge via `/codify` proposals, never pull. USE templates are distribution targets — downstream projects merge from them via `/sync`."
- **Craft skill hint**: Designing a variant overlay system where global artifacts are the default and language-specific overlays compose on top, with manifest-level declaration of what is global vs variant vs excluded.
- **Notes**: This is the foundational brokerage-craft entry. Sets up the artifact-flow model that 0014, 0019, 0020, 0021, 0028, 0029, 0048 all elaborate.

### Entry: loom/journal/0014-DISCOVERY-drift-audit.md

- **Type**: DISCOVERY
- **Date**: 2026-03-29
- **Topic**: Audit of py vs rs COC templates revealed 85% drift: only 15% truly shared, 66% of skill files drifted, RS aggressively gutted Python-specific rules (5 reduced to frontmatter), RS expanded 9 rules with Rust patterns, RS had 31 unique skill files vs py's 17.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §3.2 (Convention Drift), CO §15 (Progressive Disclosure)
- **Move type**: drift detection (cross-template diff)
- **Evidence polarity**: NEGATIVE (85% of artifacts had drifted with no mechanism to distinguish intentional vs accidental drift)
- **Key quote**: "No mechanism existed to distinguish intentional (language-specific) from accidental (sync failure) drift. BUILD repos synced directly to templates, bypassing any alignment review."
- **Craft skill hint**: Quantifying artifact drift across templates and naming the structural cause: when there's no Gate-1-style classification step, every BUILD-side change is silently classified as "accidental sync".
- **Notes**: This is the empirical case that motivated the variant architecture (0013). Provides the percentages — 15% shared, 66% drifted, 9 rs-only expansions — that justify the variant overlay model.

### Entry: loom/journal/0015-DECISION-version-tracking-cascade.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Implemented a version cascade where each repo tracks its upstream's version in `.claude/VERSION` (JSON), checks for updates on session start via GitHub raw URL fetch (3s timeout), and `/sync` commands auto-bump target's `upstream.version` after distribution.
- **Spec concepts touched**: CO §13 (Layer 2 — Context), CO §17 (Single Source of Truth), EATP §15 (Element 1 — Genesis Record), EATP §19 (Element 5 — Audit Anchor)
- **Move type**: artifact authoring (VERSION schema + version-utils.js + session-start integration)
- **Evidence polarity**: POSITIVE (move made — version cascade across the chain)
- **Key quote**: "co/ v1.0.0 → co-template, kailash/ → kailash-coc-claude-py/rs → downstream projects ... `.claude/VERSION` (JSON) in every repo: own version + upstream version + changelog"
- **Craft skill hint**: Implementing repo-level provenance via a tracked VERSION file that is read on session-start and written on /sync — making sync currency observable from within any session.
- **Notes**: Functions as a lightweight Genesis-Record-and-Audit-Anchor for artifact lineage; not literal EATP, but the same provenance pattern.

### Entry: loom/journal/0016-DECISION-co-authority-chain.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Established `co/` as authority for CC and CO artifacts, `kailash/` as authority for COC artifacts. Both have artifact-flow.md rules referencing each other consistently.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §13 (Layer 2 — Context), PACT §11 (Role Envelope), PACT §10 (Monotonic Tightening Invariant)
- **Move type**: brokerage action (authority chain declaration)
- **Evidence polarity**: POSITIVE (move made — methodology drift between CO domains prevented by single CC+CO authority)
- **Key quote**: "kailash/ no longer independently edits CC or CO artifacts. It receives them from co/ and adapts for codegen context. This prevents methodology drift between research, finance, compliance, and codegen domains."
- **Craft skill hint**: Splitting authority across two repos by artifact tier (CC+CO at one source, COC adaptation at another) with explicit unidirectional flow.
- **Notes**: Foundational brokerage decision that informs the entire FORGE dual-destination routing model (co-codegen vs forge in current rules).

### Entry: loom/journal/0017-DISCOVERY-session-notes-broken.md

- **Type**: DISCOVERY
- **Date**: 2026-03-29
- **Topic**: The session-notes mechanism was completely broken — session-start.js found notes only in workspaces/<dir>/.session-notes (kailash/ had them at repo root), printed only age to stderr (which goes to terminal not Claude), and user-prompt-rules-reminder.js had no session-notes awareness. Fixed by adding findAllSessionNotes() and injecting [SESSION-NOTES] reminder via UserPromptSubmit.
- **Spec concepts touched**: CO §3.1 (Amnesia), CO §5.3 (Anti-Amnesia), CO §65 (COC Anti-Amnesia Hook), COC §65 (COC Anti-Amnesia Hook)
- **Move type**: drift detection (broken anti-amnesia primitive); artifact authoring (fix)
- **Evidence polarity**: NEGATIVE (anti-amnesia core mechanism silently broken across all repos)
- **Key quote**: "Without this fix, every new session started from zero with no continuity — the core anti-amnesia mechanism was silently broken."
- **Craft skill hint**: Diagnosing that an anti-amnesia mechanism is broken not by seeing it fail, but by tracing each step (file lookup → output target → Claude's actual context) and discovering that "successful" stderr output never reached the model.
- **Notes**: Direct application of CO §3.1 (Amnesia) and §5.3 (Anti-Amnesia). The failure mode — output to wrong stream — is exactly the kind of silent breakage CO Principle 5 warns about ("MUST NOT rely solely on probabilistic instruction-following").

### Entry: loom/journal/0018-DECISION-cc-audit-convergence.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Ran full cc-audit (fidelity + sync integrity) finding 11 CRITICAL + 9 HIGH issues, fixed with 5 parallel agents, re-audited to convergence in 2 rounds. Key fixes: nexus-specialist 497→144 lines, kaizen-specialist 411→302, release.md 259→96, 174+ files added to COC tier, all 22 agent descriptions under 120 chars.
- **Spec concepts touched**: CO §49 (Quality Criteria — Enforcement Reliability), CO §50 (Quality Criteria — Workflow Discipline), CO §10 (Agent Definition), CO §15 (Progressive Disclosure)
- **Move type**: governance call (multi-round audit-fix-converge)
- **Evidence polarity**: POSITIVE (move made — converged at 0 CRITICAL/0 HIGH)
- **Key quote**: "Skills 01-05 (~158 files) had no tier — silently dropped from sync. 11 rules and 5 commands with variant overlays had no tier. 15 py variant_only files had global duplicates. 8 leaked meta files in USE templates."
- **Craft skill hint**: Running parallel agent teams to fix audit findings then re-running the audit until convergence — and recording the convergence threshold (0 CRITICAL + 0 HIGH).
- **Notes**: Demonstrates how artifact extraction (agents → skills) reduces line count and exposes load-bearing reference material.

### Entry: loom/journal/0019-DECISION-sync-merge-not-copy.md

- **Type**: DECISION
- **Date**: 2026-03-29
- **Topic**: Both `/sync` Gate 2 and `/sync-to-build` were rewritten to require reading the target before writing — per-file merge decisions replace bulk "Apply all". Numbering conflicts halt sync until human decides.
- **Spec concepts touched**: CO §28 (Approval Gate), CO §17 (Single Source of Truth), PACT §10 (Monotonic Tightening Invariant), CARE §6 (Trust Verification Bridge)
- **Move type**: brokerage action (sync semantics change)
- **Evidence polarity**: POSITIVE (move made — corrects the rsync-style semantics)
- **Key quote**: "The original commands used rsync-like semantics — compute expected state, copy to target. This caused damage when kailash-rs had a completely different skill numbering scheme: 14 old-numbered Rust-specific skill dirs were overwritten with generic globals at canonical numbers, creating 28 duplicate directories."
- **Craft skill hint**: Recognising when bulk-copy semantics are unsafe (because target has divergent state) and replacing with read-then-merge semantics that surface conflicts to a human decision point.
- **Notes**: This is a directly observable case of the "fail closed" principle (CARE §42) applied to artifact distribution: when in doubt, halt and ask the human.

### Entry: loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md

- **Type**: DISCOVERY
- **Date**: 2026-03-29
- **Topic**: kailash-rs BUILD repo had 24 BUILD-only skill directories using a numbering scheme completely different from canonical. 12 numbers had different meanings (05=enterprise in rs vs 05=kailash-mcp globally). 14 directories were Rust-specific versions of globals at different numbers.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §3.2 (Convention Drift), CO §15 (Progressive Disclosure)
- **Move type**: drift detection
- **Evidence polarity**: NEGATIVE (24 directories drifted because they were created by /codify before the variant system existed)
- **Key quote**: "Skills were created by `/codify` sessions at kailash-rs before the variant system existed. Each session assigned sequential numbers without reference to the global scheme. No Gate 1 review ever ran to upstream or align them."
- **Craft skill hint**: Tracing artifact divergence to its causal moment — the absence of a Gate-1-equivalent review during the period when the variant system did not yet exist.
- **Notes**: Provides an exact case study for "convention drift" (CO §3.2) at the artifact-naming layer, not just at the code-style layer.

### Entry: loom/journal/0021-DECISION-autonomous-gate1-classification.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Gate 1 artifact classification (global vs variant vs skip) is delegated to a sync-reviewer agent team operating in parallel. Human approves consolidated result, not individual file decisions. 5 sync-reviewer agents processed 79 files in ~2 minutes with unanimous agreement (0 global, 64 variant:rs, 15 skip).
- **Spec concepts touched**: CO §4 (Human-on-the-Loop), CO §28 (Approval Gate), CARE §4 (Human-on-the-Loop Framework Model), CARE §6 (Trust Verification Bridge), PACT §19 (Verification Gradient 4-Zone)
- **Move type**: governance call (re-interpretation of "human classifies" rule)
- **Evidence polarity**: POSITIVE (move made — explicit re-interpretation with rationale)
- **Key quote**: "artifact-flow.md rule 4 ('Human Classifies Every Inbound Change') is now interpreted as 'Human approves classification' not 'Human performs classification' ... Gate 1 reviews complete in minutes instead of hours"
- **Craft skill hint**: Re-interpreting a "human MUST" rule from "human performs" to "human approves" when an agent team can produce trustworthy parallel proposals — and recording the re-interpretation in the journal so future sessions don't revert.
- **Notes**: Direct application of the Human-on-the-Loop principle (CARE §4) — humans observe and approve, agents execute. Notable as a deliberate, recorded re-interpretation rather than silent rule-erosion.

### Entry: loom/journal/0022-DECISION-sdk-version-awareness.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Linked COC artifact versions to SDK code versions: /codify stamps `sdk_version` (from pyproject.toml/Cargo.toml) into proposals, /sync Gate 1 cross-checks at distribution time, downstream /sync warns developers about SDK version mismatches.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §13 (Layer 2 — Context), EATP §19 (Element 5 — Audit Anchor)
- **Move type**: artifact authoring (proposal schema extension); brokerage action (cross-track version coupling)
- **Evidence polarity**: POSITIVE (move made — two version tracks now linked)
- **Key quote**: "Two version tracks existed independently: `.claude/VERSION` (COC artifact currency) and `pyproject.toml` (SDK release). When /codify runs after implementing SDK v2.2.1 features, the resulting artifacts reference APIs that may not exist in projects still on v2.0.0."
- **Craft skill hint**: Linking two formerly-independent version dimensions (artifact currency, SDK release) at the proposal creation boundary so that future syncs can detect version-skew issues.
- **Notes**: A second-order brokerage move — the version-tracking system from 0015 acquires another dimension. Demonstrates iterative refinement of a sync model.

### Entry: loom/journal/0023-DECISION-sync-sdk-dependency-pins.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Added step 9 to /sync Gate 2: after updating VERSION, read all Kailash package versions from BUILD repo's pyproject.toml/Cargo.toml and update USE template's dependency pins to match.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §13 (Layer 2 — Context), CARE §13 (Principle 7 — Evolutionary Trust)
- **Move type**: brokerage action (extending /sync to manage dependency pins as artifacts)
- **Evidence polarity**: POSITIVE (move made — pin drift closed)
- **Key quote**: "USE template repos each have a `pyproject.toml` with recommended Kailash SDK dependency pins ... These pins drifted behind the BUILD repo's actual release versions. The py template had `kailash>=2.2.1` while the BUILD repo was at 2.3.0."
- **Craft skill hint**: Extending /sync's scope from "COC artifacts" to "any file the BUILD repo has authoritative knowledge of" — including pyproject.toml dependency pins, which are not strictly COC artifacts but share the same authority chain.
- **Notes**: Continues the pattern of 0022: identifying yet-another drift dimension and absorbing it into the existing brokerage primitive.

### Entry: loom/journal/0024-DECISION-kailash-as-private-repo.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Initialised ~/repos/kailash/ as a git repository hosted at esperie-enterprise/kailash (private), gitignoring all child repos while tracking only the orchestration artifacts (.claude/, CLAUDE.md, journal/, scripts/). 784 files now version-controlled.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §47 (Knowledge Curator), CARE §35 (Knowledge Ledger Architecture & Access), CARE §36 (Knowledge Ledger Provenance and Evolution)
- **Move type**: governance call (versioning the orchestration repo itself)
- **Evidence polarity**: POSITIVE (move made — institutional knowledge now has version history)
- **Key quote**: "Until now it was an unversioned directory containing 7 child git repos ... The artifacts themselves had no version history ... Artifact changes now have version history — diffs, blame, rollback. Journal entries are version-controlled alongside the artifacts they describe."
- **Craft skill hint**: Recognising when an unversioned orchestration directory is itself the most valuable institutional knowledge in the system — and treating it as a first-class git repo with its own gitignore for child repos.
- **Notes**: Maps directly to CARE §36 (Knowledge Ledger Provenance and Evolution): "Every entry carries provenance ... Knowledge is a living thing; evolves through contribution, corroboration, integration, application, reflection, maturation."

### Entry: loom/journal/0025-DECISION-parallel-backlog-resolution.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Used a team of 5+ parallel agents to resolve the entire sync backlog (Gate 1 rs 15 dirs, 5 BUILD-only py skills, co→co-template drift, 30 downstream sync items, branch protection, GH issues) in a single session. 7 of 12 backlog items were already resolved when checked.
- **Spec concepts touched**: CO §4 (Human-on-the-Loop), CARE §10 (Principle 4 — Continuous Operation), PACT §19 (Verification Gradient 4-Zone)
- **Move type**: governance call (parallel agent execution for backlog clearance)
- **Evidence polarity**: POSITIVE (move made — full backlog cleared in one session, 0 open GH issues across 5 repos)
- **Key quote**: "Sequential sessions: Address one backlog item per session. Rejected — autonomous execution model (rules/autonomous-execution.md) explicitly favors maximum parallelization."
- **Craft skill hint**: Defaulting to parallel agent teams over sequential sessions even for "small" backlog items, because cold-start cost dominates execution cost when items are independent.
- **Notes**: 7 of 12 items were already self-resolved when checked. The lesson is reinforcing the autonomous-execution rule: sequential is never the default when parallel is possible.

### Entry: loom/journal/0026-DECISION-loom-atelier-naming.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Renamed `kailash/` (the artifact management platform) to **Loom** and `co/` (the CO upstream authority) to **Atelier**. GitHub repos and local directories updated. SDK names (kailash-py, kailash-rs, etc.) preserved unchanged.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §13 (Layer 2 — Context)
- **Move type**: brokerage action (identity rename)
- **Evidence polarity**: POSITIVE (move made — repo identity disambiguated from SDK product name)
- **Key quote**: "'kailash/' was ambiguous — it shared its name with the Kailash SDK product (kailash-py, kailash-rs). The platform needed its own identity: Loom — weaves global artifacts + language-specific variants into target-specific templates."
- **Craft skill hint**: Renaming a meta-system once its function has stabilised so the name describes the function (variant overlay weaving) rather than the original placeholder.
- **Notes**: A naming move that acknowledges the meta-system has crystallised into something with its own identity. Useful as evidence that brokerage authority (loom) is now distinct from the codegen subject (kailash-py/rs).

### Entry: loom/journal/0027-DECISION-rb-template-bootstrap.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Populated kailash-coc-claude-rb as a third USE template alongside py and rs. Initial sync delivered 515 global artifacts + 12 rb variant overlays (patterns.md for FFI idioms + Express-first, RSpec testing, bundle exec for zero-tolerance, 8 commands in Ruby, Magnus bindings for SKILL.md).
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §16 (Framework-First), CO §11 (Routing Rules)
- **Move type**: brokerage action (variant onboarding); artifact authoring (12 rb variants)
- **Evidence polarity**: POSITIVE (move made — third target onboarded cleanly)
- **Key quote**: "The Ruby SDK ... needed its own COC template. Sharing the rs template was wrong — rs targets Rust SDK internals developers, rb targets Ruby application developers."
- **Craft skill hint**: Distinguishing between two consumers (Rust SDK internals developers vs Ruby application developers) when deciding whether to add a new variant tier or extend an existing one.
- **Notes**: A clean variant-onboarding case. Demonstrates how the variant system designed in 0013 scales to a third target.

### Entry: loom/journal/0028-RISK-manifest-integrity-gaps.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: Red team identified 8 sync system findings, 4 CRITICAL: 3 orphan py variant files (existed on disk but not in manifest, never synced), `skills/project/` exclude vs variant contradiction, 5 rs variant files misclassified, rb VERSION was plain text not JSON. All fixed in-session; residual systemic risk is no automated pre-sync validation.
- **Spec concepts touched**: CO §49 (Quality Criteria — Enforcement Reliability), CO §17 (Single Source of Truth), CARE §42 (Constraint Resolution — Formal Specification)
- **Move type**: drift detection (orphan + misclassification audit)
- **Evidence polarity**: NEGATIVE (variants drifted into manifest invisibility; no automated guard exists yet)
- **Key quote**: "Orphan variants: py template was missing 3 skills (nexus-agent-reference, kaizen-multi-provider, kaizen-tool-hydration) since they were created. Misclassified variants: future syncs would have overwritten rs-specific content with generic globals."
- **Craft skill hint**: Auditing a manifest for orphans (files on disk not in manifest), stale entries (manifest pointing to nonexistent files), and misclassifications (variant_only files that have global equivalents) as three distinct integrity classes.
- **Notes**: Sets up the automation decision in 0029. Useful as evidence that drift accumulates silently between manual red teams.

### Entry: loom/journal/0029-DECISION-manifest-parity-automation.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Created `scripts/check-manifest-parity.sh` to detect three classes of sync manifest integrity issues: orphan variants, stale entries, and parity gaps. First run found 0 orphans, 0 stale, 91 parity gaps (pre-existing rs-only replacements).
- **Spec concepts touched**: CO §5 (Deterministic Enforcement), CO §5.1 (Hard Enforcement), CO §19 (Layer 3 — Guardrails), CO §49 (Quality Criteria — Enforcement Reliability)
- **Move type**: artifact authoring (script for deterministic manifest validation)
- **Evidence polarity**: POSITIVE (move made — automation exists for the integrity classes that 0028 found manually)
- **Key quote**: "Pre-commit hook: Rejected — too aggressive for a repo where most edits are artifact management. Would block legitimate work-in-progress. CI check: Not yet feasible — loom has no CI pipeline. Manual audit via /cc-audit: Already exists but is heavyweight ... The script is a fast, targeted check."
- **Craft skill hint**: Choosing the right enforcement tier (script invocation vs pre-commit hook vs CI) by matching enforcement frequency to the cost of false-positive blocking.
- **Notes**: Direct application of CO §5 (Deterministic Enforcement). Notable for choosing not to use a pre-commit hook because the hook would be too aggressive — a thoughtful enforcement-tier choice.

### Entry: loom/journal/0030-DECISION-stack-gaps-architectural-decisions.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Consolidated 9 architectural decisions for kailash-py and kailash-rs stack gap resolution covering kailash-ml as separate package, polars-only, PyTorch Lightning, TRL bridge, Nexus transport abstraction (B0), MCP server in SDK, kailash-ml Rust crate with PyO3 bindings, and decision to implement all ~25 proposals.
- **Spec concepts touched**: CO §16 (Framework-First), CO §17 (Single Source of Truth)
- **Move type**: governance call (architectural decision consolidation)
- **Evidence polarity**: POSITIVE (move made — 9 decisions recorded with rationale)
- **Key quote**: "Execution model: Autonomous agent teams, parallel workstreams. Resources are not a constraint — COC handles this."
- **Craft skill hint**: Consolidating multi-domain architectural decisions into a single journal entry with explicit rationale and rejected alternatives, so downstream sessions don't relitigate.
- **Notes**: ⚠ Out-of-scope content note: This entry is primarily Kailash SDK product architecture (which framework, which dataframe library, which deep learning library). The COC-craft-relevant moves are limited to "decision recording" and "parallel autonomous execution framing". The substance (polars vs pandas, PyTorch vs TF) is SDK-product methodology, not COC craft. Kept in-batch because the framing of the decisions and the "implement all" commitment exemplifies the autonomous-execution stance.

### Entry: loom/journal/0031-DECISION-stack-gaps-dependency-sequencing.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Cross-cutting dependency analysis sequencing 35 stack gap proposals into 17 workspaces across 5 phases. Identifies B0 (Nexus transport abstraction) and C1 (kailash-ml bootstrap) as two keystones with 8-item-deep critical path.
- **Spec concepts touched**: CO §28 (Approval Gate), CO §31 (Standard Six-Phase Workflow), CO §4 (Human-on-the-Loop)
- **Move type**: governance call (workspace sequencing under autonomous execution model)
- **Evidence polarity**: POSITIVE (move made — explicit dependency graph)
- **Key quote**: "B0 (Nexus transport abstraction) and C1 (kailash-ml bootstrap) are the two keystone items. They have zero dependencies themselves but block the most downstream work. Both must start on day 1."
- **Craft skill hint**: Identifying keystone items in a dependency graph by counting downstream blockers, not by counting upstream dependencies.
- **Notes**: Useful primarily as a workspace-sequencing case study, less so as COC craft per se.

### Entry: loom/journal/0032-RISK-kailash-ml-architectural-analysis.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: Deep architectural risk analysis for kailash-ml: critical risks identified as circular dependency (FM-09: kailash-ml ↔ kailash-kaizen), install size (FM-05: 5GB GPU), ONNX fidelity (FM-06: feature pipeline not captured), and schema evolution (FM-04). Each has a proposed resolution.
- **Spec concepts touched**: CARE §44 (Failure Modes — Architectural Resilience)
- **Move type**: drift detection (forward-looking architectural risk analysis)
- **Evidence polarity**: NEGATIVE (multiple critical risks named before implementation begins)
- **Key quote**: "kailash-ml depends on kailash-kaizen for agent-augmented AutoML. kailash-kaizen depends on kailash-ml for ML-aware tools. This creates a hard import cycle that prevents installation."
- **Craft skill hint**: Performing a forward failure-mode analysis (FM-01..FM-09) of an unbuilt system based on its proposed architecture, then proposing concrete mitigations per FM.
- **Notes**: ⚠ Mostly product-architecture risk analysis. The COC-craft-relevant move is the FM-numbered taxonomy and the analyst-agent-team approach. CARE §44 (Failure Modes) is the only direct concept mooring; the rest is SDK-product analysis.

### Entry: loom/journal/0033-DECISION-mcp-consolidation.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Decision to consolidate 6 separate MCP implementations across kailash-py into one canonical primitive (FastMCP from official SDK). Each implementation classified Delete / Keep as Reference / Refactor / Keep.
- **Spec concepts touched**: CO §16 (Framework-First), CO §17 (Single Source of Truth)
- **Move type**: governance call (technical-debt reduction via consolidation)
- **Evidence polarity**: POSITIVE (move made — 6 → 1 primitive)
- **Key quote**: "The user's directive: 'keep to the best one and everybody use that primitive.'"
- **Craft skill hint**: Consolidating a proliferation of similar implementations to one canonical primitive with a per-existing-implementation classification table (delete/keep/refactor) and a clear migration path.
- **Notes**: A single-source-of-truth move at the code level rather than at the COC artifact level. Direct application of CO §17.

### Entry: loom/journal/0034-RISK-stack-gaps-redteam-attack.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: Red team attack on the 17-workspace dependency graph. 3 critical and 5 major risks identified including B0 estimate underestimation (2-3 sessions vs deep-analysis 7 sessions), missing integration validation workspace, MCP tools built against moving target, 17-workspace context overhead, kailash-ml greenfield penalty, fallback creating debt, silent failure mode for circular dependency, and no cross-SDK alignment enforcement.
- **Spec concepts touched**: CARE §44 (Failure Modes — Architectural Resilience), CO §28 (Approval Gate)
- **Move type**: governance call (red-team of dependency planning); drift detection (estimate consistency check)
- **Evidence polarity**: NEGATIVE (eight findings against the prior-session plan including a stark 2-3 vs 7 session estimate contradiction)
- **Key quote**: "The contradiction is stark: the detailed refactor plan says 7 sessions for the full scope; the workspace plan says 2-3 sessions. Even accounting for the fact that WS-1C covers only B0 ... the registry extraction + HTTPTransport extraction + MCPTransport extraction is 2.5 sessions minimum -- and that assumes zero rework from the 7 risks identified."
- **Craft skill hint**: Finding contradictions between two analyses produced by the same agent process (deep analysis vs dependency analysis) and using the contradiction itself as evidence the compressed estimate is unreliable.
- **Notes**: ⚠ The substance is product architecture but the move-craft (red team of own prior estimates, listing per-finding mitigations, identifying the missing integration workspace) is generalisable to any dependency planning context.

### Entry: loom/journal/0035-DECISION-redteam-round1-resolutions.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Round-1 red-team resolutions: kailash-rl as separate package (later reversed in 0038), agent infusion in v1, scheduler as BackgroundService (not Transport), B0 split into B0a (registry) + B0b (transports), sklearn+LightGBM in base install, ModelRegistry compatible with all major platforms, mandatory integration workspace WS-4.5.
- **Spec concepts touched**: CO §28 (Approval Gate), CARE §40 (Uncertainty Structured Pipeline)
- **Move type**: governance call (per-finding resolution)
- **Evidence polarity**: POSITIVE (move made — explicit resolution per finding)
- **Key quote**: "RL/RLHF is critical for fine-tuning SLMs for on-prem secured environments. This is a strategic capability, not scope creep."
- **Craft skill hint**: Producing resolutions to red-team findings as named decisions with rationale, so a future round can stress-test each named decision rather than re-litigating the original finding.
- **Notes**: Decision 1 (kailash-rl as separate package) is later reversed in 0038 after Round 2 finds package identity confusion. The contradiction itself is the lesson: even a deliberate decision can be wrong, and the red-team-round mechanism is what catches it.

### Entry: loom/journal/0036-RISK-kailash-rl-redteam-round2.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: Deep stress-test of the kailash-rl separate-package decision (0035 D1). Finds 3 critical and 2 high risks: package identity confusion (RL environments + RL algorithms + LLM alignment are three unrelated paradigms), dependency bloat (~3GB), missing serving story for the on-prem SLM use case. Recommends splitting into kailash-align (LLM fine-tuning) + kailash-ml[rl] (classical RL).
- **Spec concepts touched**: CARE §44 (Failure Modes — Architectural Resilience)
- **Move type**: governance call (single-decision stress test); drift detection (forward FMA on a freshly-made decision)
- **Evidence polarity**: NEGATIVE (the decision being stress-tested fails on identity, dependency, and serving grounds)
- **Key quote**: "The name 'RL' in 'RLHF' is a historical artifact — DPO (the dominant alignment method in 2026) does not use reinforcement learning at all. It is direct preference optimization using a contrastive loss."
- **Craft skill hint**: Stress-testing a decision by examining its dependency graph, user persona match, and end-to-end use-case fulfilment, not just by checking whether the decision is internally consistent.
- **Notes**: ⚠ The substance is product architecture; the COC-craft move is "subject a freshly-made decision to immediate adversarial review and reverse it if needed". This is the convergence loop in action. Demonstrates that the convergence threshold (3 HIGH+) catches even well-reasoned decisions.

### Entry: loom/journal/0037-RISK-redteam-round2-resolution-stress-test.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: Round 2 of red team: 4 findings at HIGH+ severity (1 CRITICAL kailash-rl undefined dependency graph, 3 HIGH on B0a workspace overload, ModelRegistry "compatible with all" unbounded, agent infusion ships without guardrails). Convergence threshold (3 HIGH+) not met, Round 3 recommended.
- **Spec concepts touched**: CO §28 (Approval Gate)
- **Move type**: governance call (convergence threshold check); drift detection
- **Evidence polarity**: NEGATIVE (convergence not met after Round 2)
- **Key quote**: "Convergence Not reached. 4 findings at HIGH+ severity exceeds the threshold of 3."
- **Craft skill hint**: Naming a convergence threshold (e.g. fewer than 3 HIGH+ findings) and refusing to converge until it is met, even when the temptation is to declare convergence and move on.
- **Notes**: Useful as the canonical definition of a quantitative convergence threshold. The number 3 is arbitrary but the principle is not.

### Entry: loom/journal/0038-DECISION-redteam-round2-resolutions.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: All 8 Round-2 findings resolved: kailash-rl split into kailash-align (LLM fine-tuning) + kailash-ml[rl] (classical RL), B0a internal ordering specified, ModelRegistry v1 = MLflow MLmodel format only, 5 mandatory agent guardrails, MCP sequencing in B0b, event bridge auto-enable per DerivedModel, accept abstraction-count divergence Python 5 vs Rust 4, WS-4.5 scope = 3 focused test scenarios.
- **Spec concepts touched**: CO §28 (Approval Gate), CO §40 (Observe-Capture-Evolve Pipeline)
- **Move type**: governance call (full-cycle resolution)
- **Evidence polarity**: POSITIVE (move made — all 8 findings resolved with named decisions)
- **Key quote**: "Decision 1 (kailash-rl as separate package) is superseded ... C11 splits into two items: C11a (kailash-align, new workspace) and C11b (kailash-ml[rl], absorbed into kailash-ml workspace)."
- **Craft skill hint**: Explicitly recording when a prior decision is superseded (rather than silently replaced), so the audit trail shows both the original decision and the supersession reason.
- **Notes**: Direct supersession of 0035 D1. This is the explicit-supersession craft pattern.

### Entry: loom/journal/0039-DECISION-redteam-round3-convergence.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Round 3 verified all ~23 prior findings plus 9 specific consistency checks. Found 3 gaps (BackgroundService workspace assignment, kailash-align dependency list omitting kailash-ml, kailash-align serving scope undecided). All resolved. Final architecture: 8 frameworks (Core, DataFlow, Nexus, Kaizen, PACT, Trust, ML, Align). CONVERGED → /todos.
- **Spec concepts touched**: CO §28 (Approval Gate), CO §31 (Standard Six-Phase Workflow), CO §34 (Phase — Plan)
- **Move type**: governance call (convergence + phase transition)
- **Evidence polarity**: POSITIVE (move made — 3 rounds of red team to convergence, then phase gate to /todos)
- **Key quote**: "All findings from Rounds 1, 2, and 3 are now resolved ... CONVERGED. Ready for /todos."
- **Craft skill hint**: Producing a final convergence entry that summarises all rounds, lists the resolved final architecture, and explicitly authorises the phase transition to /todos.
- **Notes**: 12 final architectural decisions enumerated. This is the canonical example of a multi-round convergence cycle ending in phase transition.

### Entry: loom/journal/0040-DECISION-python-venv-enforcement.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: All Python projects must use `.venv` at project root, managed by `uv`. Global Python is BLOCKED. Created `python-environment.md` rule (path-scoped to \*.py, pyproject.toml, tests/) and added venv check to `session-start.js` hook.
- **Spec concepts touched**: CO §5 (Deterministic Enforcement), CO §5.1 (Hard Enforcement), CO §5.2 (Soft Enforcement), CO §19 (Layer 3 — Guardrails), COC §65 (COC Anti-Amnesia Hook)
- **Move type**: artifact authoring (rule + hook for environment isolation)
- **Evidence polarity**: POSITIVE (move made — venv enforcement at hook + rule layer)
- **Key quote**: "Projects were using the machine's global Python for testing. Tests passing with global Python may fail in CI (clean environment). The rule enforces `uv venv && uv sync` and `uv run` for all Python operations."
- **Craft skill hint**: Designing a Layer 3 enforcement using rule + session-start hook combination, with the hook providing the deterministic check at session boundary.
- **Notes**: Open question on whether venv check should BLOCK (exit 2) or WARN. This is a Hard-vs-Soft enforcement design point.

### Entry: loom/journal/0041-DECISION-kailash-ml-rs-architecture.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Architecture for native Rust ML compute engine: own the stack (no upstream ML dep), unified gradient boosting engine, type-state pattern (unfitted/fitted as separate types), optimal API not sklearn clone, single crate with feature flags, hybrid linear algebra (faer + optional BLAS).
- **Spec concepts touched**: CO §16 (Framework-First)
- **Move type**: governance call (technical architecture decision)
- **Evidence polarity**: POSITIVE (move made — 6 architecture decisions recorded with rationale)
- **Key quote**: "Use foundation crates (ndarray, polars, rayon, faer, Arrow) as dependencies. Do NOT depend on upstream ML algorithm crates (linfa, smartcore) — study their code, write from scratch on our trait system. No PyCaret-style dependency breakage risk."
- **Craft skill hint**: (limited generalisable craft — substance is Rust SDK product architecture)
- **Notes**: ⚠ Out-of-scope rationale: This is product architecture for kailash-ml-rs, not COC craft. Included for completeness but the COC-craft signal is weak. The 6 decisions are technical, not brokerage or guardrail moves.

### Entry: loom/journal/0042-RISK-kailash-ml-rs-redteam-round1.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Three red team agents attacked the kailash-ml-rs architecture from three angles (architecture gaps, gradient boosting design, feasibility risks). Total: 2 CRITICAL, 14 HIGH, 16 MEDIUM, 2 LOW (34 findings).
- **Spec concepts touched**: CARE §44 (Failure Modes — Architectural Resilience), CO §28 (Approval Gate)
- **Move type**: governance call (multi-angle parallel red team)
- **Evidence polarity**: NEGATIVE (34 findings against architecture)
- **Key quote**: "1 CRITICAL: Three incompatible trait systems across documents (doc 02, doc 05, deep analysis) ... 7 research agents produced 500KB of analysis with no single canonical trait design."
- **Craft skill hint**: Running parallel red-team agents against orthogonal angles (architecture gaps, design correctness, feasibility) so each angle catches issues invisible to the others.
- **Notes**: Demonstrates that a single agent process can produce internally inconsistent outputs (three trait systems) when no convergence step exists. This is the inverse of the convergence moves in 0037-0039.

### Entry: loom/journal/0043-DECISION-kailash-ml-rs-redteam-resolutions.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Eight architectural resolutions for kailash-ml-rs: separate library from node integration, three-layer trait architecture, DataSet enum as universal carrier, FitOpts struct for extended capabilities, native Rust pipeline (not workflow), multi-crate workspace, faer primary BLAS optional, gradient boosting honest performance targets.
- **Spec concepts touched**: CO §28 (Approval Gate)
- **Move type**: governance call (per-finding architectural decision)
- **Evidence polarity**: POSITIVE (move made — 8 named architectural decisions resolving ~32 findings)
- **Key quote**: "Type-state (algorithm implementors target this) ... Object-safe erasure (Pipeline/GridSearchCV use this) ... Node adapter (kailash-ml-nodes): any DynEstimator → EstimatorFitNode — automatically generated via macro"
- **Craft skill hint**: Resolving N findings with M < N architectural decisions by recognising that multiple findings often share a common root cause (e.g. trait system contradictions resolve simultaneously with one canonical design).
- **Notes**: ⚠ Substance is Rust ML product architecture. COC-craft signal: "resolve N findings with fewer-than-N decisions by finding common root causes".

### Entry: loom/journal/0044-DECISION-kailash-ml-rs-redteam-convergence.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Round-2 verification of kailash-ml-rs: 32 RESOLVED, 2 PARTIALLY RESOLVED (multi-output, DynEstimator clone+deserialization — both solvable but non-trivial implementation details), 0 UNRESOLVED. Architecture validated.
- **Spec concepts touched**: CO §28 (Approval Gate)
- **Move type**: governance call (convergence verification)
- **Evidence polarity**: POSITIVE (move made — convergence verified with explicit "partially resolved" category)
- **Key quote**: "32 RESOLVED — decisions fully address the findings. 2 PARTIALLY RESOLVED — solvable implementation details, not architectural blockers. 0 UNRESOLVED."
- **Craft skill hint**: Distinguishing "partially resolved" from "unresolved" by asking whether the remaining work is an architectural blocker or an implementation detail — and only the former should block phase transition.
- **Notes**: Useful refinement of the convergence threshold model: "0 UNRESOLVED" is the gate, not "0 outstanding".

### Entry: loom/journal/0045-DECISION-kailash-ml-rs-todos-redteam.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Red team of the 173-todo list produced 34 completeness findings + 12 dependency findings. Critical fixes: split KML-014 into 014a/014b/014c (5 optimization solvers unbundled), added KML-174 sklearn bug archaeology (CRITICAL testing infrastructure), added KML-175 continuous numerical cross-validation CI, added KML-176 multi-config CI matrix, fixed dependency diagram (M5 gradient boosting INDEPENDENT of M3 trees → 8-way parallelization), MVP defined (V0.1 50 todos, V0.2 64 todos). Final: 179 todos across 16 milestones.
- **Spec concepts touched**: CO §28 (Approval Gate), CO §29 (Evidence-Based Completion), CO §49 (Quality Criteria — Enforcement Reliability)
- **Move type**: governance call (red team of a TODO list)
- **Evidence polarity**: POSITIVE (move made — todos refined before phase transition)
- **Key quote**: "M5 (gradient boosting) is INDEPENDENT of M3 (trees) — GB engine builds its own histogram-based trees. Opens 8-way parallelization after M0."
- **Craft skill hint**: Red-teaming a TODO list (not just code or architecture) by checking dependency links, P0 priority assignments, granularity, and missing testing infrastructure.
- **Notes**: Useful as the canonical example of red-teaming work-decomposition itself. The KML-174 sklearn bug archaeology task is especially interesting — it's a meta-task that mines another project's git history to capture institutional knowledge.

### Entry: loom/journal/0046-DECISION-downstream-proposal-guard.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: /codify Step 7 (upstream proposal creation) is now scoped to BUILD repos only. Downstream project repos skip this step entirely — their artifact changes stay local. The command checks git remote or package manifest to determine repo type and skips proposal creation otherwise.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §28 (Approval Gate), PACT §11 (Role Envelope), PACT §10 (Monotonic Tightening Invariant)
- **Move type**: brokerage action (authority chain enforcement at command boundary)
- **Evidence polarity**: POSITIVE (move made — corrects authority-chain violation)
- **Key quote**: "The /codify command had Step 7 marked as 'MANDATORY' with no repo-type guard. Since /codify is synced unchanged to USE templates ..., every downstream project that ran /codify would create `.claude/.proposals/latest.yaml` — a proposal with no upstream path. This violated the authority chain: downstream repos consume COC artifacts, they don't contribute them back."
- **Craft skill hint**: Adding a runtime repo-type check to a synced command so that the same command file can have different mandatory steps depending on which tier of the authority chain it executes in.
- **Notes**: Direct application of monotonic tightening: downstream cannot widen authority by submitting upstream proposals that have nowhere to go. The runtime detection is a workaround for the absence of `.claude/repo-type` metadata.

### Entry: loom/journal/0047-DECISION-spec-coverage-gate.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Post-mortem of a project (kz-engage) that passed 11 red team rounds, 94 tests, and 2 clean convergence rounds — yet shipped with frontend pages using mock data instead of real API calls, architecture plans approved but never implemented, and todos marked complete because files existed not because features were wired. Identified 5 systemic process failures and made 6 corrective changes across /redteam, /todos, /implement, validate-workflow.js, and no-stubs.md.
- **Spec concepts touched**: CO §29 (Evidence-Based Completion), CO §50 (Quality Criteria — Workflow Discipline), CO §3.3 (Safety Blindness), CO §5 (Deterministic Enforcement), CO §5.1 (Hard Enforcement), CO §19 (Layer 3 — Guardrails)
- **Move type**: governance call (post-mortem of a 5-failure-mode escape); artifact authoring (6 corrective changes)
- **Evidence polarity**: NEGATIVE (5 process failures shipped despite extensive validation) → POSITIVE (corrected)
- **Key quote**: "Frontend mock data invisible to hooks — `validate-workflow.js` detects Python stubs but not `MOCK_USERS`, `generateHourly*()` in JS/TS ... Architecture plans treated as aspirational — 366-line data fabric design approved, no implementation todo created, bypassed with ad-hoc `db.express.*` calls"
- **Craft skill hint**: Diagnosing process-level failure modes from a post-mortem: "tests pass" vs "spec covered" vs "feature wired" are three different things, and validation that checks only quality misses coverage and wiring failures.
- **Notes**: This is the canonical entry for "five systemic COC process failures" — a load-bearing reference for any FORGE skill on red-team coverage criteria, frontend mock data detection, evidence-based completion, and context anchoring across implementation cycles. Direct application of CO §3.3 (Safety Blindness) and §5 (Deterministic Enforcement).

### Entry: loom/journal/0048-DECISION-gate1-rs-variant-classification.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: After thorough analysis of 43 skill directories in kailash-rs, only 2 dirs (06-cheatsheets, 07-development-guides) needed upstreaming as new rs variants. The remaining 24 were already correctly placed, 15 were language-independent and correctly using globals, 1 (project) was correctly local-only.
- **Spec concepts touched**: CO §17 (Single Source of Truth), CO §16 (Framework-First), CO §11 (Routing Rules)
- **Move type**: brokerage action (variant classification)
- **Evidence polarity**: MIXED (the original audit overestimated the variant gap; the actual classification shows the variant system was working better than feared)
- **Key quote**: "After detailed analysis with Rust keyword grep counts across all dirs, the actual gap was much smaller — most had already been placed during the initial variant population, and the 'language-independent' dirs (flutter-patterns, conversation-ux, care-reference, etc.) had incidental Rust mentions but no content warranting a variant overlay."
- **Craft skill hint**: Distinguishing "incidental language mentions" from "content warranting a variant overlay" using objective grep counts plus per-directory inspection — avoid the false positive of treating any Rust appearance as evidence of a needed variant.
- **Notes**: Demonstrates the four-category classification framework: already-placed / language-independent (use global) / variant-needed / local-only. Reusable for any variant audit.

### Entry: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md

- **Type**: DISCOVERY
- **Date**: 2026-04-03
- **Topic**: Empirical token-ablation experiment to determine the minimum viable rule set for behavioural compliance. Phase 1 (in-session ablation) failed due to contamination from agents overcorrecting to "appear vanilla". Phase 2 (clean-room ablation in /tmp/ablation-clean) gave reliable results. Phase 3 ran same scenarios against MiniMax M2.7. Found that Claude's baseline is stronger than expected (security basics, communication style, path security, SQL safety, time estimates are all model-native), MiniMax has different/worse gaps, and zero rules can be safely eliminated without testing. Produced 4-tier variant architecture (High Reliability / Baseline / Max Lean / MiniMax).
- **Spec concepts touched**: CO §1 (Institutional Knowledge Thesis), CO §3 (Three Failure Modes), CO §3.2 (Convention Drift), CO §5 (Deterministic Enforcement), CO §13 (Layer 2 — Context), CO §15 (Progressive Disclosure), CO §19 (Layer 3 — Guardrails), CO §22 (Advisory Rules), CARE §13 (Principle 7 — Evolutionary Trust)
- **Move type**: governance call (empirical rule-necessity testing); drift detection (ablation methodology)
- **Evidence polarity**: MIXED (in-session method was contaminated; clean-room method was reliable; some assumptions were wrong, others were validated)
- **Key quote**: "Agents within a rule-loaded session overcorrect when told to 'act vanilla.' The contamination runs in both directions — some agents performed better than true baseline (residual rule influence), others performed worse (overcorrection). Clean-room tests via `claude -p` in a clean directory are the only reliable method."
- **Craft skill hint**: Performing model-vs-rule ablation tests by running the same scenarios in a clean repo (no .claude/, no CLAUDE.md, no hooks) so the model's true baseline is measured rather than the rule-loaded session contaminated by overcorrection.
- **Notes**: The most methodologically rigorous entry in the batch. Directly challenges CO Principle 1 (Institutional Knowledge Thesis) by asking which institutional knowledge is actually load-bearing versus which is redundantly encoded in the model's training. The 4-tier variant architecture (High Reliability / Baseline / Max Lean / MiniMax) is itself a brokerage move: same artifacts, four target audiences. CONTRADICTION with 0006 (Learning must work): 0049 ends up demoting many Layer 5 / Layer 3 rules because the model was already capable; 0006 said L5 is non-negotiable. The contradiction is partial (0049 doesn't demote Layer 5 itself, only specific rules) but worth flagging.

### Entry: loom/journal/0050-DISCOVERY-session-start-command-injection.md

- **Type**: RISK
- **Date**: 2026-04-04
- **Topic**: Red team security review discovered command injection vulnerabilities in two hook files: CRITICAL in version-utils.js (execSync with URL from .claude/VERSION file), HIGH in session-start.js (execSync with constructed Python path in checkSdkPinFreshness). Fixed by converting both to execFileSync and adding isFinite() check on sync marker.
- **Spec concepts touched**: CO §3.3 (Safety Blindness), CO §5 (Deterministic Enforcement), CO §19 (Layer 3 — Guardrails), CARE §44 (Failure Modes — Architectural Resilience)
- **Move type**: drift detection (security audit of hook code itself); artifact authoring (fix)
- **Evidence polarity**: NEGATIVE (the enforcers themselves had attack surface) → POSITIVE (fixed)
- **Key quote**: "Any repo with a malicious `.claude/VERSION` file (e.g., from a cloned repo) could have executed arbitrary commands on session start."
- **Craft skill hint**: Auditing the Layer 3 enforcement code (hooks themselves) for the same security issues those hooks are designed to catch — the enforcers must themselves obey the rules they enforce.
- **Notes**: A direct case of "the cc-artifacts that enforce cc-artifacts violate cc-artifacts" pattern (also see 0003). Layer 3 hooks consuming untrusted input from another part of the same system creates a self-injection surface.

### Entry: terrene/journal/0001-DECISION-orchestrator-architecture.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Established `terrene/` as the Foundation ecosystem orchestrator (a git repo with `.claude/`, `scripts/hooks/`, `CLAUDE.md`, gitignored child repos), modeled on the atelier/loom pattern. Inherits CO-tier artifacts from atelier/loom rather than writing from scratch.
- **Spec concepts touched**: CO §13 (Layer 2 — Context), CO §17 (Single Source of Truth), CO §16 (Framework-First), PACT §11 (Role Envelope)
- **Move type**: brokerage action (orchestrator instantiation)
- **Evidence polarity**: POSITIVE (move made — terrene becomes a third orchestrator following the loom/atelier pattern)
- **Key quote**: "The atelier/loom pattern is proven: a git-tracked orchestrator with `.claude/` artifacts, hooks for session management, and commands for cross-repo work. Terrene needed the same treatment — it manages 7 git repos + 1 planned repo across corporate governance, publications, website, and 5 OSS stacks."
- **Craft skill hint**: Recognising when a multi-repo working area has reached the threshold where it needs its own .claude/ orchestration layer (versus being managed from a parent directory).
- **Notes**: Establishes that the brokerage pattern (atelier → loom → templates) is itself a reusable structure that can be instantiated for other ecosystems (terrene-foundation).

### Entry: terrene/journal/0002-DECISION-inherit-not-reinvent.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: All CO-tier artifacts (commands, rules, agents, skills, guides) for terrene must be copied from authoritative sources (atelier for commands, loom for rules, foundation for agents/skills). Custom orchestration artifacts are additive, never replacements. User caught a mistake during red team where custom commands lacked workspace resolution, journal protocol, oversight checklists, and hook integration of atelier originals.
- **Spec concepts touched**: CO §16 (Framework-First), CO §17 (Single Source of Truth), CO §15 (Progressive Disclosure)
- **Move type**: brokerage action (inheritance over reinvention)
- **Evidence polarity**: NEGATIVE → POSITIVE (initial attempt to write custom artifacts failed red team; corrected by inheriting from upstream)
- **Key quote**: "User caught the mistake during red-team: custom commands lacked the proven workspace resolution, journal protocol, oversight checklists, and hook integration of the atelier originals. The /wrapup command failed as 'Unknown skill' because the custom version wasn't properly structured. Inheriting from upstream ensures compatibility with the hooks and learning system."
- **Craft skill hint**: Defaulting to inheritance from the upstream authority over local reinvention, even when local reinvention seems faster — because the upstream version embeds invisible compatibility constraints (workspace resolution, hook contracts) that reinvention loses.
- **Notes**: Direct application of CO §16 (Framework-First — composition over creation). The "Unknown skill" error is the load-bearing evidence: a syntactically valid but structurally divergent command failed at runtime because it didn't follow the upstream structure.

## Summary

**Total processed**: 52
**Clean mappings**: 52 (every entry maps to at least one spec concept by number and name)
**Flagged**: 0 (no entries needed `⚠ no clean mapping`)
**Out-of-scope**: 0 strictly out of scope. Several entries (0030, 0031, 0032, 0033, 0034, 0035, 0036, 0037, 0038, 0039, 0041, 0042, 0043, 0044, 0045) have **mostly product-architecture substance** (Kailash SDK and kailash-ml-rs design) but each contains a generalisable governance/convergence/red-team move that justifies their inclusion. Where the product-architecture substance dominates I have flagged this in the entry's Notes field with `⚠ Out-of-scope rationale` or similar.

**Top 5 most-cited spec concepts in this batch** (counted across all 52 entries):

1. **CO §17 (Single Source of Truth)** — cited in 19 entries (0001, 0003, 0008, 0013, 0014, 0015, 0016, 0018, 0019, 0020, 0022, 0023, 0024, 0026, 0027, 0028, 0030, 0033, 0046, 0048, terrene 0001, terrene 0002). The most load-bearing concept for brokerage and artifact-flow craft.
2. **CO §28 (Approval Gate)** — cited in 14 entries (0012, 0019, 0021, 0031, 0034, 0035, 0037, 0038, 0039, 0042, 0043, 0044, 0045, 0046, 0047). The convergence threshold and phase-transition concept.
3. **CO §19 (Layer 3 — Guardrails)** — cited in 13 entries (0002, 0003, 0007, 0008, 0009, 0010, 0017, 0029, 0040, 0047, 0049, 0050). The load-bearing concept for hook + rule + anti-amnesia patterns.
4. **CO §13 (Layer 2 — Context)** — cited in 13 entries (0001, 0002, 0003, 0007, 0008, 0010, 0015, 0016, 0022, 0023, 0026, 0027, 0049, terrene 0001). The master-directive and progressive-disclosure concept.
5. **CO §49 (Quality Criteria — Enforcement Reliability)** — cited in 8 entries (0003, 0005, 0007, 0011, 0018, 0028, 0029, 0045). The "do critical rules have hard enforcement?" check that drives most red-team gap findings.

**Honourable mentions** (also notably cited):

- CO §5 (Deterministic Enforcement) and CO §5.1 (Hard Enforcement) — cited together in entries about hook authoring (0029, 0040, 0047, 0049, 0050)
- CO §3.1 (Amnesia), §5.3 (Anti-Amnesia), and COC §65 (COC Anti-Amnesia Hook) — cited in entries about session-start, user-prompt-rules-reminder, and pre-compact mechanisms (0006, 0017, 0040)
- CO §39 (Layer 5 — Learning) and §40 (Observe-Capture-Evolve) — cited in entries about the L5 broken/fixed cycle (0004, 0005, 0006, 0007, 0009, 0010)
- PACT §10 (Monotonic Tightening Invariant) and §11 (Role Envelope) — cited in brokerage entries about authority chains (0008, 0013, 0016, 0019, 0046, terrene 0001)
- CARE §6 (Trust Verification Bridge) and CARE §44 (Failure Modes) — cited in entries about read-then-merge sync semantics and forward FMA (0019, 0028, 0032, 0034, 0036, 0042, 0050)

**Out-of-scope entries**: None strictly out of scope, but the following have weak COC-craft signal because their substance is Kailash product architecture: 0030, 0031, 0032, 0033, 0041, 0042, 0043, 0044, 0045. These are kept in the index because each contains at least one COC-craft move (red-team-of-decisions, workspace sequencing, parallel autonomous execution, sklearn bug archaeology task design, todo-list red team) that downstream skill atoms can cite.

**Contradictions noted**:

1. **0005 vs 0011 (orphan skill audit)**: Entry 0005 reports "59% of skill files are orphans (204/350 in kailash-py)". Entry 0011 retracts this: "The numbered skill directories ARE the distilled Kailash documentation. They are discovered by Claude Code's skill system via SKILL.md trigger descriptions ... TODO-3.1 is deleted." The contradiction is the lesson — progressive disclosure (CO §15) is invisible to a "is this file referenced by name?" audit.

2. **0006 vs 0049 (Layer 5 mandatory vs token-ablation)**: Entry 0006 declares "The L5 Learning System is NOT aspirational — it is a core CO principle that MUST work." Entry 0049 finds via empirical ablation that "Several rules we considered essential turned out to be model-native for Claude" and demotes 10 rules to on-demand. The contradiction is partial — 0049 does not demote Layer 5 itself, only specific rules — but the tension between "L5 must work" and "rules can be safely demoted" is real and worth capturing.

3. **0035 D1 vs 0038 RT2-01 (kailash-rl as separate package)**: Entry 0035 Decision 1 creates `kailash-rl` as a first-class Kailash framework. Entry 0038 RT2-01 supersedes this: "Replace `kailash-rl` with two scoped solutions: `kailash-align` (LLM fine-tuning) and `kailash-ml[rl]` (classical RL)". This is an explicit, journal-traceable supersession across two red-team rounds. The pattern itself — make a decision, stress-test it the next round, supersede if needed — is the convergence-craft move.

4. **0034 vs deep-analysis (B0 estimate)**: Entry 0034 Finding 1 documents the contradiction between two analyses produced by the same agent process: "the detailed refactor plan says 7 sessions for the full scope; the workspace plan says 2-3 sessions." Both were produced from the same materials. This is a self-contradiction in the analytical pipeline, surfaced by red team — and it justifies treating compressed estimates with suspicion.

5. **0021 (re-interpretation of "human classifies")**: Entry 0021 explicitly re-interprets `artifact-flow.md` rule 4 from "Human Classifies Every Inbound Change" to "Human approves classification, agents perform". This is not strictly a contradiction, but it is a semantic shift in a MUST rule and is worth flagging because future sessions could interpret the rule the original way and revert.

**Notes on craft signal density**:

- **Highest craft-signal entries** (most useful for skill atoms): 0003 (cc artifact audit), 0004 (L5 broken), 0006 (L5 must work), 0007 (CO completeness audit), 0013 (variant architecture), 0014 (drift audit), 0017 (session notes broken), 0019 (sync merge not copy), 0021 (autonomous Gate 1 classification), 0028 (manifest integrity gaps), 0029 (manifest parity automation), 0046 (downstream proposal guard), 0047 (spec coverage gate), 0049 (token ablation), 0050 (command injection in hooks). These 15 entries contain the substance most of the FORGE skill catalog will draw on.
- **Medium craft-signal entries**: 0001, 0002, 0005, 0008, 0009, 0011, 0015, 0016, 0018, 0020, 0022, 0023, 0024, 0025, 0026, 0027, 0040, 0048, terrene 0001, terrene 0002. Most of these are brokerage moves (sync, version tracking, authority chain).
- **Lower craft-signal entries** (mostly product architecture): 0010, 0012, 0030, 0031, 0032, 0033, 0034, 0035, 0036, 0037, 0038, 0039, 0041, 0042, 0043, 0044, 0045. The convergence-loop pattern across 0034-0039 is the strongest craft signal in this band — repeating multi-round red team until convergence, then explicit supersession.
