---
atom_id: SC-B-003
name: Variant classification — single-sentence test rule
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - CO §16 — Framework-First
journal_evidence:
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0012-DECISION-core-vs-plugin-classification.md
    polarity: POSITIVE
    quote: 'Test: "Does this rule apply if the user is NOT using Kailash?" YES → Core. NO → Plugin.'
  - path: loom/journal/0013-DECISION-variant-architecture.md
    polarity: POSITIVE
    quote: "Implemented a variant overlay system making kailash/ the single source of truth for all CO/COC artifacts, with language-specific overlays for py and rs."
  - path: loom/journal/0048-DECISION-gate1-rs-variant-classification.md
    polarity: MIXED
    quote: "After thorough analysis of all 43 skill directories in kailash-rs/.claude/skills/, only 2 dirs needed upstreaming as new rs variants to loom… The remaining dirs fell into three categories: 24 dirs already correctly placed as rs variants, 15 dirs using global versions, 1 dir kept as kailash-rs local only."
practice_modality: drill
status: draft
---

## The move

When asked to place a CC artifact (rule, skill, agent, command, hook) into the variant overlay system, run a single-sentence counterfactual test before opening any directory: **"Does this rule apply if the user is NOT using Kailash?"** YES → global. NO → variant. The test is intentionally one sentence so the practitioner cannot dilute it with secondary considerations (file size, sync convenience, who wrote it last). Subsidiary tests apply only after the primary test resolves: language specificity (py vs rs), tier membership (BUILD vs USE), and project-locality (kept local vs upstreamed).

## When it fires

Three trigger conditions:

1. A `/codify` session in a BUILD repo produces a new artifact and asks "where does this land in the variant tree?"
2. A Gate 1 sync review encounters an inbound artifact and must classify global / variant / skip.
3. A drift audit finds an artifact in a USE template that disagrees with the global, and the question is "which one is right?"

## What it serves

CO §17 (Single Source of Truth) is the structural reason — every artifact must have exactly one authoritative location in the variant overlay tree, and the test is what produces that placement deterministically. CO §16 (Framework-First) is the secondary reason at the artifact level — framework-specific guidance composes from the framework-agnostic base, so misclassification (a framework rule placed as global, or a universal rule placed as a plugin) breaks the composition contract for every downstream consumer.

## Evidence walkthrough

- **0012-DECISION-core-vs-plugin-classification** (POSITIVE) — The test rule itself, applied verbatim to 19 rule files + ~30 agents + ~38 skill directories. The entry is the canonical worked example: every row in its tables is a reusable case for the drill below. The result was a clean partition (19 CORE rules / 10 PLUGIN rules; ~20 CORE agents / ~12 PLUGIN agents) and a 42% token-budget saving — the test paid for itself in measurable terms.
- **0013-DECISION-variant-architecture** (POSITIVE) — The architectural commitment that _makes_ the test load-bearing: the variant overlay tree is the single source of truth, so every misclassification has a repo-wide blast radius. Reading this entry establishes why the one-sentence test is not over-engineering — it is the smallest possible invariant the variant system can survive on.
- **0048-DECISION-gate1-rs-variant-classification** (MIXED) — Records what happens when the test is applied at scale post-hoc rather than at authoring time. Of 43 skill directories reviewed, only 2 needed upstreaming as new rs variants — but 24 had already been "correctly placed" by aggressive initial population, meaning the test was being applied implicitly by earlier sessions without being named. Mixed polarity because the after-the-fact application worked but cost an entire codify session that the at-authoring-time application would have saved.

## How to drill it

Hand the practitioner ten unfamiliar artifact names (mix of file paths from real repos — five rules, three agents, two skills) with no hint about content. For each, they must:

1. Predict global / variant-py / variant-rs / skip in under 30 seconds, citing the one-sentence test.
2. Open the file and verify or revise.
3. If revised, write one sentence on why their first instinct was wrong — was it a content surprise, or did they apply a secondary test (size, author, sync convenience) before the primary one?

Score on the _order_ of reasoning, not just the final answer. Practitioners who reach the right answer via "this looks like a Rust file" are failing the drill even when correct, because they will misclassify the next ambiguous case. The Gate 1 rs review (entry 0048) is a ready-made answer key — 43 directories with disposition + reason already recorded.

## Common failure mode

Practitioner classifies by file location instead of by content. "It's in `kailash-rs/.claude/rules/`, so it's a variant" feels obvious and is wrong half the time — most rules in `kailash-rs/.claude/rules/` are global rules merged from upstream, not Rust-specific overlays. The single-sentence test exists to break this shortcut. The Gate 1 rs review surfaced 15 directories where the practitioner's first instinct ("it's in the rs tree, so it must be a variant") would have manufactured 15 unnecessary variants, doubling the surface area for drift in subsequent syncs.

## Related atoms

- SC-B-001 (read-then-merge sync semantics) — the operational layer where this classification halts a sync at numbering or content collisions.
- SC-B-002 (authority-chain escalation) — the move that takes a misclassification discovered downstream and pushes the fix back to the authoring layer.
