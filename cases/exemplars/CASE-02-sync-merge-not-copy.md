---
case_id: CASE-02
source_path: loom/journal/0019-DECISION-sync-merge-not-copy.md
title: "Sync Must Merge, Not Copy"
atoms_served: [read-then-merge-sync, authority-chain-enforcement]
spec_concepts: [CO §17, CO §28, CARE §6]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-02: Sync Must Merge, Not Copy

## 1. The Situation

The COC ecosystem maintained a sync pipeline that distributed artifacts from the central `loom/` orchestrator to USE templates (Python and Rust). Two sync commands existed: `/sync-to-build` (pushing from BUILD repos to loom/) and `/sync` at Gate 2 (distributing from loom/ to templates). Both commands used rsync-like semantics -- they computed the expected end state at the source and then copied it to the target, overwriting whatever was there.

This worked as long as the source and target were structurally similar. The moment the Rust template diverged in structure, the sync commands became destructive.

## 2. The Trigger

When `/sync` ran against the Rust template, it discovered that the Rust template used a completely different skill numbering scheme from the Python template. The rsync-style copy overwrote 14 Rust-specific skill directories with generic globals from the canonical numbering. This created 28 duplicate directories: the 14 old Rust directories (now containing generic content) and 14 new directories at the canonical numbers (also containing generic content). Every Rust-specific skill was destroyed.

The attentional pattern is **damage from a trusted operation**: the sync command was expected to be safe, it appeared to complete successfully, and the damage was invisible until someone opened the Rust skill files and found Python-centric content.

## 3. The Move

The move is **read-then-merge-sync** with **authority-chain-enforcement**. Both `/sync-to-build` and `/sync` were rewritten to require reading the target repository before writing. Instead of computing the expected state at the source and pushing it, the commands now inventory the target's current state, compare per-file, and present merge decisions to the human operator. Numbering conflicts halt the sync until the human decides how to resolve them.

Two alternatives were considered and rejected. First, keeping rsync semantics with smarter exclusions was rejected because the problem was not which files to exclude but that overwriting without reading loses content. Second, creating separate sync commands per repository was rejected because the merge semantics should be universal regardless of target.

## 4. The Outcome

The sync pipeline became read-then-merge instead of compute-then-copy. `/sync-to-build` now requires an inventory of the BUILD repo before computing changes. `/sync` at Gate 2 requires an inventory of the USE template before distributing. Numbering conflicts halt the process and require human intervention. The spec for the new flow was documented in `guides/co-setup/07-sync-flow.md`.

The deeper consequence is that sync operations became transparent -- the operator sees what will change before it changes, instead of trusting that the computed state is correct.

## 5. The Spec Connection

**CO §17 (Single Source of Truth)** is the principle that motivated the sync pipeline in the first place -- loom/ is the single source, and templates should reflect it. But §17 does not say "copy the source state verbatim." It says every artifact has one authoritative location. When a Rust-specific skill file has loom/ as its authority through the variant system, the sync must respect that authority -- not overwrite it with the Python-oriented global.

**CO §28 (Approval Gate)** requires that changes crossing a structural boundary must pass through a human-judgment gate. The original rsync-style sync had no gate: it computed and applied. The rewrite adds the gate -- numbering conflicts halt the sync and require human classification. This is a concrete instantiation of §28 applied to the artifact-distribution pipeline rather than to code review.

**CARE §6 (Trust Verification Bridge)** states that transitions between trust boundaries require verification. The sync pipeline moves artifacts across a trust boundary (from the orchestrator's authority to the template's authority). The original pipeline skipped verification entirely. The read-then-merge rewrite is a trust verification bridge: it reads the target's state, compares, and requires human verification before the transition completes.

## 6. Discussion Questions

1. The rsync-style sync appeared to work correctly for months while the Python and Rust templates were structurally similar. What does this tell you about the relationship between test coverage and structural assumptions? Could any test suite have caught this failure before the Rust template diverged?

2. If the sync had been read-then-merge from the beginning, the 28-directory destruction would never have occurred -- but every sync operation would have been slower and required human attention. Is there a way to get the safety of merge semantics without the overhead, or is the overhead the necessary cost of the safety?

3. The decision rejected "separate sync commands per repo" because merge semantics should be universal. Under what circumstances would per-repo sync commands actually be the right choice? What would have to be true about the repositories for universal merge semantics to become a burden rather than a benefit?
