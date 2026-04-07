---
atom_id: SC-P-008
name: Trace deep dependency chains to find where a single broken link breaks the install path
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - CO §3.3 — Safety Blindness
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash-align/journal/0007-DISCOVERY-trl-dependency-chain-fragility.md
    polarity: NEGATIVE
    quote: "The architecture doc pins `trl>=0.8`, but the API changed substantially between 0.8 and 0.29 (current, March 2026). A user installing kailash-align today with `trl>=0.8` could get TRL 0.12 (which lacks SFTConfig entirely), and AlignmentPipeline would fail at import time with an obscure ImportError."
  - path: loom/kailash-py/workspaces/kailash-align/journal/0002-RISK-gguf-conversion-fragility.md
    polarity: NEGATIVE
    quote: "Critically, failures can be **silent** -- producing a valid-looking GGUF file that crashes only at inference time."
practice_modality: case
status: draft
---

## The move

Before declaring a dependency specification complete, trace the full transitive dependency chain of every heavyweight dependency. For each link in the chain, answer three questions: (1) Is this dependency maintained by more than one person? (2) Does it publish binary wheels for all target platforms, or does it require compilation from source? (3) Has its API broken between the minimum pinned version and the current version? Any link that fails all three questions is a fragility point. Pin the fragile link tightly, add a CI version-matrix test covering the minimum and maximum of the pinned range, and document the fragility in the package's dependency notes.

## When it fires

Two trigger conditions:

1. A new package or feature introduces a dependency chain more than 2 levels deep (A depends on B depends on C depends on D). The deeper the chain, the higher the probability that one link is fragile.
2. A user reports a cryptic installation or import failure that traces to a version mismatch in a transitive dependency. This is the after-the-fact signal that the move should have fired during analysis.

## What it serves

CO §3.3 (Safety Blindness) states that "the AI optimizes for the most direct path to task completion. Safety measures are never the most direct path. The AI deprioritizes them unless explicitly enforced." Dependency chain auditing is a safety measure: the most direct path is to declare `trl>=0.8` and move on. The safe path is to trace what TRL 0.8 vs 0.29 actually means for the API surface, discover that `SFTConfig` does not exist in 0.12, and tighten the pin to `>=0.25,<1.0`. Without explicit enforcement (a dependency-audit step in analysis), this safety measure is skipped every time.

## Evidence walkthrough

- **0007-DISCOVERY-trl-dependency-chain-fragility** (NEGATIVE) -- The entry documents a 5-level dependency chain (`torch -> transformers -> trl -> peft -> accelerate`) with four specific fragility points. Fragility 1 is the most concrete: the architecture doc's `trl>=0.8` pin allows installing a TRL version where `SFTConfig` does not exist, causing an import-time failure. Fragility 2 is subtler: PEFT adapters saved with one transformers version may not load with another due to internal class hierarchy changes, meaning the AdapterRegistry can silently corrupt adapters across upgrades. Fragility 3 is dangerous: QLoRA combined with gradient checkpointing on specific GPU architectures produces silent numerical errors where training appears to converge but the adapter is degraded. Each fragility was discoverable by tracing the chain and asking the three questions -- but none was discovered during initial architecture, only during red team.
- **0002-RISK-gguf-conversion-fragility** (NEGATIVE) -- GGUF conversion depends on llama.cpp's `convert_hf_to_gguf.py`, which is a single external script maintained by the llama.cpp project. The entry documents that this conversion can fail silently: producing a valid-looking GGUF file that crashes at inference time. The dependency chain here is not package-level but tool-level: kailash-align depends on a compiled C++ binary (`llama-quantize`) that must be separately installed and on PATH. This is a fragility point that fails all three questions: llama.cpp is community-maintained (bus factor risk), the binary requires compilation from source on many platforms, and the converter's supported architecture list changes between versions.

## How to drill it

Give the practitioner a `pyproject.toml` for a hypothetical package that depends on 3 libraries, each with loose version pins (`>=X`). Provide a dependency tree showing the transitive closure (8-12 total packages). Embed three fragility points:

1. One package where the minimum pinned version lacks an API that the code uses (the TRL pattern).
2. One package that requires source compilation on one major platform (no binary wheel for ARM Linux, for example).
3. One package maintained by a single contributor with irregular release cadence.

The practitioner must:

1. Trace the full dependency tree and identify all three fragility points.
2. For each fragility, state which of the three questions it fails and what the concrete risk is.
3. Propose tightened version pins with a rationale for each bound.
4. Describe what a CI version-matrix test for this dependency set would look like (which combinations to test).

Score on whether all three fragility points are found, whether the version pins are specific enough to prevent the failure but not so tight that they block legitimate upgrades, and whether the CI matrix covers the minimum and maximum of each tightened range.

## Common failure mode

The practitioner writes `>=X` for every dependency because "we want the latest" and moves on. The fragility is invisible until a user on a different platform or with a different resolver version installs a combination that breaks. The TRL entry is the canonical example: `>=0.8` looks reasonable (0.8 is old, we want at least that), but the range admits versions where the required API does not exist. The fix is not to pin exactly (which blocks all upgrades) but to pin a floor that guarantees API presence and a ceiling that prevents major-version surprises.

## Related atoms

- SC-P-006 (package boundary decision) -- package boundaries determine which dependency chains are exposed to which consumers. A wrong boundary decision can force heavyweight transitive dependencies on users who do not need them.
- SC-A-006 (negative tests for deny-by-default) -- dependency fragility tests are a form of negative testing: verifying that the system fails gracefully (not silently) when a transitive dependency is at its minimum pinned version.
