---
atom_id: SC-B-010
name: Cross-repo divergence audit with numbered drift-gap list
craft_layer: brokerage
destination: co-codegen
craft_areas: [attend-how, institutional-memory]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - CO §49 — Quality Criteria — Enforcement Reliability
journal_evidence:
  - path: loom/journal/0014-DISCOVERY-drift-audit.md
    polarity: NEGATIVE
    quote: "- Only 15% of artifacts were truly shared (identical across all three)\n- 66% of skill files had drifted"
  - path: loom/journal/0028-RISK-manifest-integrity-gaps.md
    polarity: NEGATIVE
    quote: "3 orphan py variant files — existed on disk but not in manifest, never synced to templates."
  - path: loom/journal/0029-DECISION-manifest-parity-automation.md
    polarity: POSITIVE
    quote: "Created `scripts/check-manifest-parity.sh` to detect three classes of sync manifest integrity issues:\n1. **Orphan variants** — files on disk with no manifest entry (silent sync bypasses)\n2. **Stale entries** — manifest entries pointing to nonexistent files (sync errors)\n3. **Parity gaps** — variants covering some targets but not all declared targets"
practice_modality: brokerage-rep
status: verified
---

## The move

Before any distribution pass, audit every downstream repo against upstream to produce a numbered drift-gap list. For each file in scope, classify the divergence as intentional-variant, unintentional-drift, or orphan. Halt the distribution if unintentional drift exceeds a declared threshold. The output is a numbered list, not a narrative — each gap gets a number, a classification, and a one-line description of the divergence.

## When it fires

Any moment the broker is about to push artifacts from a source repo to one or more targets — or when a red team pass needs to verify that prior syncs did not silently lose content. The trigger is "I am about to assert that these repos are in alignment" or "I need to know the actual alignment state."

## What it serves

CO §17 (Single Source of Truth) requires that institutional knowledge live in exactly one place with no contradictions. A drift-gap audit is the empirical check that this invariant holds across the repo graph. Without the audit, the single-source-of-truth claim is aspirational, not verified. CO §49 (Enforcement Reliability) requires that critical rules have hard enforcement and that you intentionally attempt to violate them; a drift audit is the intentional check that the sync mechanism actually enforced what it claimed.

## Evidence walkthrough

- **0014-DISCOVERY-drift-audit** (NEGATIVE) — A deep audit of kailash/, kailash-coc-claude-py, and kailash-coc-claude-rs found 85% artifact drift, with 66% of skill files diverged and no mechanism to distinguish intentional (language-specific) from accidental (sync failure) drift. The audit data directly informed the variant architecture decision. This is the canonical case of what happens when drift detection is not a routine operation.
- **0028-RISK-manifest-integrity-gaps** (NEGATIVE) — Red team found 3 orphan py variant files that existed on disk but had no manifest entry, meaning they were invisible to /sync and had never been distributed. The systemic risk identified was that no automated validation catches these classes of errors before they cause silent data loss.
- **0029-DECISION-manifest-parity-automation** (POSITIVE) — Created a bash script to detect orphan variants, stale manifest entries, and parity gaps. First run: 0 orphans, 0 stale, 91 parity gaps (pre-existing by design). This is the move being institutionalised — the numbered-list output format is exactly what makes drift visible and actionable.

## How to drill it

Take two mock repos (upstream U, downstream D) with 20 files in scope. Seed D with five divergences: one orphan (file on disk, not in manifest), one stale entry (in manifest, not on disk), one intentional variant (language-specific content), one unintentional drift (older version of a global file), and one true duplicate. Produce a numbered drift-gap list with columns: number, filename, classification (orphan/stale/variant/drift/aligned), one-line description. Score: did the practitioner correctly classify all five, and did they halt before proposing a sync for the unintentional drift?

## Common failure mode

Practitioner runs the sync without auditing first, relying on the sync command's own conflict resolution to surface problems. This misses orphans entirely (they are invisible to sync because they have no manifest entry) and misses stale entries (the manifest points to nothing, so sync silently skips them). The audit must precede the sync, not be embedded within it.

## Related atoms

- SC-B-001 (read-then-merge sync) — the sync mechanism that the drift audit validates.
- SC-B-003 (variant classification) — the classification step that the drift-gap list depends on for each file.
- SC-B-011 (version tracking cascade) — version drift is a special case of cross-repo divergence; the audit catches it, the cascade prevents it from propagating through the chain.
- SC-B-012 (manifest parity automation) — the automated check that operationalises the numbered drift-gap list.
- SC-B-014 (explicit supersession journaling) — when a drift audit reveals that a prior sync decision was wrong, the correction must be journaled with explicit supersession.
- SC-P-020 (second-occurrence structural signal) — the audit surfaces drift occurrences; at the second occurrence the practitioner must escalate to structural intervention.
