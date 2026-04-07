---
atom_id: SC-B-002
name: Authority-chain escalation when local fix gets reverted
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory, intervene-when]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - PACT §10 — Monotonic Tightening
  - PACT §11 — Role Envelope
journal_evidence:
  - path: loom/journal/0016-DECISION-co-authority-chain.md
    polarity: POSITIVE
    quote: "Established ~/repos/co/ as the authority for CC (Claude Code) and CO (Cognitive Orchestration) artifacts. kailash/ is the authority for COC (codegen) artifacts. Both have artifact-flow.md rules that reference each other consistently."
  - path: loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md
    polarity: NEGATIVE
    quote: "GLOBAL BUG in loom/ source: Skills reference `07-eatp-reference/` and `08-care-reference/` but actual directories are `26-` and `27-`. Fixed locally but sync from loom/ reverts it — must be fixed at loom/ source."
practice_modality: brokerage-rep
status: verified
---

## The move

When you find a defect in a synced artifact (a rule, a skill, an agent, a hook) inside a USE repo, do not fix it locally and call it done. Trace the artifact back through the authority chain — USE template ← BUILD repo ← upstream methodology source — and file the fix at the highest layer where the artifact is _authored_. The local fix is at most a tourniquet that buys time for the upstream proposal to land.

## When it fires

Two trigger conditions: (1) you discover a bug in a file that is on the sync receiving end (anything inside `.claude/` of a downstream repo, anything inside a template repo), or (2) a fix you made in a previous session has been silently overwritten by `/sync` or `/sync-to-coc`. The second is the louder signal — a reverted fix is direct evidence the authority chain was bypassed.

## What it serves

CO §17 (Single Source of Truth) is the structural reason: an artifact has exactly one authoritative source, and a fix anywhere else is by definition not the truth. PACT §10 (Monotonic Tightening) is the dynamics reason: governance state must only become stricter over time, and reverting a fix is a non-monotonic loosening masquerading as a sync. PACT §11 (Role Envelope) is the accountability reason: the fixer must be operating in the role envelope of the layer that _owns_ the artifact, otherwise the fix has no governance footprint.

## Evidence walkthrough

- **0016-DECISION-co-authority-chain** (POSITIVE) — Establishes the explicit chain. Co/ is the authority for CC + CO; kailash/ is the authority for COC; downstream USE repos are receivers, not authors. The entry's "consequence" line is the operational form of the rule: kailash/ no longer independently edits CC or CO artifacts; it receives them and adapts. Any session that edits a CC/CO artifact in kailash/ has bypassed the chain.
- **0005-DECISION-artifact-redteam-convergence** in v3.8-issues (NEGATIVE) — The exact failure pattern this skill is designed to prevent. A red team found that skills referenced wrong directory numbers (07/08 instead of 26/27). The team fixed it locally, then realised the next sync from loom/ would revert the fix. The entry names the consequence in plain terms: "must be fixed at loom/ source." The skill atom is the move that would have been made _first_ — the local fix would have been recorded as a tourniquet, and the upstream proposal would have been the actual deliverable.

## How to drill it

Take the v3.8-issues 07/08 case as a fixed scenario. Hand the practitioner the local-fix patch and ask: "What happens at the next sync? Walk the artifact back to its authoring layer. Where is the upstream proposal landed?" Score on three checkpoints — (1) named the authoring layer correctly (loom/), (2) wrote the upstream proposal as a separate artifact, (3) marked the local fix as time-bounded with an explicit re-check date keyed to when the upstream lands.

## Common failure mode

Practitioner sees a bug, fixes it where they found it, runs tests, ships. The fix lasts until the next sync. The worst variant is silent — the practitioner does not even notice the revert because there is no test that runs against the synced artifact's content (only against the application code that uses it). The cure is the second trigger: any time you see a fix you made before is missing, treat it as a high-priority signal that an entire class of fixes is being lost, not as a one-off.

## Related atoms

- SC-B-001 (read-then-merge sync semantics) — the structural protection that catches the same class of error from the sync side.
- SC-B-003 (variant classification) — the test that decides whether the upstream fix lands as global or as a variant.
- SC-B-004 (specialist delegation) — the move that reduces these failures upstream by routing the original work through someone who knows the authoring layer.
- SC-B-009 (cross-SDK upstream brokerage) — upstream brokerage is authority-chain escalation applied across SDK boundaries; the fix belongs at the SDK level, not the consumer level.
- SC-B-015 (MUST rule reinterpretation) — when a rule's literal text blocks the escalation path, the reinterpretation move amends the rule to restore it.
- SC-B-017 (verification-gradient zone assignment) — the zone assigned to the escalating agent determines whether the upstream fix proposal requires held approval or can proceed as flagged.
