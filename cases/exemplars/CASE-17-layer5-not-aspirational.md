---
case_id: CASE-17
source_path: loom/journal/0006-DECISION-learning-must-work.md
title: "Layer 5 Is Not Aspirational"
atoms_served:
  [institutional-knowledge-mandate, structural-vs-functional-completeness]
spec_concepts: [CO §39, CO §7, CO §42]
craft_layer: artifact
recommended_use: teaching
destination: both
application: COC
---

# CASE-17: Layer 5 Is Not Aspirational

## 1. The Situation

The Kailash project's COC implementation had all five layers of the CO architecture structurally in place. Layer 5 -- the Learning System -- had infrastructure: session-end hooks captured observations to `.claude/learning/observations.jsonl`, an instinct processor extracted patterns, and an evolution pipeline was designed to promote learned patterns into evolved skills. The infrastructure existed, the files were created, the hooks fired on schedule. By any structural checklist, L5 was "done."

An audit of the actual output of this infrastructure revealed a different picture. The observations captured were overwhelmingly infrastructure events (session start, session stop, test patterns) rather than decision points. The instinct processor used file-name matching rather than real pattern analysis, producing hardcoded 0.9 confidence scores. The `/codify` command consumed documentation to produce agents and skills but never read from `.claude/learning/evolved/`. The evolution pipeline had never produced a single evolved skill that made it into canonical skill directories.

## 2. The Trigger

The practitioner confronted the gap between structural completeness and functional completeness. The L5 Learning System was structurally complete -- every component existed, every hook fired, every file was written to. But it was functionally inert: zero useful output. No session was made better by a previous session's learning. The 10x autonomous-execution multiplier described in the project's own methodology document depended on knowledge compounding across sessions, and that compounding was not happening.

The attentional skill here is **structural-vs-functional audit**: recognising that the presence of infrastructure (hooks, files, pipelines) does not imply the presence of function (useful output, behavioral change). The trigger was not a failure -- nothing broke. It was the absence of success in a system designed to produce it.

## 3. The Move

The move is **institutional-knowledge-mandate**: a decision that the L5 Learning System MUST work, rejecting both "simplify" (reduce L5 to something trivially satisfiable) and "remove" (acknowledge L5 as aspirational and delete the infrastructure). The decision declared five specific changes that must happen: `/codify` must consume AND produce learning artifacts; observation capture must target decision points, not infrastructure events; instinct quality must use real confidence scoring instead of hardcoded values; evolution must actually fire and produce artifacts in `.claude/learning/evolved/skills/`; and `/codify` must integrate evolved artifacts into canonical skill directories.

The non-obvious aspect is the rejection of the two easy paths. "Simplify" would have reduced L5 to a checkbox (capture some observations, declare victory). "Remove" would have honestly acknowledged the gap but abandoned the principle. The decision instead held the principle firm and declared the current implementation as debt to be repaid -- real implementation work, not just artifact management.

## 4. The Outcome

The decision established a concrete fix path: rewrite session-end hooks to capture decision-point observations, rebuild the instinct processor with real pattern analysis, and add a learning-integration step to `/codify`. The entry frames this as implementation work, not configuration. The consequences are explicit: hooks need rewriting, `/codify` needs a new step, and the instinct processor needs replacement. The decision converted an invisible non-failure (everything runs, nothing works) into visible debt with a named fix path.

## 5. The Spec Connection

**CO §39 (Layer 5 -- Learning)** defines the fifth layer of the CO architecture as the mechanism by which the system improves over time. The case is a direct test of whether Layer 5 is a real architectural requirement or a decorative aspiration. The decision affirms it as real: if the layer exists in the architecture, it must produce functional output. A structurally present but functionally inert L5 violates the layer's own specification.

**CO §7 (Knowledge Compounds)** is the principle that each session should make the next session better. The audit showed that this principle was not operative -- every session started cold, patterns discovered in one session were lost, and institutional knowledge did not accumulate. The decision directly invokes §7 as the reason why "simplify" and "remove" are rejected: without knowledge compounding, the autonomous execution model cannot achieve its multiplier.

**CO §42 (Knowledge Growth)** addresses the mechanisms by which institutional knowledge grows. The failed instinct processor (hardcoded confidence, file-name matching) represents a knowledge-growth mechanism that exists structurally but produces no actual growth. The decision demands that the mechanism produce real growth: observations that capture decision points, instincts with genuine confidence scores, and evolved skills that reach canonical directories.

## 6. Discussion Questions

1. If the practitioner had chosen "simplify" -- reducing L5 to basic observation capture with no evolution pipeline -- what would the long-term cost be? Consider a project that runs 200 sessions over 6 months: what is the compounding difference between sessions that learn from predecessors and sessions that start cold?

2. The instinct processor produced hardcoded 0.9 confidence scores. Why is a fake confidence score worse than no confidence score at all? What downstream decisions does a consumer of instinct data make differently when confidence is 0.9 vs 0.5 vs "unknown"?

3. The entry says observation capture should target "decision points" rather than "infrastructure events." Define what makes an observation a decision point vs an infrastructure event. Give an example of an observation that looks like a decision point but is actually infrastructure noise, and explain how a processor could distinguish the two.
