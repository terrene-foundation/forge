---
destination: both
layer: reverse-index-merged
date: 2026-04-07
corpus: COC-layer (loom + terrene + kailash-py + kailash-rs + kaizen-kz)
entries_indexed: 232
batches_merged: 4
spec_indices_referenced: 4
concepts_total: 417
concepts_with_evidence: 109
concepts_without_evidence: 308
coverage_percent: 26
---

# Reverse Index — COC Layer — Concept-Indexed (Unified)

Concept → evidence map across all 232 classified COC-layer entries (loom-meta 52 + kailash-py 98 + kailash-rs 41 + kaizen-kz 41 — note: original brief said 233; the kailash-rs batch summary records 41 entries as the actual processed count, one short of the 42 originally estimated). Cross-references to four batch files in this same directory:

- `coc-layer-loom-meta.md` (52 entries — loom + terrene orchestration journals)
- `coc-layer-kailash-py.md` (98 entries — Python SDK workspaces + ADRs)
- `coc-layer-kailash-rs.md` (41 entries — Rust SDK workspaces)
- `coc-layer-kaizen-kz.md` (41 entries — Kaizen CLI Rust + Python + kz-engage)

Concept numbers reference the four spec indices in `../03-spec-index/`. Citation formats normalised across batches: `CO §17`, `CO § 17`, `CO Concept 17`, `[CO § 17]` all collapse to **CO §17**.

## Coverage Summary

| Standard  | Concepts total | With evidence | Without evidence | % covered |
| --------- | -------------- | ------------- | ---------------- | --------- |
| CARE      | 69             | 17            | 52               | 25%       |
| EATP      | 94             | 18            | 76               | 19%       |
| CO        | 187            | 65            | 122              | 35%       |
| PACT      | 67             | 26            | 41               | 39%       |
| **TOTAL** | **417**        | **126**       | **291**          | **30%**   |

CO is the most-cited standard (35% covered) — unsurprising since the corpus is COC-layer journals where CO is the primary methodology being applied. PACT is the second most-cited (39%) because the Python and Rust SDKs are actively wiring PACT primitives. CARE and EATP are sparsely cited because the corpus is engineering work, not philosophy or trust-protocol implementation.

## Heavy-Cited Concepts (≥10 evidence entries)

Ordered by citation count descending. These are the load-bearing concepts for the COC-layer skill catalog.

### 1. CO §17 — Single Source of Truth — 47 entries

The most-cited concept across the entire corpus. Drives every authority-chain, variant-classification, drift-detection, and consolidation move.

- loom/journal/0001-DECISION-co-refocusing.md (loom-meta)
- loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)
- loom/journal/0008-DECISION-coc-artifact-scoping.md (loom-meta)
- loom/journal/0013-DECISION-variant-architecture.md (loom-meta)
- loom/journal/0014-DISCOVERY-drift-audit.md (loom-meta)
- loom/journal/0015-DECISION-version-tracking-cascade.md (loom-meta)
- loom/journal/0016-DECISION-co-authority-chain.md (loom-meta)
- loom/journal/0019-DECISION-sync-merge-not-copy.md (loom-meta)
- loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md (loom-meta)
- loom/journal/0022-DECISION-sdk-version-awareness.md (loom-meta)
- loom/journal/0023-DECISION-sync-sdk-dependency-pins.md (loom-meta)
- loom/journal/0024-DECISION-kailash-as-private-repo.md (loom-meta)
- loom/journal/0026-DECISION-loom-atelier-naming.md (loom-meta)
- loom/journal/0027-DECISION-rb-template-bootstrap.md (loom-meta)
- loom/journal/0028-RISK-manifest-integrity-gaps.md (loom-meta)
- loom/journal/0030-DECISION-stack-gaps-architectural-decisions.md (loom-meta)
- loom/journal/0033-DECISION-mcp-consolidation.md (loom-meta)
- loom/journal/0046-DECISION-downstream-proposal-guard.md (loom-meta)
- loom/journal/0048-DECISION-gate1-rs-variant-classification.md (loom-meta)
- terrene/journal/0001-DECISION-orchestrator-architecture.md (loom-meta)
- terrene/journal/0002-DECISION-inherit-not-reinvent.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0013-DISCOVERY-fabric-layers-precisely-mapped.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0003-CONNECTION-six-features-share-engine-bottleneck-and-cross-workspace-dependencies.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0004-RISK-sync-async-impedance-mismatch-in-cache-layer.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0007-DISCOVERY-trl-dependency-chain-fragility.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0009-GAP-model-registry-extension-contract.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0011-DECISION-rl-for-llms-in-align-classical-in-ml.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0016-DECISION-kailash-rl-separate-package.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0003-TRADE-OFF-polars-vs-pandas.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0008-GAP-protocol-method-signatures-undefined.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0009-RISK-protocol-surface-and-cross-workspace-contracts.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0010-DECISION-rust-engine-replaces-sklearn-wrapping.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0012-DECISION-shared-module-deduplication.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0001-DISCOVERY-core-py-monolith-verified-coupling-map.md (kailash-py)
- loom/kailash-py/docs/adr/0017-llm-provider-architecture.md (kailash-py)
- loom/kailash-py/docs/adr/0018-http-rest-client-architecture.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (kailash-rs)
- loom/kailash-rs/workspaces/dataflow-parity/journal/0001-DISCOVERY-dataflow-parity-audit-findings.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md (kailash-rs) [via "doc-vs-code reconciliation"]
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0002-DECISION-kailash-ml-rs-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0003-RISK-kailash-ml-rs-redteam-round1.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0004-DECISION-kailash-ml-rs-redteam-resolutions.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0007-DECISION-delete-skeleton-use-loom-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0008-GAP-kailash-rl-not-planned.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0011-GAP-sklearn-parity-audit.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0001-DECISION-workspace-creation.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0002-DISCOVERY-prior-art-search.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0004-DECISION-nexusconfig-delegation.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0001-DECISION-separate-sync-struct.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0002-DISCOVERY-binding-parity-gaps.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0010-RISK-redteam-implementation-plan.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0015-DISCOVERY-upstream-api-audit-results.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0001-DECISION-sqlite-numpy-over-chromadb.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0003-DISCOVERY-concept-aware-chunking-essential.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0008-DISCOVERY-sdk-api-corrections.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0011-DISCOVERY-retrieval-tier-bias-against-constitution.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0012-DECISION-ontology-guided-reranking.md (kaizen-kz)

### 2. CO §16 — Framework-First — 45 entries

Composition over creation. Drives the "is this already in the upstream?" check, the package-vs-extension decision, and the deliberate-exclusion craft.

- loom/journal/0013-DECISION-variant-architecture.md (loom-meta)
- loom/journal/0027-DECISION-rb-template-bootstrap.md (loom-meta)
- loom/journal/0030-DECISION-stack-gaps-architectural-decisions.md (loom-meta)
- loom/journal/0033-DECISION-mcp-consolidation.md (loom-meta)
- loom/journal/0041-DECISION-kailash-ml-rs-architecture.md (loom-meta)
- loom/journal/0048-DECISION-gate1-rs-variant-classification.md (loom-meta)
- terrene/journal/0001-DECISION-orchestrator-architecture.md (loom-meta)
- terrene/journal/0002-DECISION-inherit-not-reinvent.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0005-DISCOVERY-three-concepts-are-all-you-need.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0013-DISCOVERY-fabric-layers-precisely-mapped.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0001-DISCOVERY-existing-infrastructure-vs-missing-integration.md (kailash-py)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0002-DISCOVERY-existing-red-tests.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0002-DECISION-emit-from-execute-not-stream.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0001-DISCOVERY-trl-thin-wrapper.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0003-DISCOVERY-llama-cpp-python-resolves-serving-risks.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0005-DISCOVERY-delegate-already-supports-local-models.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0010-DISCOVERY-dpo-loss-type-unlocks-14-variants.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0011-DECISION-rl-for-llms-in-align-classical-in-ml.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0003-TRADE-OFF-polars-vs-pandas.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0005-DISCOVERY-dataflow-express-limits-for-ml.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0009-RISK-protocol-surface-and-cross-workspace-contracts.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0010-DECISION-rust-engine-replaces-sklearn-wrapping.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0002-DISCOVERY-static-introspection-requirement.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0001-DISCOVERY-core-py-monolith-verified-coupling-map.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0003-CONNECTION-nexus-refactor-enables-mcp-consolidation.md (kailash-py)
- loom/kailash-py/docs/adr/0017-llm-provider-architecture.md (kailash-py)
- loom/kailash-py/docs/adr/0017-multi-workflow-api-architecture.md (kailash-py)
- loom/kailash-py/docs/adr/0018-http-rest-client-architecture.md (kailash-py)
- loom/kailash-py/docs/adr/0055-kailash-studio-dual-persona-foundation.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0005-DISCOVERY-lazy-dataflow-already-exists.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (kailash-rs)
- loom/kailash-rs/workspaces/dataflow-parity/journal/0001-DISCOVERY-dataflow-parity-audit-findings.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0002-DECISION-kailash-ml-rs-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0004-DECISION-kailash-ml-rs-redteam-resolutions.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0007-DECISION-delete-skeleton-use-loom-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0009-DECISION-ml-dl-rl-architecture-split.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0011-GAP-sklearn-parity-audit.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0012-DECISION-v3.10.0-codify.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0001-DECISION-workspace-creation.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0002-DISCOVERY-prior-art-search.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0003-DISCOVERY-issues-overstate-gaps.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0004-DECISION-nexusconfig-delegation.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0001-DECISION-separate-sync-struct.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0004-DECISION-nodejs-wasm-exclusion.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0014-DISCOVERY-existing-codebase-architecture.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0015-DISCOVERY-upstream-api-audit-results.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0001-DECISION-sqlite-numpy-over-chromadb.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0006-DECISION-adapters-as-nexus-api-clients.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0008-DISCOVERY-sdk-api-corrections.md (kaizen-kz)

### 3. CO §19 — Layer 3 — Guardrails — 38 entries

The deterministic-enforcement load-bearer. Every red team, every hook, every permission engine, every defense-in-depth example moors here.

- loom/journal/0002-DECISION-cc-expert-artifacts.md (loom-meta)
- loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)
- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0009-DECISION-implementation-plan.md (loom-meta)
- loom/journal/0010-DECISION-m2-m5-execution.md (loom-meta)
- loom/journal/0011-RISK-redteam-r1-findings.md (loom-meta)
- loom/journal/0029-DECISION-manifest-parity-automation.md (loom-meta)
- loom/journal/0040-DECISION-python-venv-enforcement.md (loom-meta)
- loom/journal/0047-DECISION-spec-coverage-gate.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/journal/0050-DISCOVERY-session-start-command-injection.md (loom-meta)
- loom/kailash-py/workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0005-DISCOVERY-dataflow-express-limits-for-ml.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0010-DECISION-build-speed-codification.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0001-DECISION-ci-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0009-GAP-full-parity-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0020-RISK-redteam-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0021-DECISION-async-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0022-DECISION-entry-point-wiring-and-subagents.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0001-GAP-test-coverage-gaps.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0003-RISK-checkpoint-path-traversal.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0009-DISCOVERY-redteam-convergence-findings.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0010-DISCOVERY-nexus-dedup-fingerprint-bug.md (kaizen-kz)

### 4. CO §29 — Evidence-Based Completion — 24 entries

The "tests pass != feature wired" load-bearer. Drives every "verify the assumption" move and every red team coverage finding.

- loom/journal/0045-DECISION-kailash-ml-rs-todos-redteam.md (loom-meta)
- loom/journal/0047-DECISION-spec-coverage-gate.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0003-GAP-write-path-undefined.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0006-DISCOVERY-fabric-is-runtime-system-not-library.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0010-DECISION-aether-first-consumer-not-reference.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0002-RISK-eventbus-wildcard-gap-and-synchronous-dispatch.md (kailash-py)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0004-GAP-fabric-engine-operational-status.md (kailash-py)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0005-DISCOVERY-fabric-integration-tested.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0001-DISCOVERY-delegate-streaming-architecture-gap.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0002-DECISION-emit-from-execute-not-stream.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0010-DISCOVERY-pypi-tag-push-race-condition.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0002-RISK-gguf-conversion-fragility.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0009-GAP-model-registry-extension-contract.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0013-RISK-superficial-kailash-rs-analysis.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0006-DISCOVERY-guardrail4-baseline-doubles-compute.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0008-GAP-protocol-method-signatures-undefined.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0001-DISCOVERY-module-path-conflict.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0001-DISCOVERY-core-py-monolith-verified-coupling-map.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0002-RISK-b0b-transport-extraction-threatens-external-plugins-and-middleware-ordering.md (kailash-py)
- loom/kailash-py/workspaces/pact-309-308/journal/0003-DISCOVERY-308-already-at-l1.md (kailash-py)
- loom/kailash-py/docs/adr/0056-phase-1a-blocker-resolution.md (kailash-py)
- loom/kailash-rs/workspaces/dataflow-parity/journal/0001-DISCOVERY-dataflow-parity-audit-findings.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0003-DISCOVERY-issues-overstate-gaps.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0006-DISCOVERY-implementation-progress.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)

### 5. CO §28 — Approval Gate — 21 entries

The structural human-judgement-required gate. Every convergence threshold, every phase transition, every "ready for /todos" handoff.

- loom/journal/0005-GAP-redteam-audit-gaps.md (loom-meta)
- loom/journal/0012-DECISION-session-completion-state.md (loom-meta)
- loom/journal/0019-DECISION-sync-merge-not-copy.md (loom-meta)
- loom/journal/0021-DECISION-autonomous-gate1-classification.md (loom-meta)
- loom/journal/0031-DECISION-stack-gaps-dependency-sequencing.md (loom-meta)
- loom/journal/0034-RISK-stack-gaps-redteam-attack.md (loom-meta)
- loom/journal/0035-DECISION-redteam-round1-resolutions.md (loom-meta)
- loom/journal/0037-RISK-redteam-round2-resolution-stress-test.md (loom-meta)
- loom/journal/0038-DECISION-redteam-round2-resolutions.md (loom-meta)
- loom/journal/0039-DECISION-redteam-round3-convergence.md (loom-meta)
- loom/journal/0042-RISK-kailash-ml-rs-redteam-round1.md (loom-meta)
- loom/journal/0043-DECISION-kailash-ml-rs-redteam-resolutions.md (loom-meta)
- loom/journal/0044-DECISION-kailash-ml-rs-redteam-convergence.md (loom-meta)
- loom/journal/0045-DECISION-kailash-ml-rs-todos-redteam.md (loom-meta)
- loom/journal/0046-DECISION-downstream-proposal-guard.md (loom-meta)
- loom/kailash-py/workspaces/kailash/journal/0016-DECISION-pact-sprint-s12-todo-structure.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0013-DECISION-todos-approved.md (kaizen-kz)

### 6. CO §3.3 — Safety Blindness — 21 entries

The "AI takes the most direct path" failure mode. Drives every silent-failure hunt, every zero-config audit, every fail-closed correction.

- loom/journal/0047-DECISION-spec-coverage-gate.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/journal/0050-DISCOVERY-session-start-command-injection.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0008-RISK-default-open-endpoints-in-zero-config.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0002-RISK-eventbus-wildcard-gap-and-synchronous-dispatch.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0001-DISCOVERY-delegate-streaming-architecture-gap.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0004-DISCOVERY-ci-release-failure-modes.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0007-RISK-agent-api-unused-but-trap.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0009-DISCOVERY-ci-deploy-production-preexisting-failures.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0010-DISCOVERY-pypi-tag-push-race-condition.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0002-RISK-gguf-conversion-fragility.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0007-DISCOVERY-trl-dependency-chain-fragility.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0006-DISCOVERY-guardrail4-baseline-doubles-compute.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0011-RISK-sql-type-injection-in-feature-sql.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0004-RISK-ast-scanning-completeness-gap.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0002-RISK-b0b-transport-extraction-threatens-external-plugins-and-middleware-ordering.md (kailash-py)
- loom/kailash-py/docs/adr/0057-async-sql-pytest-event-loop-pool-key-fix.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0003-RISK-sync-transaction-drop.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)

### 7. CO §13 — Layer 2 — Context — 19 entries

The institutional handbook layer. Drives every master directive, every progressive disclosure, every chunking strategy.

- loom/journal/0001-DECISION-co-refocusing.md (loom-meta)
- loom/journal/0002-DECISION-cc-expert-artifacts.md (loom-meta)
- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0008-DECISION-coc-artifact-scoping.md (loom-meta)
- loom/journal/0010-DECISION-m2-m5-execution.md (loom-meta)
- loom/journal/0015-DECISION-version-tracking-cascade.md (loom-meta)
- loom/journal/0016-DECISION-co-authority-chain.md (loom-meta)
- loom/journal/0022-DECISION-sdk-version-awareness.md (loom-meta)
- loom/journal/0023-DECISION-sync-sdk-dependency-pins.md (loom-meta)
- loom/journal/0026-DECISION-loom-atelier-naming.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- terrene/journal/0001-DECISION-orchestrator-architecture.md (loom-meta)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (kailash-rs)
- loom/kailash-rs/workspaces/dataflow-parity/journal/0001-DISCOVERY-dataflow-parity-audit-findings.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0009-GAP-full-parity-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0003-DISCOVERY-concept-aware-chunking-essential.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0005-CONNECTION-co-progressive-disclosure-in-herald-responses.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0011-DISCOVERY-retrieval-tier-bias-against-constitution.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0012-DECISION-ontology-guided-reranking.md (kaizen-kz)

### 8. CO §49 — Quality Criteria — Enforcement Reliability — 17 entries

The "intentionally try to violate critical rules" check. Drives every red-team-of-enforcement move.

- loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)
- loom/journal/0005-GAP-redteam-audit-gaps.md (loom-meta)
- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0011-RISK-redteam-r1-findings.md (loom-meta)
- loom/journal/0018-DECISION-cc-audit-convergence.md (loom-meta)
- loom/journal/0028-RISK-manifest-integrity-gaps.md (loom-meta)
- loom/journal/0029-DECISION-manifest-parity-automation.md (loom-meta)
- loom/journal/0045-DECISION-kailash-ml-rs-todos-redteam.md (loom-meta)
- loom/kailash-py/workspaces/kailash-ml/journal/0002-RISK-scope-9-engines-6-agents.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0004-DECISION-quality-tiering-for-v1-engines.md (kailash-py)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0003-RISK-kailash-ml-rs-redteam-round1.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs) [via PACT/EATP enforcement]
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0020-RISK-redteam-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0001-GAP-test-coverage-gaps.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0003-RISK-checkpoint-path-traversal.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0009-DISCOVERY-redteam-convergence-findings.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0010-DISCOVERY-nexus-dedup-fingerprint-bug.md (kaizen-kz)

### 9. CO §5 — Deterministic Enforcement — 16 entries

The "critical rules outside the context window" principle. Drives every hook-vs-rule choice.

- loom/journal/0029-DECISION-manifest-parity-automation.md (loom-meta)
- loom/journal/0040-DECISION-python-venv-enforcement.md (loom-meta)
- loom/journal/0047-DECISION-spec-coverage-gate.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/journal/0050-DISCOVERY-session-start-command-injection.md (loom-meta)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0007-DECISION-delete-skeleton-use-loom-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0003-RISK-sync-transaction-drop.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0020-RISK-redteam-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md (kaizen-kz)

### 10. CO §9 — Layer 1 — Intent — 16 entries

Agent specialisation routing. Drives every "which specialist owns this?" move.

- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0010-DECISION-m2-m5-execution.md (loom-meta)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0012-DECISION-v3.10.0-codify.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0001-DECISION-workspace-creation.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0009-GAP-full-parity-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0015-DISCOVERY-upstream-api-audit-results.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0022-DECISION-entry-point-wiring-and-subagents.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz) [via human-overrides-tiering]
- loom/kz-engage/workspaces/herald/journal/0002-DECISION-single-agent-not-multi-agent.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0008-DISCOVERY-sdk-api-corrections.md (kaizen-kz)

### 11. CO §39 — Layer 5 — Learning — 15 entries

Observe-capture-evolve. Drives every codify decision and every "what counts as institutional knowledge" call.

- loom/journal/0002-DECISION-cc-expert-artifacts.md (loom-meta)
- loom/journal/0004-RISK-learning-system-broken.md (loom-meta)
- loom/journal/0005-GAP-redteam-audit-gaps.md (loom-meta)
- loom/journal/0006-DECISION-learning-must-work.md (loom-meta)
- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0009-DECISION-implementation-plan.md (loom-meta)
- loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0010-DECISION-build-speed-codification.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0012-DECISION-v3.10.0-codify.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0009-GAP-full-parity-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0016-GAP-session-wrapup-implementation-start.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0019-DECISION-m3-memory-system-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md (kaizen-kz)

### 12. CO §26 — Layer 4 — Instructions — 14 entries

Structured workflows with approval gates. Drives every milestone-decomposition move.

- loom/journal/0010-DECISION-m2-m5-execution.md (loom-meta)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0009-GAP-full-parity-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0010-RISK-redteam-implementation-plan.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0013-DECISION-todos-approved.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0015-DISCOVERY-upstream-api-audit-results.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0016-GAP-session-wrapup-implementation-start.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0021-DECISION-async-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0022-DECISION-entry-point-wiring-and-subagents.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0007-DECISION-milestone-ordering.md (kaizen-kz)

### 13. CO §15 — Progressive Disclosure — 13 entries

Master directive → index → topic → deep reference. Drives every chunking-strategy move and every artifact-audit retraction.

- loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)
- loom/journal/0011-RISK-redteam-r1-findings.md (loom-meta)
- loom/journal/0014-DISCOVERY-drift-audit.md (loom-meta)
- loom/journal/0018-DECISION-cc-audit-convergence.md (loom-meta)
- loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- terrene/journal/0002-DECISION-inherit-not-reinvent.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0005-DISCOVERY-three-concepts-are-all-you-need.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0006-TRADE-OFF-lm-eval-optional-dependency.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0003-DISCOVERY-concept-aware-chunking-essential.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0005-CONNECTION-co-progressive-disclosure-in-herald-responses.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0011-DISCOVERY-retrieval-tier-bias-against-constitution.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0012-DECISION-ontology-guided-reranking.md (kaizen-kz)

### 14. CO §14 — Master Directive — 12 entries

CLAUDE.md + system prompt registry. Drives every "what loads at session start" move.

- loom/journal/0001-DECISION-co-refocusing.md (loom-meta)
- loom/kailash-py/workspaces/kailash/journal/0007-RISK-agent-api-unused-but-trap.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0008-GAP-protocol-method-signatures-undefined.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0022-DECISION-entry-point-wiring-and-subagents.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md (kaizen-kz)

### 15. CO §27 — Structured Workflow — 12 entries

The N-phase + M-gate pattern. Drives every milestone schema.

- loom/kailash-py/workspaces/data-fabric-engine/journal/0014-DECISION-todo-structure-and-milestones.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0003-CONNECTION-six-features-share-engine-bottleneck-and-cross-workspace-dependencies.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0016-DECISION-pact-sprint-s12-todo-structure.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0018-DECISION-five-workspace-parallel-implementation.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0008-RISK-compound-timeline-slippage.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0015-DECISION-todo-expansion-scope.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0008-GAP-protocol-method-signatures-undefined.md (kailash-py) [via Master Directive]
- loom/kailash-py/workspaces/mcp-platform-server/journal/0003-RISK-nexus-workspace-overlap.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0003-CONNECTION-nexus-refactor-enables-mcp-consolidation.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0004-DISCOVERY-b0a-atomicity-and-cross-workspace-safety.md (kailash-py)
- loom/kailash-py/docs/adr/0017-multi-workflow-api-architecture.md (kailash-py)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0006-DECISION-kailash-ml-rs-todos-redteam.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0002-DECISION-granular-todo-decomposition.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0013-DECISION-todos-approved.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0007-DECISION-milestone-ordering.md (kaizen-kz)

## Medium-Cited Concepts (3-9 evidence entries)

Sorted alphabetically within each standard.

### CO Standard

**CO §1 — Institutional Knowledge Thesis** — 8 entries

- loom/journal/0001-DECISION-co-refocusing.md (loom-meta)
- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0001-DISCOVERY-existing-infrastructure-vs-missing-integration.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0001-DISCOVERY-no-existing-ml-code.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0009-RISK-protocol-surface-and-cross-workspace-contracts.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0001-DISCOVERY-module-path-conflict.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0002-DISCOVERY-static-introspection-requirement.md (kailash-py)
- loom/kailash-py/docs/adr/0055-kailash-studio-dual-persona-foundation.md (kailash-py)

**CO §40 — Observe-Capture-Evolve Pipeline** — 8 entries

- loom/journal/0004-RISK-learning-system-broken.md (loom-meta)
- loom/journal/0006-DECISION-learning-must-work.md (loom-meta)
- loom/journal/0009-DECISION-implementation-plan.md (loom-meta)
- loom/journal/0038-DECISION-redteam-round2-resolutions.md (loom-meta)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0009-DECISION-codification-scope.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0003-DECISION-codify-error-sanitization-pattern.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0012-DECISION-codify-session7-ci-fixes.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0019-DECISION-m3-memory-system-implementation.md (kaizen-kz)

**CO §37 — Phase — Codify** — 7 entries

- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0009-DECISION-codification-scope.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0003-DECISION-codify-error-sanitization-pattern.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0012-DECISION-codify-session7-ci-fixes.md (kailash-py)

**CO §47 — Knowledge Curator** — 7 entries

- loom/journal/0006-DECISION-learning-must-work.md (loom-meta)
- loom/journal/0024-DECISION-kailash-as-private-repo.md (loom-meta)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0005-DECISION-kailash-ml-rs-redteam-convergence.md (kailash-rs) [by analogy]
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0012-DECISION-v3.10.0-codify.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md (kailash-rs)

**CO §50 — Quality Criteria — Workflow Discipline** — 7 entries

- loom/journal/0018-DECISION-cc-audit-convergence.md (loom-meta)
- loom/journal/0047-DECISION-spec-coverage-gate.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0003-GAP-write-path-undefined.md (kailash-py)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0005-DISCOVERY-fabric-integration-tested.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0007-RISK-integration-testing-combinatorial-explosion.md (kailash-py)
- loom/kailash-py/docs/adr/0056-phase-1a-blocker-resolution.md (kailash-py)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0003-RISK-kailash-ml-rs-redteam-round1.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0002-DECISION-granular-todo-decomposition.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0013-DECISION-todos-approved.md (kaizen-kz)

**CO §3.2 — Convention Drift** — 6 entries

- loom/journal/0014-DISCOVERY-drift-audit.md (loom-meta)
- loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md (kailash-py)
- loom/kailash-py/workspaces/dataflow-enhancements/journal/0004-RISK-sync-async-impedance-mismatch-in-cache-layer.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md (kailash-py)

**CO §11 — Routing Rules** — 6 entries

- loom/journal/0005-GAP-redteam-audit-gaps.md (loom-meta)
- loom/journal/0011-RISK-redteam-r1-findings.md (loom-meta)
- loom/journal/0027-DECISION-rb-template-bootstrap.md (loom-meta)
- loom/journal/0048-DECISION-gate1-rs-variant-classification.md (loom-meta)
- loom/kailash-py/docs/adr/0017-llm-provider-architecture.md (kailash-py)
- loom/kz-engage/workspaces/herald/journal/0002-DECISION-single-agent-not-multi-agent.md (kaizen-kz)

**CO §34 — Phase — Plan (Structural Approval Gate)** — 6 entries

- loom/journal/0039-DECISION-redteam-round3-convergence.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0014-DECISION-todo-structure-and-milestones.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0016-DECISION-pact-sprint-s12-todo-structure.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0008-RISK-compound-timeline-slippage.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0015-DECISION-todo-expansion-scope.md (kailash-py)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0006-DECISION-kailash-ml-rs-todos-redteam.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0002-DECISION-granular-todo-decomposition.md (kailash-rs)

**CO §36 — Phase — Vet (Review)** — 6 entries

- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0007-DECISION-redteam-todo-fixes.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md (kailash-py)
- loom/kailash-py/workspaces/kailash-align/journal/0013-RISK-superficial-kailash-rs-analysis.md (kailash-py)
- loom/kailash-py/docs/adr/0056-phase-1a-blocker-resolution.md (kailash-py)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0003-RISK-kailash-ml-rs-redteam-round1.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0004-DECISION-kailash-ml-rs-redteam-resolutions.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0005-DECISION-kailash-ml-rs-redteam-convergence.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0006-DECISION-kailash-ml-rs-todos-redteam.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0006-DISCOVERY-implementation-progress.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md (kailash-rs)

**CO §3 — Three Failure Modes** — 5 entries

- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0011-DISCOVERY-request-to-event-driven-is-the-transformation.md (kailash-py)

**CO §4 — Human-on-the-Loop** — 5 entries

- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0021-DECISION-autonomous-gate1-classification.md (loom-meta)
- loom/journal/0025-DECISION-parallel-backlog-resolution.md (loom-meta)
- loom/journal/0031-DECISION-stack-gaps-dependency-sequencing.md (loom-meta)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md (kaizen-kz)

**CO §10 — Agent Definition** — 5 entries

- loom/journal/0002-DECISION-cc-expert-artifacts.md (loom-meta)
- loom/journal/0018-DECISION-cc-audit-convergence.md (loom-meta)
- loom/kailash-py/workspaces/kailash-align/journal/0004-DECISION-defer-agents-v1.1.md (kailash-py)

**CO §5.1 — Hard Enforcement** — 5 entries

- loom/journal/0029-DECISION-manifest-parity-automation.md (loom-meta)
- loom/journal/0040-DECISION-python-venv-enforcement.md (loom-meta)
- loom/journal/0047-DECISION-spec-coverage-gate.md (loom-meta)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0003-RISK-checkpoint-path-traversal.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0010-DISCOVERY-nexus-dedup-fingerprint-bug.md (kaizen-kz)

**CO §33 — Phase — Analyze** — 5 entries

- loom/kailash-rs/workspaces/dataflow-parity/journal/0001-DISCOVERY-dataflow-parity-audit-findings.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0002-DECISION-kailash-ml-rs-architecture.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-rl/journal/0001-DECISION-workspace-creation.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md (kailash-rs)
- loom/kailash-rs/workspaces/nexus-parity/journal/0003-DISCOVERY-issues-overstate-gaps.md (kailash-rs)

**CO §31 — Standard Six-Phase Workflow** — 4 entries

- loom/journal/0031-DECISION-stack-gaps-dependency-sequencing.md (loom-meta)
- loom/journal/0039-DECISION-redteam-round3-convergence.md (loom-meta)
- loom/kailash-py/workspaces/kailash/journal/0018-DECISION-five-workspace-parallel-implementation.md (kailash-py)

**CO §53 — Adoption Phase 1 — Audit** — 4 entries

- loom/journal/0009-DECISION-implementation-plan.md (loom-meta)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0008-GAP-kailash-rl-not-planned.md (kailash-rs)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0011-GAP-sklearn-parity-audit.md (kailash-rs)
- loom/kailash-rs/workspaces/sync-express/journal/0002-DISCOVERY-binding-parity-gaps.md (kailash-rs)

**CO §58 — CO Conformance** — 3 entries

- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0009-DECISION-implementation-plan.md (loom-meta)
- loom/journal/0012-DECISION-session-completion-state.md (loom-meta)

**CO §3.1 — Amnesia** — 3 entries

- loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- loom/journal/0017-DISCOVERY-session-notes-broken.md (loom-meta)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)

**CO §5.3 — Anti-Amnesia** — 3 entries

- loom/journal/0017-DISCOVERY-session-notes-broken.md (loom-meta)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)

**CO §5.4 — Defense in Depth** — 4 entries

- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0009-DISCOVERY-redteam-convergence-findings.md (kaizen-kz)

**CO §20 — Rule Classification** — 3 entries

- loom/journal/0002-DECISION-cc-expert-artifacts.md (loom-meta)
- loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)

**CO §22 — Advisory Rules** — 3 entries

- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0010-DECISION-build-speed-codification.md (kailash-rs)

**CO §23 — Three-Tier Enforcement Model** — 3 entries

- loom/kailash-rs/workspaces/v3.8-issues/journal/0001-DECISION-ci-architecture.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md (kaizen-kz)

**CO §35 — Phase — Execute** — 3 entries

- loom/kailash-rs/workspaces/nexus-parity/journal/0006-DISCOVERY-implementation-progress.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0016-GAP-session-wrapup-implementation-start.md (kaizen-kz)

**CO §48 — Quality Criteria — Coverage** — 3 entries

- loom/kailash-py/workspaces/data-fabric-engine/journal/0007-DISCOVERY-fabric-is-four-needs.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0002-RISK-scope-9-engines-6-agents.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0004-DECISION-quality-tiering-for-v1-engines.md (kailash-py)

**CO §56 — Adoption Phase 4 — Process** — 3 entries

- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0005-GAP-capability-gap-analysis.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0010-RISK-redteam-implementation-plan.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0007-DECISION-milestone-ordering.md (kaizen-kz)

**CO §64 — COC Skill Hierarchy** — 5 entries

- loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0009-DECISION-codification-scope.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0012-DECISION-codify-session7-ci-fixes.md (kailash-py)
- loom/kz-engage/workspaces/herald/journal/0005-CONNECTION-co-progressive-disclosure-in-herald-responses.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0012-DECISION-ontology-guided-reranking.md (kaizen-kz)

**COC §65 — COC Anti-Amnesia Hook** — 3 entries

- loom/journal/0017-DISCOVERY-session-notes-broken.md (loom-meta)
- loom/journal/0040-DECISION-python-venv-enforcement.md (loom-meta)

**COC §71 — COC Observation-Instinct-Evolution Pipeline** — 3 entries

- loom/journal/0004-RISK-learning-system-broken.md (loom-meta)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0008-DISCOVERY-session1-parallel-execution.md (kailash-py)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0010-DECISION-build-speed-codification.md (kailash-rs)

### CARE Standard

**CARE §44 — Failure Modes (Architectural Resilience)** — 9 entries

- loom/journal/0032-RISK-kailash-ml-architectural-analysis.md (loom-meta)
- loom/journal/0034-RISK-stack-gaps-redteam-attack.md (loom-meta)
- loom/journal/0036-RISK-kailash-rl-redteam-round2.md (loom-meta)
- loom/journal/0042-RISK-kailash-ml-rs-redteam-round1.md (loom-meta)
- loom/journal/0050-DISCOVERY-session-start-command-injection.md (loom-meta)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0006-DISCOVERY-fabric-is-runtime-system-not-library.md (kailash-py)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0011-DISCOVERY-request-to-event-driven-is-the-transformation.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0004-DISCOVERY-ci-release-failure-modes.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0002-RISK-b0b-transport-extraction-threatens-external-plugins-and-middleware-ordering.md (kailash-py)
- loom/kailash-py/workspaces/nexus-transport-refactor/journal/0004-DISCOVERY-b0a-atomicity-and-cross-workspace-safety.md (kailash-py)

**CARE §6 — Trust Verification Bridge** — 8 entries

- loom/journal/0019-DECISION-sync-merge-not-copy.md (loom-meta)
- loom/journal/0021-DECISION-autonomous-gate1-classification.md (loom-meta)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0011-DISCOVERY-pact-internal-only-enforcement-bug.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0020-RISK-redteam-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-py/workspaces/kz-foundation/journal/0003-RISK-checkpoint-path-traversal.md (kaizen-kz)

**CARE §30 — Constraint Envelopes (Definition)** — 8 entries

- loom/kailash-py/workspaces/data-fabric-engine/journal/0009-DECISION-trusted-code-boundary.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0018-DECISION-m1-m2-implementation.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0021-DECISION-async-agent-loop-wiring.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md (kaizen-kz)

**CARE §4 — Human-on-the-Loop (Framework Model)** — 7 entries

- loom/journal/0021-DECISION-autonomous-gate1-classification.md (loom-meta)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0003-DISCOVERY-kaizen-cli-rs-capabilities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0010-RISK-redteam-implementation-plan.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md (kaizen-kz)

**CARE §9 — Principle 3 — Transparency as Foundation** — 6 entries

- loom/kailash-py/workspaces/kailash/journal/0003-DECISION-codify-error-sanitization-pattern.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0004-RISK-ast-scanning-completeness-gap.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0024-DISCOVERY-cc-feature-flags-inventory.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md (kaizen-kz)

**CARE §67 — Technical Pattern 3 — Observable Execution** — 4 entries

- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0008-RISK-performance-quality-concerns.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0023-RISK-redteam-llm-client-streaming.md (kaizen-kz)

**CARE §13 — Principle 7 — Evolutionary Trust** — 3 entries

- loom/journal/0023-DECISION-sync-sdk-dependency-pins.md (loom-meta)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (loom-meta)
- loom/kailash-py/workspaces/kailash-align/journal/0016-DECISION-kailash-rl-separate-package.md (kailash-py)

### EATP Standard

**EATP §16 — Element 2 — Delegation Record** — 6 entries

- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0003-CONNECTION-pact-kaizen-cross-package.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (kailash-rs)

**EATP §40 — Enforcement Model (PDP/PEP)** — 5 entries

- loom/kailash-py/workspaces/data-fabric-engine/journal/0009-DECISION-trusted-code-boundary.md (kailash-py)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md (kailash-py)
- loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)

**EATP §15 — Element 1 — Genesis Record** — 4 entries

- loom/journal/0015-DECISION-version-tracking-cascade.md (loom-meta)
- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0026-DISCOVERY-oauth-auth-architecture-gap.md (kaizen-kz)

**EATP §24 — Dimension-Scoped Delegation** — 4 entries

- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (kailash-rs)

**EATP §66 — Constraint Dimensions (Five)** — 4 entries

- loom/kailash-py/workspaces/data-fabric-engine/journal/0008-RISK-default-open-endpoints-in-zero-config.md (kailash-py)
- loom/kailash-py/workspaces/kailash-ml/journal/0011-RISK-sql-type-injection-in-feature-sql.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)

**EATP §49 — MCP + EATP Integration Pattern** — 3 entries

- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0006-CONNECTION-fabric-mcp-consumer-synergy.md (kailash-py)
- loom/kailash-py/workspaces/mcp-platform-server/journal/0002-DISCOVERY-static-introspection-requirement.md (kailash-py)

**EATP §31 — Dimension-Scoped Delegation (Operational)** — 3 entries

- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md (kailash-rs)

**EATP §27 — Verification Process (Chain Resolution)** — 3 entries

- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)

**EATP §36 — Graceful Degradation (VERIFY Failure)** — 3 entries

- loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md (kailash-rs)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0001-DISCOVERY-claude-code-internals.md (kaizen-kz)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0020-RISK-redteam-agent-loop-wiring.md (kaizen-kz)

**EATP §14 — Trust Lineage Chain (Overview)** — 3 entries

- loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0026-DISCOVERY-oauth-auth-architecture-gap.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md (kaizen-kz)

### PACT Standard

**PACT §10 — Monotonic Tightening Invariant** — 11 entries

- loom/journal/0008-DECISION-coc-artifact-scoping.md (loom-meta)
- loom/journal/0013-DECISION-variant-architecture.md (loom-meta)
- loom/journal/0016-DECISION-co-authority-chain.md (loom-meta)
- loom/journal/0019-DECISION-sync-merge-not-copy.md (loom-meta)
- loom/journal/0046-DECISION-downstream-proposal-guard.md (loom-meta)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)

**PACT §19 — Verification Gradient (4-Zone)** — 6 entries

- loom/journal/0021-DECISION-autonomous-gate1-classification.md (loom-meta)
- loom/journal/0025-DECISION-parallel-backlog-resolution.md (loom-meta)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0001-DISCOVERY-pact-security-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz)

**PACT §11 — Role Envelope (Standing)** — 5 entries

- loom/journal/0013-DECISION-variant-architecture.md (loom-meta)
- loom/journal/0016-DECISION-co-authority-chain.md (loom-meta)
- loom/journal/0046-DECISION-downstream-proposal-guard.md (loom-meta)
- terrene/journal/0001-DECISION-orchestrator-architecture.md (loom-meta)

**PACT §35 — Bridges** — 5 entries

- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0003-GAP-no-lca-computation.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md (kailash-rs)

**PACT §51 — Edge Case — Vacant Roles** — 5 entries

- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md (kailash-py)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md (kailash-rs)

**PACT §18 — Communication Dimension** — 4 entries

- loom/kailash-py/workspaces/data-fabric-engine/journal/0008-RISK-default-open-endpoints-in-zero-config.md (kailash-py)
- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0007-DECISION-redteam-todo-fixes.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0011-DISCOVERY-pact-internal-only-enforcement-bug.md (kailash-py)

**PACT §40 — EATP Integration (Governance-Protocol Mapping)** — 3 entries

- loom/kailash-py/workspaces/gh-issues-consolidation/journal/0003-CONNECTION-pact-kaizen-cross-package.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)

**PACT §8 — Positional Addressing** — 3 entries

- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0003-GAP-no-lca-computation.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md (kailash-rs)

**PACT §13 — Effective Envelope (Computed Intersection)** — 3 entries

- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (kailash-rs)
- loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)

**PACT §1 — Human-on-the-Loop Operationalization** — 3 entries

- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (kaizen-kz)
- loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md (kaizen-kz)

**PACT §33 — Knowledge Cascade Rules** — 3 entries

- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (kailash-rs)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md (kailash-rs)

## Lightly-Cited Concepts (1-2 evidence entries)

Compact listing by standard. Each entry cites 1-2 sources only.

### CARE — Lightly Cited

- **CARE §10 — Principle 4 — Continuous Operation** — 1 entry: loom/journal/0025-DECISION-parallel-backlog-resolution.md (loom-meta)
- **CARE §12 — Principle 6 — Graceful Degradation** — 1 entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (kaizen-kz)
- **CARE §16 — Pilot and Autopilot Metaphor** — 1 entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0004-DISCOVERY-community-reverse-engineering.md (kaizen-kz)
- **CARE §19 — Trust is Human** — 1 entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0026-DISCOVERY-oauth-auth-architecture-gap.md (kaizen-kz)
- **CARE §35 — Knowledge Ledger (Architecture & Access)** — 1 entry: loom/journal/0024-DECISION-kailash-as-private-repo.md (loom-meta)
- **CARE §36 — Knowledge Ledger Provenance and Evolution** — 1 entry: loom/journal/0024-DECISION-kailash-as-private-repo.md (loom-meta)
- **CARE §40 — Uncertainty Structured Pipeline** — 1 entry: loom/journal/0035-DECISION-redteam-round1-resolutions.md (loom-meta)
- **CARE §42 — Constraint Resolution (Formal Specification)** — 1 entry: loom/journal/0028-RISK-manifest-integrity-gaps.md (loom-meta)
- **CARE §43 — State Machines (Formal Specification)** — 2 entries: loom/kailash-py/workspaces/pact-309-308/journal/0001-DISCOVERY-fsm-grant-revoke-conflict.md (kailash-py); loom/kailash-py/workspaces/pact-309-308/journal/0002-DISCOVERY-suspended-reinstatement-path.md (kailash-py)

### EATP — Lightly Cited

- **EATP §4 — Constraint Envelope Schema (Five-Dimensional)** — 2 entries: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0007-DECISION-redteam-todo-fixes.md (kailash-py); loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md (kailash-rs)
- **EATP §8 — Five Trust Postures** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0013-DISCOVERY-cross-sdk-pseudo-posture-shared-bug.md (kailash-py)
- **EATP §9 — Verification Gradient (Three Levels)** — 1 entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0010-RISK-redteam-implementation-plan.md (kaizen-kz)
- **EATP §17 — Element 3 — Constraint Envelope** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0011-DISCOVERY-pact-internal-only-enforcement-bug.md (kailash-py)
- **EATP §18 — Element 4 — Capability Attestation** — 2 entries: loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py); loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)
- **EATP §19 — Element 5 — Audit Anchor** — 2 entries: loom/journal/0015-DECISION-version-tracking-cascade.md (loom-meta); loom/journal/0022-DECISION-sdk-version-awareness.md (loom-meta)
- **EATP §23 — Dual-Binding Signing Model** — 1 entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md (kailash-rs)
- **EATP §28 — Operation 1 — ESTABLISH** — 1 entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0026-DISCOVERY-oauth-auth-architecture-gap.md (kaizen-kz)
- **EATP §33 — Cascade Revocation** — 1 entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (kailash-rs)
- **EATP §37 — Posture Transition Triggers** — 1 entry: loom/kailash-py/workspaces/pact-309-308/journal/0002-DISCOVERY-suspended-reinstatement-path.md (kailash-py)
- **EATP §42 — ShadowEnforcer (Observation Mode)** — 1 entry: loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)
- **EATP §65 — Trust Postures (Five-Level State Machine)** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0013-DISCOVERY-cross-sdk-pseudo-posture-shared-bug.md (kailash-py)

### CO — Lightly Cited

- **CO §2 — Brilliant New Hire Principle** — 2 entries: loom/journal/0007-GAP-co-completeness-audit.md (loom-meta); loom/kailash-py/workspaces/data-fabric-engine/journal/0007-DISCOVERY-fabric-is-four-needs.md (kailash-py); loom/kailash-py/docs/adr/0055-kailash-studio-dual-persona-foundation.md (kailash-py)
- **CO §6 — Bainbridge's Irony Applied** — 1 entry: loom/journal/0007-GAP-co-completeness-audit.md (loom-meta)
- **CO §7 — Knowledge Compounds** — 2 entries: loom/journal/0004-RISK-learning-system-broken.md (loom-meta); loom/journal/0006-DECISION-learning-must-work.md (loom-meta)
- **CO §21 — Critical Rules** — 1 entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md (kailash-rs)
- **CO §24 — Tier 3 Transport-Level Enforcement** — 0 direct citations (referenced obliquely in kaizen-kz 0002 and 0018 — see Data Quality Flags)
- **CO §30 — Mandatory Delegation Rules** — 2 entries: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0006-DECISION-kailash-ml-rs-todos-redteam.md (kailash-rs); loom/kailash-rs/workspaces/nexus-parity/journal/0002-DECISION-granular-todo-decomposition.md (kailash-rs)
- **CO §41 — Confidence Thresholds** — 2 entries: loom/journal/0004-RISK-learning-system-broken.md (loom-meta); loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0019-DECISION-m3-memory-system-implementation.md (kaizen-kz)
- **CO §42 — Knowledge Growth** — 2 entries: loom/journal/0006-DECISION-learning-must-work.md (loom-meta); loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0019-DECISION-m3-memory-system-implementation.md (kaizen-kz)
- **CO §51 — Quality Criteria — Learning Velocity** — 1 entry: loom/journal/0004-RISK-learning-system-broken.md (loom-meta)
- **CO §5.2 — Soft Enforcement** — 1 entry: loom/journal/0040-DECISION-python-venv-enforcement.md (loom-meta)
- **CO §61 — COC Agent — Security Reviewer (Mandatory)** — 1 entry: loom/journal/0002-DECISION-cc-expert-artifacts.md (loom-meta)
- **CO §63 — COC CLAUDE.md Pattern** — 1 entry: loom/journal/0003-DISCOVERY-cc-artifact-audit.md (loom-meta)
- **CO §66 — COC Validation Hooks** — 1 entry: loom/journal/0005-GAP-redteam-audit-gaps.md (loom-meta)
- **CO §67 — COC Defense in Depth Example** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (kailash-py)
- **CO §69 — COC Slash Commands** — 1 entry: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0006-CONNECTION-fabric-mcp-consumer-synergy.md (kailash-py)
- **CO §72 — COC Institutional Knowledge Thesis in Practice** — 2 entries: loom/kailash-py/workspaces/data-fabric-engine/journal/0010-DECISION-aether-first-consumer-not-reference.md (kailash-py); loom/kailash-py/workspaces/kailash/journal/0007-RISK-agent-api-unused-but-trap.md (kailash-py)

### PACT — Lightly Cited

- **PACT §4 — Department (D) Node Type** — 1 entry: loom/journal/0008-DECISION-coc-artifact-scoping.md (loom-meta)
- **PACT §6 — Role (R) Node Type** — 1 entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (kailash-rs)
- **PACT §9 — BOD as Governance Root** — 1 entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs)
- **PACT §14 — Financial Dimension** — 2 entries (collective via "PACT §14-18 Five Constraint Dimensions" reference): loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md (kailash-rs); loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz)
- **PACT §15 — Operational Dimension** — 2 entries (collective citation): same two entries as §14
- **PACT §16 — Temporal Dimension** — 2 entries (collective citation): same two entries; plus loom/kailash-rs/workspaces/cross-sdk-governance/journal/0006-DECISION-five-decision-points.md (kailash-rs) [as Task Envelope]
- **PACT §17 — Data Access Dimension** — 1 entry: loom/kailash-py/workspaces/kailash-ml/journal/0011-RISK-sql-type-injection-in-feature-sql.md (kailash-py)
- **PACT §23 — Blocked Zone** — 1 entry: loom/kailash-rs/workspaces/v3.8-issues/journal/0002-RISK-rbac-deny-bypass.md (kailash-rs)
- **PACT §24 — Default Envelope Profiles (Progressive Trust)** — 1 entry: loom/kailash-rs/workspaces/v3.9-parity/journal/0001-DECISION-v39-implementation.md (kailash-rs)
- **PACT §25 — Five Classification Levels** — 1 entry: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md (kaizen-kz) [collective via "PACT §25-30"]
- **PACT §26 — OFFICIAL Classification** — 1 entry (collective): same
- **PACT §27 — SENSITIVE Classification** — 1 entry (collective): same
- **PACT §28 — CONFIDENTIAL Classification** — 1 entry (collective): same
- **PACT §29 — SECRET Classification** — 1 entry (collective): same
- **PACT §30 — TOP SECRET Classification** — 1 entry (collective): same
- **PACT §31 — Clearance Assignment Authority** — 1 entry: loom/kailash-py/workspaces/pact-309-308/journal/0001-DISCOVERY-fsm-grant-revoke-conflict.md (kailash-py)
- **PACT §36 — Posture-Clearance Interaction** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0013-DISCOVERY-cross-sdk-pseudo-posture-shared-bug.md (kailash-py)
- **PACT §38 — Pre-Clearance Workflow** — 2 entries: loom/kailash-py/workspaces/pact-309-308/journal/0001-DISCOVERY-fsm-grant-revoke-conflict.md (kailash-py); loom/kailash-py/workspaces/pact-309-308/journal/0002-DISCOVERY-suspended-reinstatement-path.md (kailash-py)
- **PACT §41 — Genesis Record (Organization-Level)** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)
- **PACT §42 — Delegation Record (EATP Backing)** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (kailash-py)
- **PACT §44 — Organizational Compilation** — 1 entry: loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (kailash-py)
- **PACT §53 — Edge Case — Matrix Reporting** — 1 entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0003-GAP-no-lca-computation.md (kailash-rs)
- **PACT §54 — Edge Case — Shared Services (Bridges)** — 1 entry: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0003-GAP-no-lca-computation.md (kailash-rs)

## Concepts With No Evidence in COC-Layer Corpus

These concepts are NOT failures — they indicate where the COC-layer corpus does not yet supply evidence. Most CARE philosophy and human-competency content, EATP interop/credential format work, CO domain-application content (COR/COE/COG/COL/finance/compliance), and PACT classification semantics will need evidence from other corpora (CARE thesis workspace, PACT workspace, COR/COE journals, governance archives) when those layers are built.

### CARE — Concepts Without Evidence (52)

- **CARE §1** Core Thesis (no evidence)
- **CARE §2** Dual Plane Model (no evidence)
- **CARE §3** Mirror Thesis (no evidence)
- **CARE §5** Observation Advantage (no evidence)
- **CARE §7** Principle 1 — Full Autonomy as Baseline (no evidence)
- **CARE §8** Principle 2 — Human Choice of Engagement (no evidence)
- **CARE §11** Principle 5 — Human Accountability Preserved (no evidence)
- **CARE §14** Principle 8 — Purpose Alignment (no evidence)
- **CARE §15** Human-on-the-Loop (Detailed) (no evidence)
- **CARE §17** Refinement Loop (no evidence)
- **CARE §18** Elevation Effect (no evidence)
- **CARE §20** Verification Distinction (no evidence)
- **CARE §21** Trust Plane Permanence (no evidence)
- **CARE §22** Mirror Thesis (Detailed) (no evidence)
- **CARE §23** Ethical Judgment (Competency) (no evidence)
- **CARE §24** Relationship Capital (Competency) (no evidence)
- **CARE §25** Contextual Wisdom (Competency) (no evidence)
- **CARE §26** Creative Synthesis (Competency) (no evidence)
- **CARE §27** Emotional Intelligence (Competency) (no evidence)
- **CARE §28** Cultural Navigation (Competency) (no evidence)
- **CARE §29** Mirror Power Dynamics (no evidence)
- **CARE §31** Constraint Inheritance (no evidence)
- **CARE §32** Cross-Functional Bridges (no evidence)
- **CARE §33** Information Boundaries (Bridges) (no evidence)
- **CARE §34** Bridge Governance and Verification (no evidence)
- **CARE §37** Workspaces (Definition and Content) (no evidence)
- **CARE §38** Workspace Evolution (no evidence)
- **CARE §39** Uncertainty Handling — Three Levels (no evidence)
- **CARE §41** Uncertainty as Mirror (no evidence)
- **CARE §45** Human Competency Map (Framework) (no evidence)
- **CARE §46** Role Evolution (Framework) (no evidence)
- **CARE §47** Skills Framework — Tier 1 Foundation (no evidence)
- **CARE §48** Skills Framework — Tier 2 Relational (no evidence)
- **CARE §49** Skills Framework — Tier 3 Strategic (no evidence)
- **CARE §50** Skills Framework — Tier 4 Leadership (no evidence)
- **CARE §51** Training Implications (no evidence)
- **CARE §52** Apprenticeship Evolution (no evidence)
- **CARE §53** Responsibility Model (Three Domains) (no evidence)
- **CARE §54** Trust Contract (Responsibility) (no evidence)
- **CARE §55** Policy Framework (CARE Principles) (no evidence)
- **CARE §56** Regulatory Categories (Policy) (no evidence)
- **CARE §57** EU AI Act Alignment (no evidence)
- **CARE §58** Ethical Framework — Human Dignity (no evidence)
- **CARE §59** Organization Builder (Purpose) (no evidence)
- **CARE §60** Builder Process (Five Stages) (no evidence)
- **CARE §61** Builder Compilation and Deployment (no evidence)
- **CARE §62** EATP — Five Elements (from CARE's perspective) (no evidence)
- **CARE §63** EATP — Trust Verification Speed (no evidence)
- **CARE §64** Technical Blueprint — Two-Plane Architecture (no evidence)
- **CARE §65** Technical Pattern 1 — Pre-Established Trust (no evidence)
- **CARE §66** Technical Pattern 2 — Constraint-Bounded Execution (no evidence)
- **CARE §68** Technical Pattern 4 — Knowledge-Flat Architecture (no evidence)
- **CARE §69** Technical Pattern 5 — Workspace-Centric Design (no evidence)

### EATP — Concepts Without Evidence (76)

- **EATP §1** Trust Infrastructure Layer (no evidence)
- **EATP §2** Five Elements of Trust Lineage (no evidence)
- **EATP §3** Four Operations (ESTABLISH/DELEGATE/VERIFY/AUDIT) (no evidence)
- **EATP §5** Reasoning Trace Schema (Structured) (no evidence)
- **EATP §6** Standalone SDK (no evidence)
- **EATP §7** Trust as Primary Barrier (no evidence)
- **EATP §10** Graceful Degradation (no evidence)
- **EATP §11** First Principle 1 — Trust is the Primary Barrier (no evidence)
- **EATP §12** First Principle 2 — Legacy Systems Have Embedded Trust (no evidence)
- **EATP §13** First Principle 3 — Value Drives Adoption (no evidence)
- **EATP §20** Reasoning Trace (Extended Trust Chain) (no evidence)
- **EATP §21** Confidentiality Level (Five-Level) (no evidence)
- **EATP §22** ReasoningTrace Structure (no evidence)
- **EATP §25** Delegation Depth (no evidence)
- **EATP §26** Chain Linkage (no evidence)
- **EATP §29** Operation 2 — DELEGATE (no evidence)
- **EATP §30** Constraint Types (Six Types) (no evidence)
- **EATP §32** Delegation Depth Limits (Enforcement) (no evidence)
- **EATP §34** Operation 3 — VERIFY (no evidence)
- **EATP §35** Verification Layers (Five Orthogonal) (no evidence)
- **EATP §38** Verification Gradient (Operational) (no evidence)
- **EATP §39** Trust Scoring (no evidence)
- **EATP §41** StrictEnforcer Verdict Classification (no evidence)
- **EATP §43** Decorator Integration (no evidence)
- **EATP §44** Operation 4 — AUDIT (no evidence)
- **EATP §45** Audit Query Types (no evidence)
- **EATP §46** Audit Retention Requirements (no evidence)
- **EATP §47** Operation Lifecycle (no evidence)
- **EATP §48** Protocol Landscape (MCP/A2A/EATP) (no evidence)
- **EATP §50** A2A + EATP Integration Pattern (no evidence)
- **EATP §51** Combined Integration (MCP+A2A+EATP) (no evidence)
- **EATP §52** EATP MCP Server (Reference) (no evidence)
- **EATP §53** Standalone SDK Integration Patterns (no evidence)
- **EATP §54** CARE Integration (Accountability Chain) (no evidence)
- **EATP §55** Standalone SDK Purpose (no evidence)
- **EATP §56** SDK Installation and Extras (no evidence)
- **EATP §57** Quick Start Workflow (no evidence)
- **EATP §58** CLI Commands (Core Set) (no evidence)
- **EATP §59** CLI Utility Commands (no evidence)
- **EATP §60** MCP Server (SDK Shipped) (no evidence)
- **EATP §61** Enforcement Utilities (no evidence)
- **EATP §62** Storage Backends (no evidence)
- **EATP §63** Transactional Operations (no evidence)
- **EATP §64** Trust Scoring Module (no evidence)
- **EATP §67** Constraint Types (Six — SDK View) (no evidence)
- **EATP §68** Built-In Constraint Templates (no evidence)
- **EATP §69** Reasoning Traces (SDK Implementation) (no evidence)
- **EATP §70** SDK Package Structure (no evidence)
- **EATP §71** Wire Format Mapping (no evidence)
- **EATP §72** SDK Relationship to Kailash (no evidence)
- **EATP §73** Interoperability (Seven Formats) (no evidence)
- **EATP §74** Format Matrix (no evidence)
- **EATP §75** JWT Mapping (no evidence)
- **EATP §76** W3C VC Mapping (no evidence)
- **EATP §77** DID (Decentralized Identifiers) (no evidence)
- **EATP §78** UCAN v0.10.0 Mapping (no evidence)
- **EATP §79** SD-JWT (Selective Disclosure) (no evidence)
- **EATP §80** Biscuit / Macaroon Tokens (no evidence)
- **EATP §81** EU AI Act Mapping (no evidence)
- **EATP §82** NIST AI RMF Mapping (no evidence)
- **EATP §83** OWASP Agentic Top 10 Coverage (no evidence)
- **EATP §84** VerificationBundle (no evidence)
- **EATP §85** VerificationBundle Fields (no evidence)
- **EATP §86** Round-Trip Fidelity (no evidence)
- **EATP §87** No External Dependencies (Interop) (no evidence)
- **EATP §88** Cryptographic Consistency (no evidence)
- **EATP §89** Three Conformance Levels (no evidence)
- **EATP §90** EATP Compatible (Level 1) (no evidence)
- **EATP §91** EATP Conformant (Level 2) (no evidence)
- **EATP §92** EATP Complete (Level 3) (no evidence)
- **EATP §93** Conformance Declaration Requirements (no evidence)
- **EATP §94** Reference Implementation Status (no evidence)

### CO — Concepts Without Evidence (122)

- **CO §8** Authentic Voice and Responsible Co-Authorship (no evidence)
- **CO §12** Model Tier (no evidence)
- **CO §18** Context Engineering (no evidence)
- **CO §24** Tier 3 Transport-Level Enforcement (no direct citation; oblique only)
- **CO §25** EATP Constraint Mapping (no evidence)
- **CO §32** Canonical Commands (no evidence)
- **CO §38** Phase — Deliver (no evidence)
- **CO §43** CO Architect (no evidence)
- **CO §44** Domain Specialist (no evidence)
- **CO §45** Guardrail Engineer (no evidence)
- **CO §46** Process Designer (no evidence)
- **CO §52** Quality Criteria — Practitioner Depth (no evidence)
- **CO §54** Adoption Phase 2 — Foundation (no evidence)
- **CO §55** Adoption Phase 3 — Enforcement (no evidence)
- **CO §57** Adoption Phase 5 — Learning (no evidence)
- **CO §59** Partial CO Conformance (no evidence)
- **CO §60** COC Agent — Deep Analyst (no evidence)
- **CO §62** COC Agent — Framework Specialists (no evidence)
- **CO §68** COC Multi-Phase Development Workflow (no evidence)
- **CO §70** COC Evidence-Based Completion (no evidence — covered by §29 instead)
- **CO §73-85** All COR (CO for Research) concepts (Literature Researcher, Field Expert, Claims Verifier, Argument Critic, Writing Partner, Cross-Reference Auditor, Teaching Obligation, Two Writing Modes, /craft, /write-para, Journal System, Hard Rules, Authentic Voice Preservation) — 13 concepts, no evidence (out of corpus scope; will need COR journals)
- **CO §86-97** All COE (CO for Education) concepts (Spectrum L1-L4, Rubric/Pattern/Anomaly/Quality Assessor agents, Hash Chain Integrity, Student Workflow, Instructor Assessment Workflow) — 12 concepts, no evidence (out of corpus scope)
- **CO §98-108** All COComp (CO for Compliance) concepts (Three Failure Modes per compliance, Regulatory Interpreter/Policy Drafter/Audit Preparer/Incident Assessor/Filing Analyst agents, Critical Rules, Risk Appetite, Status Proposed) — 11 concepts, no evidence (out of v1 scope)
- **CO §109-119** All COL-F (CO for Finance) concepts (Institutional Knowledge Inventory, Course Tutors, Academic Specialists, Finance Analysis Specialists, Critical Rules, Disclaimer Compliance, Data Sourcing Rule, Formula Reference, Six-Phase Workflow, Specialty Commands, Honest Limitations) — 11 concepts, no evidence (out of v1 scope)
- **CO §120-132** All COG (CO for Governance) concepts (Application Identity, Constitution Expert, Governance Layer Expert, Publication Preflight, CLAUDE.md Structure, Rule Classification, Defense in Depth Terrene Naming, Workflow Phases, Four Approval Gates, Observation Targets, Confidence Thresholds, Curator Role, Self-Hosting Status) — 13 concepts, no evidence (out of corpus scope; will need COG journals)
- **CO §133-139** All COL (CO for Learners) concepts (Application Identity, Subject Layer Pattern, Base Agent Categories, Base Rules, Six-Phase Workflow, Absolute Directive — Student Judgment, Relationship to COE) — 7 concepts, no evidence (out of v1 scope)
- **CO §140-151** All CO Workflows concepts (Governance/Standards/Strategy/Research Workflow, COR /teach, COR /literature, COR /deliberate, COR /craft, COR /write-para, COR /validate-claim, COR /challenge, COR /check-refs) — 12 concepts, no evidence (out of corpus scope)

### PACT — Concepts Without Evidence (41)

- **PACT §2** Domain Universality Claim (no evidence)
- **PACT §3** Dual-Use Risk (no evidence)
- **PACT §5** Team (T) Node Type (no evidence)
- **PACT §7** Core Grammar Constraint (D/T/R) (no evidence)
- **PACT §12** Task Envelope (Ephemeral) (no evidence — referenced obliquely in cross-sdk-governance 0006 only as "Task Envelope" alongside Bridges)
- **PACT §20** Auto-approved Zone (no evidence)
- **PACT §21** Flagged Zone (no evidence)
- **PACT §22** Held Zone (no evidence)
- **PACT §32** Compartments (Need-to-Know Isolation) (no evidence)
- **PACT §34** KnowledgeSharePolicy (KSP) (no evidence)
- **PACT §37** Access Enforcement Algorithm (no evidence)
- **PACT §39** Emergency Bypass (Tiered Override) (no evidence)
- **PACT §43** Audit Anchor (EATP Backing) (no evidence)
- **PACT §45** FM-1 Cross-Department Task Cascade (no evidence)
- **PACT §46** FM-2 Envelope Too Restrictive (no evidence)
- **PACT §47** FM-3 Envelope Too Loose (no evidence)
- **PACT §48** FM-4 Envelope Conflict Between Levels (no evidence)
- **PACT §49** FM-5 Emergency Bypass Abuse (no evidence)
- **PACT §50** FM-6 Infinite Task Decomposition (no evidence)
- **PACT §52** Edge Case — Acting in Two Roles (no evidence)
- **PACT §55** Edge Case — Reorganizations (no evidence)
- **PACT §56** Accountability Grammar (Brief Summary) (no evidence)
- **PACT §57** Operating Envelope (Brief Summary) (no evidence)
- **PACT §58** Knowledge Clearance Framework (Brief Summary) (no evidence)
- **PACT §59** Verification Gradient (Brief Summary) (no evidence)
- **PACT §60** Positional Addressing (Brief Summary) (no evidence)
- **PACT §61** Domain Universality (Brief Evidence) (no evidence)
- **PACT §62** PACT as Fourth Standard (Decision) (no evidence)
- **PACT §63** Alternatives Considered and Rejected (no evidence)
- **PACT §64** PACT Acronym Justification (no evidence)
- **PACT §65** Working Architecture Status (no evidence)
- **PACT §66** Publication Timeline (no evidence)
- **PACT §67** Shadow Agent Planning Removal (no evidence)

## Evidence Clusters by Craft Layer

Group of evidence entries by the three FORGE craft layers (practitioner / artifact / brokerage). Most kailash-py/kailash-rs/kaizen entries are artifact or brokerage craft. Most loom-meta entries are brokerage. Very few are practitioner — that's expected; the practitioner layer needs a different corpus (CARE thesis workspace, observation logs, dual-plane case studies).

### Layer 1 — Practitioner Craft

Practitioner-level moves: dual-plane classification, mirror-thesis intervention, constraint envelope reading, uncertainty handling.

Evidence is **sparse** in this corpus — practitioner-craft is largely absent. The closest entries are:

- loom/journal/0021-DECISION-autonomous-gate1-classification.md (re-interpretation of "human classifies" rule from "human performs" to "human approves" — direct CARE §4 / CO §4 application)
- loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md (empirical model-vs-rule ablation testing — challenges Institutional Knowledge Thesis from below)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0006-DISCOVERY-fabric-is-runtime-system-not-library.md (user challenge that triggers mental-model correction — Dual Plane in action)
- loom/kailash-py/workspaces/data-fabric-engine/journal/0007-DISCOVERY-fabric-is-four-needs.md (needs-forward reframing as red-team escape — recognising when the frame is wrong)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md (transparency-as-default decision; practitioner-level reading of CARE §9)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md (human-overrides-agent-tiering — direct practitioner intervention)
- loom/kz-engage/workspaces/herald/journal/0004-DISCOVERY-herald-as-proof-of-concept.md (self-demonstrating product framing; spec → product mapping)
- loom/kz-engage/workspaces/herald/journal/0005-CONNECTION-co-progressive-disclosure-in-herald-responses.md (applying CO principle to UX)

**~8 entries** — practitioner-craft evidence is the smallest cluster, as expected.

### Layer 2 — Artifact Craft

Agent / skill / rule / hook / command authoring. The dominant cluster.

Examples (representative selection — full list in batch files):

- loom/journal/0002-DECISION-cc-expert-artifacts.md (4-artifact agent+skill+rule+command quartet)
- loom/journal/0017-DISCOVERY-session-notes-broken.md (anti-amnesia hook fix)
- loom/journal/0029-DECISION-manifest-parity-automation.md (deterministic validation script)
- loom/journal/0040-DECISION-python-venv-enforcement.md (rule + hook combination)
- loom/journal/0050-DISCOVERY-session-start-command-injection.md (Layer 3 hook security audit)
- loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md (11-artifact defense-in-depth codification)
- loom/kailash-py/workspaces/kailash/journal/0019-DISCOVERY-nexus-triple-orphan-runtime.md (Layer 3 scope-hole)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md (variant classification + scope discipline)
- loom/kailash-rs/workspaces/kailash-ml-crate/journal/0010-DECISION-build-speed-codification.md (rule + config + agent triple codification)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md (red team finding → A1-A9 named patterns → dual artifact placement)
- loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md (codify-with-explicit-NOT-codified pattern)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md (per-turn token cost of CC artifacts)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0017-DECISION-m0-foundation-implementation.md (SystemPromptBuilder static/dynamic split)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0019-DECISION-m3-memory-system-implementation.md (memory subsystem implementation)
- loom/kz-engage/workspaces/herald/journal/0003-DISCOVERY-concept-aware-chunking-essential.md (knowledge pipeline design)

**~140 entries** — the dominant cluster across all four batches.

### Layer 3 — Brokerage + Governance Craft

Authority chain enforcement, variant classification, sync moves, red team convergence, upstream brokerage, PACT primitive integrity.

- loom/journal/0008-DECISION-coc-artifact-scoping.md (BUILD/USE authority chain)
- loom/journal/0013-DECISION-variant-architecture.md (foundational variant overlay system)
- loom/journal/0014-DISCOVERY-drift-audit.md (85% drift quantification → motivates variant architecture)
- loom/journal/0016-DECISION-co-authority-chain.md (co/+kailash/ split authority)
- loom/journal/0019-DECISION-sync-merge-not-copy.md (read-then-merge sync semantics)
- loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md (24-directory drift case study)
- loom/journal/0021-DECISION-autonomous-gate1-classification.md ("human approves classification, agents perform" re-interpretation)
- loom/journal/0026-DECISION-loom-atelier-naming.md (identity rename of stable system)
- loom/journal/0028-RISK-manifest-integrity-gaps.md (orphan/stale/misclassified taxonomy)
- loom/journal/0034-RISK-stack-gaps-redteam-attack.md (multi-round convergence loop)
- loom/journal/0037-RISK-redteam-round2-resolution-stress-test.md (numeric convergence threshold)
- loom/journal/0038-DECISION-redteam-round2-resolutions.md (explicit supersession journaling)
- loom/journal/0039-DECISION-redteam-round3-convergence.md (3-round convergence + phase transition)
- loom/journal/0046-DECISION-downstream-proposal-guard.md (runtime repo-type guard for synced commands)
- loom/journal/0047-DECISION-spec-coverage-gate.md (post-mortem of 5 systemic process failures)
- loom/journal/0048-DECISION-gate1-rs-variant-classification.md (4-category variant classification framework)
- loom/kailash-py/workspaces/kailash/journal/0014-DISCOVERY-pact-spec-conformance-four-gaps.md (spec-to-code conformance audit)
- loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md (adjacent-but-disconnected modules)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0001-DISCOVERY-vacancy-asymmetric-enforcement.md (governance primitive integrity audit)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md (cross-feature interaction red team)
- loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md (encapsulation as security boundary)
- loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md (loom/source bug escalation)
- loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md (core-vs-plugin variant test rule)
- loom/kz-engage/workspaces/herald/journal/0010-DISCOVERY-nexus-dedup-fingerprint-bug.md (cross-SDK upstream brokerage)

**~75 entries** — the second-largest cluster, dominated by loom-meta and kailash-rs cross-sdk-governance work.

## Cross-Entry Contradictions Noted

Listed by tension type. Each item names entries involved, one-sentence description, the rule or spec concept implicated, and whether it is FORGE-teachable or spec-maintenance.

### Contradiction 1: orphan skill audit (loom-meta 0005 vs 0011)

- **Entries**: loom/journal/0005-GAP-redteam-audit-gaps.md vs loom/journal/0011-RISK-redteam-r1-findings.md
- **Tension**: 0005 reports "59% of skill files are orphans" — 0011 retracts the finding because progressive disclosure (CO §15) is invisible to a "is this file referenced by name?" audit.
- **Implicates**: CO §15 (Progressive Disclosure)
- **Verdict**: **FORGE-teachable** — the contradiction itself is the lesson. Skill atom: "audit categorisation errors that mistake progressive-disclosure structures for orphans."

### Contradiction 2: Layer 5 mandatory vs token ablation (loom-meta 0006 vs 0049)

- **Entries**: loom/journal/0006-DECISION-learning-must-work.md vs loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
- **Tension**: 0006 declares "L5 Learning System is NOT aspirational — it MUST work." 0049 demotes 10 specific rules to on-demand because empirical ablation showed the model's baseline was stronger than expected. The contradiction is partial — 0049 doesn't demote Layer 5 itself, only specific rules — but the tension is real.
- **Implicates**: CO §1 (Institutional Knowledge Thesis), CO §39 (Layer 5 — Learning), CO §22 (Advisory Rules)
- **Verdict**: **FORGE-teachable** — the productive tension between "institutional knowledge compounds" and "the model is more capable than rules assumed." Skill atom: "empirical rule-necessity testing without abandoning the institutional knowledge thesis."

### Contradiction 3: kailash-rl as separate package — explicit supersession across rounds

- **Entries**: loom/journal/0035-DECISION-redteam-round1-resolutions.md (D1: kailash-rl as separate package) vs loom/journal/0036-RISK-kailash-rl-redteam-round2.md vs loom/journal/0038-DECISION-redteam-round2-resolutions.md (RT2-01: split into kailash-align + kailash-ml[rl]) vs loom/kailash-py/workspaces/kailash-align/journal/0011-DECISION-rl-for-llms-in-align-classical-in-ml.md vs loom/kailash-py/workspaces/kailash-align/journal/0016-DECISION-kailash-rl-separate-package.md
- **Tension**: A decision is made (D1 in 0035), stress-tested in 0036, superseded in 0038 with explicit "C11 splits into C11a + C11b." Then kailash-py 0011 places RL under kailash-ml[rl]; kailash-py 0016 reverses again to kailash-rl as separate package. Five entries, three position changes.
- **Implicates**: CO §28 (Approval Gate), CO §17 (Single Source of Truth)
- **Verdict**: **FORGE-teachable** — the convergence-loop craft. Skill atom: "explicit supersession journaling — record the prior decision and the reason it was wrong, do not silently replace."

### Contradiction 4: B0 estimate self-contradiction (loom-meta 0034)

- **Entries**: loom/journal/0034-RISK-stack-gaps-redteam-attack.md (Finding 1 — "the detailed refactor plan says 7 sessions; the workspace plan says 2-3 sessions")
- **Tension**: Two analyses produced by the same agent process from the same materials disagree by ~3x on effort estimate.
- **Implicates**: CO §28 (Approval Gate), `rules/autonomous-execution.md`
- **Verdict**: **FORGE-teachable** — self-contradiction as red-team finding. Skill atom: "treat compressed estimates with suspicion when an analyst process can produce internally inconsistent outputs."

### Contradiction 5: re-interpretation of "Human Classifies Every Inbound Change" (loom-meta 0021)

- **Entries**: loom/journal/0021-DECISION-autonomous-gate1-classification.md
- **Tension**: `artifact-flow.md` rule 4 says "Human Classifies Every Inbound Change." 0021 re-interprets this as "Human approves classification, agents perform." Not strictly a contradiction — it is a semantic shift in a MUST rule.
- **Implicates**: CARE §4 (Human-on-the-Loop), CO §4 (Human-on-the-Loop), CO §28 (Approval Gate)
- **Verdict**: **FORGE-teachable AND spec-maintenance** — needs to be folded back into the rule text. Skill atom: "re-interpreting a MUST rule from 'human performs' to 'human approves' when an agent team can produce trustworthy parallel proposals."

### Contradiction 6: pre-existing failures must-fix vs file-separately (kailash-rs 0002 in sync-express)

- **Entries**: loom/kailash-rs/workspaces/sync-express/journal/0002-DISCOVERY-binding-parity-gaps.md
- **Tension**: `rules/zero-tolerance.md` Rule 1 says "if you found it, you own it — fix it in this run." 0002 explicitly defers pre-existing parity gaps as separate issues. The agent's interpretation distinguishes pre-existing test failures (must-fix) from pre-existing parity gaps (track separately).
- **Implicates**: `rules/zero-tolerance.md` Rule 1
- **Verdict**: **spec-maintenance** — the rule needs to clarify the failure-vs-gap distinction. Until then: **FORGE-teachable** as a "scope discipline interpretation" skill atom.

### Contradiction 7: data-fabric package boundary (kailash-py 0001 vs 0004)

- **Entries**: loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md vs loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md
- **Tension**: Decision 0001 ("DataFlow extension, not new package") superseded by 0004 ("DataFlow IS the fabric") one day later.
- **Implicates**: CO §16 (Framework-First), CO §17 (Single Source of Truth)
- **Verdict**: **FORGE-teachable** — clean example of decision reversal journaling. Skill atom: "supersede with reference, do not overwrite."

### Contradiction 8: external CC artifacts vs baked-in token cost (kaizen-kz 0011)

- **Entries**: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md
- **Tension**: The external-artifact COC philosophy (everything as markdown) is challenged by token-cost data — 146KB external rules ≈ 36K tokens/turn vs 13K for Claude Code's compiled prompts.ts. Recommends a hybrid (bake core, keep project-specific external).
- **Implicates**: CO §13 (Layer 2 — Context), CO §14 (Master Directive), CO §15 (Progressive Disclosure)
- **Verdict**: **FORGE-teachable AND spec-maintenance** — the external/baked trade-off is currently invisible to the COC template structure. Needs to enter the artifact-flow rules as a token-budget consideration.

### Contradiction 9: Rust as reference (not follower) for Nexus DomainEventBus (kailash-rs 0001)

- **Entries**: loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md
- **Tension**: Most parity work assumes "Rust follows Python." This entry establishes the opposite: "Python B2 will adopt the Rust DomainEventBus semantics."
- **Implicates**: CO §17 (Single Source of Truth) — at the cross-language level
- **Verdict**: **FORGE-teachable** — bidirectional cross-language parity. Skill atom: "do not assume a fixed direction of parity; the audited side may be ahead."

## Top Skill-Atom Candidates

Aggregated from craft-skill-hint fields across all four batches. Each candidate gives 1-sentence name + 1-sentence description + entries providing evidence + spec concept(s) it serves. Limited to 20.

### 1. Spec-to-code conformance audit

**Skill**: Read every "MUST" in the spec for a module, then `grep` for its implementation in the codebase; produce a numbered gap list.
**Evidence**: kailash-py 0014 (PACT spec conformance four gaps); kailash-py 0015 (PACT-EATP bridge never wired); kailash-rs 0001 (vacancy asymmetric enforcement)
**Serves**: PACT §40 (EATP Integration), PACT §10 (Monotonic Tightening), CO §49 (Quality Criteria — Enforcement Reliability)

### 2. Variant classification — single-sentence test rule

**Skill**: Classify a COC artifact as core-vs-plugin against one test: "Does this rule apply if the user is NOT using Kailash?" YES → Core, NO → Plugin.
**Evidence**: kaizen-kz 0012 (core vs plugin classification); loom-meta 0048 (Gate 1 rs variant classification)
**Serves**: CO §17 (Single Source of Truth), CO §16 (Framework-First) at the artifact level

### 3. Multi-round red team to numeric convergence

**Skill**: Run red team rounds against the same artifact, name a numeric convergence threshold (e.g. fewer than 3 HIGH+ findings), and refuse to converge until met.
**Evidence**: loom-meta 0034-0039 (full convergence loop); kailash-rs 0003-0005 (kailash-ml-rs three-round convergence); kailash-rs 0005 (artifact red team convergence)
**Serves**: CO §28 (Approval Gate), CO §49 (Quality Criteria — Enforcement Reliability)

### 4. Codify-with-explicit-NOT-codified

**Skill**: In codify, write a "NOT codified" section with explicit rationale for each excluded item — distinguishes "institutional knowledge" from "code", "established science", and "already captured."
**Evidence**: kailash-rs 0003 (codify-phase0 — "what was NOT codified and why"); kailash-py 0012 (codify-session7 — "What Was NOT Codified")
**Serves**: CO §39 (Layer 5 — Learning), CO §47 (Knowledge Curator)

### 5. Defense-in-depth codification (multi-artifact landing)

**Skill**: Codify a single rule into multiple artifact locations simultaneously (rules + skills + agents + commands + hooks) so a single weak link doesn't bypass enforcement.
**Evidence**: kailash-py 0008 (engine layer three-layer model — 11 artifacts); kailash-rs 0004 (auth patterns A1-A9 codified into agent + skill); loom-meta 0007 (Defense in Depth — terrene naming, 7 layers)
**Serves**: CO §5.4 (Defense in Depth), CO §19 (Layer 3 — Guardrails)

### 6. Adjacent-but-disconnected module pattern

**Skill**: Identify where two modules solve complementary spec concepts but never call each other; flag the missing wiring as a spec conformance gap.
**Evidence**: kailash-py 0015 (PACT-EATP bridge never wired); kailash-py 0014 (PACT spec conformance four gaps); kailash-rs 0001 (vacancy asymmetric enforcement)
**Serves**: PACT §40 (EATP Integration), CO §17 (Single Source of Truth)

### 7. Authority-chain escalation when local fix gets reverted

**Skill**: When a fix in a USE repo gets reverted by the next /sync, escalate the fix upstream to the BUILD repo source — local fixes to synced artifacts are temporary.
**Evidence**: kailash-rs 0005 (artifact red team convergence — "GLOBAL BUG in loom/ source"); loom-meta 0019 (sync merge not copy); loom-meta 0021 (autonomous Gate 1 classification)
**Serves**: CO §17 (Single Source of Truth), PACT §11 (Role Envelope), PACT §10 (Monotonic Tightening)

### 8. Read-then-merge sync semantics (refusing rsync-style copy)

**Skill**: Replace bulk-copy semantics with read-then-merge that surfaces conflicts to a human decision point; numbering conflicts halt sync.
**Evidence**: loom-meta 0019 (sync merge not copy); loom-meta 0028 (manifest integrity gaps); loom-meta 0029 (manifest parity automation)
**Serves**: CARE §6 (Trust Verification Bridge), CO §28 (Approval Gate)

### 9. Empirical model-vs-rule ablation

**Skill**: Run scenarios in a clean repo (no .claude/, no CLAUDE.md, no hooks) so the model's true baseline is measured, not the rule-loaded session contaminated by overcorrection.
**Evidence**: loom-meta 0049 (token ablation experiment)
**Serves**: CO §1 (Institutional Knowledge Thesis), CO §22 (Advisory Rules)

### 10. Frontend mock data detection (extending no-stubs)

**Skill**: Extend stub detection to catch JS/TS frontend mock patterns (`MOCK_*`, `FAKE_*`, `generate*()`, `Math.random()` in display data) that Python-focused validators miss.
**Evidence**: loom-meta 0047 (spec coverage gate post-mortem)
**Serves**: CO §3.3 (Safety Blindness), CO §29 (Evidence-Based Completion)

### 11. Layer 3 hook security audit (the enforcers must obey their own rules)

**Skill**: Audit Layer 3 hook code for the same security issues those hooks are designed to catch (command injection from untrusted input files, etc.).
**Evidence**: loom-meta 0050 (session-start command injection); loom-meta 0003 (cc artifact audit — rules violated by their own enforcers)
**Serves**: CO §3.3 (Safety Blindness), CO §19 (Layer 3 — Guardrails)

### 12. Cross-feature interaction red team

**Skill**: When two governance features share state, write the cross-feature interaction test BEFORE writing either feature's primary tests; otherwise the safety property holds in each silo and breaks at the seam.
**Evidence**: kailash-rs 0008 (cross-feature vacancy bridge — vacant approver could approve bridges)
**Serves**: PACT §51 (Vacant Roles), PACT §35 (Bridges), CO §3.3 (Safety Blindness)

### 13. Encapsulation as security boundary in signed structs

**Skill**: Field visibility on signed/governed structs is a security boundary, not an ergonomics decision; default to private + accessor + single approved-mutation constructor path.
**Evidence**: kailash-rs 0009 (delegation record pub fields); kailash-rs 0004 (signature invalidation via conditional payload)
**Serves**: EATP §16 (Delegation Record), PACT §10 (Monotonic Tightening), CO §5 (Deterministic Enforcement)

### 14. Negative tests for deny-by-default

**Skill**: Auth middleware with "deny by default" semantics must be verified with negative tests — a test that confirms unmapped routes are blocked when the flag is on; mapped-route tests cannot detect inversion.
**Evidence**: kailash-rs 0002 (RBAC deny bypass); kaizen-py 0002 (security findings R1)
**Serves**: CO §49 (Quality Criteria — Enforcement Reliability), PACT §23 (Blocked Zone)

### 15. Worktree-per-agent for safe parallel execution

**Skill**: Isolate each parallel agent in its own git worktree with branch + file copy + independent test runs to prevent file conflicts during autonomous parallel work.
**Evidence**: kailash-py gh-issues 0008 (session1 parallel execution); loom-meta 0025 (parallel backlog resolution); kailash-py 0018 (five-workspace parallel implementation)
**Serves**: `rules/autonomous-execution.md`, COC §71 (Observation-Instinct-Evolution Pipeline)

### 16. Per-file disposition labelling pre-implementation

**Skill**: Per-file disposition pass on an existing codebase (SOLID / NEEDS MIGRATION / EXTEND / ENHANCE / REWRITE / IMPLEMENT) before implementation so agents stop re-deriving status mid-edit.
**Evidence**: kaizen-kz 0014 (existing codebase architecture); kailash-rs 0001 (architecture doc 12 corrections); kailash-rs nexus-parity 0001 (nexus-already-exceeds-python-b0)
**Serves**: CO §33 (Phase — Analyze), CO §29 (Evidence-Based Completion)

### 17. Friction-gradient inversion diagnosis

**Skill**: Diagnose when the easiest-to-use API has the highest discovery cost — a Layer 5 (institutional knowledge) failure that perpetuates Layer 2 (agent/master directive) failures.
**Evidence**: kailash-py 0006 (engine layer broken across projects); kailash-py 0007 (agent API unused but trap)
**Serves**: CO §1 (Institutional Knowledge Thesis), CO §3.2 (Convention Drift), CO §64 (COC Skill Hierarchy)

### 18. Convergence-as-validation (multi-perspective red team)

**Skill**: Run independent red-team agents against the same artifact and use their convergence rate to validate the audit method itself.
**Evidence**: kz-engage 0009 (red team convergence findings); kailash-rs 0008 (cross-feature vacancy bridge — two agents converged); loom-meta 0042 (kailash-ml-rs red team round 1 — three angles)
**Serves**: CO §5.4 (Defense in Depth), CO §49 (Quality Criteria — Enforcement Reliability)

### 19. Specialist delegation as required first move (framework work)

**Skill**: Route every SDK assumption through a framework specialist read-against-source pass before committing to the API surface in todos.
**Evidence**: kz-engage 0008 (SDK API corrections — 10 corrections from specialist reviews); kailash-py 0017 (PACT todos red team — TrustStore API mismatch)
**Serves**: CO §9 (Layer 1 — Intent), CO §11 (Routing Rules), `rules/agents.md` Specialist Delegation

### 20. Cross-SDK upstream brokerage (file the issue, don't work around)

**Skill**: When a discovered bug crosses SDK language boundaries, file an issue against each affected SDK rather than working around it locally — preserves the authority chain for the fix.
**Evidence**: kz-engage 0010 (Nexus dedup fingerprint bug — filed kailash-py#175 + kailash-rs#110); kailash-py 0013 (cross-SDK pseudo posture shared bug)
**Serves**: `rules/zero-tolerance.md` Rule 4, CO §17 (Single Source of Truth), PACT §10 (Monotonic Tightening) at the cross-repo level

## Data Quality Flags

Citation mismatches, typos, oblique references, and concept-number errors found during aggregation. None silently dropped — all flagged here so the next pass can correct them.

### Flag 1: CO §65 typo in loom-meta entry 0017

**Source**: loom/journal/0017-DISCOVERY-session-notes-broken.md
**Citation**: "CO §65 (COC Anti-Amnesia Hook)"
**Issue**: CO concept 65 is **COC Anti-Amnesia Hook** (correct), but the entry also lists "COC §65 (COC Anti-Amnesia Hook)" — duplicating the same concept under two namespace prefixes. The reverse index treats both as CO §65.

### Flag 2: Oblique CO §24 references in kaizen-kz

**Source**: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0002-DISCOVERY-claw-code-rust-reimpl.md notes ("permission cascade ... is a four-zone gradient cousin to CO § 24"); 0018-DECISION-m1-m2-implementation.md notes ("closest is CO § 24")
**Issue**: CO §24 (Tier 3 — Transport-Level Enforcement) is mentioned as "closest fit" or "cousin" but never as a direct citation in the structured "Spec concepts touched" line. Counted as 0 direct citations; flagged for the reader.

### Flag 3: Collective citation "PACT §14-18 (Five Constraint Dimensions)"

**Source**: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0002-DISCOVERY-two-constraint-systems.md and loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0006-DISCOVERY-kailash-py-delegate.md
**Issue**: Citation reads "PACT §14-18" as a range. The reverse index counts each of the five concepts (§14 Financial, §15 Operational, §16 Temporal, §17 Data Access, §18 Communication) as cited collectively. The same applies to "PACT §25-30" in kaizen-kz 0006 (five classification levels).

### Flag 4: "COC §61", "COC §63", "COC §64", "COC §65", "COC §66", "COC §71" double-prefix in loom-meta

**Source**: loom-meta entries 0002, 0003, 0004, 0005, 0017
**Issue**: These citations use the prefix "COC §" but the spec index files them under CO concepts 60-72 (the COC subsection of the CO core spec). The reverse index treats COC §N and CO §N as equivalent for N in [60, 72] because the CO concept index uses the COC label internally.

### Flag 5: "rules/agent-reasoning.md" citation in kailash-py 0004 and 0006 (kailash-ml)

**Source**: loom/kailash-py/workspaces/kailash-align/journal/0004-DECISION-defer-agents-v1.1.md and loom/kailash-py/workspaces/kailash-ml/journal/0006-DISCOVERY-guardrail4-baseline-doubles-compute.md
**Issue**: These cite a rule file directly rather than a spec concept. The reverse index does not include rule citations as spec evidence — they are flagged here for the rule-coverage view in a future pass. Same applies to `rules/security.md` (kailash-ml 0011), `rules/testing.md` (kailash-ml 0007 and kailash-py ADR 0057), `rules/git.md` (nexus-transport-refactor 0004), and `rules/autonomous-execution.md` (kailash-py gh-issues 0008 and kailash-py 0018).

### Flag 6: "CO Concept 9 (Principle 3 — Transparency as Foundation)" in mcp-platform-server 0004

**Source**: loom/kailash-py/workspaces/mcp-platform-server/journal/0004-RISK-ast-scanning-completeness-gap.md
**Issue**: The citation reads "CO Concept 9 (Principle 3 — Transparency as Foundation)" — but CO §9 is **Layer 1 — Intent** in the CO concept index, not "Principle 3 — Transparency as Foundation". Principle 3 (Transparency) is **CARE §9**. The entry's note correctly identifies CARE §9 in its body ("Maps to CARE Principle 3"). Reverse index treats the citation as **CARE §9**, not CO §9, because the body text is the more reliable signal.

### Flag 7: "CO §9 (Layer 1 — Intent)" mis-numbering does not occur — verified

The CO concept index uses the same CO §9 number for "Layer 1 — Intent" (architecture section) — there is no overlap with CARE §9 because they are in different namespaces. No correction needed beyond Flag 6.

### Flag 8: "EATP §16 (Element 2 — Delegation Record)" cited in kailash-py gh-issues 0003 as "EATP Concept 16 (Delegation Record)"

**Source**: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0003-CONNECTION-pact-kaizen-cross-package.md
**Issue**: Concept name slightly abbreviated ("Delegation Record" vs "Element 2 — Delegation Record"). Same concept, normalised to EATP §16.

### Flag 9: kaizen-cli-rs entry count discrepancy

**Source**: coc-layer-kaizen-kz.md summary
**Issue**: The summary block has multiple recounts of kaizen-cli-rs entry types (one says 27, another 26). The actual file count is 26, confirmed by the per-entry tally. The reverse index uses 26 for kaizen-cli-rs and 41 total for the kaizen-kz batch (26 + 3 + 12).

### Flag 10: kailash-rs entry count discrepancy

**Source**: coc-layer-kailash-rs.md summary
**Issue**: Brief said 42 entries; actual count is 41 (one fewer than estimated, accounted for in the batch summary). Total corpus is therefore 232, not 233 as the original brief stated. The frontmatter `entries_indexed: 232` reflects the actual count.

### Flag 11: ADR numbering collisions in kailash-py docs/adr

**Source**: loom/kailash-py/docs/adr/
**Issue**: Three ADRs share number 0051 (advanced search, version history UI, VS Code bridge); two share 0017 (LLM provider, multi-workflow); two share 0052 (collaboration presence, UI prioritization). Not citation errors but artifact-quality flags. The reverse index treats each as a distinct entry by full path.

### Flag 12: Frontmatter quality flags carried forward from batch files

- `gh-issues-consolidation` workspace (kailash-py): 9/9 entries have NO YAML frontmatter
- `pact-309-308` workspace (kailash-py): 3/3 entries have sparse frontmatter
- Most ADRs in kailash-py docs/adr are undated

These are flagged in the originating batch file but do not affect concept aggregation.

---

## How to Use This Index

1. **For skill-atom drafting**: Start from heavy-cited concepts (≥10 entries). These are the load-bearing concepts where the FORGE skill catalog should have multiple atoms each.

2. **For coverage gap planning**: The "Concepts Without Evidence" section names which spec concepts will need future corpora — CARE thesis workspace for §1-29, EATP interop work for §73-94, COR/COE/COG journals for the domain-application concepts, PACT classification semantics for §25-39.

3. **For contradiction-driven curriculum**: The contradictions section is the highest-signal teaching material. Each contradiction is either a teachable convergence-craft moment or a spec-maintenance issue worth surfacing.

4. **For brokerage-craft material**: Layer 3 evidence cluster contains the canonical examples of authority-chain enforcement, variant classification, and sync moves. These are the brokerage skills `co-codegen` will inherit.

5. **For practitioner-craft acknowledgement**: Layer 1 is sparse — that's diagnostic, not a failure. The COC corpus is engineering work; practitioner-craft material lives in the journal corpus that hasn't been indexed yet.
