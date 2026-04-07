---
case_id: CASE-07
source_path: loom/kailash-rs/workspaces/v3.8-issues/journal/0005-DECISION-artifact-redteam-convergence.md
title: "The GLOBAL BUG in Loom Source"
atoms_served: [authority-chain-escalation, multi-round-redteam-convergence]
spec_concepts: [CO §17, CO §49, CO §50, PACT §10]
craft_layer: brokerage
recommended_use: discussion
destination: both
application: COC
---

# CASE-07: The GLOBAL BUG in Loom Source

## 1. The Situation

The Kailash Rust SDK repository (`kailash-rs`) maintained a full set of COC artifacts: 52 agents, 39 skills, and 31 rules. These artifacts were synced from the central `loom/` orchestrator and supplemented with Rust-specific variants. A three-round red team was conducted across all artifacts, using 13 specialized agents, to validate artifact quality before a release.

The red team found 30+ issues across three rounds: 3 CRITICAL, 2 HIGH, and 15+ MEDIUM. Most were routine fixes -- broken cross-references, stale node counts, incorrect package names, stub rules. But one finding created a structural dilemma that could not be resolved locally.

## 2. The Trigger

The red team discovered that skill files in the Rust repository referenced `07-eatp-reference/` and `08-care-reference/` as directory paths. But the actual directories were numbered `26-` and `27-` respectively. Every skill that loaded these references would fail silently -- the progressive-disclosure system would look for a directory that did not exist and proceed without the reference, leaving the agent blind to EATP and CARE context.

The practitioner fixed the paths locally in the Rust repository. The fix worked. But the next sync from `loom/` would overwrite the fix, because the bug was in the loom/ source -- the upstream authority. The wrong directory numbers existed in loom/'s canonical skill files. Fixing the bug locally was not a fix; it was a temporary divergence that sync would revert.

The attentional pattern is **authority-chain awareness**: recognizing that a local fix to a synced artifact is not stable because the artifact's authority lies upstream.

## 3. The Move

The move is **authority-chain-escalation** combined with **multi-round-redteam-convergence**. The practitioner did two things. First, they fixed the paths locally so the Rust repository was immediately correct. Second, they escalated the bug to loom/ by creating an upstream proposal at `.claude/.proposals/latest.yaml` with 12 changes (4 global, 7 rs-specific, 1 skip).

The escalation is the critical skill. The practitioner could have stopped at the local fix and moved on. But they understood that the bug would recur on next sync, and that the same bug existed in every other downstream repository that consumed these skill files from loom/. The local fix plus upstream escalation pattern ensures both immediate correctness and systemic correction.

The three-round red team convergence is also instructive: round 1 found the bulk of issues, round 2 verified fixes and found edge cases missed in round 1, round 3 confirmed convergence. The skill-path bug was found in round 1 but the escalation decision was made during fix assessment, when the practitioner traced the path values back to their source.

## 4. The Outcome

The Rust repository was locally correct with the fixed paths. The upstream proposal was filed for loom/ to fix the bug globally before the next sync. Other downstream repositories would receive the fix through the next sync cycle after loom/ accepted the proposal.

The deeper consequence is a rule: when a red team finds a bug in a synced artifact, the fix must happen at the artifact's authority (loom/), not at the consumer (kailash-rs). A consumer-level fix is a temporary workaround that sync will overwrite. The authority chain is: loom/ publishes, templates consume, downstream repos inherit. Bugs must flow upstream to be truly fixed.

## 5. The Spec Connection

**CO §17 (Single Source of Truth)** is directly invoked. The skill files have loom/ as their single source of truth. Fixing the files in kailash-rs creates two versions: the loom/ version (wrong) and the kailash-rs version (right). This violates §17 until the upstream is corrected. The practitioner's escalation is the §17-compliant move: acknowledge the local fix as temporary and push the correction to the source of truth.

**CO §49 (Enforcement Reliability)** states that enforcement mechanisms must actually enforce. The progressive-disclosure system was designed to load reference files on demand, but it failed silently when the directory path was wrong. §49 requires that the enforcement mechanism signal failure rather than proceeding without the reference. The silent failure mode meant the bug was invisible -- agents operated without EATP and CARE context and nobody noticed.

**CO §50 (Workflow Discipline)** requires that workflow artifacts be maintained with the same rigor as code. The wrong directory numbers were a maintenance failure in skill files -- workflow artifacts that were assumed to be correct because they had been synced from the authority. §50 says: workflow artifacts drift just like code and must be validated.

**PACT §10 (Monotonic Tightening)** states that governance constraints must become tighter over time, never looser. The upstream proposal mechanism is an instance of monotonic tightening: once the bug is identified, the correction is propagated to the authority so that all future syncs are correct. The alternative -- leaving the bug in loom/ and fixing each downstream repo individually -- would be a non-monotonic approach that allows the same bug to re-emerge in new consumers.

## 6. Discussion Questions

1. The skill files referenced directories `07-eatp-reference/` and `08-care-reference/`, but the actual directories were `26-` and `27-`. When were the directories renumbered, and why did the references not update? Is this a sync failure (the renumbering should have propagated) or a maintenance failure (nobody checked cross-references after renumbering)? What process would prevent this class of bug?

2. The progressive-disclosure system failed silently when the directory path was wrong. If it had failed loudly (error on startup, refusing to proceed), the bug would have been caught immediately. But loud failure would also mean that any path typo in any skill file would block the entire session. How should a progressive-disclosure system balance "fail on missing references" against "proceed gracefully when optional content is absent"?

3. The practitioner filed the upstream proposal but had no guarantee that loom/ would accept it before the next sync. If loom/ delays the fix and sync runs, the local fix is overwritten. What should the downstream repo do in the interim -- block sync until the upstream fix is merged, or accept the overwrite and re-apply the local fix? What are the costs of each approach?
