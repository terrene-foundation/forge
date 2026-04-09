---
case_id: CASE-16
source_path: loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md
title: "24 Directories of Silent Drift"
atoms_served: [drift-detection-audit, variant-classification]
spec_concepts: [CO §17, CO §3.2, CO §15]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-16: 24 Directories of Silent Drift

## 1. The Situation

The Kailash project maintained skill directories across both Python and Rust BUILD repositories. Each repository's `/codify` command generated numbered skill directories (e.g., `01-core`, `05-enterprise`) as sessions accumulated craft knowledge. The Python repository's numbering scheme had become the de facto canonical scheme referenced by the global skill index. The Rust BUILD repository (`kailash-rs`) had been running its own `/codify` sessions independently for weeks.

No mechanism existed to coordinate skill numbering across the two BUILD repositories. Each `/codify` session in `kailash-rs` assigned sequential numbers without consulting the global scheme. No Gate 1 review had ever been run to upstream or align the Rust skills with the canonical index.

## 2. The Trigger

An audit of the Rust BUILD repo revealed 24 BUILD-only skill directories using a numbering scheme completely different from the canonical scheme. Twelve numbers had entirely different meanings -- for example, `05` meant "enterprise" in the Rust repo but `05` meant "kailash-mcp" in the global scheme. Fourteen directories were Rust-specific versions of global skills but assigned to different numbers. The numbering had diverged so far that an rsync-style sync between the two repositories would have created 28 duplicate directories -- every Rust skill would have landed alongside its Python counterpart at a different number, producing two directories for the same concept.

The attentional skill here is **divergence surface-area measurement**: not just noticing that "some skills are different" but quantifying the full scope -- 24 directories, 12 semantic collisions, 14 misaligned duplicates -- which reveals that the problem is structural, not cosmetic.

## 3. The Move

The move combines **drift-detection-audit** with **variant-classification**. The practitioner merged Rust-specific content from old-numbered directories into canonical-numbered directories (62 files merged), deleted 14 stale old-numbered directories and 3 true duplicates, and kept BUILD-only directories (like `01-core`, `05-enterprise`, `06-python-bindings`) as-is because they documented internal crate APIs not relevant to USE templates.

The non-obvious aspect is what was NOT done: the merged Rust content was not immediately upstreamed to the canonical index as Rust variants. The practitioner recognised that upstreaming without a Gate 1 classification review would repeat the same failure -- content placed without human classification of global vs variant. The fix restored numbering alignment but deliberately left the variant-classification step as a separate, human-gated action.

## 4. The Outcome

Sixty-two files were merged into canonically numbered directories. Seventeen stale or duplicate directories were removed. The Rust BUILD repo's skill tree became navigable alongside the Python skill tree without semantic collisions. However, the entry explicitly notes what remains unresolved: the merged Rust content has NOT been upstreamed to the canonical index as `rs` variants, and the Rust USE template still carries generic globals rather than Rust-specific content. A Gate 1 review session is still needed to classify and upstream the Rust content properly.

## 5. The Spec Connection

**CO §17 (Single Source of Truth)** requires that every artifact has exactly one authoritative location. The 24 divergent directories violated this principle not through disagreement but through independent creation -- each `/codify` session in `kailash-rs` created skills as if it were the only numbering authority. The result was two parallel numbering authorities with no mechanism to detect, let alone resolve, conflicts between them.

**CO §3.2 (Convention Drift)** names the specific failure mode. The convention was "skill directories are numbered sequentially from a global scheme." That convention held within each repository but silently diverged between repositories. The 12 semantic collisions (same number, different meaning) are the textbook §3.2 outcome: a convention that appears to hold locally while being broken globally.

**CO §15 (Progressive Disclosure)** is relevant because the BUILD-only directories (`01-core`, `05-enterprise`, `06-python-bindings`) were correctly retained -- they document internal crate APIs that USE template consumers should not see. The practitioner applied progressive disclosure by distinguishing what belongs in the canonical skill tree (visible to all consumers) from what belongs only in the BUILD repo (visible only to SDK contributors).

## 6. Discussion Questions

1. If the practitioner had immediately upstreamed all 62 merged files as Rust variants without a Gate 1 review, what categories of error could have resulted? Consider the difference between a file that is Rust-specific (legitimate variant) and a file that is language-agnostic but happened to be written in a Rust context (should be global).

2. The root cause was that `/codify` sessions assigned sequential numbers without consulting the global scheme. What structural change to the `/codify` command would prevent this class of divergence? Would it be better to enforce global numbering at creation time or to detect divergence at sync time -- and what are the failure modes of each approach?

3. The entry notes that 14 directories were "Rust-specific versions of globals at different numbers." If you had to classify these 14 without reading any of them, what heuristic would you use to predict which are true variants (Rust-specific content) vs accidental drift (same content, wrong number)? How would you validate that heuristic?
