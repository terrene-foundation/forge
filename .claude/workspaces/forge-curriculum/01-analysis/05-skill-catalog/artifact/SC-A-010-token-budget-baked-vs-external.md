---
atom_id: SC-A-010
name: Token budget — baked-in vs external boundary decision
craft_layer: artifact
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §13 — Layer 2 — Context
  - CO §14 — Master Directive
  - CO §15 — Progressive Disclosure
journal_evidence:
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md
    polarity: MIXED
    quote: "Our external rules alone cost **2.7x more tokens** than Claude Code's entire baked-in system prompt. And that's JUST rules — add agents, skills, guides and we're at 5-10x."
practice_modality: case
status: draft
---

## The move

When the external-artifact philosophy (everything as markdown in `.claude/rules/`) produces a per-turn token cost that exceeds the budget threshold (~30K tokens for rules alone, or >15% of the context window), evaluate a hybrid architecture: bake universal core rules into a compiled prompt, keep project-specific and domain-specific rules as external files. The decision is not "all baked" vs "all external" — it is which rules cross the boundary. Each rule is evaluated on three axes: (a) does it change between projects (external) or is it universal (bake candidate), (b) is it in the cacheable prefix or the dynamic suffix (baked rules get caching; external rules bust the cache), and (c) does it need to be user-visible and editable (external) or is it the agent's core personality (bake candidate).

## When it fires

When the total external artifact token count crosses ~30K tokens/turn, or when compaction fires earlier than expected because system-prompt overhead leaves less room for conversation. The secondary trigger is discovering that external rules are in the dynamic (uncached) suffix of the prompt — every turn pays the full token cost with no prompt-cache benefit. The tertiary trigger is building a custom agent binary (like `kz`) where baking is architecturally possible.

## What it serves

CO §13 (Layer 2 — Context) defines the institutional knowledge layer. The external-artifact model is one implementation of Layer 2 — it makes institutional knowledge visible, editable, and syncable. But CO §14 (Master Directive) says the master directive is loaded at the start of every session, and CO §15 (Progressive Disclosure) says minimum context by default. When the Layer 2 implementation produces 36K tokens of always-loaded context, it violates §15's minimum-by-default principle. The hybrid resolves the tension: universal directives are baked (satisfying §14's "always loaded" without token cost), project-specific knowledge stays external (satisfying §13's "living documents" and §15's "on demand").

## Evidence walkthrough

- **kaizen-cli-rs 0011** (MIXED) — This entry is the single most detailed analysis of the baked-vs-external trade-off in the corpus. It quantifies the cost: 146KB of external rules produces ~36K tokens/turn, 2.7x more than Claude Code's entire baked-in system prompt (54KB prompts.ts, ~13K tokens). The entry compares three options: Option A (all external, current), Option B (all baked, Anthropic's approach), and Option C (hybrid). Option C is the recommendation: bake core directives into the binary's `system_prompt.rs`, keep project-specific instructions in `.kz/CLAUDE.md`, keep skills and agents as on-demand external files. The mixed polarity reflects that the entry is negative about the current model's token cost but positive about the hybrid solution. The entry also surfaces a critical secondary cost: external rules are in the uncached prompt suffix, so every turn pays full price, while baked rules land in the cacheable prefix and cost nothing after the first turn.

## How to drill it

Present the practitioner with the kaizen-cli-rs 0011 data as a case study. The drill: (a) given the full list of 20+ external rules from the entry, classify each as "bake" (universal, personality-level, cacheable) or "keep external" (project-specific, user-editable, domain-scoped), (b) estimate the resulting token budget for each category, (c) identify which rules are in the cacheable prefix vs dynamic suffix under the current architecture, and (d) calculate the per-session cost difference between the current all-external model and the proposed hybrid. The entry's own classification (zero-tolerance, communication, agents rules as bake candidates; project CLAUDE.md and agent/skill definitions as external) is the answer key. Score on whether the practitioner's boundary matches the three-axis test (project-variance, cache-position, editability-need).

## Common failure mode

Two opposite failure modes. First: the practitioner bakes everything, producing a monolithic compiled prompt that cannot be updated without recompilation and cannot be inspected by users — this violates CO §13's requirement that institutional knowledge be living documents. Second: the practitioner refuses to bake anything, arguing that editability is always paramount — this accepts the 36K token/turn cost and the cache-busting penalty as the price of transparency. Both failures come from treating the boundary as a philosophical stance rather than a quantitative decision. The correction is the three-axis test: classify each rule individually, not the entire set as a bloc.

## Related atoms

- SC-A-008 (rule classification critical vs advisory) — the critical/advisory classification is complementary: critical rules that need deterministic enforcement often belong in hooks (neither baked nor external), while advisory rules are the population split between baked and external.
- SC-A-009 (progressive disclosure architecture) — the 4-level hierarchy is the external side of the boundary; the baked side is what sits below the hierarchy as compiled infrastructure.
