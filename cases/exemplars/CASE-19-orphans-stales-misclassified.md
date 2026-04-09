---
case_id: CASE-19
source_path: loom/journal/0028-RISK-manifest-integrity-gaps.md
title: "Orphans, Stales, and Misclassifications"
atoms_served: [manifest-integrity-audit, orphan-stale-misclassified-taxonomy]
spec_concepts: [CO §17, CO §49, CARE §42]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-19: Orphans, Stales, and Misclassifications

## 1. The Situation

The Kailash project used a `sync-manifest.yaml` to declare which COC artifacts were global (shared across all templates), which were variants (language-specific), and which were excluded from sync. The manifest was the single authority that the `/sync` command consulted when propagating artifacts from the source-of-truth repository (`loom/`) to the Python and Rust USE templates. Any artifact not in the manifest would not be synced; any artifact misclassified in the manifest would be synced incorrectly.

A red team exercise was conducted to test the structural integrity of the manifest against the actual filesystem state across all repositories.

## 2. The Trigger

The red team identified 8 findings, 4 of them critical. Three Python variant files existed on disk but were not listed in the manifest -- they had been created by `/codify` sessions and never registered, meaning they had never been synced to the Python USE template. Five Rust variant files were misclassified as `variant_only` (meaning they had no global equivalent) when in fact they did have global equivalents -- future syncs would have overwritten the Rust-specific content with generic globals. One exclusion rule contradicted a variant declaration for `skills/project/`, producing undefined sync behavior that depended on which rule the sync engine evaluated first. And the Ruby template's `VERSION` file was plain text instead of the expected JSON schema.

The attentional skill here is **taxonomy-based audit**: instead of checking individual files, the red team applied a structural taxonomy -- orphaned (on disk, not in manifest), stale (in manifest, not on disk or outdated), and misclassified (in manifest, wrong category) -- and systematically swept for all three failure classes.

## 3. The Move

The move is **manifest-integrity-audit** combined with **orphan-stale-misclassified-taxonomy**. The red team did not just find and fix individual errors. They established three named categories of sync failure -- orphans, stales, and misclassifications -- each with a distinct failure signature and a distinct downstream consequence. Orphans cause silent omission (the template never gets the artifact). Misclassifications cause silent overwrite (the template gets the wrong version). Stale references cause sync noise (the manifest points to something that no longer exists or has changed meaning).

The non-obvious aspect is the residual risk assessment. All 8 findings were fixed and committed, and templates were re-synced to 0 diffs. But the entry explicitly flags that no automated validation exists to prevent recurrence. New orphans and misclassifications can accumulate silently until the next manual red team. The fix is complete; the prevention is not.

## 4. The Outcome

All 8 findings were fixed in-session. Templates were re-synced and verified at 0 diffs -- no discrepancy between manifest declarations and actual filesystem state across all three templates. The concrete impact of the orphans was quantified: the Python template had been missing 3 skills (`nexus-agent-reference`, `kaizen-multi-provider`, `kaizen-tool-hydration`) since those skills were created. The 5 misclassified Rust variants would have been silently overwritten on the next sync. The entry recommends adding a pre-flight scan to `/sync` that warns about variant files on disk not in the manifest, estimating the cost at approximately 1 second of file listing.

## 5. The Spec Connection

**CO §17 (Single Source of Truth)** requires that every artifact has exactly one authoritative location. The manifest is the mechanism by which Single Source of Truth is implemented for the sync system -- it declares which version of each artifact is authoritative. Orphan variants violate §17 because they create artifacts with no declared authority: they exist, they are language-specific, but no manifest entry says whether they are the authoritative version or a stale copy. Without a declaration, the sync system treats them as if they do not exist.

**CO §49 (Enforcement Reliability)** requires that governance mechanisms produce reliable, predictable outcomes. The exclude-vs-variant contradiction for `skills/project/` directly violates enforcement reliability: the outcome of a sync depended on implementation-order evaluation of conflicting rules, not on a deterministic policy. Two implementations of the same sync logic could produce different results for the same input.

**CARE §42 (Constraint Resolution)** addresses how the system resolves conflicting constraints. The exclude-vs-variant contradiction is a constraint conflict: one rule says "do not sync this path" while another says "sync this path as a variant." CARE §42 requires that such conflicts be resolved explicitly, not by silent precedence. The red team finding exposed that the sync system had no constraint-resolution mechanism -- whichever rule evaluated first won, silently.

## 6. Discussion Questions

1. If the 5 misclassified Rust variants had not been caught and the next sync had overwritten them with generic globals, how would the error manifest downstream? Would it be a build failure, a behavioral regression, or a silent degradation -- and which is hardest to diagnose?

2. The entry recommends a pre-flight scan that warns about variant files on disk not in the manifest. Design the inverse check as well: manifest entries that reference files not on disk. What class of error does each direction catch that the other misses? Is one direction strictly more important than the other?

3. The 3 orphan Python variant files had been missing from the template since creation. If you discovered this 6 months later instead of during a red team, what would be different about the fix? Consider whether downstream projects that consumed the template during those 6 months might have built workarounds for the missing skills, and what happens when the skills suddenly appear.
