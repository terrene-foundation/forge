---
case_id: CASE-21
source_path:
  - loom/journal/0005-GAP-redteam-audit-gaps.md
  - loom/journal/0011-RISK-redteam-r1-findings.md
title: "The Orphan Audit That Was Wrong"
atoms_served:
  [audit-categorization-error-detection, progressive-disclosure-awareness]
spec_concepts: [CO §15, CO §49, CO §20]
craft_layer: artifact
recommended_use: discussion
destination: both
application: COC
---

# CASE-21: The Orphan Audit That Was Wrong

## 1. The Situation

The Kailash Python SDK's `.claude/` directory contained 350 skill files organized into numbered directories (01-core-sdk through 30-claude-code-patterns). A red team pass was auditing the CC artifact suite for quality and completeness. One audit goal was to determine whether skill files were actually being used or whether they were dead weight consuming token budget and maintenance attention.

The initial audit (journal/0003) had already surfaced some findings about orphan frontend skills. A deeper red team pass (journal/0005) went further: it counted every skill file that was not explicitly referenced by any agent or command file and reported the result.

## 2. The Trigger

The red team reported that 204 of 350 skill files -- 59% -- were orphans. "Never referenced by any agent or command." The number was alarming: more than half the skill library appeared to be dead code. The audit recommended a full orphan-skill audit and cleanup as TODO-3.1 in the implementation plan.

But the finding came with a question the auditors themselves flagged: "The 59% orphan skill rate could mean skills are designed for progressive disclosure (agent reads SKILL.md, then discovers sub-files). If so, is it a problem that agents don't explicitly reference them? Or does it mean 204 files are never read?" The attentional skill is **audit-categorization-error-detection** -- recognizing that a metric (orphan count) depends on the categorization method (what counts as "referenced"), and that the categorization method may not fit the architecture.

## 3. The Move

The move came during the M2-M5 red team round (journal/0011). When the implementation plan's TODO-3.1 reached execution, the red team re-examined the premise. The numbered skill directories are not referenced by name in agent files because they do not need to be. Claude Code's skill system discovers them via SKILL.md trigger descriptions -- agents do not explicitly reference skill files the way functions call other functions. The skills are loaded on demand when the model's context matches a trigger description.

This is **progressive-disclosure-awareness** combined with **audit-categorization-error-detection**: the initial audit applied a static-reference model (grep for filenames in agent files) to a system that uses dynamic discovery (SKILL.md triggers matched at runtime). The 204 "orphans" were the entire distilled Kailash documentation library -- the most valuable part of the skill suite. TODO-3.1 was deleted. The journal records the correction explicitly: "TODO-3.1 ('audit 204 orphan skill files') was based on a flawed premise. The numbered skill directories ARE the distilled Kailash documentation. They are discovered by Claude Code's skill system via SKILL.md trigger descriptions -- agents don't need to explicitly reference them by name. These skills are NOT orphans."

## 4. The Outcome

The orphan audit finding was fully retracted. No skill files were deleted. Had TODO-3.1 been executed without re-examination, the team would have spent a session auditing 204 files that were functioning correctly, and potentially deleted or archived the SDK's core documentation-as-skill library. The cost of the error was one false finding carried across two journal entries; the cost of not catching it would have been destruction of working infrastructure.

## 5. The Spec Connection

**CO §15 (Progressive Disclosure)** defines the principle that not all information needs to be loaded at once -- artifacts can be discovered and loaded on demand based on context. The skill system's SKILL.md trigger mechanism is a direct implementation of progressive disclosure. The initial audit failed because it measured "reference" using a static, eager model (explicit imports) when the architecture uses a dynamic, lazy model (trigger-based discovery). Any audit of a progressive-disclosure system that uses static-reference counting will systematically misclassify on-demand artifacts as orphans.

**CO §49 (Enforcement Reliability)** requires that enforcement mechanisms (including audits) actually measure what they claim to measure. The orphan audit claimed to measure "unused files" but actually measured "files not statically referenced by name." Those are different properties in a progressive-disclosure architecture. An unreliable audit is worse than no audit -- it produces false confidence that triggers destructive action.

**CO §20 (Rule Classification)** distinguishes between rules that are always loaded and rules that are conditionally loaded. The skill files in question are conditionally loaded (on-demand via trigger match). Classifying them alongside always-loaded artifacts and applying the same "must be explicitly referenced" criterion is a category error. Rule classification requires understanding the loading semantics before assessing presence or absence.

## 6. Discussion Questions

1. If the red team had not flagged the progressive-disclosure question in journal/0005 ("could mean skills are designed for progressive disclosure"), and TODO-3.1 had been executed as a cleanup task, what would the likely outcome have been? How many of the 204 files would have been deleted before someone noticed that skills were disappearing from Claude Code's on-demand repertoire?

2. The initial audit used "grep for filename in agent/command files" as the definition of "referenced." What alternative audit method would correctly account for progressive-disclosure loading? Is there a general principle for choosing audit methods that match the architecture's loading semantics?

3. The red team both created the false finding (journal/0005) and retracted it (journal/0011). Is self-correction sufficient, or should a different agent role have validated the orphan claim before it became a TODO? What is the cost of carrying a false finding across multiple sessions versus the cost of adding a validation step to every audit finding?
