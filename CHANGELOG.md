# Changelog

All notable changes to FORGE are documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.4.0] - 2026-04-09

### Added

- 10 COR drill exemplars: SC-P-025 (tier-ranked literature search), SC-P-026 (citation integrity audit),
  SC-P-027 (hostile reviewer simulation), SC-P-028 (multi-perspective synthesis),
  SC-P-029 (post-publication gap check), SC-P-030 (academic register calibration),
  SC-P-031 (venue strategy as constraint envelope), SC-P-032 (margin note as deliberation),
  SC-P-033 (reflexivity diagnosis), SC-P-034 (overclaim prevention). Total exemplars: 15 -> 25
- 10 new teaching cases: CASE-16 (silent drift divergence), CASE-17 (layer5 not aspirational),
  CASE-18 (vacancy asymmetric enforcement), CASE-19 (orphans stales misclassified),
  CASE-20 (adjacent disconnected), CASE-21 (orphan audit wrong), CASE-22 (decision reversal),
  CASE-23 (audited side ahead), CASE-24 (146kb rules tokens), CASE-25 (timing sidechannel).
  Total cases: 15 -> 25. All 25 candidates now expanded.

### Fixed

- Upstream "trinity" language in terrene-naming.md and behavioral-guidelines.md updated to "quartet"
  at loom/ source; forge local copy synced manually. upstream-flags.md Flag 1 resolved.

## [0.3.0] - 2026-04-09

### Added

- COE application pass coordination moved to `lyceum/courses/rr-coe/`
- Release packaging: LICENSE (Apache 2.0), CHANGELOG, public README
- 5 new drill exemplars: SC-B-006 (red team convergence), SC-B-008 (convergence-as-validation),
  SC-B-017 (verification gradient zones), SC-P-002 (model-vs-rule ablation),
  SC-P-011 (audit categorization blindness). Total: 10 -> 15
- 5 new teaching cases: CASE-11 (spec conformance gaps), CASE-12 (human approves agent performs),
  CASE-13 (pub fields as security holes), CASE-14 (deny bypass), CASE-15 (rule plus hook).
  Total: 10 -> 15

### Changed

- `catalog/future-passes.md` Pass 3 (COE) now redirects to rr-coe for coordination

## [0.2.0] - 2026-04-08

### Added

- **COR application pass**: 10 new research-craft atoms (SC-P-025 through SC-P-034)
  - Tier-ranked literature search, citation integrity audit, hostile reviewer simulation,
    multi-perspective synthesis, post-publication gap check, academic register calibration,
    venue strategy as constraint envelope, margin note as deliberation, reflexivity diagnosis,
    overclaim prevention
- COR workspace at `.claude/workspaces/forge-cor/` with spec index, corpus scan, reverse index
- COR red team rounds 1-3 converged clean
- 4 COR atoms also tagged COE (SC-P-025, SC-P-026, SC-P-032, SC-P-034)

### Changed

- Catalog total: 57 -> 67 atoms (18 brokerage + 15 artifact + 34 practitioner)

## [0.1.0] - 2026-04-07

### Added

- **57-atom COC skill catalog** across three craft layers:
  - 18 brokerage atoms (operator + governance moves)
  - 15 artifact atoms (CC artifact authoring craft)
  - 24 practitioner atoms (session-level moves)
- **57 drill specs** with structured practice format (one per atom)
- **10 fully expanded drill exemplars** with setup, task, model answer, scoring, extensions
- **10 teaching cases** from DISCOVERY/RISK journal entries with 6-section format
- **25 case candidates** identified and ranked for future expansion
- **Routing manifest** for dual-destination delivery (36 atoms to co-codegen)
- **13 co-codegen variants** for `both`-tagged atoms
- **Spec coverage report** documenting 126/417 concepts covered with gap justification
- **5 index views** in catalog/README.md (layer, craft area, spec lineage, modality, application)
- Red team rounds 2-4 converged clean (quote-verifier, overlap-detector, framing-compliance)
- Analysis workspace with decision log (D1-D27), spec index (417 concepts), reverse index (232 entries)

### Foundation

- Four standards quartet: CARE (philosophy), EATP (protocol), CO (methodology), PACT (governance)
- Evidence base: 232 COC journal entries from loom/ + terrene/
- Observation corpus: ~8,181 events across 38 files (~893 high-signal)
- Seven CO applications declared in scope (COC, COR, COE, compliance, finance, governance, learners)
