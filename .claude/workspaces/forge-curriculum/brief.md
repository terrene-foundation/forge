# FORGE Skill Library — Curriculum Workspace Brief

## What FORGE is

**FORGE is the canonical, contributor-neutral skill library for the CO ecosystem.** Not a course. Not a module-based curriculum. Not a dual-track programme. The library that downstream role-tailored courses in `lyceum/courses/` and the methodology repo at `atelier/co-codegen/` pull from.

Authority: the programme owner (Terrene Foundation) declares what counts as a standard and what is in scope. The filesystem reflects current state, not authority.

## Four standards as first principles

FORGE teaches practitioner craft moored in the four Terrene Foundation standards:

- **CARE** (philosophy) — Dual Plane Model, Mirror Thesis, Human-on-the-Loop, Constraint Envelopes
- **EATP** (protocol) — Trust Lineage Chain, posture/operations, interoperability, conformance
- **CO** (methodology) — 8 first principles, 5-layer architecture, process model, application template
- **PACT** (governance) — D/T/R accountability grammar, operating envelopes, knowledge clearance, verification gradient

All four are peers. COC is the codegen application of CO, not a separate standard.

## What's in scope

All seven formalised CO applications are in scope: **codegen, compliance, education, finance, governance, learners, research**. Build order is usage-weighted — COC (heaviest) first, then COR, then COE, then the rest.

**Out of scope**: Aegis (commercial implementation, reserved for downstream courses), Aether (DataFlow fabric interface, deferred), any commercial product references, any module-based course framing.

## What's produced

The workspace produces **atom-based library content**, organised by:

1. **Responsibility + accountability** — what the practitioner owes upward (to specs, to truth, to humans above the loop) and downward (to artifacts, to operators downstream)
2. **Skillset** — the moves the practitioner can actually execute (drilled, not just understood)
3. **Spec lineage** — which CARE/EATP/CO/PACT principles each atom serves
4. **Application** — which CO applications the atom is evidence for (COC / COR / COE / etc.)
5. **Practice modality** — drill / case / observation / build / brokerage-rep / governance-call

Current output (analysis phase complete):

- **57 skill atoms** in 3 craft layers (18 brokerage + 15 artifact + 24 practitioner)
- **232 journal entries** indexed as evidence corpus
- **417 spec concepts** enumerated across the four standards
- **D1-D26 decision log** capturing accumulated framing decisions

## Dual-destination routing

Every atom is tagged with a destination. Post-M1 red team final distribution:

- `forge` — uniquely FORGE practitioner pedagogy (21 atoms)
- `co-codegen` — agnostic methodology for any CO ecosystem consumer (23 atoms)
- `both` — canonical in co-codegen, referenced from FORGE (13 atoms)

Craft layers and destinations do not map 1:1. The 24 Layer 1 (practitioner) atoms split across destinations: 21 are `forge`, three (SC-P-013, SC-P-015, SC-P-019) are `both`. The 13 `both` atoms each have a co-codegen variant in `catalog/co-codegen-variants/` with drill → "How to apply" transformation.

## Production deliverables

M1-M5 (in `todos/active/`) are all complete as of 2026-04-08:

```
lyceum/programs/forge/
├── CLAUDE.md                (updated with catalog reference)
├── catalog/                 (see catalog/README.md for the full substructure)
│   ├── brokerage/           18 atoms (operator + governance craft)
│   ├── artifact/            15 atoms (CC artifact authoring craft)
│   ├── practitioner/        24 atoms (session-level moves)
│   ├── co-codegen-variants/ 13 alternate framings for `both` atoms
│   ├── README.md            5 index views + craft-area balance audit
│   ├── spec-coverage.md     57 atoms → 417 spec concepts (30% coverage)
│   ├── teaching-sequences.md  5 workflow clusters
│   ├── routing-manifest.yaml  dual-destination manifest (authoritative)
│   ├── co-codegen-staging.md  36 atoms queued for next sync
│   ├── upstream-flags.md      stale upstream debt (don't edit here)
│   └── future-passes.md       COR/COE/compliance/finance/governance/learners queue
├── drills/                  57 drill specs + 10 full exemplars + 4-view README
└── cases/                   10 exemplar cases + index + case-candidates
```

Methodology extractions in `catalog/co-codegen-staging.md` + `catalog/co-codegen-variants/` are ready for routing to `atelier/co-codegen/` on the next sync pass.

## Reference

- **Decision log** (read first): `01-analysis/02-synthesis/02-decision-log.md`
- **Three-layer craft decomposition**: `01-analysis/02-synthesis/01-three-layer-craft-decomposition.md`
- **Production catalog**: `../../../../catalog/README.md`
- **Rules**: `../../../rules/forge-scope.md`, `rules/specs-first.md`, `rules/dual-track.md`

## Previous brief is superseded

This brief replaces an earlier version that described FORGE as "12 modules (4 foundation + 4 engineer + 4 consultant)" with "F-track / E-track / C-track" structure. That framing was rejected in decision log D2: **FORGE is a library, not a course. Role-tailored sequences live downstream.** Any content in this workspace referring to F/E/C tracks, foundation/specialist modules, or "12 modules" is pre-D2 debt and should be ignored.
