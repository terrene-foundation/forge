---
atom_id: SC-A-008
name: Rule classification — critical vs advisory
craft_layer: artifact
destination: co-codegen
craft_areas: [intervene-how]
applications: [COC]
spec_lineage:
  - CO §19 — Layer 3 — Guardrails
  - CO §5 — Deterministic Enforcement Over Probabilistic Compliance
  - CO §20 — Rule Classification
journal_evidence:
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: POSITIVE
    quote: "Several rules we considered essential turned out to be model-native for Claude: Security basics: Parameterized queries, bcrypt, env vars — all model-native. Communication style: PM summaries are naturally outcome-focused."
  - path: loom/journal/0040-DECISION-python-venv-enforcement.md
    polarity: POSITIVE
    quote: "All Python projects MUST use `.venv` at project root, managed by `uv`. Global Python is BLOCKED."
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0011-TRADE-OFF-external-artifacts-vs-baked-in.md
    polarity: NEGATIVE
    quote: "Our external rules alone cost **2.7x more tokens** than Claude Code's entire baked-in system prompt. And that's JUST rules — add agents, skills, guides and we're at 5-10x."
practice_modality: drill
status: draft
---

## The move

Classify every rule in the CC artifact set as either **critical** (must be enforced deterministically by a hook or script outside the context window) or **advisory** (acceptable as probabilistic instruction-following inside the context window). The classification is not a judgment of importance — it is a judgment of what happens when the model forgets. A critical rule is one where a single violation causes irreversible damage (data loss, security breach, broken CI). An advisory rule is one where occasional deviation is tolerable and correctable in review.

## When it fires

When authoring or auditing a rule set. Specifically: (a) when a new rule is added and its enforcement tier has not been declared, (b) when the token budget for always-loaded rules exceeds a threshold (the 0011 entry suggests >30K tokens/turn is the pressure point), and (c) when a rule that was classified as advisory produces a violation pattern showing it should have been critical.

## What it serves

CO §20 (Rule Classification) requires that rules be classified as critical or advisory. CO §5 (Deterministic Enforcement) requires that critical rules be enforced by mechanisms outside the AI's context window. CO §19 (Layer 3 — Guardrails) provides the architectural slot for that enforcement. The classification is the decision that connects the rule to its enforcement mechanism — without it, all rules default to advisory (soft enforcement via rule files) and the guardrails layer is empty.

## Evidence walkthrough

- **loom-meta 0049** (POSITIVE) — The token ablation experiment empirically tested which rules the model follows without enforcement. The discovery that security basics, communication style, and path security were model-native for Claude provides the classification data: those rules can be advisory (or even removed for Claude) because the model's training already covers them. Rules where the model failed without enforcement (zero-tolerance, framework-first, independence) are the critical candidates. The experiment's methodology — clean-room ablation in an empty git repo — is itself the drill for this atom.
- **loom-meta 0040** (POSITIVE) — Python venv enforcement was classified as critical and given a hook (`session-start.js` venv check). The rule does not just instruct — it detects missing `.venv` and warns. This is the pattern: the classification decision (critical) drove the enforcement mechanism (hook), not the other way around. Without the classification, the venv rule would have been another markdown file in `.claude/rules/` that the model might or might not follow.
- **kaizen-cli-rs 0011** (NEGATIVE) — The token cost explosion (146KB external rules = 36K tokens/turn) is the consequence of not classifying. When all rules are treated as always-loaded advisory instructions, the token budget grows without bound. Classification is the mechanism that decides which rules must be in context (advisory) and which can be moved to hooks (critical, no longer needing token budget). The entry's "Option C Hybrid" recommendation is downstream of this classification — you cannot build the hybrid until you know which rules cross which boundary.

## How to drill it

Give the practitioner 10 rules from a real CC artifact set (use the loom/kailash-py rule set). For each rule, the drill: (a) state the consequence of a single violation (irreversible or correctable?), (b) classify as critical or advisory, (c) for each critical rule, name the enforcement mechanism (hook, pre-commit, CI check, or session-start injection), and (d) estimate the token savings if all advisory rules are moved to on-demand loading. Score on whether the classification matches the ablation data from 0049 — if the practitioner classifies a model-native rule as critical, they are over-enforcing; if they classify a non-model-native rule as advisory, they are under-enforcing.

## Common failure mode

Practitioner classifies by importance rather than by failure consequence. A rule about code style feels important and gets classified as critical, consuming a hook slot and enforcement complexity. A rule about never committing secrets feels less urgent (it hasn't happened yet) and gets classified as advisory. The correct classification is the reverse: style drift is correctable in review (advisory), secret leakage is irreversible (critical). The test is not "how much do I care about this rule" but "what happens if the model ignores it once."

## Related atoms

- SC-A-003 (defense-in-depth codification) — critical rules require multiple enforcement mechanisms, which is the next move after classification.
- SC-P-002 (empirical model-vs-rule ablation) — the practitioner skill of running the ablation experiment that produces the classification data.
