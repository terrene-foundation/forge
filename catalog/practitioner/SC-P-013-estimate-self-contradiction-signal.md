---
atom_id: SC-P-013
name: Treat internal estimate inconsistency as a high-confidence signal that at least one estimate is wrong
craft_layer: practitioner
destination: both
craft_areas: [attend-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §28 — Approval Gate
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: NEGATIVE
    quote: "The contradiction is stark: the detailed refactor plan says 7 sessions for the full scope; the workspace plan says 2-3 sessions."
  - path: loom/journal/0035-DECISION-redteam-round1-resolutions.md
    polarity: POSITIVE
    quote: "The Nexus deep analysis estimates 7 sessions total; dependency plan compressed to 2-3. Both can't be right. Splitting eliminates the single-point-of-failure: B0a unblocks EventBus and new features immediately."
  - path: loom/journal/0036-RISK-kailash-rl-redteam-round2.md
    polarity: POSITIVE
    quote: "This is 3-6x the original 1-2 session estimate from the dependency analysis, and 1.5-2x the Round 1 revised estimate of 4-6 sessions. The increase comes from: (a) serving integration was not previously scoped, (b) the on-prem workflow was not previously scoped, (c) the AdapterRegistry requires non-trivial design (LoRA adapters are not the same artifact shape as sklearn models)."
practice_modality: drill
status: verified
observation_search: "no corroborating high-signal observation event found as of 2026-04-08; observation telemetry captures code-execution patterns (workflow_pattern, error_occurrence, error_fix, connection_pattern), not prompt-level or review-time practitioner moves"
---

## The move

When a single analysis process produces two effort estimates that disagree by more than 2x, stop and investigate the source of the divergence before proceeding. Do not average the two numbers. Do not pick the optimistic one. Do not pick the pessimistic one "to be safe." The disagreement itself is the finding: the analysis process is internally inconsistent, which means at least one of its outputs was produced without full consideration of the inputs that informed the other. Trace the divergence to its root: which estimate incorporated more detail? Which estimate compressed or omitted information? The answer tells you which estimate to distrust and what the analysis process missed.

## When it fires

An approval gate reviews a plan that contains effort estimates. The plan was produced by a single analytical process (one agent, one session, one workspace). Two sections of the plan give different numbers for the same scope. The trigger is the numerical inconsistency within a single document or set of documents produced by the same process — not disagreement between two independent estimators (which is expected and healthy).

## What it serves

CO §28 (Approval Gate) defines "a structured point where human judgment is exercised before the workflow proceeds to the next phase." An approval gate that passes a plan with internally inconsistent estimates is failing at its core function: the human cannot exercise judgment on a number that the plan itself cannot agree on. The self-contradiction is a quality signal that the gate should catch. If the plan's own analysis produces 7 sessions in one section and 2-3 sessions in another for the same work, the gate must reject the plan until the estimates are reconciled — not choose one and proceed.

## Evidence walkthrough

- **0034-RISK-stack-gaps-redteam-attack** (NEGATIVE) — The red team report identified that a 17-workspace dependency analysis allocated the Nexus transport refactor (B0) 2-3 sessions in the workspace plan, while the same analytical process's detailed refactor analysis estimated 7 sessions across 11 internal phases for the same scope. The red team called this "CRITICAL" severity. The entry traced the root cause: "the deep analysis was produced by the same analytical process; the dependency analysis compressed it without justification." The compression happened silently — the workspace plan did not acknowledge or explain the difference from the detailed analysis. The red team's recommendation was to reestimate at 4-6 sessions and split B0 into independently gated sub-deliverables. The broader finding was that B0 sat on the critical path and a 2-session slip would cascade across the entire 8-item dependency chain.
- **0035-DECISION-redteam-round1-resolutions** (POSITIVE) — The decision entry that resolved the contradiction surfaced in 0034 demonstrates the correct response. Rather than averaging the two estimates or picking either one, the practitioner named the contradiction directly ("Both can't be right") and treated it as a structural signal that the work itself was wrongly scoped. The resolution was to split B0 into B0a (HandlerRegistry + EventBus, 1 session) and B0b (HTTPTransport + MCPTransport, 2-3 sessions), then schedule them against the dependency chain independently. The split is the move: instead of forcing the contradicting estimates into one number, the practitioner subdivides the work until both estimates can be true of different sub-deliverables. This is the diagnostic-not-noise response in its mature form.
- **0036-RISK-kailash-rl-redteam-round2** (POSITIVE) — A second, independent occurrence of the same pattern: a Round 2 red team caught that the kailash-rl scope had been estimated three times by the same analytical process and produced three contradictory numbers (1-2 sessions in the original dependency analysis, 4-6 sessions after Round 1 revision, 7-12 sessions after Round 2 deep analysis — a 3-6x spread). Each re-estimate happened because the previous one had compressed away non-trivial scope items (serving integration, on-prem workflow, AdapterRegistry design). The response was again structural: the package was split into kailash-ml[rl] (1-2 sessions, classical RL only) and kailash-align (7-12 sessions, the LLM alignment scope that had been silently absorbed). Two independent occurrences four entries apart confirms this is a recurring failure mode of single-process estimation, not a one-off accident.

## How to drill it

Give the practitioner a plan document (1-2 pages) that contains two effort estimates for the same deliverable, embedded in different sections. The estimates disagree by 2-4x. The plan reads smoothly — the inconsistency is not flagged or discussed. The practitioner must:

1. Identify the two inconsistent estimates and state the disagreement ratio.
2. Trace the inconsistency to a root cause. Which section incorporated more detail? Which section compressed or abstracted away complexity?
3. State which estimate they distrust and why (not "the higher one" or "the lower one" — the reasoning must reference what each estimate accounted for that the other did not).
4. Write a one-sentence gate rejection: "This plan cannot proceed because [specific inconsistency] indicates [specific analytical failure]."

Variations:

- Easy: the two estimates are in adjacent paragraphs and the disagreement is 5x.
- Medium: the estimates are in different sections (executive summary vs detailed breakdown) and the disagreement is 3x.
- Hard: the estimates are expressed in different units (sessions vs days) and the disagreement is hidden behind a conversion factor that the plan never states.

Score on whether the practitioner catches the inconsistency unprompted, traces it to the compression step, and writes a rejection that names the analytical failure rather than just picking a number.

## Common failure mode

The practitioner averages the two estimates ("the truth is probably somewhere in the middle") or picks the pessimistic one ("better to overestimate than underestimate"). Both responses treat the inconsistency as noise rather than signal. Averaging two estimates from the same process does not produce a better estimate — it produces a number that neither analysis supports. Picking the pessimistic one is marginally safer but still fails to diagnose why the process contradicted itself. The correct response is to reject the plan and demand reconciliation, because an analysis that cannot produce internally consistent outputs has a flaw that affects all its outputs, not just the one with the visible inconsistency.

## Related atoms

- SC-B-006 (multi-round red team to numeric convergence) — the red team that caught the estimate inconsistency in 0034 was operating within a multi-round convergence loop; the self-contradiction was a finding from the first round.
- SC-P-004 (per-file disposition labelling) — both atoms require the practitioner to examine each item individually rather than accepting a batch characterization; here the "batch" is the summary estimate that glosses over the detailed analysis.
- SC-P-009 (architecture doc as contract) — architecture documents that contain internally inconsistent claims are a source of estimate contradictions; treating docs as testable contracts catches the inconsistency earlier.
- SC-P-015 (ask for contradictions) — the falsification question "what would invalidate this?" is the prompt-level move that surfaces estimate contradictions before they propagate into plans.
