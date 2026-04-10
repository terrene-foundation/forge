---
drill_id: DR-SC-P-006-FULL
source_atom: SC-P-006
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Decide Whether New Functionality Is a Module Inside an Existing Package or a Separate Package

## Setup

The practitioner receives three proposed features for the Kailash SDK ecosystem. Each proposal includes a brief, the expected consumers, the dependency footprint, and the release cadence. The practitioner must apply the three-question test to decide the package boundary for each.

**Feature Alpha — Data Fabric Engine:**

> **Brief:** A new engine that lets DataFlow connect to non-database sources (REST APIs, CSV files, cloud storage) using the same model-to-node pattern. It builds on DataFlow's existing cache API and data model patterns.
>
> **Expected consumers:** Existing DataFlow users who want to pull data from non-database sources into the same DataFlow pipelines they already use.
>
> **Dependency footprint:** `httpx` (for REST), `fsspec` (for cloud storage). Both are lightweight. DataFlow already depends on `httpx` for its test suite.
>
> **Release cadence:** Ships with DataFlow releases. A new DataFlow minor version would include fabric engine improvements. No independent release pressure.

**Feature Beta — Reinforcement Learning Framework:**

> **Brief:** A framework for reinforcement learning — environment wrappers, policy training loops, reward shaping, and evaluation. Shares no code with the existing kailash-ml classical/deep learning pipeline.
>
> **Expected consumers:** RL researchers and robotics engineers. The existing kailash-ml user base (tabular data practitioners, classification/regression tasks) has minimal overlap — RL users do not typically use kailash-ml's feature stores or AutoML.
>
> **Dependency footprint:** `gymnasium` (environment standard), `stable-baselines3` (policy algorithms), `tensorboard` (experiment tracking). Total: ~400 MB of dependencies. None of these are used by kailash-ml.
>
> **Release cadence:** RL algorithm research moves on a different cycle than classical ML. gymnasium and stable-baselines3 release independently of scikit-learn/polars. The RL framework would need to release when new algorithms land, regardless of kailash-ml's schedule.

**Feature Gamma — Workflow Visualization:**

> **Brief:** A visualization layer that renders Kailash Core SDK workflows as interactive diagrams — node graphs, execution traces, cycle animations. Useful for debugging and documentation.
>
> **Expected consumers:** Mixed. Core SDK users who want to debug workflows (same audience as Core SDK). But also documentation teams and non-developer stakeholders who want to understand workflow architecture without reading code (different audience).
>
> **Dependency footprint:** `graphviz` (graph layout), `rich` (terminal rendering). Moderate — `graphviz` requires a system-level install. Core SDK does not depend on either.
>
> **Release cadence:** Unclear. Visualization should update when Core SDK adds new node types, suggesting shared cadence. But documentation-focused features (export to PNG, interactive HTML) might ship independently.

**The three-question test** (from the atom):

1. Does this functionality have its own release cycle?
2. Does it have consumers distinct from the parent package's consumers?
3. Does it carry dependencies that the parent package's users should not be forced to install?

## Task

1. Apply the three-question test to each feature. For each feature, answer all three questions with YES or NO and a one-sentence justification for each answer.
2. Write a one-paragraph rationale for each boundary decision. The rationale must reference the three-question answers, not just state the conclusion.
3. For the ambiguous case, state which direction you chose AND identify the single factor that would flip the decision to the opposite choice.
4. For the ambiguous case, assess the extraction cost if you chose "start inside." Rate as LOW or HIGH and ground the assessment in specifics: how much API surface would need to be replicated, how deeply does the feature couple to parent internals, and how many consumers would need to update their imports?

## Model Answer

**Step 1 — Three-question test:**

| Question                 | Alpha (Data Fabric)                                  | Beta (RL Framework)                                                                      | Gamma (Workflow Viz)                                                                            |
| ------------------------ | ---------------------------------------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Own release cycle?       | NO — ships with DataFlow                             | YES — RL algorithms release independently of classical ML                                | UNCLEAR — some features share Core SDK cadence, documentation features could be independent     |
| Distinct consumers?      | NO — existing DataFlow users                         | YES — RL researchers, not tabular data practitioners                                     | MIXED — debugging serves Core SDK users (same), documentation serves non-developers (different) |
| Heavyweight unique deps? | NO — httpx already a test dep, fsspec is lightweight | YES — 400 MB of gymnasium + stable-baselines3 + tensorboard, none shared with kailash-ml | MODERATE — graphviz requires system install but is not 400 MB; rich is lightweight              |

**Step 2 — Rationale for each decision:**

**Alpha → Extension of DataFlow** (`pip install kailash-dataflow[fabric]`). All three answers are NO. The fabric engine uses DataFlow's public cache API, serves DataFlow's existing users, adds no heavyweight dependencies, and releases on DataFlow's schedule. Creating a separate `kailash-fabric` package would mean a new pyproject.toml, a new CI pipeline, a new PyPI publisher, and a new version-tracking entry — all for something that is functionally a DataFlow feature. The optional-extra pattern (`[fabric]`) lets users who want non-database sources opt in without forcing the dependencies on users who do not.

**Beta → Separate package** (`pip install kailash-rl`). All three answers are YES. RL has its own release cycle (gymnasium updates do not wait for scikit-learn), its own consumers (RL researchers do not use kailash-ml's feature stores), and its own dependency tree (400 MB that tabular-data users should never download). Shipping as `kailash-ml[rl]` would mean that `pip install kailash-ml` pulls in gymnasium metadata, kailash-ml's release notes would include RL entries irrelevant to 90% of its users, and a gymnasium breaking change would block a kailash-ml release. The journal evidence (0016-DECISION) confirms: "different primitives, different users, different dependencies."

**Gamma → Start inside Core SDK as optional extra** (`pip install kailash[viz]`), with noted extraction risk. Two of three answers lean toward "start inside" (shared cadence for the debugging use case, partial consumer overlap) and one leans toward separation (documentation audience is distinct). The tiebreaker is extraction cost: if visualization stays coupled to Core SDK internals (node type registry, execution trace format), extraction later would require duplicating those internals. But if the visualization layer consumes only the public workflow API (`workflow.build()` output), extraction cost is low. Starting inside with a clean API boundary preserves optionality.

**Step 3 — Ambiguous case (Gamma) — flip factor:**

Decision: Start inside as `kailash[viz]`.

The single factor that would flip to a separate package: **if the documentation/non-developer audience becomes the primary consumer** and requests features incompatible with Core SDK's release cadence (e.g., interactive HTML export, hosted diagram server, Figma integration). At that point, the release-cycle answer flips from NO to YES, making it 2 YES / 1 MIXED, and the balance tips toward separation.

**Step 4 — Extraction cost assessment (Gamma):**

**Rating: LOW**, contingent on one architectural constraint.

- **API surface to replicate:** The visualization layer needs only the workflow graph structure (nodes, connections, parameters) and execution trace data. If it consumes `workflow.build()` output (a serializable graph object), extraction means the new package imports `kailash` and calls the same public API. Zero internal coupling.
- **Coupling depth to parent internals:** The risk is if visualization accesses Core SDK's internal node registry, execution runtime state, or unpublished graph formats. If it stays on the public API, coupling is shallow. If it reaches into `_internal` modules for node metadata, extraction cost jumps to HIGH because those internals would need to be stabilized or duplicated.
- **Consumer import updates:** Users currently doing `from kailash.viz import render_workflow` would need to change to `from kailash_viz import render_workflow`. One import path change per consumer — manageable if the user base is small (early stage).

The architectural constraint that keeps extraction cost LOW: the visualization layer must consume only public API outputs, never internal registry state. A rule in `.claude/rules/` could enforce this: "viz module MUST NOT import from `kailash._internal` or `kailash.runtime._*`."

## Scoring

| Criterion                                                                                          | Points |
| -------------------------------------------------------------------------------------------------- | ------ |
| Alpha correctly identified as extension (all three answers NO)                                     | 1      |
| Beta correctly identified as separate package (all three answers YES)                              | 1      |
| Gamma identified as ambiguous with the ambiguity grounded in mixed answers                         | 1      |
| Three-question test applied systematically to all three features (not skipped for "obvious" cases) | 1      |
| Rationale for Alpha references the overhead of a separate package (CI, PyPI, versioning)           | 1      |
| Rationale for Beta references the dependency burden on non-RL users                                | 1      |
| Flip factor for Gamma is specific (names the condition and the answer it changes)                  | 1      |
| Extraction cost rated with specifics (API surface, coupling depth, import changes)                 | 1      |
| Extraction cost assessment names the architectural constraint that keeps cost low                  | 1      |
| No decision defaults to "separate package because it's cleaner" without accounting for overhead    | 1      |
| **Total**                                                                                          | **10** |

## Extensions

1. **Reversed ambiguity.** Modify Gamma so the documentation audience is already the primary consumer (80% of downloads are from non-developers using the HTML export). The practitioner must re-run the three-question test with this new information and arrive at the opposite decision (separate package). This tests whether the practitioner's framework adapts to new data or locks into the original answer.

2. **Post-decision validation.** After deciding Alpha is an extension, present a scenario 6 months later: the fabric engine now depends on `boto3` (AWS SDK, 50 MB), `google-cloud-storage` (30 MB), and `azure-storage-blob` (20 MB). The practitioner must reassess: do the new heavyweight dependencies flip the answer? Should fabric be extracted to a separate package now? What is the extraction cost given 6 months of development inside DataFlow?

3. **Boundary enforcement.** For the Gamma decision (start inside), the practitioner writes a `.claude/rules/viz-boundary.md` rule that prevents the visualization layer from coupling to Core SDK internals. The rule must specify exactly which import paths are allowed and which are blocked, using DO/DO NOT format. Score on whether the rule is enforceable by grep (can a hook verify compliance?) or is merely advisory.
