---
case_id: CASE-01
source_path: loom/journal/0014-DISCOVERY-drift-audit.md
title: "The 85% Drift Audit"
atoms_served: [variant-classification, drift-detection-audit]
spec_concepts: [CO §17, CO §15, CO §3.2]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-01: The 85% Drift Audit

## 1. The Situation

The Kailash project maintained COC artifacts (agents, skills, rules, hooks) across three repositories: the central `loom/` orchestrator, a Python USE template (`kailash-coc-claude-py`), and a Rust USE template (`kailash-coc-claude-rs`). The design intent was that both templates would share a common core of COC artifacts, with language-specific variants overlaid where needed. No formal mechanism existed to track which differences between the templates were intentional adaptations and which were accidental drift. The repositories had been synced periodically using ad hoc copy operations.

A deep audit was initiated to determine the actual state of alignment across all artifacts in the three repositories.

## 2. The Trigger

The practitioner ran a systematic comparison of every artifact file across all three repositories and discovered that the shared-core assumption was false. Only 15% of artifacts were truly shared (identical across all three). Sixty-six percent of skill files had drifted. The Rust template had aggressively gutted Python-specific rules -- five rules had been reduced to frontmatter-only stubs. Conversely, the Rust template had expanded nine rules with Rust-specific patterns, adding 40 to 190 lines each. The Rust template had 31 unique skill files; the Python template had 17 unique skill files. The Rust Kaizen skills had been completely restructured with a different format and different file organization.

The attentional pattern here is **quantified divergence detection**: instead of spot-checking a few files, the practitioner measured the full surface area and found the actual ratio was the inverse of what had been assumed.

## 3. The Move

The move is **drift-detection-audit** combined with **variant-classification**. The practitioner did not try to fix the drift immediately. Instead, the audit data was used to classify every divergence into one of two categories: intentional (language-specific adaptation) and accidental (sync failure or unreviewed edit). This classification became the input for a structural decision -- the variant architecture (documented in journal/0013) and the sync-manifest.yaml that would formalize variant declarations going forward.

The skill is in resisting the urge to "just sync everything back" and instead building the taxonomy that makes future sync safe.

## 4. The Outcome

The audit data directly informed the creation of the variant architecture. Every artifact was classified as global (shared across all templates), variant (language-specific, intentional), or skip (not upstreamed). The sync-manifest.yaml was created to carry these declarations. Future sync operations could consult the manifest to distinguish "this file is different because Rust needs it different" from "this file is different because nobody noticed the last sync broke it." The 85/15 split became the motivating evidence for the entire variant system.

## 5. The Spec Connection

**CO §17 (Single Source of Truth)** states that every artifact must have exactly one authoritative location. The audit revealed that three repositories each claimed authority over overlapping artifacts with no mechanism to resolve conflicts. The Single Source of Truth principle was violated not by explicit disagreement but by silent drift -- each template evolved independently and nobody checked whether the evolution was coordinated.

**CO §15 (Progressive Disclosure)** is relevant because the Rust template's restructuring of Kaizen skills into a different format was a legitimate progressive-disclosure choice for a different audience (Rust developers), but it was invisible to the sync system. The principle says complexity should be revealed incrementally; the audit showed that variant complexity was not revealed at all.

**CO §3.2 (Convention Drift)** names the specific failure mode: conventions established in one context silently diverge in another. The 66% skill-file drift is a textbook case of §3.2 -- the convention was "these files are shared," but the convention had drifted without anyone declaring a new convention.

## 6. Discussion Questions

1. The audit found that 15% of artifacts were truly shared. If the practitioner had instead found that 85% were shared, would the variant architecture still have been necessary? At what threshold of drift does a formalized variant system become worth the overhead of maintaining a manifest?

2. The Rust template gutted five Python-specific rules to frontmatter-only stubs. Is this a drift (accidental loss of content) or a variant (intentional decision that those rules are irrelevant to Rust)? What evidence would you need to distinguish the two, and what happens if you classify wrong in each direction?

3. If you were designing the sync system from scratch knowing this audit data, would you default to "everything is global until proven variant" or "everything is variant until proven global"? What are the failure modes of each default?
