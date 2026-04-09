# FORGE — Foundation for Orchestration, Reasoning, and Generative Engineering

A Terrene Open Academy programme. 67 skill atoms. 57 drills. 15 teaching cases. The canonical practitioner library for the CO ecosystem.

FORGE distils the four Terrene Foundation standards — **CARE** (philosophy), **EATP** (protocol), **CO** (methodology), **PACT** (governance) — into concrete, drillable skills grounded in 232 journal entries of real practitioner craft. It is a library, not a course. Downstream courses in `lyceum/courses/` pull atoms from here and sequence them for specific audiences.

## What's in the Box

| Content                 | Count | Description                                                               |
| ----------------------- | ----- | ------------------------------------------------------------------------- |
| **Skill atoms**         | 67    | Minimum teachable units of practitioner, artifact, and brokerage craft    |
| **Drill specs**         | 57    | Structured practice exercises, one per COC-layer atom                     |
| **Drill exemplars**     | 15    | Fully expanded drills with setup, task, model answer, scoring, extensions |
| **Teaching cases**      | 15    | Journal-sourced narrative cases with spec-mooring overlay                 |
| **Case candidates**     | 25    | Ranked backlog for future case expansion (10 remaining)                   |
| **Co-codegen variants** | 13    | Methodology atoms adapted for the broader CO ecosystem                    |

## Four Standards, One Quartet

FORGE treats all four Foundation standards as peers. Every skill atom cites spec lineage from one or more of them.

| Standard | What it is                                                                    | Role in FORGE                   |
| -------- | ----------------------------------------------------------------------------- | ------------------------------- |
| **CARE** | Philosophy — Dual Plane Model, Mirror Thesis, Constraint Envelopes            | Why we build the way we build   |
| **EATP** | Protocol — Trust Lineage, posture management, interoperability                | How trust flows through systems |
| **CO**   | Methodology — 5-layer architecture, 8 principles, process model               | How we orchestrate work         |
| **PACT** | Governance — D/T/R accountability, operating envelopes, verification gradient | How we govern what we build     |

## Three Craft Layers

| Layer            | Count    | What it covers                                                                                        |
| ---------------- | -------- | ----------------------------------------------------------------------------------------------------- |
| **Brokerage**    | 18 atoms | Operator and governance moves: sync, authority chains, variant classification, cross-SDK coordination |
| **Artifact**     | 15 atoms | What makes a Claude Code artifact work: rules, hooks, defense-in-depth, progressive disclosure        |
| **Practitioner** | 34 atoms | Session-level moves: attention, intervention, institutional memory, research craft                    |

## Seven CO Applications

CO is a domain-agnostic methodology. FORGE covers all seven formalised applications, built in usage-weight order:

| Application                | Atoms                    | Status                                               |
| -------------------------- | ------------------------ | ---------------------------------------------------- |
| **COC** (CO for Codegen)   | 57                       | Complete — primary layer, 232 journal entries        |
| **COR** (CO for Research)  | 10 new + 18 cross-tagged | Complete — initial pass                              |
| **COE** (CO for Education) | 22 cross-tagged          | In scope — coordinated from `lyceum/courses/rr-coe/` |
| Compliance                 | --                       | Queued                                               |
| Finance                    | --                       | Queued                                               |
| Governance                 | --                       | Queued                                               |
| Learners                   | --                       | Queued                                               |

## What is a Skill Atom?

A skill atom is the minimum teachable unit of practitioner craft. Each one has:

- **A name** — imperative, describing what the practitioner _does_
- **Spec lineage** — which CARE/EATP/CO/PACT concepts it derives from
- **Journal evidence** — verbatim quotes from real craft journals, with polarity tags (POSITIVE/NEGATIVE/MIXED)
- **A drill** — a concrete exercise that makes the atom teachable
- **A failure mode** — the most likely way practitioners skip or fake the move
- **Related atoms** — how this atom composes with others

Example: **SC-B-005 Spec-to-code conformance audit** maps 8 normative spec statements to source code, classifying each as CONFIRMED / GAP / PARTIAL. Spec lineage: CO §49, PACT §40. Evidence: 4 journal entries from PACT module audits. Failure mode: auditing only what tests already cover.

## Repository Structure

```
forge/
  catalog/               67 skill atoms (brokerage/, artifact/, practitioner/)
    README.md            Production index with 5 views
    routing-manifest.yaml    Dual-destination routing (forge / co-codegen / both)
    co-codegen-variants/     13 variants for atelier/co-codegen/
    co-codegen-staging.md    Sync staging manifest
    spec-coverage.md         Gap report: 126/417 concepts covered
    future-passes.md         Application pass queue (COR done, COE moved, 4 queued)
    upstream-flags.md        Template-synced artifacts carrying stale language
  drills/
    specs/               57 drill spec files
    exemplars/           15 fully expanded exemplar drills
  cases/
    exemplars/           15 teaching cases (6-section format)
    case-candidates.md   25 ranked candidates for expansion
  README.md              This file
  CHANGELOG.md           Release history
  LICENSE                Apache 2.0
  pyproject.toml         Project configuration
```

## For Course Designers

FORGE is a library. Courses consume it. The pattern:

1. **Select atoms** by craft area, application, spec lineage, or modality from `catalog/README.md`
2. **Sequence them** into your course structure (FORGE imposes no sequence)
3. **Use drills** from `drills/specs/` or expand exemplars from `drills/exemplars/`
4. **Use cases** from `cases/exemplars/` for narrative teaching
5. **Propose new atoms** when you discover gaps — they go into FORGE first, then your course inherits them

Atoms tagged `destination: both` also appear in `atelier/co-codegen/` for the broader CO ecosystem.

## For CO Ecosystem Contributors

Atoms tagged `destination: co-codegen` or `destination: both` are routed to `atelier/co-codegen/` via the routing manifest. This is the contributor-neutral methodology repo. If you are building CO applications outside FORGE, you consume the co-codegen atoms directly.

## Kailash Platform

All technical content uses the Kailash SDK framework family. FORGE never teaches raw sklearn/pandas/PyTorch without the Kailash wrapper.

| Framework       | Purpose                                          |
| --------------- | ------------------------------------------------ |
| Core SDK        | Workflow orchestration, 140+ nodes               |
| DataFlow        | Zero-config database operations                  |
| Nexus           | Multi-channel deployment (API + CLI + MCP)       |
| Kaizen          | AI agent framework                               |
| PACT primitives | D/T/R types, envelope decorators, clearance APIs |
| ML              | ML lifecycle (13 engines, polars-native)         |
| Align           | LLM fine-tuning and serving                      |

## Getting Started

```bash
uv venv && uv sync
```

## License

Apache 2.0 — [Terrene Foundation](https://terrene.foundation) (Singapore CLG)

Specifications (CARE, EATP, CO, PACT) are licensed under CC BY 4.0.
