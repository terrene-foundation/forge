# FORGE — Foundation for Orchestration, Reasoning, and Generative Engineering

Official Terrene Foundation programme teaching the four foundation standards (**CARE / EATP / CO / PACT**) and their applications as practitioner skillsets distilled from the journal corpus. FORGE is the canonical, contributor-neutral library for the CO ecosystem. Role-tailored client courses live downstream in `lyceum/courses/`.

**Authority:** The programme owner declares what counts as a standard and what is in scope. The filesystem reflects the state of graduation, not the source of truth about peer status. FORGE artifacts must treat programme-owner declarations as authoritative over filesystem state when they conflict.

## Absolute Directives

### 0. Foundation Independence

Kailash Python SDK is a Terrene Foundation project (Singapore CLG). No commercial references. See `rules/independence.md`. Aegis is **not** in FORGE scope (commercial implementation, reserved for downstream client courses). The open-sourced reference implementation of the PACT standard is the **PACT platform**. Aether is the DataFlow data fabric interface and is out of FORGE scope until the courses phase.

### 1. Four Standards as First Principles

**CARE** (philosophy), **EATP** (protocol), **CO** (methodology), **PACT** (governance) are a **quartet**, not a trinity. All four are peers. Every skill atom cites spec lineage from one or more of them with equal authority. See `rules/forge-scope.md` § 1.

### 2. Specs as Index, Craft as Substance

Standards name the _space_ of skills. The **journal corpus** supplies the _substance_: **232 CO/COC practitioner craft entries** across `loom/` (50 top-level meta journal + 85 kailash-py workspaces + 13 kailash-py ADRs + 41 kailash-rs + 26 kaizen-cli-rs + 3 kaizen-cli-py + 12 kz-engage) and `terrene/` (2 journal), verified by read-and-classify pass on 2026-04-07. Plus ~66 academic/thesis entries in `terrene/foundation/workspaces/care-thesis/` and `publications/`, and ~29 Foundation contrib/policy entries. Both the index and the substance are required. See `rules/specs-first.md`.

### 3. CO Applications — All In Scope, Usage-Weighted

CO is the base methodology with multiple domain applications. All are in scope. Build order follows usage weight:
**COC** (codegen, heaviest) → **COR** (research) → **COE** (education) → compliance / finance / governance / learners. No "v1 = COC only" gate. See `rules/forge-scope.md` § 2.

### 4. Framework-First

All technical content uses Kailash frameworks. Never raw sklearn/pandas/PyTorch without the Kailash wrapper. See `rules/framework-first.md`.

### 5. Contributor-Neutral Library, Not Tracks

FORGE is organised by **responsibilities + accountabilities + skillsets + spec lineage + application + practice modality** — not by role names. Role-tailored sequences (AI Engineer, AI Business Consultant, future others) live downstream in `lyceum/courses/`. Different clients mix and match. See `rules/dual-track.md`.

### 6. Dual-Destination Routing

Every output is tagged `forge` / `co-codegen` / `both`. Practitioner pedagogy → `forge`. Artifact-authoring craft + brokerage/governance craft → `co-codegen` or `both`. Distill once, route both ways. See `rules/forge-scope.md` § 4.

### 7. Rigor Bar

FORGE artifacts sit alongside CARE / EATP / CO / PACT in the Foundation's public presentation. Enumerate spec concepts concretely (not summarise). Cite every craft claim with a verifiable path. Read entries before citing them — no synthesising from memory. Name gaps as gaps. See `rules/forge-scope.md` § 5.

## Spec Sources (Authoritative Paths)

| Standard              | Canonical location                                                                                                                           |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| CARE                  | `terrene/foundation/docs/02-standards/care/` (7 subsections, 37 files)                                                                       |
| EATP                  | `terrene/foundation/docs/02-standards/eatp/` (7 files, 01-first-principles → 07-conformance)                                                 |
| CO                    | `terrene/foundation/docs/02-standards/co/` (4 core + `applications/` + `workflows/`)                                                         |
| PACT                  | `terrene/foundation/docs/02-standards/publications/PACT-Core-Thesis.md` + `terrene/foundation/workspaces/pact/` (treated as fourth standard) |
| COC (application)     | `terrene/foundation/docs/02-standards/publications/COC-Core-Thesis.md` (documented as thesis; the first and most developed CO application)   |
| COR (application)     | `terrene/foundation/docs/02-standards/co/applications/co-for-research.md`                                                                    |
| COE (application)     | `terrene/foundation/docs/02-standards/co/applications/co-for-education.md`                                                                   |
| Other CO applications | `terrene/foundation/docs/02-standards/co/applications/co-for-{compliance,finance,governance,learners}.md`                                    |

## Journal Corpus (Authoritative Locations)

| Source                                                         | Count | Nature                                                        |
| -------------------------------------------------------------- | ----- | ------------------------------------------------------------- |
| `loom/journal/`                                                | 50    | Top-level CO/COC meta journal                                 |
| `loom/kailash-py/workspaces/`                                  | 85    | Python SDK craft journals (+13 ADRs under `docs/adr/`)        |
| `loom/kailash-rs/workspaces/`                                  | 41    | Rust SDK craft journals                                       |
| `loom/kaizen-cli-rs/workspaces/`                               | 26    | Kaizen CLI (Rust) craft                                       |
| `loom/kaizen-cli-py/workspaces/`                               | 3     | Kaizen CLI (Python) craft                                     |
| `loom/kz-engage/workspaces/`                                   | 12    | kz-engage craft                                               |
| `terrene/journal/`                                             | 2     | Top-level terrene journal                                     |
| `terrene/claude-squad/`                                        | 3     | Squad coordination journal                                    |
| `terrene/foundation/workspaces/care-thesis/` + `publications/` | 66    | Academic/thesis entries (CARE theory, not practitioner craft) |
| `terrene/contrib/` + `terrene/policy/`                         | 29    | Foundation contrib + policy                                   |

**Not sources** (either empty or out of scope): `atelier/*` (zero journal entries), `dev/*` (only aegis has 5, and aegis is out of scope per §0), and anything under `hmi/`, `tpc/`, `projects/`, `rr/`, `research/`.

## Kailash Platform (In-Scope Frameworks)

| Framework       | Purpose                                             | Install                        |
| --------------- | --------------------------------------------------- | ------------------------------ |
| Core SDK        | Workflow orchestration, 140+ nodes                  | `pip install kailash`          |
| DataFlow        | Zero-config database operations                     | `pip install kailash-dataflow` |
| Nexus           | Multi-channel deployment (API+CLI+MCP)              | `pip install kailash-nexus`    |
| Kaizen          | AI agent framework                                  | `pip install kailash-kaizen`   |
| PACT primitives | D/T/R types, envelope decorators, clearance APIs    | `pip install kailash-pact`     |
| ML              | ML lifecycle (13 engines, polars)                   | `pip install kailash-ml`       |
| Align           | LLM fine-tuning & serving                           | `pip install kailash-align`    |
| MCP platform    | FastMCP server, contributor plugins, security tiers | (in Core SDK + extensions)     |

## Workspace Commands

| Command      | Purpose                                    |
| ------------ | ------------------------------------------ |
| `/analyze`   | Load analysis phase for current workspace  |
| `/todos`     | Load todos phase; stops for human approval |
| `/implement` | Load implementation phase                  |
| `/redteam`   | Load validation phase                      |
| `/codify`    | Update agents & skills with new knowledge  |
| `/ws`        | Read-only workspace status dashboard       |
| `/wrapup`    | Write session notes before ending          |

## FORGE Library Structure

The production FORGE library lives at the program root. Analysis artifacts live under `.claude/workspaces/forge-curriculum/`.

| Path                                   | Contents                                                                                                                                                                                                                                                                                          |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `catalog/`                             | **67 skill atoms** (18 brokerage + 15 artifact + 34 practitioner). The canonical, contributor-neutral library. Each atom has spec mooring, journal evidence, drill, and failure mode. Downstream courses and atelier/co-codegen pull from here.                                                   |
| `catalog/README.md`                    | Production index with 5 views (layer, craft area, spec lineage, modality, application)                                                                                                                                                                                                            |
| `drills/`                              | **67 drill specs + 67 fully expanded exemplars**. `specs/` holds one structured drill spec per atom (57 COC + 10 COR). `exemplars/` holds 67 full-form drills with setup, task, model answer, scoring, and extensions. `README.md` indexes by craft area, modality, difficulty, and spec lineage. |
| `cases/`                               | **25 exemplar teaching cases** lifted from DISCOVERY/RISK journal entries with spec-mooring overlay. Each case: situation → trigger → move → outcome → spec connection → discussion questions. All 25 candidates from `case-candidates.md` now expanded.                                          |
| `teaching/`                            | **Reusable teaching scaffolding** for downstream course designers. 67 learning outcomes, prerequisite graph (33 hard + ~190 soft edges, 7 topological layers), 67 mastery rubrics (novice → advanced), 8 themed atom clusters, 5 practice modality guides. See `teaching/README.md`.              |
| `.claude/workspaces/forge-curriculum/` | Analysis workspace — decision log D1-D26+, spec index (417 concepts), reverse index (232 entries), 7 pre-rigor-bar specialist reports, M1-M5 todo milestones, red team round 2 findings                                                                                                           |

## Rules Index

| Concern                                                                    | Rule File                       |
| -------------------------------------------------------------------------- | ------------------------------- |
| Foundation independence (no commercial references)                         | `rules/independence.md`         |
| FORGE scope, four standards, applications, PACT layers, routing, rigor bar | `rules/forge-scope.md`          |
| Specs as index, craft as substance                                         | `rules/specs-first.md`          |
| Library-not-tracks organisation                                            | `rules/dual-track.md`           |
| Framework-first technical content                                          | `rules/framework-first.md`      |
| CC artifact quality (descriptions, sizes, progressive disclosure)          | `rules/cc-artifacts.md`         |
| Canonical terminology                                                      | `rules/terrene-naming.md`       |
| Communication style (plain language by default)                            | `rules/communication.md`        |
| Autonomous execution model (no human-team framing)                         | `rules/autonomous-execution.md` |
| Agent orchestration & specialist delegation                                | `rules/agents.md`               |
| Git workflow                                                               | `rules/git.md`                  |
| Security                                                                   | `rules/security.md`             |
| Zero-tolerance (no stubs, no fallbacks, no workarounds)                    | `rules/zero-tolerance.md`       |

## Current Analysis State

Active workspace: `forge-curriculum` (`.claude/workspaces/forge-curriculum/`). **Read `01-analysis/02-synthesis/02-decision-log.md` first** when continuing this work — it carries the accumulated framing decisions (D1–D26+). The 7 specialist reports in `01-analysis/01-research/` are evidence-base drafts written before the rigor bar was set and **must be re-verified** before being used as curriculum source.
