---
atom_id: SC-B-001
name: Read-then-merge sync semantics
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - CO §28 — Approval Gate
  - CARE §6 — Trust Verification Bridge
journal_evidence:
  - path: loom/journal/0019-DECISION-sync-merge-not-copy.md
    polarity: POSITIVE
    quote: 'Both `/sync` (Gate 2) and `/sync-to-build` were rewritten to require reading the target before writing. Per-file merge decisions replace bulk "Apply all". Numbering conflicts require human intervention.'
  - path: loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md
    polarity: NEGATIVE
    quote: "kailash-rs BUILD repo had 24 BUILD-only skill directories using a numbering scheme completely different from the kailash/ canonical scheme. 12 numbers had different meanings (e.g., 05=enterprise in rs vs 05=kailash-mcp globally)."
practice_modality: brokerage-rep
status: draft
---

## The move

Before any cross-repo file distribution (sync, propagate, broadcast, mirror), inventory the target's current state, compute the per-file diff, and present each diff to a human decision point that can resolve conflicts. Refuse rsync-style "compute desired state, overwrite target." Numbering collisions or content conflicts halt the run until the human classifies them as global / variant / skip.

## When it fires

Any operation that writes one repo's artifacts into another — sync, codify→propagate, template distribution, mirror. The trigger is not "sync" the verb but "write into a tree I do not own as a single transaction."

## What it serves

CO §17 (Single Source of Truth) requires that institutional knowledge live in exactly one place. A bulk-overwrite sync silently violates this in the opposite direction: it asserts the source repo's state as truth while erasing target-only content that may itself be the more recent truth. CO §28 (Approval Gate) requires human judgment at structural transitions; cross-repo writes are exactly such a transition. CARE §6 (Trust Verification Bridge) names the trust handoff that a read-then-merge satisfies and that a blind copy bypasses.

## Evidence walkthrough

- **0019-DECISION-sync-merge-not-copy** (POSITIVE) — Records the decision to rewrite both `/sync` and `/sync-to-build` to inventory-then-merge after the original copy semantics damaged kailash-rs. The fix is the move; the entry names the alternatives considered (smarter exclusions, separate per-repo commands) and rejects them on the grounds that "the problem isn't which files to exclude, it's that overwriting without reading loses content."
- **0020-DISCOVERY-rs-skill-numbering-divergence** (NEGATIVE) — Documents the 24 BUILD-only directories where numbering had drifted before the variant system existed. The skill atom is grounded in the specific failure: a sync that did not read the target first would have collapsed 14 Rust-specific directories into generic globals, destroying months of `/codify` work. The DISCOVERY entry is what made the DECISION in 0019 unavoidable.

## How to drill it

Take a small mock pair of repos (source A, target B). Seed B with three files that disagree with A: one drift (B has newer content), one numbering collision (different file at the same canonical slot), one true duplicate. The drill: write the merge plan as a per-file diff with three columns — `source`, `target`, `decision (global/variant/skip/halt)` — and refuse to produce a single bulk command. Score the drill on whether the practitioner halted at the numbering collision or tried to "fix" it inline.

## Common failure mode

Practitioner accepts the source repo's state as authoritative because the source repo is the BUILD repo, and reasons "the BUILD repo is upstream, so its state wins." This is the rsync mental model leaking back in. The cure is the per-file diff: a drift in B is not a numbering question, it is a _content authority_ question that the source repo cannot answer alone.

## Related atoms

- SC-B-002 (authority-chain escalation) — what to do when the per-file diff reveals a drift that the local fix cannot resolve.
- SC-B-003 (variant classification) — the single-sentence test that resolves global-vs-variant questions surfaced by the diff.
