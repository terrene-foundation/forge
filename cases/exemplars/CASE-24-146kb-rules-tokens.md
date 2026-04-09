---
case_id: CASE-24
source_path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md
title: "146KB of Rules at 36K Tokens Per Turn"
atoms_served: [token-budget-analysis, external-vs-baked-tradeoff]
spec_concepts: [CO §13, CO §14, CO §15, CO §5]
craft_layer: artifact
recommended_use: discussion
destination: both
application: COC
---

# CASE-24: 146KB of Rules at 36K Tokens Per Turn

## 1. The Situation

The Kailash COC artifact suite had grown organically over months of development. Rules covering zero-tolerance, autonomous execution, communication style, agent orchestration, independence, git workflow, security, framework-first, and 20+ more topics lived as individual markdown files in `.claude/rules/`. Each file was well-crafted and addressed a real concern. The total size: 146KB of rule files alone, before counting agents, skills, and guides.

Separately, an analysis of Claude Code's internal architecture (the `prompts.ts` source) revealed how Anthropic handles the same problem. Their entire system prompt -- including all behavioral directives, tool instructions, and output formatting -- compiles to approximately 54KB, producing roughly 13,580 tokens. It is baked into the binary, placed in the cacheable prefix of the context window, and contains zero redundancy.

## 2. The Trigger

The token arithmetic made the problem concrete. The 146KB of external COC rules produced approximately 36,561 tokens injected into every turn. Claude Code's baked-in system prompt cost approximately 13,580 tokens. The COC rules alone were 2.7x more expensive than Claude Code's entire system prompt. And rules were only part of the COC artifact suite -- adding agents, skills, and guides pushed the ratio to 5-10x.

The attentional skill is **token-budget-analysis**: translating file sizes into token counts and then into operational consequences. 36K tokens per turn multiplied by 20 turns equals 720K wasted input tokens per session. At $3 per million tokens, that is $2.16 per session just for rules. More critically, 36K tokens of rules plus 13K tokens of base system prompt consume 49K tokens before the conversation starts -- 25% of a 200K context window gone before a single user message. This accelerates compaction (context eviction fires earlier), which means the agent loses conversation history sooner.

## 3. The Move

The move is **external-vs-baked-tradeoff** analysis leading to a hybrid architecture recommendation. The journal entry did not simply report "our rules are too big" -- it decomposed the problem into five distinct costs (token waste, context pressure, compaction acceleration, redundancy, and cache-busting) and then analyzed three architectural options against those costs.

Option A (keep external) preserved ease of update and user visibility but accepted all five costs. Option B (bake everything into the binary, as Anthropic does) solved all five costs but required owning the binary and sacrificed visibility. Option C (hybrid) baked core behavioral directives into the binary's system prompt (cacheable, zero redundancy, ~13K tokens) while keeping project-specific instructions external (~5K tokens via CLAUDE.md) and loading skills and agents on demand rather than always in context. The total system prompt dropped from ~49K tokens to ~18K tokens -- a 63% reduction.

The non-obvious insight was the cache distinction: Anthropic's baked-in prompt sits in the static cacheable prefix (zero marginal cost after the first turn), while external COC rules are injected as dynamic content in the uncached suffix (full cost every turn). Anthropic's employee-only additions (~2K extra tokens via `USER_TYPE === 'ant'` compile-time gates) cost nothing at runtime because they are in the same cached prefix. The COC equivalent (~36K tokens) costs 18x more because it sits in the wrong part of the context window.

## 4. The Outcome

The analysis produced a clear recommendation: Option C (hybrid), with a concrete separation table specifying what gets baked in (core directives, zero-tolerance, communication style), what gets generated dynamically (tool usage instructions, permission guidance), and what stays external but loads on demand (project CLAUDE.md, agent definitions, skills). The 146KB-to-13K distillation was identified as requiring human judgment about which rules are load-bearing versus redundant. The analysis also identified that if rules are baked into the binary, the `/sync` mechanism from the COC template no longer applies for those rules, creating a new alignment challenge.

## 5. The Spec Connection

**CO §13 (Layer 2 -- Context)** defines the institutional-knowledge layer as the persistent context that the agent carries across turns. The 146KB rule suite is Layer 2 content. But §13 does not say "load everything always" -- it says the context layer provides what the agent needs. When the context layer consumes 25% of the available window before work begins, it is undermining its own purpose: the agent has institutional knowledge but insufficient room to apply it. The case shows that Layer 2 design must account for the carrying cost of context, not just its content.

**CO §14 (Master Directive)** defines the top-level instruction that governs agent behavior. In the baked-in model, the master directive is a single crafted prompt with no redundancy. In the external-files model, the master directive is fragmented across 20+ files that overlap (zero-tolerance, no-stubs, and agents rules all restate "fix what you find"). §14 implies that the master directive should be coherent and non-redundant; fragmentation is a design smell, not just a token cost.

**CO §15 (Progressive Disclosure)** requires that information be loaded on demand based on context rather than all at once. The analysis recommended exactly this for skills and agents: load them when invoked, not always in context. The rule files themselves lacked progressive disclosure -- every rule was loaded on every turn regardless of relevance. A progressive-disclosure approach to rules would load security rules only during security-sensitive work and git rules only during commit operations.

**CO §5 (Deterministic Enforcement)** requires that enforcement mechanisms produce consistent results. The token-budget problem creates a subtle enforcement failure: as rules consume more of the context window, the agent is more likely to lose conversation context during compaction, which means earlier instructions may be evicted. Rules that are "always loaded" can paradoxically cause the agent to forget what it was asked to do. Deterministic enforcement requires that the enforcement mechanism (rules) does not degrade the enforcement target (agent behavior).

## 6. Discussion Questions

1. The hybrid approach bakes behavioral rules into the binary, which means they can no longer be updated by editing markdown files and running `/sync`. What governance mechanism replaces `/sync` for baked-in rules? Is the trade-off (cheaper tokens, harder updates) worth it, or does it create a maintenance debt that eventually exceeds the token savings?

2. If you had to reduce the 146KB rule suite to 13K tokens (roughly a 10x compression), which rules would you keep, which would you merge, and which would you drop? What principle distinguishes a rule that must be in the always-loaded prompt from one that can be loaded on demand?

3. The analysis found that many rules overlap ("fix what you find" appears in zero-tolerance, no-stubs, and agents rules). Is redundancy in rules always a cost, or does it serve a reinforcement purpose -- making the agent more likely to follow a directive if it appears in multiple places? How would you test whether removing redundancy degrades compliance?
