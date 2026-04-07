---
atom_id: SC-P-017
name: Detect false-positive learning pipelines that report healthy metrics but produce noise
craft_layer: practitioner
destination: forge
craft_areas: [attend-how, attend-when]
applications: [COC]
spec_lineage:
  - CO §39 — Layer 5 (Learning)
  - CO §3.3 — Safety Blindness (Failure Mode)
journal_evidence:
  - path: loom/journal/0004-RISK-learning-system-broken.md
    polarity: NEGATIVE
    quote: "- 4,579 observations in kailash-py, 99% are infrastructure noise (session start/stop)\n- 8 instincts generated, all \"workflow_builder pattern\" with hardcoded 0.9 confidence"
practice_modality: case
status: verified
---

## The move

Inspect the output end of a learning pipeline, not just the input end. A pipeline that captures thousands of observations and reports healthy collection metrics can still produce zero useful knowledge. The diagnostic is to read the pipeline's actual outputs (instincts, evolved rules, promoted patterns) and check three things: (a) are the outputs distinct from each other or near-duplicates? (b) do the confidence scores vary or are they hardcoded to a single value? (c) has any output ever been promoted to institutional knowledge, or is the evolved directory empty? If the answers are "duplicates, hardcoded, empty," the pipeline is structurally complete and functionally dead.

## When it fires

Two trigger conditions:

1. A Layer 5 (Learning) system reports impressive collection numbers (thousands of observations captured) but no one can point to a single institutional-knowledge artifact that originated from it. The signal is the gap between volume metrics and knowledge outcomes.
2. A `/codify` or `/evolve` command runs without error but the resulting artifacts are duplicates of existing content or contain hardcoded confidence values. The signal is that the pipeline's processing stage is either a passthrough or a stub.

## What it serves

CO §39 (Layer 5 — Learning) requires that captured patterns "MUST be reviewed and approved by human before formalization" and that implementations "SHOULD demonstrate measurable knowledge growth over time." A pipeline that generates 8 duplicate instincts at hardcoded 0.9 confidence satisfies neither requirement: there is nothing worth reviewing (duplicates), and knowledge is not growing (same output regardless of input). CO §3.3 (Safety Blindness) applies because the pipeline's healthy input metrics create a false sense that learning is operational, blinding the practitioner to the fact that the feedback loop from observation to knowledge is broken. The blindness is structural: the metrics that are easy to measure (observation count) are not the metrics that matter (knowledge growth).

## Evidence walkthrough

- **0004-RISK-learning-system-broken** (NEGATIVE) — The L5 learning system across all 4 repos was structurally complete (4 commands, 4 lib files, auto-processing in session-end.js) but functionally broken. The kailash-py repo alone had 4,579 observations, 99% of which were infrastructure noise (session start/stop events). The pipeline processed these into 8 instincts, all of which were "workflow_builder pattern" with hardcoded 0.9 confidence — identical outputs regardless of input variation. The evolved directory was empty in all repos, meaning zero observations had ever been promoted to institutional knowledge. The `/codify` command never read from the learning system, completing the broken loop. Every component worked in isolation; the pipeline as a whole produced nothing.

## How to drill it

Give the practitioner a learning pipeline with: 5,000 logged observations, a processing script that generates instinct files, and a `/codify` command that reads from those instinct files. The observation logs contain 4,800 session-start/stop events and 200 genuine decision-point events. The processing script outputs 6 instinct files, all with confidence 0.9 and near-identical descriptions. The practitioner must: (1) identify the input-quality problem (99% noise), (2) identify the processing-quality problem (hardcoded confidence, duplicate output), (3) identify the promotion-quality problem (empty evolved directory), and (4) propose a fix that addresses all three stages. The drill fails if the practitioner proposes only "capture better observations" without checking the processing and promotion stages.

## Common failure mode

The practitioner inspects only the observation-capture stage and concludes that the fix is better event targeting (log decision points instead of session events). This is necessary but insufficient. The journal entry reveals that even after fixing observation quality, the processing script would still produce hardcoded 0.9 confidence scores and duplicate instincts — because the script itself uses regex-based semantic analysis that cannot distinguish between genuinely different patterns. The practitioner who fixes input without auditing processing will produce a pipeline that captures better observations and still outputs the same 8 duplicates.

## Related atoms

- SC-P-008 (dependency chain fragility) — a broken learning pipeline is a dependency chain where the weakest link (processing quality) determines the value of the entire chain
- SC-A-001 (codify-with-explicit-NOT-codified) — the explicit gap: "/codify never reads from the learning system" is the kind of NOT-codified fact that SC-A-001 teaches practitioners to surface
- SC-A-003 (defense-in-depth codification) — a learning pipeline with only one processing stage is a single point of failure; defense-in-depth would add independent validation between observation capture and instinct generation.
- SC-P-011 (audit categorization progressive disclosure) — both atoms teach the practitioner to distrust structurally plausible metrics; healthy collection numbers hide a broken pipeline just as high coverage numbers hide a structurally blind audit method
