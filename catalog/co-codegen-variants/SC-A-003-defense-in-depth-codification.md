---
atom_id: SC-A-003
variant_of: SC-A-003
name: Defense-in-depth codification across multiple artifact locations
craft_layer: artifact
destination: co-codegen
craft_areas: [institutional-memory, attend-how]
applications: [COC]
spec_lineage:
  - CO §5.4 — Defense in Depth
  - CO §19 — Layer 3 — Guardrails
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash/journal/0008-DECISION-codify-engine-layer-three-layer-model.md
    polarity: POSITIVE
    quote: "11 COC artifacts created or updated at the kailash/ source of truth to establish the three-layer model (Raw → Primitives → Engine) and engine-first principle as institutional knowledge:"
  - path: loom/kailash-rs/workspaces/v3.8-issues/journal/0004-CONNECTION-auth-patterns-codified.md
    polarity: POSITIVE
    quote: "9 Nexus auth middleware patterns (A1-A9) extracted from v3.8 red team findings and added to:\n1. **security-reviewer agent** (`.claude/agents/security-reviewer.md`) — Section 10: Nexus Auth Middleware Patterns with checklist items A1-A9\n2. **security-patterns skill** (`.claude/skills/18-security-patterns/SKILL.md`) — Rust Auth Middleware Patterns section with code examples"
  - path: loom/journal/0007-GAP-co-completeness-audit.md
    polarity: NEGATIVE
    quote: "CC artifact quality rules are PROMPT-enforced, not DETERMINISTICALLY enforced. validate-workflow.js blocks stubs but nothing blocks a 500-line agent or 300-line command."
practice_modality: drill
status: verified
---

## The move

When codifying a critical rule or principle, land it in multiple independent artifact locations simultaneously — rules, skills, agents, hooks, and commands — so that no single weak link bypasses enforcement. Count the distinct enforcement surfaces before declaring codification complete. A rule that lives in only one artifact type has zero redundancy; the first session that does not load that artifact type silently loses the constraint.

## When it fires

Every codify session that produces a new rule or updates an existing one. The trigger is the transition from "pattern identified" to "pattern encoded" — the moment the practitioner decides where the artifact lands. If the answer is one location, this move has not yet fired.

## What it serves

CO §5.4 (Defense in Depth) specifies that critical rules should have multiple independent enforcement layers — the spec recommends five or more independent mechanisms for highest-risk rules. CO §19 (Layer 3 — Guardrails) establishes that enforcement must operate outside the AI's context window, independent of whether the AI remembers the rule. Landing a rule in multiple artifact types is the implementation pattern: a rule file provides the constraint text, a hook provides deterministic blocking, a specialist agent provides contextual guidance, and a skill provides the teaching material.

## Evidence walkthrough

- **kailash-py 0008** (POSITIVE) — The three-layer model codification landed in 11 artifacts across five artifact categories (1 new rule, 1 new skill, 3 updated specialists, 1 updated advisor, 1 updated skill, 1 updated patterns doc, 2 updated SKILL.md files, 1 manifest update). The entry explicitly considered and rejected both a rule-only approach ("rules establish 'what to do' but specialists establish 'how to think about it'") and a skill-only approach ("skills are loaded on-demand; rules and specialist agents load contextually"). The multi-artifact landing was a deliberate defense-in-depth choice.
- **kailash-rs 0004** (POSITIVE) — The 9 auth middleware patterns (A1-A9) landed in two artifact types simultaneously: the security-reviewer agent (checklist items) and the security-patterns skill (code examples). Each artifact type serves a different enforcement surface — the agent provides review-time checking, the skill provides implementation-time guidance.
- **loom-meta 0007** (NEGATIVE) — The CO completeness audit found that cc-artifacts quality rules (agent description limits, command line limits, rule scoping) existed only as prompt-enforced rules, not as hook-enforced deterministic checks. The entry records the consequence: "validate-workflow.js blocks stubs but nothing blocks a 500-line agent or 300-line command." The rule existed in one artifact type (rule file) but not in a second (hook), so enforcement was probabilistic, not deterministic.

## How to apply

When codifying a new rule or principle, enumerate every artifact type it should land in (rule file, hook, specialist agent, skill, command step) and confirm each landing independently. Count the distinct enforcement surfaces: a rule in one artifact type has zero redundancy and is probabilistic. Use the heuristic that the most important landing is the one that operates outside the model's context window (hooks, deterministic validators) because that is the landing the model cannot skip.

## Common failure mode

Practitioner codifies into the artifact type they are most familiar with — usually a rule file — and stops. The codification looks complete because the rule text is good. But when a session does not trigger that rule (wrong file scope, context window overflow, or the rule is advisory not deterministic), the constraint vanishes. The cure is the count: "How many independent enforcement surfaces does this rule have?" If the answer is one, the codification is incomplete regardless of the quality of the text.

## Related atoms

- SC-A-001 (codify-with-explicit-NOT-codified) — the complementary move: what was excluded from codification and why. Defense-in-depth codification is about breadth of landing; NOT-codified is about precision of exclusion.
- SC-A-004 (Layer 3 hook security audit) — hooks are one of the independent enforcement surfaces; this atom ensures the hook is present, SC-A-004 ensures the hook itself is secure.
- SC-A-005 (encapsulation as security boundary) — encapsulation is a struct-level enforcement surface that complements rule-level and hook-level surfaces in the defense-in-depth stack.
- SC-A-006 (negative tests for deny-by-default) — each enforcement surface in the defense-in-depth stack needs negative testing to verify it actually blocks the thing it claims to block.
- SC-A-007 (zero-config security audit) — zero-config paths are the enforcement surface most likely to be missing from the defense-in-depth stack because they bypass explicit configuration.
- SC-B-002 (authority-chain escalation) — when the multi-artifact landing crosses repo boundaries, the authority-chain escalation move determines which repo owns each landing location.
