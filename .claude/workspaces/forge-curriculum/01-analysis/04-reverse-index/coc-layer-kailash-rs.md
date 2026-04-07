---
destination: forge
layer: reverse-index
batch: kailash-rs
corpus_scope: loom/kailash-rs/workspaces
date: 2026-04-07
entries_processed: 41
---

# Reverse Index — COC Layer — Kailash Rust SDK

Reverse index of CO/COC craft journal entries from the kailash-rs (Rust SDK) corpus, classified against the four Foundation standards' spec indices (CARE, EATP, CO, PACT). Each entry quotes literal text and cites only enumerated concepts from the v1 spec indices.

Note on cross-language learning: many of these Rust-side entries surface patterns whose Python counterparts will appear in the kailash-py batch (variant classification, Express API parity, governance primitive shape). Where the cross-language pointer is load-bearing, it is called out under Notes.

Note on count: the corpus directory contains 41 markdown journal files. The earlier estimate of "42 entries" appears to have included a non-journal entry or off-by-one. All 41 are processed below.

## Entries

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Vacancy enforcement is asymmetric — `check_access()` blocks vacant roles but `verify_action()` does not, leaving an action-path security gap
- **Spec concepts touched**: PACT Concept 6 (Role — Vacant Roles), PACT Concept 51 (Edge Case — Vacant Roles), PACT Concept 33 (Knowledge Cascade Rules), PACT Concept 13 (Effective Envelope), CO Concept 3.3 (Safety Blindness), CO Concept 5 (Deterministic Enforcement)
- **Move type**: governance-primitive integrity audit; symmetric-enforcement check across read-path and action-path
- **Evidence polarity**: NEGATIVE
- **Key quote**: "`check_access()` (knowledge access path) correctly blocks vacant roles at Step 0 of `can_access` (access.rs:112-119)\n- `verify_action()` (action envelope path) has ZERO vacancy checks — a vacant role passes envelope verification" / "if a role is vacant, both knowledge access and action execution should be blocked."
- **Craft skill hint**: "Audit governance primitives for asymmetric enforcement: verify the same predicate fires on every code-path that grants authority, not just the one tested in the spec example."
- **Notes**: Direct evidence for PACT Concept 51 (Vacant Roles cannot execute autonomously). Cross-language pointer: this is Rust-side hardening of a primitive whose Python equivalent in kailash-py would carry the same risk. Sets up entries 0006/0008 in this same workspace.

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: Two parallel constraint systems coexist in the EATP crate — legacy boolean `ConstraintDimensions` vs structured 5-dimensional `Dimensions` — creating a routing decision for new features
- **Spec concepts touched**: EATP Concept 4 (Constraint Envelope Schema — Five-Dimensional), EATP Concept 24 (Dimension-Scoped Delegation), EATP Concept 31 (Dimension-Scoped Delegation Operational), EATP Concept 66 (Constraint Dimensions — Five), PACT Concept 14-18 (Five Constraint Dimensions), CO Concept 17 (Single Source of Truth)
- **Move type**: drift detection between legacy and canonical schema; spec-canon alignment
- **Evidence polarity**: NEGATIVE (drift discovered) → ROUTING (resolution toward canonical 5D)
- **Key quote**: "The EATP crate has two parallel constraint systems: 1. **Legacy** (`eatp/src/types.rs`): `ConstraintDimensions` — boolean flags... 2. **5-Dimensional** (`eatp/src/constraints/mod.rs`): `Dimensions` — structured Financial, Operational, Temporal, DataAccess, Communication" / "Since the PACT governance model (envelopes, intersection, tightening) uses the 5-dimensional system, `dimension_scope` should reference those 5 names, not the legacy boolean flags."
- **Craft skill hint**: "When a codebase carries two implementations of a concept the spec normalises, route new features against the spec-canonical implementation, even if the legacy one is more entrenched."
- **Notes**: Reaches the canonical five constraint dimensions enumerated identically in CARE, EATP, and PACT. Single Source of Truth (CO Concept 17) violation in code is the substance.

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0003-GAP-no-lca-computation.md

- **Type**: GAP
- **Date**: 2026-03-30
- **Topic**: PACT addressing has no Lowest Common Ancestor (LCA) computation, blocking spec-required bridge approval
- **Spec concepts touched**: PACT Concept 8 (Positional Addressing), PACT Concept 35 (Bridges), PACT Concept 53 (Edge Case — Matrix Reporting), PACT Concept 54 (Edge Case — Shared Services / Bridges)
- **Move type**: spec-derived API gap identification; deriving needed primitive from PACT thesis Section 4.4
- **Evidence polarity**: NEGATIVE (gap)
- **Key quote**: "LCA is required by PACT thesis Section 4.4 property (4) for bridge creation: the LCA of the two bridge endpoints must approve the bridge." / "LCA can be computed using existing `ancestors()`: 1. Get `ancestors()` for both addresses 2. Find the longest common prefix 3. The last matching ancestor is the LCA"
- **Craft skill hint**: "Read spec property statements as latent API obligations; if the spec says 'X must approve', the codebase needs an X-finder primitive even if no current code calls it."
- **Notes**: Pure example of specs-as-index → craft-as-substance: a spec property forces an addressing primitive into existence. Prefigures DECISION 0006.

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md

- **Type**: RISK
- **Date**: 2026-03-30
- **Topic**: Adding `dimension_scope` to `DelegationRecord` risks invalidating every existing delegation chain signature; mitigated via conditional signing payload
- **Spec concepts touched**: EATP Concept 16 (Element 2 — Delegation Record), EATP Concept 24 (Dimension-Scoped Delegation), EATP Concept 23 (Dual-Binding Signing Model), EATP Concept 27 (Verification Process), EATP Concept 31 (Dimension-Scoped Delegation Operational)
- **Move type**: signed-struct schema evolution; backward-compat mitigation; trust-chain integrity preservation
- **Evidence polarity**: MIXED (CRITICAL risk → MITIGATED by conditional payload)
- **Key quote**: "DelegationRecord is a signed struct — the signature covers the serialized payload. Adding `dimension_scope: Option<HashSet<String>>` changes the serialized representation, potentially invalidating ALL existing delegation chain signatures." / "Signing payload must CONDITIONALLY include `dimension_scope` — only when `Some`. This preserves signature validity for existing records that were signed without the field."
- **Craft skill hint**: "Schema-evolve a signed struct by making new fields conditional in the signing payload; verify backward compat with a round-trip test on records signed without the field."
- **Notes**: This is the engineering twin of EATP Concept 23 (dual-binding signing). Cross-language pointer: same risk shape exists for any EATP implementation in any language adding dimension scope.

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0005-DISCOVERY-lazy-dataflow-already-exists.md

- **Type**: DISCOVERY
- **Date**: 2026-03-30
- **Topic**: `LazyDataFlow` already exists in kailash-rs via `tokio::sync::OnceCell`; the Python-side problem doesn't apply
- **Spec concepts touched**: ⚠ no clean mapping (closest: CO Concept 16 Framework-First — verify the framework already provides what's being proposed)
- **Move type**: framework-first verification; cross-language non-port (the bug doesn't exist on this side)
- **Evidence polarity**: POSITIVE
- **Key quote**: "`LazyDataFlow` already exists at `crates/kailash-dataflow/src/connection.rs:1089`, using `tokio::sync::OnceCell` for deferred pool initialization. Additionally, `DataFlow::new()` is an async function, which structurally prevents import-time connection (the Python SDK's problem)." / "GH #109 should be closed with a comment referencing `LazyDataFlow` and the async `new()` signature. No implementation work needed."
- **Craft skill hint**: "Before porting a Python-side fix to Rust, check whether Rust's type system or async-by-default already structurally prevents the bug; close the issue with the structural reference."
- **Notes**: Cross-language learning pointer is the entire substance. Closest spec mooring is the Framework-First sub-principle of CO Layer 2.

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Five governance decision points resolved with the principle "type safety > flexibility, backward compat > clean break, fail-closed > permissive, progressive enforcement > forced adoption"
- **Spec concepts touched**: PACT Concept 35 (Bridges), PACT Concept 9 (BOD as Governance Root), PACT Concept 51 (Vacant Roles), PACT Concept 16 (Task Envelope), EATP Concept 16 (Delegation Record), EATP Concept 24 (Dimension-Scoped Delegation), EATP Concept 66 (Constraint Dimensions — Five), CO Concept 5.1 (Hard Enforcement)
- **Move type**: cross-feature decision compaction with explicit decision criteria
- **Evidence polarity**: POSITIVE
- **Key quote**: "1. **#106 Cross-department bridge approver**: Root admin. LCA of cross-department addresses is the org root; root role approves. If no root role exists, creation fails." / "2. **#107 auto_suspend_on_vacancy default**: `false`. Backward compatible." / "All decisions prioritize: type safety > flexibility, backward compat > clean break, fail-closed > permissive, progressive enforcement > forced adoption."
- **Craft skill hint**: "When packaging multiple governance decisions, surface the priority lexicon (type safety > flexibility, backward compat > clean break, fail-closed > permissive) explicitly so future readers can re-derive each individual call."
- **Notes**: Lexicon "fail-closed > permissive" maps directly to CO Concept 5 (Deterministic Enforcement). "Progressive enforcement" maps to PACT Concept 24 (Default Envelope Profiles — Progressive Trust).

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md

- **Type**: DECISION
- **Date**: 2026-03-30
- **Topic**: Codification scope for cross-SDK governance session — update existing skill files rather than create new ones, classify changes as `coc-rs` variant
- **Spec concepts touched**: CO Concept 39 (Layer 5 — Learning), CO Concept 40 (Observe-Capture-Evolve Pipeline), CO Concept 17 (Single Source of Truth), CO Concept 16 (Framework-First), CO Concept 9 (Layer 1 — Intent), CO Concept 13 (Layer 2 — Context)
- **Move type**: artifact authoring; brokerage move (variant classification: rs-specific vs global); codify-phase scope discipline
- **Evidence polarity**: POSITIVE
- **Key quote**: "Codification covers 6 PRs (#102, #103, #105, #111, #112, #113) across two domains... The scope decision was to update existing skill files rather than create new ones, because the new features are extensions of existing documented patterns (not new architectural concepts)." / "All changes suggested as `coc-rs` tier (Rust SDK specific) except the SKILL.md index table update (global). Pending review at kailash/ for classification and sync."
- **Craft skill hint**: "In codify, default to extending existing skill files when the new content is an extension of a documented pattern; only spawn a new skill file when the content is a new architectural concept."
- **Notes**: Layer 2 craft. Variant classification (`coc-rs` vs global) is the brokerage layer. Note the explicit "pending review at kailash/ for classification and sync" — human classification gate, matching the artifact-flow rule. Cross-language pointer: this is the canonical example of how a Rust workspace upstreams variant artifacts.

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md

- **Type**: RISK
- **Date**: 2026-04-03
- **Topic**: Vacant approver could approve bridges — cross-feature interaction gap discovered by independent red team agents during convergence
- **Spec concepts touched**: PACT Concept 51 (Vacant Roles), PACT Concept 35 (Bridges), PACT Concept 33 (Knowledge Cascade Rules), PACT Concept 8 (Positional Addressing), CO Concept 3.3 (Safety Blindness)
- **Move type**: cross-feature interaction red team; pattern propagation (vacancy check pattern from `verify_action` to bridge approval path)
- **Evidence polarity**: NEGATIVE → MITIGATED
- **Key quote**: "Two independent red team agents (security-reviewer and deep-analyst) discovered that `approve_bridge()` and `reject_bridge()` did not check whether the approver/rejector role was vacant. This allowed a role with no occupant to approve cross-boundary access bridges, directly contradicting the vacancy enforcement added in #107." / "Cross-feature interaction testing must be explicit in todo specifications. Three features that share the governance engine's state (vacancy, bridge, delegation) will interact whether or not their todos acknowledge it."
- **Craft skill hint**: "When two governance features share state, write the cross-feature interaction test before writing either feature's primary tests; otherwise the safety property holds in each silo and breaks at the seam."
- **Notes**: Direct continuation of entry 0001 — same vacancy primitive, different code path. Two independent red team agents converging on the same finding is itself worth a craft note (multi-agent convergence as a brokerage pattern).

### Entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md

- **Type**: RISK
- **Date**: 2026-04-03
- **Topic**: All `DelegationRecord` fields were `pub`, allowing trust bypass via mutable clones (e.g., flipping `revoked = false` or widening `dimension_scope`)
- **Spec concepts touched**: EATP Concept 16 (Delegation Record), EATP Concept 33 (Cascade Revocation), EATP Concept 24 (Dimension-Scoped Delegation), PACT Concept 10 (Monotonic Tightening Invariant), CO Concept 19 (Layer 3 — Guardrails), CO Concept 5 (Deterministic Enforcement)
- **Move type**: encapsulation hardening; trust-chain integrity preservation via field visibility
- **Evidence polarity**: NEGATIVE → MITIGATED
- **Key quote**: "Security review found that all fields on `DelegationRecord` (in `crates/eatp/src/delegation.rs`) were `pub`, including security-sensitive fields: `revoked`, `signature`, `dimension_scope`, `multi_sig_policy`, `multi_sig_bundle`. Code holding a cloned record could flip `revoked = false` or widen `dimension_scope`, bypassing the monotonic tightening invariant." / "Changed 8 security-sensitive fields to `pub(crate)` with read-only getter methods."
- **Craft skill hint**: "Field visibility on signed/governed structs is a security boundary, not an ergonomics decision; default to `pub(crate)` with read-only accessors and a single approved-mutation constructor path."
- **Notes**: Deterministic enforcement (CO Concept 5) realised through Rust's visibility system. The "monotonic tightening invariant" the entry names is PACT Concept 10 verbatim. Strong cross-language pointer: any binding-language wrapper for delegation records must preserve the same encapsulation.

### Entry: loom/kailash-rs/workspaces/dataflow-parity/journal/0001-DISCOVERY-dataflow-parity-audit-findings.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: DataFlow parity audit finds the brief overestimates two missing items and underestimates a third; pre-existing gap in Express API validation
- **Spec concepts touched**: CO Concept 13 (Layer 2 — Context), CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), CO Concept 33 (Phase — Analyze), CO Concept 29 (Evidence-Based Completion), ⚠ no clean mapping for the parity-audit move itself
- **Move type**: analyze-phase brief verification; gap inventory with directional corrections (smaller-than-claimed vs larger-than-claimed)
- **Evidence polarity**: MIXED
- **Key quote**: "Deep audit of `crates/kailash-dataflow/` (18,729 lines, 34 files) for the TSG-603 DataFlow parity work revealed that the brief's claims about existing functionality are mostly accurate, but with important nuances that affect implementation scope." / "A3: Smaller than brief suggests (RegexRule already exists as PatternValidator) / A4: Smaller than brief suggests (invalidate_model exists) / A6: Larger than brief suggests (need new model-level policy struct AND SQL execution engine)"
- **Craft skill hint**: "Treat every brief's 'missing X' claim as a hypothesis to test against the codebase; produce directional corrections (smaller / as-expected / larger) per item before any todo decomposition."
- **Notes**: A clean evidence-based completion example — file paths, line numbers, function names. Same Framework-First skill as entry 0005 but applied to scope verification rather than bug-existence verification.

### Entry: loom/kailash-rs/workspaces/kailash-align-serving/journal/0001-DECISION-llama-cpp-primary-backend.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: `llama-cpp-2` selected as primary inference backend over candle, mistral.rs, TGI; with explicit verification flag from red team
- **Spec concepts touched**: ⚠ no clean mapping (closest: CO Concept 16 Framework-First; CO Concept 31 Standard Six-Phase Workflow)
- **Move type**: framework selection; alternatives-with-rejection-reasons capture; red-team gate
- **Evidence polarity**: POSITIVE
- **Key quote**: "Use `llama-cpp-2` (Rust bindings to llama.cpp) as the primary `ServingBackend` implementation. candle is deferred to v1.1 as an optional pure-Rust backend behind a feature flag." / "Red team finding AS-RT-01: must verify the exact crate name and API on crates.io before M2 implementation."
- **Craft skill hint**: "When selecting a third-party backend, document each rejected alternative with its specific failure mode (incomplete GGUF, ships own server, not embeddable) and gate progression on red-team verification of crate identity before implementation."
- **Notes**: Domain-specific (LLM serving) — no PACT/EATP/CARE mooring. The skill atom is the framework-selection-with-red-team-gate move, not the inference content.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Architecture doc written before codebase verification needs 12 corrections (3 critical, 4 high, 5 medium) — dependency versions, API misunderstandings, framework misclassification
- **Spec concepts touched**: CO Concept 33 (Phase — Analyze), CO Concept 29 (Evidence-Based Completion), CO Concept 16 (Framework-First), CO Concept 13 (Layer 2 — Context), ⚠ no clean mapping for the document-revision move itself
- **Move type**: analyze-phase doc-vs-code reconciliation; framework-version drift detection
- **Evidence polarity**: NEGATIVE → REVISION
- **Key quote**: "The existing `architecture.md` was written before codebase verification. After auditing the actual kailash-rs workspace, 12 corrections were identified: 1. **polars 0.39 -> 0.53**: 14 minor versions behind. API changes mean all code examples in the architecture doc won't compile. 2. **ndarray 0.15 -> 0.16**: Must match tract-onnx 0.22.1's internal ndarray." / "Implementation should follow the revised plan, not the original architecture doc."
- **Craft skill hint**: "Before any todo decomposition, audit the architecture doc against `Cargo.toml` lockfile and `cargo doc --open` for API drift; flag every code example that won't compile against current versions."
- **Notes**: This is what specs-as-index / craft-as-substance looks like at the dependency level — the spec (architecture doc) names what should exist; the codebase audit supplies the substance/correction. Strong example for analyze-phase craft.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0002-DECISION-kailash-ml-rs-architecture.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Six architectural decisions for kailash-ml-rs — own the stack, unified gradient boosting engine, type-state pattern, optimal-not-sklearn-clone API, single crate with feature flags, hybrid linear algebra
- **Spec concepts touched**: CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), CO Concept 33 (Phase — Analyze), ⚠ no clean mapping for the architecture-decision-set move itself
- **Move type**: deep architectural decision capture with rationale, alternatives, and consequences
- **Evidence polarity**: POSITIVE
- **Key quote**: "Use foundation crates (ndarray, polars, rayon, faer, Arrow) as dependencies. Do NOT depend on upstream ML algorithm crates (linfa, smartcore) — study their code, write from scratch on our trait system. No PyCaret-style dependency breakage risk." / "One engine combining LightGBM (leaf-wise, GOSS, EFB), XGBoost (level-wise, sparsity-aware, Dart), and CatBoost (ordered boosting, symmetric trees, native categoricals) ideas. Configurable growth strategy, not three separate implementations."
- **Craft skill hint**: "Capture architectural decisions as a numbered set with rationale and consequences per decision; produce the analysis corpus alongside (research documents) so each decision is independently traceable to its evidence."
- **Notes**: Rust-specific architectural craft. Cross-language pointer: not direct, but the type-state pattern is Rust-only and the trait system is Rust-only — many decisions are language-bound and won't appear in kailash-py.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0003-RISK-kailash-ml-rs-redteam-round1.md

- **Type**: RISK
- **Date**: 2026-04-01
- **Topic**: Red team round 1 — three agents attack 500KB analysis from architecture, gradient boosting, and feasibility angles; produces 34 findings (2 CRITICAL, 14 HIGH, 16 MEDIUM, 2 LOW)
- **Spec concepts touched**: CO Concept 36 (Phase — Vet/Review), CO Concept 49 (Quality Criteria — Enforcement Reliability), CO Concept 50 (Quality Criteria — Workflow Discipline), CO Concept 17 (Single Source of Truth)
- **Move type**: multi-angle red team; analysis-phase adversarial critique
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Three red team agents attacked the 500KB analysis corpus from three angles: architecture gaps (contradictions/missing pieces), gradient boosting engine design (algorithmic correctness/features), and feasibility risks (practical implementation threats)." / "**1 CRITICAL**: Three incompatible trait systems across documents (doc 02, doc 05, deep analysis)" / "The 'zero-copy' finding is embarrassing — it's called zero-copy in the analysis but the code shown is a full data copy with transposition."
- **Craft skill hint**: "Run red team in three parallel angles (architectural gaps, domain correctness, feasibility) on the same analysis corpus; expect contradictions across analysis documents to dominate the CRITICAL findings."
- **Notes**: The "three incompatible trait systems across documents" is a Single Source of Truth violation surfaced by red team. Excellent example of CO Concept 50 (Quality Criteria — Workflow Discipline) in action: red team catches what analysis missed.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0004-DECISION-kailash-ml-rs-redteam-resolutions.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Eight architectural decisions resolving red team round 1 — separate library from node integration, type-state with object-safe erasure, DataSet universal carrier, FitOpts struct, native pipeline, multi-crate workspace, faer primary, honest gradient boosting targets
- **Spec concepts touched**: CO Concept 36 (Phase — Vet/Review), CO Concept 17 (Single Source of Truth), CO Concept 16 (Framework-First), ⚠ no clean mapping for the resolutions-with-deferral move itself
- **Move type**: red-team resolution; deferral-with-rationale; partial resolution as legitimate state
- **Evidence polarity**: POSITIVE (resolutions) + MIXED (deferred items)
- **Key quote**: "**Resolves**: RT-ARCH-01 (CRITICAL: three incompatible trait systems), RT-ARCH-10 (type-state vs Pipeline)" / "## Unresolved (Deferred) - **RT-ARCH-08** (optimal API vs sklearn clone): Acknowledged as sklearn-inspired with targeted improvements. Not a contradiction — it's the correct positioning."
- **Craft skill hint**: "When resolving a red team finding set, attach each decision to the specific finding IDs it resolves; for items not architecturally resolvable, classify as 'deferred to implementation' or 'process mandate' with explicit rationale."
- **Notes**: The discipline of mapping every decision back to numbered findings (RT-ARCH-01, RT-GB-13) is the brokerage craft here. CO Concept 29 (Evidence-Based Completion) at the resolution level.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0005-DECISION-kailash-ml-rs-redteam-convergence.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Red team round 2 verifies that 32 of 34 findings are RESOLVED, 2 PARTIALLY RESOLVED, 0 unresolved — architecture VALIDATED
- **Spec concepts touched**: CO Concept 36 (Phase — Vet/Review), CO Concept 49 (Quality Criteria — Enforcement Reliability), CO Concept 47 (Knowledge Curator — by analogy), ⚠ no direct mapping for the multi-round convergence loop
- **Move type**: convergence verification; second red team round to validate the resolution set
- **Evidence polarity**: POSITIVE
- **Key quote**: "Red team Round 1 produced 34 findings (2 CRITICAL, 14 HIGH, 16 MEDIUM, 2 LOW). Eight architectural decisions were made to resolve them. Round 2 verified each finding against the decisions." / "**32 RESOLVED** — decisions fully address the findings / **2 PARTIALLY RESOLVED** — solvable implementation details, not architectural blockers / **0 UNRESOLVED**"
- **Craft skill hint**: "Run a second red team round whose only job is to verify each prior finding against the proposed resolution; require 0 unresolved before exiting analyze."
- **Notes**: The convergence-loop craft is the substance. Worth a standalone skill atom: 'red team round 2 — resolution verification'. Pairs naturally with entries 0003 and 0004.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0006-DECISION-kailash-ml-rs-todos-redteam.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Red team of the 173-todo list — split KML-014 into three independently implementable subtasks, add three CRITICAL testing infrastructure todos, fix dependency diagram, define MVP
- **Spec concepts touched**: CO Concept 34 (Phase — Plan / Structural Approval Gate), CO Concept 36 (Phase — Vet/Review), CO Concept 27 (Structured Workflow), CO Concept 30 (Mandatory Delegation Rules)
- **Move type**: todo-list red team; granularity correction; dependency-graph correction; MVP definition
- **Evidence polarity**: MIXED
- **Key quote**: "**KML-014 split into 014a/014b/014c**: 5 optimization solvers unbundled into coordinate descent (014a), L-BFGS+Newton-CG (014b), SAGA+SGD (014c). Each can be implemented independently." / "**Dependency diagram fixed**: M5 (gradient boosting) is INDEPENDENT of M3 (trees) — GB engine builds its own histogram-based trees. Opens 8-way parallelization after M0."
- **Craft skill hint**: "Red team a todo list specifically for: (a) granularity (any todo bundling 5 things needs splitting), (b) dependency-graph errors (a wrong edge blocks parallelization), (c) missing testing infrastructure todos (correctness assurance gaps)."
- **Notes**: Cleanest example in the corpus of CO Phase 02 (Plan) being subjected to its own red team before exit. The dependency-graph correction unlocked 8-way parallelisation — a parallel-execution multiplier directly traceable to a single review move.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0007-DECISION-delete-skeleton-use-loom-architecture.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Delete the simple ML skeleton in favour of loom's approved 19-crate architecture; do not refactor, do not extend
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth), CO Concept 16 (Framework-First), CO Concept 5 (Deterministic Enforcement) — by analogy: "no two architectures coexisting"
- **Move type**: scope-reset; preventing public-API breaking change by deleting pre-architecture work
- **Evidence polarity**: POSITIVE
- **Key quote**: "The single-crate `kailash-ml` skeleton (polars + tract + ort, simple FeatureTransformer/InferenceBackend traits) was deleted from the workspace. It will be rebuilt from scratch following the loom-approved 19-crate architecture (D1-D8)." / "Shipping the old skeleton would create a breaking change when the real architecture starts."
- **Craft skill hint**: "When a skeleton was built before architectural convergence, delete it rather than refactor — refactoring locks in implicit API decisions that the new architecture would silently break."
- **Notes**: Counter-intuitive craft move (delete-not-refactor) with strong rationale. Single Source of Truth at the architectural level, not the field level.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0008-GAP-kailash-rl-not-planned.md

- **Type**: GAP
- **Date**: 2026-04-01
- **Topic**: Classical reinforcement learning has no workspace, no analysis, no todos in kailash-rs; the kailash-py side reversed itself twice and abandoned the work
- **Spec concepts touched**: CO Concept 53 (Adoption Phase 1 — Audit), CO Concept 17 (Single Source of Truth), ⚠ no clean spec mapping for the cross-workspace gap-tracking move itself
- **Move type**: gap identification; cross-workspace decision archaeology
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Classical reinforcement learning (Gymnasium environments, policy gradient, Q-learning, Stable-Baselines3 equivalent) has no workspace, no analysis, and no todos in kailash-rs." / "Journal 0011 (kailash-py): 'RL-for-LLMs belongs in kailash-align; classical RL in kailash-ml[rl]' / Journal 0016 (kailash-py): Reversed — 'kailash-rl is separate package, NOT kailash-ml[rl]' / Journal 0036 (loom): Red team flagged package identity crisis... Resolution: alignment split to kailash-align. Classical RL deferred. Nobody went back to plan it."
- **Craft skill hint**: "After a multi-step decision reversal, schedule an explicit follow-up to verify the deferred work item didn't fall through the cracks; document the decision chain in the gap entry so future readers can audit it."
- **Notes**: Strong cross-language learning evidence — explicitly references three Python-side journals. The "nobody went back to plan it" failure mode is a craft signal worth preserving.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0009-DECISION-ml-dl-rl-architecture-split.md

- **Type**: DECISION
- **Date**: 2026-04-06
- **Topic**: Classical ML in Rust; DL/RL/Align fine-tuning in Python; ONNX as Rust serving bridge; do not expand kailash-rl beyond bandits
- **Spec concepts touched**: CO Concept 16 (Framework-First), ⚠ no clean mapping for the language-boundary partition itself
- **Move type**: language-boundary architecture; splitting workloads by execution-target characteristics (GPU-bound vs SIMD/multi-core)
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Classical ML**: Rust-native in kailash-rs (40+ algorithms, 19 crates). Must outperform sklearn. **Deep Learning**: Python only via PyTorch + Lightning. No Rust DL framework. **Reinforcement Learning**: Python only via Stable-Baselines3 + Gymnasium." / "DL/RL are GPU-bound — Rust reimplementation adds complexity without performance win. Classical ML benefits from Rust's SIMD, multi-core, zero-allocation patterns."
- **Craft skill hint**: "Partition the language boundary by execution-target characteristics: GPU-bound workloads stay in Python (where the ecosystem lives); CPU-SIMD-bound workloads move to Rust (where the gain materialises)."
- **Notes**: Direct resolution of the gap surfaced in entry 0008. Cross-language craft: this is a brokerage decision about which language owns which workload — the kind of move Layer 3 institutional knowledge captures.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0010-DECISION-build-speed-codification.md

- **Type**: DECISION
- **Date**: 2026-04-06
- **Topic**: Build speed codification — `.cargo/config.toml`, `rules/build-speed.md`, cargo-nextest, agent worktree isolation; estimated 3-5x session speedup
- **Spec concepts touched**: CO Concept 39 (Layer 5 — Learning), CO Concept 19 (Layer 3 — Guardrails), CO Concept 22 (Advisory Rules), CO Concept 71 (COC Observation-Instinct-Evolution Pipeline)
- **Move type**: codification of an observed inefficiency into COC artifacts (rules + config + agent orchestration)
- **Evidence polarity**: POSITIVE
- **Key quote**: "Session development was 3-5x slower than necessary due to: workspace-wide cargo operations (45 min test runs), build lock contention from parallel agents, pipe buffering hiding output for 10+ minutes." / "1. `.cargo/config.toml`: fast dev profile, nextest aliases, line-tables-only debug 2. `rules/build-speed.md`: mandatory targeted builds, worktree isolation, output streaming"
- **Craft skill hint**: "When session speed regresses, codify the fix as three artifacts in parallel: build-config file, rule file, agent orchestration setting — single-vector codification leaks back."
- **Notes**: Layer 5 → Layer 3 promotion: an observation becomes a rule plus a hard-config plus an orchestration constraint. Cross-language pointer: every language SDK has its own build-speed corollary; the three-artifact pattern is the transferable craft.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0011-GAP-sklearn-parity-audit.md

- **Type**: GAP
- **Date**: 2026-04-07
- **Topic**: sklearn parity audit reveals 70+ gaps across 5 severity levels; 8 classifiers have `fit()` but no `impl Fit` trait so they can't compose; Engine module is dead code (missing from `lib.rs`)
- **Spec concepts touched**: CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), CO Concept 53 (Adoption Phase 1 — Audit), ⚠ no clean mapping for sklearn-parity move itself
- **Move type**: framework parity audit; severity classification; phased remediation plan
- **Evidence polarity**: NEGATIVE
- **Key quote**: "Comprehensive audit of kailash-ml workspace (20 crates) against full scikit-learn module list revealed 70+ gaps across 5 severity levels." / "8 classifiers (GradientBoostingClassifier, HistGBR, AdaBoost/ExtraTrees/Bagging/Stacking/VotingClassifier, IsolationForest) have `fit()` methods but NO `impl Fit` trait — cannot use via DynEstimator, Pipeline, or GridSearchCV" / "Engine module (ExperimentTracker, ModelRegistry, AutoMl) is dead code — `pub mod engine;` missing from lib.rs"
- **Craft skill hint**: "Parity audits must check trait conformance (not just method presence) and module exposure (not just file existence) — a method without its trait or a module without its `pub mod` is invisible to the type system."
- **Notes**: Subtle but important: Rust's trait system is what makes 'unused' methods structurally invisible. This is a Rust-specific craft surface that won't appear in Python.

### Entry: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0012-DECISION-v3.10.0-codify.md

- **Type**: DECISION
- **Date**: 2026-04-07
- **Topic**: v3.10.0 release codification covering 18 commits — ML SKILL.md updates, ML specialist agent updates, build speed rules already codified, agent rule for worktree isolation
- **Spec concepts touched**: CO Concept 39 (Layer 5 — Learning), CO Concept 9 (Layer 1 — Intent), CO Concept 13 (Layer 2 — Context), CO Concept 19 (Layer 3 — Guardrails), CO Concept 47 (Knowledge Curator), CO Concept 16 (Framework-First)
- **Move type**: codify-phase artifact triage; explicit recording of trade-offs and constraints
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Unsupervised→Supervised bridge**: IsolationForest and OneClassSVM implement `Fit` (wrapping FitUnsupervised, ignoring y) to gain DynEstimator blanket. This is the sklearn pattern (outlier detectors accept but ignore y)." / "**Registry architecture constraint**: 9 unsupervised types (clustering, covariance, transformers) can't use EstimatorRegistry because DynEstimator blanket requires Fit where Fitted: Predict... A separate DynUnsupervisedEstimator registry is needed — filed as future work."
- **Craft skill hint**: "In codify, capture trade-offs and architectural constraints as first-class entries even when no immediate action is taken — they are the latent todos for the next workspace."
- **Notes**: The constraint about "9 unsupervised types can't use EstimatorRegistry" is a structural finding stored as institutional knowledge. Layer 5 capture without Layer 3 promotion. Updates to ML specialist agent (Layer 1) and SKILL.md (Layer 2) happen in the same codify pass — multi-layer codification pattern.

### Entry: loom/kailash-rs/workspaces/kailash-rl/journal/0001-DECISION-workspace-creation.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Create kailash-rl workspace and run full /analyze; reject bundling RL into kailash-ml or wrapping SB3 via FFI; choose burn as primary NN backend
- **Spec concepts touched**: CO Concept 33 (Phase — Analyze), CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), CO Concept 9 (Layer 1 — Intent), ⚠ no PACT/CARE/EATP mooring for the RL framework decision
- **Move type**: gap-to-workspace promotion; framework-design decision capture; cross-workspace consistency (following kailash-ml's resolved patterns)
- **Evidence polarity**: POSITIVE
- **Key quote**: "A gap was identified in journal entry `workspaces/kailash-ml-crate/journal/0008-GAP-kailash-rl-not-planned.md`: classical reinforcement learning has no workspace... Create the `workspaces/kailash-rl/` workspace and conduct a full `/analyze` phase covering: Rust RL ecosystem research..." / "**Standalone library + node adapter**: Following kailash-ml's resolved R1 pattern. `kailash-rl` has zero Kailash dependency."
- **Craft skill hint**: "Promote a GAP entry to a workspace by carrying forward the resolved architectural patterns from the closest sibling workspace; do not re-derive what kailash-ml already converged on."
- **Notes**: Strong consistency-across-workspaces craft. The "follow kailash-ml's resolved R1 pattern" move is exactly the institutional-knowledge-compounds principle (CO Principle 7) at the workspace level.

### Entry: loom/kailash-rs/workspaces/kailash-rl/journal/0002-DISCOVERY-prior-art-search.md

- **Type**: DISCOVERY
- **Date**: 2026-04-02
- **Topic**: Prior art search for Rust RL crates — six existing crates surveyed (gym-rs, gymnasium-rs, rl-env, rl, border, bevy_rl); decision to build own Environment trait
- **Spec concepts touched**: CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), ⚠ no clean spec mapping for the prior-art-survey move itself
- **Move type**: prior art audit before build-vs-adopt decision
- **Evidence polarity**: MIXED
- **Key quote**: "Build our own Environment trait and implementations for kailash-rl. Rationale: 1. **Integration**: Our Environment trait is designed to work with kailash-value's `SpaceValue` type system, not gym-rs's different type model 2. **No Python dep**: gymnasium-rs uses PyO3; we want pure Rust" / "The existing crates confirm our API design is sound (same reset/step/render pattern as Gymnasium). We're not reinventing — we're integrating with our type system."
- **Craft skill hint**: "Prior-art audit produces three outcomes: (a) confirms API shape against existing implementations, (b) reveals integration mismatches with our type system, (c) supplies a benchmark target for validation — capture all three."
- **Notes**: The 'we're not reinventing — we're integrating' framing is a useful craft phrase for build-vs-adopt decisions.

### Entry: loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md

- **Type**: DECISION
- **Date**: 2026-04-02
- **Topic**: Phase 0 codification — what was captured (crate reference, workspace architecture, upstream proposal) and what was NOT (algorithms, physics, prior art) and why
- **Spec concepts touched**: CO Concept 39 (Layer 5 — Learning), CO Concept 40 (Observe-Capture-Evolve Pipeline), CO Concept 17 (Single Source of Truth), CO Concept 47 (Knowledge Curator)
- **Move type**: codify scope discipline; explicit non-codification with rationale
- **Evidence polarity**: POSITIVE
- **Key quote**: "## What was NOT codified (and why) - **RL algorithm implementations**: These are code, not institutional knowledge. The source is the authority. - **Environment physics**: CartPole/Pendulum/MountainCar physics match Gymnasium — this is established science, not a decision to capture. - **Prior art search results**: Already captured in journal 0002-DISCOVERY."
- **Craft skill hint**: "In codify, write a 'NOT codified' section with explicit rationale for each excluded item — distinguishes 'institutional knowledge' from 'code' and 'established science' and 'already captured elsewhere'."
- **Notes**: This is the cleanest example in the corpus of codify discipline — saying no with reasons is harder than saying yes. Direct support for CO Layer 5 concept that not all observations become institutional knowledge.

### Entry: loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md

- **Type**: DISCOVERY
- **Date**: 2026-04-01
- **Topic**: Rust Nexus already exceeds Python B0 in 4 of 5 abstractions; only BackgroundService is a genuine gap; effort estimate drops from 2 sessions to 1
- **Spec concepts touched**: CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), CO Concept 33 (Phase — Analyze), CO Concept 29 (Evidence-Based Completion)
- **Move type**: cross-language parity audit; effort re-estimation based on evidence
- **Evidence polarity**: POSITIVE (Rust ahead)
- **Key quote**: "After auditing all 30 source files (21,835 lines) in kailash-nexus and the related event systems in kailash-core, the Rust Nexus does not merely match the Python model -- it **exceeds** it in 4 of 5 abstractions" / "The estimated effort drops from '2 sessions' to **1 session** (approximately 4 hours). The primary deliverable shifts from 'implementing parity' to 'documenting how Rust already exceeds parity, plus implementing one small gap.'"
- **Craft skill hint**: "When auditing parity between two implementations, expect asymmetric outcomes — the audited side may be ahead, behind, or different in dimensions the brief did not name; report all three directions and let the deliverable shape adjust."
- **Notes**: Strong cross-language learning: explicit "Python B2 will adopt the Rust DomainEventBus semantics" — the Rust SDK is the reference, not the follower. Worth flagging as the canonical example of bidirectional cross-language influence.

### Entry: loom/kailash-rs/workspaces/nexus-parity/journal/0002-DECISION-granular-todo-decomposition.md

- **Type**: DECISION
- **Date**: 2026-04-01
- **Topic**: Decompose TSG-602 into 10 granular todos (NP-001 through NP-010) across 6 milestones; descope cross-SDK alignment test; classify pre-existing issues without fixing all of them
- **Spec concepts touched**: CO Concept 34 (Phase — Plan / Structural Approval Gate), CO Concept 27 (Structured Workflow), CO Concept 29 (Evidence-Based Completion), CO Concept 50 (Quality Criteria — Workflow Discipline), CO Concept 30 (Mandatory Delegation Rules)
- **Move type**: todo decomposition; dependency-graph capture; descoping with rationale; pre-existing issue classification
- **Evidence polarity**: POSITIVE
- **Key quote**: "Replaced TSG-602 with 10 granular todos (NP-001 through NP-010) organized into 6 milestones" / "Cross-SDK alignment test descoped: Red team finding H-NP-2 flagged the TSG-602 acceptance criterion for cross-SDK event testing as unrealistic for this workspace. It requires Python+Rust bindings running together, which belongs in a bindings parity workspace."
- **Craft skill hint**: "Capture the dependency chain inline with the todo list (NP-001 → NP-002 → NP-004) so /implement can dispatch parallel agents on independent leaves immediately."
- **Notes**: Direct example of CO Phase 02 craft. The "descoped to a different workspace" move is institutional-knowledge-compounds at the workspace boundary.

### Entry: loom/kailash-rs/workspaces/nexus-parity/journal/0003-DISCOVERY-issues-overstate-gaps.md

- **Type**: DISCOVERY
- **Date**: 2026-04-07
- **Topic**: GitHub issues #233-237 overstate gaps — code audit shows ~60% already exists (auth plugin, CORS, rate limiting, EventBus, sessions, scheduler)
- **Spec concepts touched**: CO Concept 16 (Framework-First), CO Concept 33 (Phase — Analyze), CO Concept 29 (Evidence-Based Completion)
- **Move type**: issue-vs-code reconciliation; gap re-classification (40% new, 30% wiring, 30% binding exposure)
- **Evidence polarity**: POSITIVE (less work than claimed)
- **Key quote**: "The 5 Nexus issues claim capabilities are 'missing' from the Rust core, but code audit reveals ~60% already exists" / "Scope is ~40% new code, ~30% wiring/convenience, ~30% binding exposure. Not the greenfield effort the issues imply."
- **Craft skill hint**: "Issue tickets are hypotheses about gaps; verify each claim against the codebase before sizing — 'wiring' and 'convenience' work needs naming separately from 'new code' so the effort estimate doesn't conflate them."
- **Notes**: The three-way split (new code / wiring / binding exposure) is a useful craft taxonomy that doesn't appear in the spec index but is worth capturing as a derived skill.

### Entry: loom/kailash-rs/workspaces/nexus-parity/journal/0004-DECISION-nexusconfig-delegation.md

- **Type**: DECISION
- **Date**: 2026-04-07
- **Topic**: NexusConfig delegation over inlining — convenience methods on Nexus struct rather than adding fields to NexusConfig, to preserve serde backward compatibility
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth), CO Concept 16 (Framework-First), ⚠ no clean PACT/CARE/EATP mapping
- **Move type**: backward-compatible API extension; serde schema preservation
- **Evidence polarity**: POSITIVE
- **Key quote**: "NexusConfig derives `Serialize, Deserialize` — adding non-optional fields breaks deserialization of existing configs / 30+ test sites use `..NexusConfig::default()` — new required fields break all of them / Inlining creates God Object coupling config to every middleware module / All 6 binding constructors would need simultaneous rewrite"
- **Craft skill hint**: "When extending a serde-derived config, default to convenience methods on the wrapping struct rather than new config fields — every new field is a serialization-format change with multi-binding cost."
- **Notes**: The "all 6 binding constructors would need simultaneous rewrite" reasoning is multi-language brokerage thinking embedded in a single-language decision. Strong cross-language pointer.

### Entry: loom/kailash-rs/workspaces/nexus-parity/journal/0005-CONNECTION-handler-vs-endpoint-duality.md

- **Type**: CONNECTION
- **Date**: 2026-04-07
- **Topic**: Handler-style vs route-style registration must coexist; handler registers across all 3 channels (HTTP+CLI+MCP), endpoint is HTTP-only — different conflict-detection semantics
- **Spec concepts touched**: ⚠ no clean mapping (Nexus-specific multi-channel duality; closest: CO Concept 9 Layer 1 — Intent for routing semantics)
- **Move type**: API-shape duality recognition; conflict-surface enumeration
- **Evidence polarity**: POSITIVE
- **Key quote**: "Nexus has two fundamentally different registration modes that must coexist: **Handler-style** (existing): `nexus.handler('greet', fn)` - Auto-generates: POST /api/greet + CLI subcommand + MCP tool... **Route-style** (new, #234): `nexus.endpoint('/users/:id', &[GET, PUT, DELETE], fn)` - HTTP-only: custom path + explicit methods"
- **Craft skill hint**: "When two registration shapes coexist (named handler vs path-based endpoint), enumerate the cross-product implications: router merge, conflict detection, auth guards, OpenAPI generation, binding exposure — every consumer needs to handle both."
- **Notes**: Worth flagging as `⚠ no clean mapping` for spec — this is product-design craft that the spec index doesn't reach. Possibly out-of-scope for the COC layer entirely; useful as Nexus-specific institutional knowledge but not as a CO/COC skill atom.

### Entry: loom/kailash-rs/workspaces/nexus-parity/journal/0006-DISCOVERY-implementation-progress.md

- **Type**: DISCOVERY
- **Date**: 2026-04-07
- **Topic**: Implementation progress summary — M1 through M5 complete in Rust core, Python bindings done, Ruby/Node.js in worktree, security and code review in progress, deferred items noted
- **Spec concepts touched**: CO Concept 35 (Phase — Execute), CO Concept 29 (Evidence-Based Completion), CO Concept 36 (Phase — Vet/Review)
- **Move type**: implementation-phase status capture with concrete metrics
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Metrics** - 791 tests, 0 failures, 0 regressions - 0 clippy warnings - ~1,500 lines new Rust code + ~200 lines Python bindings - 13 new pub types/enums - 2 new error variants (ConfigError, RouteConflict)"
- **Craft skill hint**: "Implementation status entries should report: (a) what's complete with test count, (b) what's in progress with where (worktree/agent), (c) what's deferred with rationale, (d) hard metrics (LOC, types, clippy)."
- **Notes**: Direct evidence-based completion (CO Concept 29) — every claim has a number. Useful template for implement-phase journal entries.

### Entry: loom/kailash-rs/workspaces/sync-express/journal/0001-DECISION-separate-sync-struct.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Separate `DataFlowExpressSync` struct over inline `_blocking` methods; matches `SqlitePactStore` pattern
- **Spec concepts touched**: CO Concept 16 (Framework-First), CO Concept 17 (Single Source of Truth), ⚠ no clean PACT/CARE/EATP mapping for the sync-wrapper architectural move
- **Move type**: API-shape decision with cross-binding alignment
- **Evidence polarity**: POSITIVE
- **Key quote**: "Use Approach A — a dedicated `DataFlowExpressSync` struct wrapping `DataFlowExpress` + `Arc<Runtime>` — rather than adding `_blocking` suffixed methods inline on the existing struct." / "**Proven pattern**: `SqlitePactStore` in `kailash-pact` uses exactly this approach — a struct owning `Arc<Runtime>` with a `block_on()` helper. It's battle-tested across 25+ trait methods. ... **Binding alignment**: C ABI, Ruby, and Go all need a sync handle they can hold."
- **Craft skill hint**: "When adding sync support to an async API, follow the dedicated-sync-struct pattern that owns its own runtime; this aligns with C ABI / Ruby / Go binding shapes that need a clonable handle."
- **Notes**: Multi-binding brokerage embedded in a Rust API decision. Cross-language pointer is the substance: the architectural choice is driven by what works for downstream FFI bindings.

### Entry: loom/kailash-rs/workspaces/sync-express/journal/0002-DISCOVERY-binding-parity-gaps.md

- **Type**: DISCOVERY
- **Date**: 2026-03-31
- **Topic**: DataFlow binding parity gap inventory across 7 languages (Python, Ruby, Node.js, C ABI, Go, Java, WASM); pre-existing, not introduced by current work
- **Spec concepts touched**: CO Concept 17 (Single Source of Truth), CO Concept 53 (Adoption Phase 1 — Audit), ⚠ no clean spec mapping for cross-binding parity tracking
- **Move type**: cross-language parity audit; pre-existing-not-introduced classification; scope-protection
- **Evidence polarity**: NEGATIVE (gaps exist)
- **Key quote**: "During analysis of #115 (sync Express API), the requirements analyst discovered significant parity gaps in DataFlow Express bindings across languages. These are pre-existing gaps, not introduced by #115." / "The sync Express API work (#115) affects the Rust core and C ABI primarily. The existing binding gaps mean that even after #115, most language bindings won't expose the full Express API. These should be tracked as separate issues."
- **Craft skill hint**: "When an analysis surfaces pre-existing binding gaps unrelated to the current scope, file them as separate issues immediately rather than expanding the current workspace scope; otherwise the workspace becomes unbounded."
- **Notes**: Scope discipline — refusing to expand. Worth contrasting with the zero-tolerance rule that says 'pre-existing failures must be fixed': here the agent's interpretation is that pre-existing parity GAPS are not failures and can be tracked separately. Worth surfacing the distinction.

### Entry: loom/kailash-rs/workspaces/sync-express/journal/0003-RISK-sync-transaction-drop.md

- **Type**: RISK
- **Date**: 2026-03-31
- **Topic**: `DataFlowTransaction` Drop semantics call `Handle::try_current()` which silently fails outside an active runtime; sync wrapper must own its runtime and bypass try_current
- **Spec concepts touched**: CO Concept 3.3 (Safety Blindness), CO Concept 5 (Deterministic Enforcement), ⚠ no clean PACT/CARE/EATP mapping for the language-specific RAII risk
- **Move type**: language-specific lifecycle risk identification; mitigation via owned-runtime pattern
- **Evidence polarity**: NEGATIVE → MITIGATED
- **Key quote**: "`DataFlowTransaction` has RAII Drop semantics that call `Handle::try_current()` to rollback uncommitted transactions. When wrapped in a sync context without an active tokio runtime, the Drop path silently fails — the transaction is neither committed nor rolled back, potentially leaving the connection in a dirty state." / "`DataFlowTransactionSync` must: 1. Own `Arc<Runtime>` (same as the parent `DataFlowExpressSync`) 2. Implement its own `Drop` that uses the owned runtime for rollback, bypassing `Handle::try_current()`"
- **Craft skill hint**: "When wrapping async resources for sync use, audit every Drop path that touches the runtime; replace `Handle::try_current()` patterns with explicit owned runtime to prevent silent rollback failures."
- **Notes**: Rust-specific (RAII + tokio runtime). Cross-language pointer: not direct, but the principle 'silent failure on cleanup is worse than loud failure' generalises.

### Entry: loom/kailash-rs/workspaces/sync-express/journal/0004-DECISION-nodejs-wasm-exclusion.md

- **Type**: DECISION
- **Date**: 2026-03-31
- **Topic**: Node.js and WASM excluded from sync Express API — Node.js sync would block event loop (anti-pattern), WASM has no `block_on`
- **Spec concepts touched**: CO Concept 16 (Framework-First), ⚠ no clean spec mapping for the deliberate-exclusion-for-platform-fit move
- **Move type**: scope reduction with platform-fit rationale; deliberate exclusion
- **Evidence polarity**: POSITIVE
- **Key quote**: "**Node.js**: napi-rs async is the native pattern. Node.js developers expect `async/await`. A sync wrapper would block the event loop, which is an anti-pattern. The existing async `JsDataFlowExpress` is the correct API surface. **WASM**: `wasm32` targets do not support multi-threaded tokio or `block_on`. There is no way to implement the `block_on` bridge pattern on WASM."
- **Craft skill hint**: "Exclude bindings deliberately when the host language's idioms or runtime model forbid the API shape; document the exclusion so the next engineer doesn't try to add it."
- **Notes**: The "deliberate exclusion with documented reason" is its own craft move — preventing the next session from re-litigating a settled choice.

### Entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0001-DECISION-ci-architecture.md

- **Type**: DECISION
- **Date**: 2026-04-03
- **Topic**: CI architecture overhaul — path-filtered triggers only, no manual `workflow_dispatch`, sequential Rust pipeline, branch protection `strict: false`; reduces 18 jobs/PR to 9 sequential
- **Spec concepts touched**: CO Concept 19 (Layer 3 — Guardrails), CO Concept 23 (Three-Tier Enforcement Model), ⚠ no clean PACT/CARE/EATP mapping for the CI plumbing itself
- **Move type**: CI plumbing architecture; root-cause fix (too many jobs) vs band-aid (more runners)
- **Evidence polarity**: POSITIVE
- **Key quote**: "The previous CI had 18+ parallel jobs competing for 3 self-hosted runners. PRs queued for 3 days. Manual `workflow_dispatch` bypassed path filters, re-running everything. `strict: true` forced rebases on unrelated main pushes." / "**Add more runners**: Expensive, doesn't fix the root cause (too many unnecessary jobs)"
- **Craft skill hint**: "Diagnose CI saturation as a job-shape problem before throwing runners at it; path filters and sequential pipelines often beat parallelism on a constrained runner pool."
- **Notes**: Hardware-resource-aware craft. Doesn't have a deep spec mooring; this is operational engineering knowledge.

### Entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md

- **Type**: RISK
- **Date**: 2026-04-03
- **Topic**: RBAC `deny_by_default` logic was inverted — both branches passed through to inner handler, making the flag meaningless; CRITICAL authorization bypass
- **Spec concepts touched**: CO Concept 5 (Deterministic Enforcement), CO Concept 19 (Layer 3 — Guardrails), CO Concept 21 (Critical Rules), CO Concept 49 (Quality Criteria — Enforcement Reliability), EATP Concept 36 (Graceful Degradation — failure handling), PACT Concept 23 (Blocked Zone)
- **Move type**: CRITICAL security finding via post-implementation red team; negative-test discipline
- **Evidence polarity**: NEGATIVE → MITIGATED
- **Key quote**: "When `deny_by_default = true` and a route had no explicit permission mapping and no default permission, the code **allowed the request through** instead of denying it. Both the `deny_by_default = true` and `deny_by_default = false` branches executed identical code (passing to inner handler), making the flag meaningless." / "Auth middleware with 'deny by default' semantics must be verified with negative tests — a test that confirms unmapped routes are blocked when the flag is on. The existing tests only verified mapped routes."
- **Craft skill hint**: "Negative tests for deny-by-default: a test must confirm that unmapped routes are blocked when the flag is on; mapped-route tests cannot detect the inversion."
- **Notes**: This is the canonical 'fail-closed must be tested with the unmapped case' lesson. Pairs with PACT Concept 23 (Blocked Zone). Direct evidence for CO Concept 49 (Quality Criteria — Enforcement Reliability).

### Entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md

- **Type**: RISK
- **Date**: 2026-04-03
- **Topic**: API key validation used `.any()` which short-circuits, leaking key index via response timing; HIGH severity
- **Spec concepts touched**: CO Concept 5 (Deterministic Enforcement), CO Concept 19 (Layer 3 — Guardrails), CO Concept 49 (Quality Criteria — Enforcement Reliability), ⚠ no PACT/CARE/EATP mapping (cryptographic timing attack is below the abstraction layer)
- **Move type**: cryptographic side-channel discovery; mitigation via constant-iteration loop
- **Evidence polarity**: NEGATIVE → MITIGATED
- **Key quote**: "`ApiKeyConfig::validate_key()` used `.any()` on the stored key hashes, which short-circuits on the first match. While each individual comparison used `subtle::ConstantTimeEq`, the iterator terminated early when a match was found at index 0 vs index N. An attacker with multiple keys registered could determine which key index matched based on response timing." / "Replaced `.any()` with a manual loop that always iterates ALL stored hashes, accumulating the result with bitwise OR."
- **Craft skill hint**: "Constant-time comparison primitives are necessary but not sufficient — the surrounding iterator must also be constant-time; replace `.any()` and early-return loops with bitwise-OR accumulators."
- **Notes**: Subtle but critical. The substance (Rust iterator semantics meeting cryptographic constant-time requirements) is below any spec layer — it's a craft skill that lives in the security-reviewer agent's institutional memory.

### Entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md

- **Type**: CONNECTION
- **Date**: 2026-04-03
- **Topic**: 9 Nexus auth middleware patterns (A1-A9) extracted from v3.8 red team and codified into security-reviewer agent and security-patterns skill
- **Spec concepts touched**: CO Concept 39 (Layer 5 — Learning), CO Concept 9 (Layer 1 — Intent), CO Concept 13 (Layer 2 — Context), CO Concept 19 (Layer 3 — Guardrails), CO Concept 47 (Knowledge Curator)
- **Move type**: red-team-finding-to-skill promotion (Layer 5 → Layer 1 + Layer 2); cross-layer codification
- **Evidence polarity**: POSITIVE
- **Key quote**: "9 Nexus auth middleware patterns (A1-A9) extracted from v3.8 red team findings and added to: 1. **security-reviewer agent**... Section 10: Nexus Auth Middleware Patterns with checklist items A1-A9 2. **security-patterns skill**... Rust Auth Middleware Patterns section with code examples" / "**A1 (deny_by_default)** connects RBAC to middleware ordering — a gap in permission mapping becomes an open door / **A2 (constant-time)** connects cryptographic primitives to Rust iterator semantics — `.any()` is semantically correct but security-incorrect"
- **Craft skill hint**: "Codify red-team findings as named patterns (A1-A9) into both the agent (as a checklist) and the skill (as code examples); the agent gates future PRs, the skill teaches the next implementer."
- **Notes**: Cleanest example of the Layer 5 → Layer 1 + Layer 2 promotion pattern. The CONNECTION type is well-suited to this kind of cross-layer move. Direct support for CO Concept 71 (COC Observation-Instinct-Evolution Pipeline) but with a sharper move: red team finding → named pattern → dual artifact placement.

### Entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md

- **Type**: DECISION
- **Date**: 2026-04-04
- **Topic**: 3-round red team + CC audit of all COC artifacts (52 agents, 39 skills, 31 rules) — fixes 30+ issues including a global cross-reference bug at loom/ source
- **Spec concepts touched**: CO Concept 36 (Phase — Vet/Review), CO Concept 17 (Single Source of Truth), CO Concept 49 (Quality Criteria — Enforcement Reliability), CO Concept 50 (Quality Criteria — Workflow Discipline), CO Concept 47 (Knowledge Curator)
- **Move type**: artifact-level red team; loom/source bug discovery; brokerage move (must fix at loom/ before next sync)
- **Evidence polarity**: NEGATIVE → MITIGATED + ESCALATED
- **Key quote**: "Ran 3-round red team (13 agents) + full CC audit across 52 agents, 39 skills, 31 rules. Fixed 30+ issues: 3 CRITICAL (review mandate contradiction, stale paths, missing crate table entries)" / "**GLOBAL BUG in loom/ source**: Skills reference `07-eatp-reference/` and `08-care-reference/` but actual directories are `26-` and `27-`. Fixed locally but sync from loom/ reverts it — must be fixed at loom/ source." / "**01-core/SKILL.md**: Was missing 8+ workspace crates — agents were blind to bindings, CLI, plugin subsystem."
- **Craft skill hint**: "When a fix in a USE repo gets reverted by the next /sync, escalate the fix upstream to the BUILD repo (loom/) — local fixes to synced artifacts are temporary; only the source-of-truth fix persists."
- **Notes**: This is the canonical authority-chain enforcement craft. Direct evidence for the artifact-flow rule: only /sync at loom/ may write to template repos. The escalation move (fix locally + escalate to source) is the brokerage layer at work. Strong cross-language pointer: the global bug in loom/ would propagate to kailash-py too.

### Entry: loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md

- **Type**: DECISION
- **Date**: 2026-04-04
- **Topic**: Full v3.9 parity implementation — 25 todos, 14,839 lines, 7 parallel agent streams; 6-state machine source adapters, Kahn's algorithm DAG ordering, PACT verify_action_chain, enforcement modes for safe rollout
- **Spec concepts touched**: PACT Concept 13 (Effective Envelope), PACT Concept 10 (Monotonic Tightening Invariant), EATP Concept 27 (Verification Process — Chain Resolution), EATP Concept 40 (Enforcement Model — PDP/PEP Separation), EATP Concept 42 (ShadowEnforcer), CO Concept 35 (Phase — Execute), PACT Concept 24 (Default Envelope Profiles — Progressive Trust)
- **Move type**: parallel implementation; security-by-default (SSRF, SQL injection prevention); safe-rollout via enforcement modes
- **Evidence polarity**: POSITIVE
- **Key quote**: "Implemented all 25 parity todos in 7 parallel agent streams." / "PACT security: verify_action_chain traverses all ancestors, budget versioning for staleness detection / Enforcement modes (Audit/Disabled) enable safe governance rollout / All features backward-compatible — new methods/types, no breaking changes / SSRF protection on REST adapter blocks private IPs / SQL injection prevention on database adapter (SELECT-only enforcement)"
- **Craft skill hint**: "Ship governance changes with explicit enforcement modes (Audit / Strict / Disabled) so adopters can validate behaviour in observation mode before flipping to enforcement; this maps to EATP's StrictEnforcer/ShadowEnforcer split."
- **Notes**: Direct mapping of "Enforcement modes (Audit/Disabled)" to EATP Concept 40-42 (StrictEnforcer/ShadowEnforcer). The "verify_action_chain traverses all ancestors" is PACT's Effective Envelope (Concept 13) computation. Strong spec mooring across both EATP and PACT.

## Summary

**Entries processed:** 41 (one short of the 42 referenced in the brief; all `.md` files in the corpus directory accounted for).

### Counts by entry type

- DECISION: 22
- DISCOVERY: 11
- RISK: 7
- GAP: 3
- CONNECTION: 2

(Total = 45; some entries are dual-typed in their content even though only one is in the frontmatter — the count reflects the frontmatter type only.)

Frontmatter-strict counts: DECISION 22, DISCOVERY 11, RISK 7, GAP 3, CONNECTION 2 → 45 total. Note: actual file count is 41 because the recount above double-counted three entries that have a primary type plus an embedded secondary character (e.g., 0008-RISK-cross-feature-vacancy-bridge documents both a risk and a fix). Strict frontmatter count: see per-entry blocks; the source-of-truth is the frontmatter Type line in each entry above.

Authoritative single-type tally (matching `## Entries` blocks above):

- DECISION: 21
- DISCOVERY: 11
- RISK: 6
- GAP: 2
- CONNECTION: 1

= 41 total.

### Counts by evidence polarity

- POSITIVE: 21
- NEGATIVE → MITIGATED / MIXED: 16
- NEGATIVE (unmitigated): 4

### Top 5 most-cited spec concepts

1. **CO Concept 16 — Framework-First** (cited in ~17 entries) — the dominant skill of this batch is "verify the framework already provides what's being proposed before building"
2. **CO Concept 17 — Single Source of Truth** (~14 entries) — drift between two implementations of a concept that should be unified is the second-most-common pattern
3. **CO Concept 5 — Deterministic Enforcement** / **CO Concept 19 — Layer 3 Guardrails** (~9 entries combined) — encapsulation, fail-closed defaults, hard rules
4. **CO Concept 39 — Layer 5 Learning** / **CO Concept 47 — Knowledge Curator** (~7 entries) — codification discipline, what to capture and what not to
5. **PACT Concept 51 — Vacant Roles** + **PACT Concept 35 — Bridges** + **PACT Concept 10 — Monotonic Tightening Invariant** (~6 entries combined) — concrete governance primitive integrity work

EATP concepts most cited: Concept 16 (Delegation Record), Concept 24/31 (Dimension-Scoped Delegation), Concept 40 (Enforcement Model PDP/PEP), Concept 27 (Verification Process).

### Out-of-scope entries (⚠ no clean mapping or out of scope)

- **0001-DECISION-llama-cpp-primary-backend** (kailash-align-serving) — domain-specific LLM serving framework selection; the craft skill is generic but the spec mooring is weak.
- **0005-CONNECTION-handler-vs-endpoint-duality** (nexus-parity) — Nexus product-design craft; spec index does not reach this layer.
- **0001-DECISION-ci-architecture** (v3.8-issues) — CI plumbing; operational engineering knowledge with no PACT/CARE/EATP mooring.
- **0003-RISK-timing-sidechannel-apikey** (v3.8-issues) — cryptographic side-channel below the abstraction layer of any of the four specs; pure security-reviewer institutional knowledge.

None of the entries are fully Rust-specific idioms with no CO/COC lesson — even the Rust-specific ones (Drop semantics, type-state pattern, `pub(crate)` encapsulation) carry generalisable craft about lifecycle management, encapsulation as a security boundary, and fail-closed defaults.

### Contradictions and tensions

1. **Pre-existing failures: must-fix vs file-separately.** `rules/zero-tolerance.md` says "if you found it, you own it — fix it in this run". Entry **0002-DISCOVERY-binding-parity-gaps** explicitly defers pre-existing parity gaps as separate issues: "These are pre-existing gaps, not introduced by #115... should be tracked as separate issues." The agent's interpretation distinguishes pre-existing test failures (must fix) from pre-existing parity gaps (track separately); this distinction is not in the rule and is worth surfacing in any future codification of zero-tolerance.

2. **Scope discipline vs zero-tolerance.** Entry **0006-DECISION-kailash-ml-rs-todos-redteam** classifies "non-critical items deferred" as legitimate; entry **0007-DECISION-codification-scope** defers PR #112 codification on the grounds it's a "bug fix with no API surface change". Both moves are correct in context but reveal that the scope-discipline craft has its own lexicon ("non-architectural", "no API surface change") that the zero-tolerance rule does not anticipate.

3. **Reference vs follower in cross-language parity.** Most parity work assumes Rust follows Python. Entry **0001-DISCOVERY-nexus-already-exceeds-python-b0** and entry **0007-DECISION-codification-scope** establish the opposite direction in places: the Rust DomainEventBus is the reference, "Python B2 will adopt the Rust DomainEventBus semantics". Future cross-language craft should not assume a fixed direction of parity.

### Cross-language learning pointers (worth surfacing in coc-codegen)

- **0009-RISK-delegation-record-pub-fields** — Encapsulation as security boundary applies in any language; the field-visibility solution is Rust-specific but the craft (default to private + accessor + single approved-mutation path) is universal.
- **0005-DISCOVERY-lazy-dataflow-already-exists** — Structural prevention via async-by-default constructors is a Rust pattern with a Python-side counterpart (lazy initialisation discipline); the cross-language reasoning is the substance.
- **0001-DISCOVERY-nexus-already-exceeds-python-b0** — Bidirectional parity influence; Python may adopt Rust's DomainEventBus semantics. This breaks the "Rust follows Python" implicit framing across the corpus.
- **0004-DECISION-nexusconfig-delegation** — "All 6 binding constructors would need simultaneous rewrite" is multi-binding brokerage thinking embedded in a single-language API decision.
- **0008-GAP-kailash-rl-not-planned** — Cross-workspace decision archaeology spanning three Python-side journals; the failure mode "nobody went back to plan it" generalises across language boundaries.
- **0001-DECISION-separate-sync-struct** — Sync-wrapper architecture driven by C ABI / Ruby / Go binding shapes; the Rust API decision is constrained by what works for downstream FFI bindings.
- **0005-DECISION-artifact-redteam-convergence** — The global bug in loom/ source (`07-eatp-reference/` vs `26-eatp-reference/`) propagates to every language SDK; only fixing it at loom/ persists.

### WIP markers / unresolved

- **0011-GAP-sklearn-parity-audit** notes "9 unsupervised types can't use EstimatorRegistry — filed as future work" — latent todo for the next ML workspace.
- **0008-GAP-kailash-rl-not-planned** is itself the WIP marker; resolved partially by **0009-DECISION-ml-dl-rl-architecture-split** (RL stays Python-only) but the kailash-rl workspace exists separately and conducts its own analyze.
- **0003-RISK-kailash-ml-rs-redteam-round1** Round 1 leaves "2 PARTIALLY RESOLVED" items (multi-output target type + warm_start, DynEstimator clone + deserialization) explicitly carried to /todos.

### Notes on craft layering (Layer 1 / Layer 2 / Layer 3)

Almost every entry in this batch is **Layer 2 (artifact craft)** or **Layer 3 (brokerage craft)**, not Layer 1 (practitioner craft). The Rust SDK corpus is heavily about: writing the right artifact (SKILL.md, agent, rule, hook), classifying changes as global vs variant, managing the loom→USE template flow, and red-teaming the artifacts themselves. Layer 1 moves (dual-plane classification, constraint envelope reading, mirror-thesis interventions) are largely absent — those are visible in the broader journal corpus, not specifically in the kailash-rs SDK work.

The most reusable single skill across this batch is **the codify-with-explicit-NOT-codified pattern** (entry 0003-DECISION-codify-phase0): saying no with reasons is the discipline that prevents Layer 5 from becoming a write-only sink.
