---
atom_id: SC-A-009
name: Progressive disclosure architecture for CC artifacts
craft_layer: artifact
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §15 — Progressive Disclosure
  - CO §13 — Layer 2 — Context
  - CO §14 — Master Directive
journal_evidence:
  - path: loom/journal/0003-DISCOVERY-cc-artifact-audit.md
    polarity: NEGATIVE
    quote: "Domain-specific rules loaded globally (~1,300 lines wasted per turn). Rules describe their intended scope in text but lack `paths:` frontmatter."
  - path: loom/journal/0018-DECISION-cc-audit-convergence.md
    polarity: POSITIVE
    quote: "nexus-specialist 497→144 lines (extracted to skills/03-nexus/). kaizen-specialist 411→302 lines (extracted to skills/04-kaizen/)."
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: NEGATIVE
    quote: "COC artifacts consume ~48,000 tokens of always-in-context budget (rules: 37k, CLAUDE.md: 2.2k, agent/skill discovery: 8.5k). For Claude Code Pro users with constrained token budgets, this overhead significantly impacts usable conversation length."
  - path: loom/kz-engage/workspaces/herald/journal/0005-CONNECTION-co-progressive-disclosure-in-herald-responses.md
    polarity: POSITIVE
    quote: "CO's Layer 2 defines 'progressive disclosure' — load minimum context by default, pull deeper reference on demand. Herald's response strategy mirrors this exactly."
practice_modality: drill
status: draft
---

## The move

Structure CC artifacts in a 4-level hierarchy that matches CO's progressive disclosure principle: **master directive** (CLAUDE.md, always loaded, 3-5 directives) --> **index** (rule/agent/skill lists, loaded on demand by CC's discovery mechanism) --> **topic** (individual rule or skill files, loaded when the file path matches or the skill is invoked) --> **deep reference** (guides, specs, and ADRs, loaded only when explicitly drilled into). Every artifact belongs to exactly one level. An artifact at a higher level must never contain content that belongs at a lower level — the master directive does not restate rules, and index-level agents do not embed skill-level detail.

## When it fires

When the total always-loaded token budget exceeds the threshold where context-window pressure degrades conversation quality. The 0049 entry quantifies this as ~48K tokens for a full COC artifact set. The move also fires when an audit (like 0003) discovers that domain-specific content is loaded globally — the symptom is unscoped rules consuming tokens on every turn regardless of task relevance.

## What it serves

CO §15 (Progressive Disclosure) prescribes the hierarchy: "minimum context by default, deeper reference on demand." CO §13 (Layer 2 — Context) is the architectural layer that holds institutional knowledge, and progressive disclosure is its structural principle. CO §14 (Master Directive) defines the top of the hierarchy: the document loaded at the start of every session that establishes baseline rules. The 4-level hierarchy is the operational instantiation of these three spec concepts applied to CC artifacts specifically.

## Evidence walkthrough

- **loom-meta 0003** (NEGATIVE) — The CC artifact audit discovered that domain-specific rules were loaded globally because they lacked `paths:` frontmatter scoping. The finding quantified the waste: ~1,300 lines per turn for rules that only applied when editing specific directories. This is progressive disclosure violated — content at the topic level (domain-specific rules) was being treated as index-level (always loaded). The audit also found CLAUDE.md restating rules already loaded by the rules system, violating the separation between master-directive level and topic level.
- **loom-meta 0018** (POSITIVE) — The audit convergence session resolved the 0003 findings by extracting oversized agents into skills (nexus-specialist 497 to 144 lines, kaizen-specialist 411 to 302 lines). The extraction pattern is progressive disclosure in action: the agent definition (index level) stays small and references the skill (topic level) for detail. The result was 0 CRITICAL, 0 HIGH findings after two rounds — convergence confirmed that the hierarchy holds under audit.
- **loom-meta 0049** (NEGATIVE) — The token ablation experiment quantified the full cost: ~48K tokens of always-in-context budget before the conversation starts. On a 200K context window, that is 24% consumed by instructions. The experiment also produced the per-rule classification data (KEEP, COMPRESS, REMOVE, DEMOTE) that maps directly to the 4-level hierarchy — KEEP rules are master-directive or index level, DEMOTE rules are topic or deep-reference level.
- **kz-engage herald 0005** (POSITIVE) — Herald's response strategy implements progressive disclosure for user-facing output: start with Tier 0 explanation, offer to go deeper, reveal advanced concepts only on request. This entry shows the principle applied beyond CC artifacts to conversational UX — the same hierarchy (minimum by default, deeper on demand) works at both the infrastructure level (artifact loading) and the interaction level (response depth).

## How to drill it

Give the practitioner a flat CC artifact set (15-20 rules, 5-10 agents, all loaded globally). The drill: (a) classify each artifact into one of the four levels, (b) add `paths:` frontmatter to every topic-level rule so it only loads when relevant, (c) extract any agent over 400 lines into an agent stub (index level) plus a skill file (topic level), (d) audit CLAUDE.md for restated rules and remove them, and (e) calculate the before/after always-loaded token count. The 0003 audit findings are the before state; the 0018 convergence is the after state. Score on whether the practitioner achieves a measurable token reduction without removing any institutional knowledge.

## Common failure mode

Practitioner flattens the hierarchy by putting everything at the index level "so it is always available." The motivation is anxiety about the model not finding a rule when it matters. The result is exactly the problem 0049 quantifies: 48K tokens of always-loaded context that degrades conversation quality. The correction is the classification from SC-A-008 — critical rules need enforcement (hooks), not presence (tokens). Advisory rules are the ones that benefit from progressive disclosure because occasional non-loading is tolerable.

## Related atoms

- SC-A-001 (codify-with-explicit-NOT-codified) — the codify decision of what enters institutional knowledge is upstream of where it lands in the hierarchy.
- SC-A-008 (rule classification critical vs advisory) — classification determines which level a rule belongs at: critical rules get hooks (outside the hierarchy entirely), advisory rules get placed in the 4-level hierarchy by relevance frequency.
- SC-P-002 (empirical model-vs-rule ablation) — the ablation experiment produces the data that drives the KEEP/COMPRESS/REMOVE/DEMOTE classification.
