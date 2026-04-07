---
atom_id: SC-A-015
name: Structure the Layer 2 institutional knowledge corpus by artifact type
craft_layer: artifact
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §13 — Layer 2 — Context
  - CO §14 — Master Directive
  - CO §39 — Layer 5 — Learning
journal_evidence:
  - path: loom/journal/0002-DECISION-cc-expert-artifacts.md
    polarity: POSITIVE
    quote: "Created 4 new artifact types:\n- **Agent**: `claude-code-architect` — 4-dimension audit (competency, completeness, effectiveness, token efficiency)\n- **Skill**: `30-claude-code-patterns/` — SKILL.md + artifact-design.md + anti-patterns.md + token-budget.md\n- **Rule**: `cc-artifacts.md` — Path-scoped (loads only when editing .claude/ files), 6 MUST + 4 MUST NOT with DO/DO NOT examples\n- **Command**: `/cc-audit` — Launches systematic artifact audit"
  - path: loom/journal/0007-GAP-co-completeness-audit.md
    polarity: NEGATIVE
    quote: "Knowledge exists but is poorly organized. Orphan skills mean knowledge is encoded but not accessible. Unscoped rules mean constraints are present but diluted by noise."
  - path: loom/journal/0015-DECISION-version-tracking-cascade.md
    polarity: POSITIVE
    quote: "- `.claude/VERSION` (JSON) in every repo: own version + upstream version + changelog\n- `version-utils.js` in hooks/lib/: reads VERSION, fetches upstream via curl (3s timeout), compares"
practice_modality: build
status: verified
---

## The move

When building the Layer 2 institutional knowledge corpus for a project, place each piece of knowledge in the correct artifact type based on its load semantics, not its topic. The four artifact types are not interchangeable containers: **rules** are always-loaded behavioural constraints (fire on every turn, or on matching path). **Skills** are on-demand domain knowledge (loaded when the agent invokes the skill or CC's discovery mechanism surfaces it). **Agents** are specialist personas with their own context window (loaded when the agent is delegated to). **Commands** are workflow phases with explicit gates (loaded when the user invokes the slash command). Putting content in the wrong type breaks the load semantics — a skill masquerading as a rule wastes always-in-context tokens; a rule masquerading as a skill loses deterministic enforcement.

## When it fires

When codifying new institutional knowledge — the output of a /codify session, a new rule discovered during implementation, a new domain skill surfaced during analysis. The trigger is the question "where does this knowledge land?" The answer is not "whichever file is open" but a classification decision based on load semantics: should this knowledge be always-present (rule), available-on-demand (skill), persona-scoped (agent), or phase-gated (command)?

## What it serves

CO §13 (Layer 2 — Context) requires that institutional knowledge be structured as "a living institutional handbook" with a hierarchy from master directive to topic-specific files. CO §14 (Master Directive) defines the top of the hierarchy and requires it to be lean. CO §39 (Layer 5 — Learning) requires that observed patterns be captured and evolved into formalised institutional knowledge — but the Learning layer's output must land in the correct Layer 2 artifact type to be usable. A corpus where everything is dumped into rules produces the 0007 finding: "constraints are present but diluted by noise." A corpus where skills are orphaned produces the 0007 finding: "knowledge is encoded but not accessible."

## Evidence walkthrough

- **loom-meta 0002** (POSITIVE) — The CC expert artifacts decision created four new artifacts, each in the correct type: an agent (claude-code-architect) for audit orchestration, a skill directory (30-claude-code-patterns/) for domain reference, a rule (cc-artifacts.md) for always-loaded behavioural constraints, and a command (/cc-audit) for launching the audit workflow. The entry also shows the negative classification decision: a validate-cc-artifacts.js hook was rejected because "it would violate anti-pattern #13 (hooks doing semantic analysis)." The classification was based on load semantics and enforcement mechanism, not topic grouping.
- **loom-meta 0007** (NEGATIVE) — The CO completeness audit found Layer 2 at 50% implementation. The core finding was structural misplacement: 59% of skill files were orphans never referenced by any agent (knowledge encoded but not accessible), many rules lacked path-scoping (loaded globally as noise instead of contextually on demand), and CLAUDE.md duplicated rules already loaded by the rules system. The audit scored each layer independently: L1 (Intent/agents) 70%, L2 (Context/skills) 50%, L3 (Guardrails/rules) 55%, L4 (Instructions/commands) 65%, L5 (Learning) 10%. The low L2 score was not missing knowledge but misplaced knowledge.
- **loom-meta 0015** (POSITIVE) — The version tracking cascade shows Layer 2 artifacts as version-tracked institutional knowledge that persists across syncs. The .claude/VERSION file, version-utils.js in hooks/lib/, and session-start.js integration form a coherent artifact set: the VERSION file is data (not a rule, not a skill), the utility is infrastructure (hooks/lib/), and the session-start integration is a hook that surfaces version information at session start. Each component is placed by its function in the load lifecycle, not by its topic ("versioning").

## How to drill it

Give the practitioner 8 pieces of new institutional knowledge from a /codify session (e.g., "always use parameterised queries," "the DataFlow pool prevention pattern," "the security-reviewer agent's audit protocol," "the /redteam workflow with convergence gate," "kailash-ml has 13 engines," "never commit .env files," "the nexus-specialist handles API deployment," "run tests before commit"). The drill: (a) classify each piece into rule, skill, agent, or command based on load semantics, (b) for each classification, state the load trigger (always, on-demand, delegation, slash-invocation), (c) identify any pieces that are currently misplaced in the practitioner's own project (e.g., a skill-level concept living in a rule file). Score on whether the classification matches the load semantics: "always use parameterised queries" is a rule (always-loaded constraint), "DataFlow pool prevention pattern" is a skill (on-demand reference), "security-reviewer audit protocol" is an agent description, "/redteam workflow" is a command.

## Common failure mode

The practitioner dumps everything into rules because rules are always loaded and therefore "safest" — the knowledge is always present. The cost is the 0007 finding amplified: every new piece of knowledge added as a rule increases the always-in-context token budget, diluting signal with noise and degrading conversation quality. The 0049 experiment (cited in SC-A-009) quantified this at ~48K tokens. The correction is the classification discipline: only behavioural constraints that must fire on every turn belong in rules. Domain reference belongs in skills. Workflow structure belongs in commands. Specialist context belongs in agents.

## Related atoms

- SC-A-009 (progressive disclosure architecture) — the 4-level hierarchy is the vertical structure; this atom covers the horizontal classification into artifact types at each level.
- SC-A-012 (CLAUDE.md as Layer 2 context) — the master directive indexes the corpus; this atom covers how to structure what the directive indexes.
- SC-A-001 (codify-with-explicit-NOT-codified) — the codify decision determines what enters the corpus; this atom determines where in the corpus it lands.
- SC-A-008 (rule classification critical vs advisory) — a sub-classification within the rule artifact type; this atom covers the higher-level classification across all four types.
