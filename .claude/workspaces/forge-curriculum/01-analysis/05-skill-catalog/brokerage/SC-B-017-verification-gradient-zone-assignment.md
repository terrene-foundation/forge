---
atom_id: SC-B-017
name: Assign verification-gradient zone explicitly before delegating to an agent
craft_layer: brokerage
destination: co-codegen
craft_areas: [intervene-when, institutional-memory]
applications: [COC, COR, COE]
spec_lineage:
  - PACT §19 — Verification Gradient (4-Zone)
  - PACT §11 — Role Envelope (Standing)
journal_evidence:
  - path: loom/journal/0021-DECISION-autonomous-gate1-classification.md
    polarity: POSITIVE
    quote: "Five sync-reviewer agents processed 79 files in ~2 minutes with unanimous agreement (0 global, 64 variant:rs, 15 skip). Human approved the batch."
  - path: loom/journal/0025-DECISION-parallel-backlog-resolution.md
    polarity: POSITIVE
    quote: "Used a team of 5+ parallel agents to resolve ALL outstanding backlog items from sessions 0019-0024 in a single session, rather than addressing items sequentially across multiple sessions."
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0025-DECISION-ship-all-ant-only-as-default.md
    polarity: MIXED
    quote: "PACT governance replaces CC's feature-flag gating as the safety boundary."
practice_modality: brokerage-rep
status: draft
---

## The move

Before delegating a task to an agent or agent team, name which of the four verification-gradient zones the agent operates in for that task: auto-approved, flagged, held, or blocked. State the zone explicitly in the delegation brief or session plan. When the task crosses a zone boundary (e.g., the agent's output will be committed to a protected branch, or the scope expands mid-session), re-evaluate the zone assignment before proceeding. Do not assume all agents operate in the same zone or that a zone assigned for one task carries over to the next.

## When it fires

At the moment of delegation: a human or orchestrating agent assigns work to one or more executing agents. The trigger is "an agent is about to act within an operating envelope." Also fires mid-session when scope changes — if the agents were in the flagged zone (proceed, notify) and the task escalates to touching a release artifact, the zone must be re-evaluated to held or blocked. The counter-trigger is single-agent, single-task work where the human is directly in the loop and the zone is implicitly held by the interaction pattern itself.

## What it serves

PACT §19 (Verification Gradient) defines four zones — auto-approved, flagged, held, blocked — as the mechanism that makes Human-on-the-Loop concrete. The gradient replaces a binary permitted/denied model with calibrated intervention: routine work proceeds without human blocking, near-boundary work proceeds with notification, boundary work pauses for a decision, and out-of-envelope work is denied. PACT §11 (Role Envelope) requires that every active role has a standing operating boundary defined by its supervisor. The zone assignment is how the supervisor's standing trust is translated into concrete verification behaviour for a specific task. Without an explicit zone, agents default to whatever the system's implicit setting is — typically auto-approved — which means the human has no observation point until after the damage is done.

## Evidence walkthrough

- **loom-meta 0021** (POSITIVE) — Five sync-reviewer agents were delegated Gate 1 classification of 79 files. The entry records that the human approved the batch result, not individual file decisions. This is a textbook flagged-zone delegation: agents proceeded immediately, results were surfaced for human review, and the human could intervene retroactively. The explicit choice to delegate to agents rather than classify manually was itself the zone assignment — the human decided these agents were trustworthy enough for flagged (proceed, human approves batch) rather than held (pause per file for human decision).
- **loom-meta 0025** (POSITIVE) — Five-plus parallel agents cleared the entire sync backlog in a single session. The entry explicitly rejects sequential execution as inconsistent with the autonomous execution model and notes that human classification was delegated to agents. Multiple agents operating simultaneously in the flagged zone required that the zone assignment be coherent across the team — any single agent producing an incorrect classification would have been caught by the batch-approval gate, not by a per-agent hold. The zone assignment shaped the verification architecture: batch-level approval rather than per-action approval.
- **kaizen-cli-rs 0025** (MIXED) — The decision to ship all 90 feature flags as default behaviour in kz replaces CC's feature-flag gating with PACT governance as the safety boundary. This is a zone-assignment decision at the product level: features that CC gates behind internal feature flags (effectively a held or blocked zone for external users) are moved to auto-approved for all kz users, with PACT governance as the replacement boundary. The entry is mixed because the zone reassignment is stated as a strategic intent ("PACT governance replaces CC's feature-flag gating") without specifying the per-feature zone mapping — which features should be auto-approved, which should be flagged, and which should remain held. The move is made; the granularity is deferred.

## How to drill it

Present the practitioner with a scenario: three agents are about to work in parallel on (1) updating documentation, (2) refactoring a public API's internal implementation, and (3) adding a new CI pipeline stage that gates releases. Ask the practitioner to assign a verification-gradient zone to each agent and justify the assignment. Score: does the practitioner correctly distinguish that documentation is auto-approved or flagged (low risk, easily reversible), the API refactoring is flagged or held (behaviour could change even if the interface is stable), and the CI pipeline change is held (it gates all future releases and cannot be easily reversed)? Does the practitioner name a different zone for each agent rather than defaulting all three to the same zone?

## Common failure mode

Practitioner delegates to multiple agents without any zone differentiation — all agents operate at the same implicit trust level. When one agent's task escalates in scope (e.g., documentation changes touch API contracts), the escalation is invisible because no zone boundary exists to trigger re-evaluation. The fix is explicit zone assignment at delegation time, recorded in the session plan, with a named escalation trigger for each agent.

## Related atoms

- SC-B-015 (MUST rule reinterpretation) — the Gate 1 delegation in loom-meta 0021 is the same event viewed from two angles: SC-B-015 teaches the rule-amendment move, SC-B-017 teaches the zone-assignment move.
- SC-B-007 (worktree-per-agent) — worktree isolation is the mechanical prerequisite for parallel agents; zone assignment is the governance prerequisite.
- SC-B-004 (specialist delegation) — specialist delegation triggers zone assignment: delegating to a specialist with a narrow Role Envelope may warrant a wider zone (auto-approved) than delegating to a generalist.
