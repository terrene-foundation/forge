---
drill_id: DR-SC-P-009-FULL
source_atom: SC-P-009
difficulty: beginner
time_box_minutes: 15
---

# Treat Architecture Documents as Testable Claims Against the Actual Codebase

## Setup

The practitioner receives an architecture document and a module containing 8 source files.

**Architecture document** (2 pages, modelled on the kailash-ml-rs architecture doc before correction):

> **kailash-ml-rs Architecture**
>
> Dependencies: polars 0.39, ndarray 0.15, ort 2.0 (stable), candle-core (ONNX backend), linfa 0.7 (required for all inference).
>
> Structure: PyO3 bindings register at root level via existing pattern. Arrow zero-copy between Python and Rust uses Arrow IPC format via `arrow2::io::ipc`.
>
> Capabilities: The ML crate provides ONNX inference via candle, classical ML via linfa, and DataFrame operations via polars. All three backends are required for v1.
>
> Missing: No feature store, no model registry, no drift monitoring. These must be implemented from scratch. The training pipeline does not exist.
>
> Estimated scope: Full greenfield implementation, 8-10 sessions.

**Source files** (simplified from real kailash-rs crate):

1. `Cargo.toml` -- declares polars = "0.53", ndarray = "0.16", ort = "2.0.0-rc.12" (pre-release)
2. `src/inference/onnx.rs` (180 lines) -- ONNX inference using `ort` crate (NOT candle)
3. `src/inference/candle.rs` (120 lines) -- safetensors model loading (NOT ONNX)
4. `src/classical/linfa_bridge.rs` (90 lines) -- optional linfa integration behind `#[cfg(feature = "linfa")]`
5. `src/dataframe/mod.rs` (200 lines) -- polars DataFrame operations with `pyo3-polars::PyDataFrame`
6. `src/bindings/mod.rs` (60 lines) -- PyO3 bindings registered at a SUBMODULE level (not root)
7. `src/registry.rs` (150 lines) -- basic model registry with save/load
8. `src/drift/basic.rs` (80 lines) -- basic statistical drift detection

## Task

1. Read each factual claim in the architecture document. Extract 10 claims into a numbered checklist.
2. For each claim, read the relevant source file(s) and classify the claim as:
   - **CONFIRMED** -- code matches the document's assertion
   - **CORRECTED** -- code contradicts the document, with the specific discrepancy
   - **ABSENT** -- code does not address the claim at all
3. Produce a numbered correction list. For each CORRECTED claim, write the specific discrepancy (not just "version is wrong" but "doc says polars 0.39, codebase uses 0.53, 14 minor versions of API changes").
4. Revise the effort estimate based on your corrections. Explain whether the estimate should go up, down, or stay the same for each correction.

## Model Answer

| # | Claim | Disposition | Evidence |
|---|---|---|---|
| 1 | polars 0.39 | CORRECTED | `Cargo.toml` declares polars = "0.53". 14 minor versions behind the doc's claim. API surface has changed significantly. Effort impact: reduces risk (newer version is better), but all code examples in the architecture doc will not compile against 0.53. |
| 2 | ndarray 0.15 | CORRECTED | `Cargo.toml` declares ndarray = "0.16". Must match tract-onnx's internal ndarray. Using 0.15 would create type incompatibilities at the tract boundary. |
| 3 | ort 2.0 (stable) | CORRECTED | `Cargo.toml` declares ort = "2.0.0-rc.12" (pre-release). Cargo semver resolution does not include pre-releases by default. Must specify the exact pre-release version or use stable 1.16.0. |
| 4 | candle-core is ONNX backend | CORRECTED | `src/inference/candle.rs` loads safetensors models, not ONNX. The ONNX backend is `ort` (see `src/inference/onnx.rs`). Candle and ONNX serve different purposes. |
| 5 | linfa 0.7 required for all inference | CORRECTED | `src/classical/linfa_bridge.rs` is behind `#[cfg(feature = "linfa")]` -- it is optional, not required. Only needed for classical ML, not for ONNX or neural inference. |
| 6 | PyO3 bindings at root level | CORRECTED | `src/bindings/mod.rs` registers at submodule level. This is a new pattern -- existing bindings in the rest of the codebase register at root. ML would be the first submodule. |
| 7 | Arrow IPC for zero-copy | CORRECTED | `src/dataframe/mod.rs` uses `pyo3-polars::PyDataFrame` for zero-copy, not Arrow IPC. The Arrow IPC pattern described in the doc does not work for this use case. |
| 8 | No model registry | CORRECTED | `src/registry.rs` has 150 lines of basic model registry with save/load. The doc says it does not exist. |
| 9 | No drift monitoring | CORRECTED | `src/drift/basic.rs` has 80 lines of statistical drift detection. The doc says it does not exist. |
| 10 | 8-10 sessions full greenfield | CORRECTED | With a working model registry, drift detection, ONNX inference, candle inference, linfa bridge, DataFrame operations, and bindings already present, the effort is closer to 3-4 sessions of enhancement, not 8-10 sessions of greenfield. |

**Score: 0 CONFIRMED, 10 CORRECTED, 0 ABSENT.** Every claim in the architecture doc was wrong. This is an extreme case, but it mirrors the real kailash-ml-crate finding (journal 0001) where 12 corrections were found, 3 of them critical.

**Effort revision:** Down from 8-10 sessions to 3-4 sessions. The codebase already has most components in partial or complete form. The primary work is enhancing existing implementations and fixing the dependency version issues, not building from scratch.

## Scoring Criteria

1. **Claim extraction completeness** (pass: 8/10 or better): The practitioner must extract at least 8 testable claims from the document. Practitioners who extract only 4-5 obvious claims (the version numbers) and skip the structural claims (binding pattern, Arrow IPC, candle purpose) are missing the highest-impact corrections.
2. **Correction specificity** (pass: 7/10 specific): Vague corrections like "version wrong" score zero. Specific corrections like "doc says polars 0.39, Cargo.toml says 0.53, 14 minor versions with significant API changes" score full. Each correction must name the doc's claim, the code's reality, and the impact.
3. **Effort estimate direction** (pass/fail): The revised estimate must decrease. A practitioner who finds 10 corrections (most of which reveal existing capabilities the doc said were missing) and arrives at the same estimate has not connected the corrections to planning.
4. **Dependency version audit** (pass/fail): The practitioner must catch all 3 dependency version corrections (polars, ndarray, ort). These are the critical corrections from journal 0001 -- missing them would have caused compilation failures.

## Common Mistakes

1. **Treating the architecture document as authoritative.** The practitioner reads the doc, nods along, and proceeds to implementation. Mid-implementation they discover that polars 0.53 has a different API than the 0.39 examples in the doc, that candle does not do ONNX, and that the model registry they were about to build already exists. Each discovery is a context switch. The journal entry shows the extreme: 12 corrections including 3 that would have caused compilation failures.

2. **Checking only dependency versions and missing structural claims.** The practitioner catches polars/ndarray/ort version differences but does not verify the PyO3 binding pattern, the Arrow IPC claim, or the candle-as-ONNX claim. Structural corrections are often higher-impact than version corrections because they change the implementation approach, not just a version pin.

3. **Assuming "doc says missing" means "genuinely missing."** The doc says no model registry and no drift monitoring. The practitioner takes this at face value. But `src/registry.rs` and `src/drift/basic.rs` exist with working code. The nexus-parity case (journal 0003) showed the same pattern: 5 GitHub issues claimed capabilities were "missing" but a code audit revealed ~60% already existed.

## Extensions

1. **Stale issue tracker audit.** Replace the architecture document with 5 GitHub issues that claim capabilities are missing (modelled on the nexus-parity issues, journal 0003). The practitioner must audit each issue against the codebase and produce a matrix showing which claims are still valid vs which are stale. Estimated genuine gap vs claimed gap should shrink by ~60%.

2. **Architecture doc with internal contradictions.** Add claims that contradict each other within the same document (e.g., "candle provides ONNX inference" in the Dependencies section vs "ONNX inference uses the ort crate" in the Capabilities section). The practitioner must detect the internal contradiction as a signal that the document was written incrementally without verification.

3. **Write the corrected document.** After producing the correction list, the practitioner rewrites the architecture document to reflect the actual codebase state. Score on whether every factual claim in the rewrite is verifiable against the source files, with file:line references for each.
