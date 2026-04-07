---
atom_id: SC-A-012
name: Author CLAUDE.md as the Layer 2 master directive
craft_layer: artifact
destination: both
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §13 — Layer 2 — Context
  - CO §14 — Master Directive
  - CO §15 — Progressive Disclosure
journal_evidence:
  - path: loom/journal/0001-DECISION-co-refocusing.md
    polarity: POSITIVE
    quote: "Rewrote CLAUDE.md to position as 'Kailash CO Artifact Management Platform.' Split commands into Management (used here) and COC Workflow (managed for sync). Classified all 26 rules as CO-General (12) or COC-Specific (14)."
  - path: loom/journal/0008-DECISION-coc-artifact-scoping.md
    polarity: POSITIVE
    quote: "COC artifact creation follows a strict scoping model: BUILD repos CAN create and update COC artifacts ANYWHERE in .claude/. USE repos can ONLY create COC artifacts in project subdirectories."
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: NEGATIVE
    quote: "COC artifacts consume ~48,000 tokens of always-in-context budget (rules: 37k, CLAUDE.md: 2.2k, agent/skill discovery: 8.5k). For Claude Code Pro users with constrained token budgets, this overhead significantly impacts usable conversation length."
practice_modality: build
status: draft
---

## The move

When authoring or revising a project's CLAUDE.md, treat it as the Layer 2 master directive — the always-loaded entry point that names what context exists, what rules fire, what skills are available, and what the absolute directives are. CLAUDE.md is not a README for humans. It is the single highest-leverage artifact in the project because it consumes always-in-context tokens every turn. Every line in it must earn its place against the token budget, and every line must point the agent toward the right subordinate artifacts rather than restating their content.

## When it fires

When creating a new project's .claude/ infrastructure from scratch, or when a CLAUDE.md has grown past its useful size (the 0049 experiment quantified the full COC artifact set at ~48K always-loaded tokens, with CLAUDE.md contributing 2.2K). Also fires when an audit reveals that CLAUDE.md restates rules already enforced by the rules system, or when repo identity has shifted and the directive no longer matches the repo's actual function.

## What it serves

CO §14 (Master Directive) requires "a master directive document loaded at the start of every session" that "establishes baseline rules, framework preferences, non-negotiable conventions." CO §13 (Layer 2 — Context) is the architectural layer that holds institutional knowledge, and the master directive is its root node. CO §15 (Progressive Disclosure) requires that the master directive be the top of a hierarchy — it indexes subordinate artifacts without restating them. A CLAUDE.md that fails to function as a master directive leaves the agent contextless (violating §14) or bloated (violating §15).

## Evidence walkthrough

- **loom-meta 0001** (POSITIVE) — The loom repo's CLAUDE.md was originally positioned as "Kailash COC Claude (Python)" — a coding environment identity, even though no coding happens in that repo. The rewrite repositioned it as "Kailash CO Artifact Management Platform," split commands into management vs workflow categories, and classified all 26 rules as CO-general or COC-specific. This is the master-directive move executed correctly: the directive names the repo's actual identity, classifies what the agent needs to know, and structures the context for the agent's real task.
- **loom-meta 0008** (POSITIVE) — The artifact scoping decision codified what belongs where in the .claude/ hierarchy: BUILD repos own global artifacts, USE repos own project/ subdirectories. This decision was made through CLAUDE.md-level authoring — the master directive must tell the agent which artifacts are read-only (synced from upstream) and which are writable (project-specific). Without this scoping in the directive, the agent would modify synced artifacts that get overwritten on the next /sync.
- **loom-meta 0049** (NEGATIVE) — The token ablation experiment measured the cost of the always-loaded context at ~48K tokens total, with CLAUDE.md at 2.2K. The experiment classified rules as KEEP, COMPRESS, REMOVE, or DEMOTE. The finding relevant to master-directive authoring: any content in CLAUDE.md that restates a rule already loaded by the rules system is pure token waste — it occupies always-in-context budget without adding information the agent does not already have. The master directive should index rules, not repeat them.

## How to drill it

Give the practitioner a bloated CLAUDE.md (400+ lines, restated rules, outdated identity, no command index, no rules classification) and a set of subordinate .claude/ artifacts (rules, agents, skills, commands). The drill: (a) identify every line in CLAUDE.md that restates content available in a subordinate artifact, (b) rewrite CLAUDE.md as a master directive under 200 lines that indexes subordinate artifacts by purpose without restating them, (c) verify that the rewritten directive correctly names the repo's identity, lists absolute directives, and classifies rules. Score on whether the rewritten CLAUDE.md is under 200 lines, contains zero restated rules, and passes a completeness check: can an agent reading only the master directive find every subordinate artifact it needs?

## Common failure mode

The practitioner treats CLAUDE.md as a documentation file rather than a machine-readable directive. Symptoms: prose paragraphs explaining the project for human readers, copy-pasted rule summaries "for convenience," and no index of subordinate artifacts. The result is a CLAUDE.md that is both too long (restated rules waste tokens) and too incomplete (missing artifact pointers mean the agent never discovers skills and commands). The 0001 entry shows the fix: rewrite from the agent's perspective, not the human reader's.

## Related atoms

- SC-A-009 (progressive disclosure architecture) — CLAUDE.md is level 1 of the 4-level hierarchy; this atom covers how to author that level specifically.
- SC-A-010 (token budget baked vs external) — the master directive is the primary baked-in artifact; its size directly affects the token budget available for conversation.
- SC-A-015 (Layer 2 context structuring) — the master directive indexes the corpus; SC-A-015 covers how to structure the corpus itself (rules vs skills vs agents vs commands).
