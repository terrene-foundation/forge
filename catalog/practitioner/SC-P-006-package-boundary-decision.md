---
atom_id: SC-P-006
name: Decide whether new functionality is a module inside an existing package or a separate package
craft_layer: practitioner
destination: forge
craft_areas: [intervene-when]
applications: [COC]
spec_lineage:
  - CO §16 — Framework-First
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md
    polarity: POSITIVE
    quote: "Ship the data fabric engine as an optional extension of `kailash-dataflow`, installed via `pip install kailash-dataflow[fabric]`."
  - path: loom/kailash-py/workspaces/kailash-align/journal/0016-DECISION-kailash-rl-separate-package.md
    polarity: POSITIVE
    quote: "`kailash-rl` is a first-class Kailash framework package, NOT `kailash-ml[rl]`. Same reasoning as kailash-align: different primitives, different users, different dependencies."
  - path: loom/kailash-py/workspaces/kailash-ml/journal/0010-DECISION-rust-engine-replaces-sklearn-wrapping.md
    polarity: MIXED
    quote: "kailash-ml (Python package) will become a PyO3 binding over kailash-ml-rs (the Rust ML compute engine) instead of wrapping scikit-learn/LightGBM/XGBoost/CatBoost as Python dependencies."
practice_modality: case
status: verified
---

## The move

When new functionality appears on the roadmap, apply a three-question test before writing the first line of code: (1) Does this functionality have its own release cycle? (2) Does it have consumers distinct from the parent package's consumers? (3) Does it carry dependencies that the parent package's users should not be forced to install? If all three answers are YES, create a separate package. If all three are NO, ship as a module or optional extra inside the existing package. If mixed, default to starting inside and extracting later -- but only if extraction cost is genuinely low (same API surface, no deep coupling to parent internals).

## When it fires

A brief or todo introduces new functionality that could plausibly live inside an existing package or as a new standalone package. The classic signal is the phrase "should this be a new package or part of X?" appearing in planning. A secondary signal is an existing package gaining optional extras with heavyweight dependencies that most users do not need.

## What it serves

CO §16 (Framework-First) says to prefer composition of existing organizational building blocks before building from scratch. A new package IS building from scratch -- new pyproject.toml, new CI workflow, new PyPI OIDC publisher, new version tracking. If the existing framework already handles the task, extending it is cheaper. CO §17 (Single Source of Truth) says each piece of institutional knowledge lives in exactly one place. When functionality is split across packages without a clear boundary, the same concept ends up defined in two places (the parent's interface and the child's implementation), violating single-source-of-truth and creating version-coupling bugs.

## Evidence walkthrough

- **0001-DECISION-dataflow-extension-not-separate-package** (POSITIVE) -- The data fabric engine was proposed as a standalone `kailash-fabric` package. Two independent red team passes converged on shipping it as `kailash-dataflow[fabric]` instead. The deciding factors: it uses only DataFlow's public cache API, existing DataFlow users discover it organically, and "easy to split later, hard to merge after separation." This is the three-question test answered NO-NO-NO: same release cycle as DataFlow, same consumers (DataFlow users who want non-database sources), and dependencies manageable as an optional extra.
- **0016-DECISION-kailash-rl-separate-package** (POSITIVE) -- kailash-rl was initially planned as `kailash-ml[rl]` (journal 0011). The programme owner reversed this: "different primitives, different users, different dependencies." RL has its own release cycle (RL algorithms evolve independently of classical ML), its own consumers (RL researchers, not tabular-data practitioners), and its own dependencies (gymnasium, stable-baselines3). All three answers YES, so separate package. The entry explicitly calls out that "start as extra, extract later" was a false economy in this case.
- **0010-DECISION-rust-engine-replaces-sklearn-wrapping** (MIXED) -- kailash-ml itself faced a boundary decision: should the Python package wrap scikit-learn or bind to a Rust engine? The answer preserved the package boundary (kailash-ml stays one package) but changed what lives inside it. The entry shows the boundary question can also be about what a package wraps, not just whether to split. The "avoid deep investment in Python-specific ML algorithm code that the Rust engine will supersede" guidance is a boundary signal: when the implementation substrate is about to change, the boundary is between orchestration (stays in Python) and compute (moves to Rust).

## How to drill it

Present the practitioner with three proposed features, each with a brief describing the feature, its expected consumers, its dependency footprint, and its release cadence. One should clearly be a separate package (different consumers, heavy unique deps, independent release cycle). One should clearly be an extension of an existing package (same consumers, no new deps, shared release cycle). One should be ambiguous (overlapping consumers, moderate deps, release cycle unclear). The practitioner must:

1. Apply the three-question test to each feature.
2. Write a one-paragraph rationale for each decision.
3. For the ambiguous case, state which direction they chose AND what would flip the decision.
4. Identify whether the ambiguous case has low or high extraction cost if they chose "start inside."

Score on whether the clear cases are correct, whether the ambiguous case's rationale addresses all three questions, and whether the extraction-cost assessment is grounded in specifics (API surface area, internal coupling depth) rather than hand-waving.

## Common failure mode

The practitioner defaults to "make everything a separate package" because it feels cleaner, without accounting for the overhead: new CI pipeline, new PyPI publisher setup, new version tracking entry, new CLAUDE.md listing, new sync target. The data-fabric-engine entry shows the corrective -- two red teams independently concluded that extension-first is cheaper when the three-question test says NO. The opposite failure (cramming everything into one package) is rarer in practice but equally costly, as the kailash-rl reversal shows.

## Related atoms

- SC-A-003 (defense-in-depth codification) -- package boundary decisions affect where guardrails are codified (parent package rules vs child package rules).
- SC-A-011 (framework-first check) -- the framework-first check fires before the package boundary decision; if the functionality already exists in a framework, the boundary question is moot.
- SC-P-001 (adjacent-but-disconnected modules) -- a wrong package boundary decision can create adjacent-but-disconnected modules that share consumers but not interfaces.
- SC-P-008 (dependency chain fragility) -- package boundaries determine which dependency chains are exposed to consumers; a wrong boundary forces heavyweight transitive dependencies on users who do not need them.
