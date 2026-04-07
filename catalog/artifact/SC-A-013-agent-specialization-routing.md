---
atom_id: SC-A-013
name: Design agent specialisation routing as explicit Layer 1 Intent
craft_layer: artifact
destination: co-codegen
craft_areas: [intervene-how, institutional-memory]
applications: [COC]
spec_lineage:
  - CO §9 — Layer 1 — Intent
  - CO §11 — Routing Rules
journal_evidence:
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0007-DECISION-capability-development-priorities.md
    polarity: POSITIVE
    quote: "Rather than copying Claude Code feature-for-feature, we prioritize shipping the capabilities that Anthropic gates from external users or leaves as implicit knowledge."
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0022-DECISION-entry-point-wiring-and-subagents.md
    polarity: POSITIVE
    quote: "Sub-agent protocol — src/agents/ module with AgentContext (tokio task-local), BuiltinAgentRegistry (5 agents), spawn_sync/spawn_background execution paths."
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0007-DECISION-codification-scope.md
    polarity: POSITIVE
    quote: "Codification covers 6 PRs across two domains: PACT governance extensions and Kaizen streaming bindings. The scope decision was to update existing skill files rather than create new ones, because the new features are extensions of existing documented patterns."
practice_modality: build
status: verified
---

## The move

When designing a multi-agent system, author the routing decision explicitly and upfront — embedded in agent descriptions, command frontmatter, or rule files — before any agent executes. Every request type must map to exactly one specialist. The routing table is an authored artifact, not an emergent property of the orchestrator's reasoning. A request that matches no routing rule is a signal that a specialist is missing, not that the orchestrator should guess.

## When it fires

When the system has more than one agent and a request must be dispatched. The trigger is the moment of dispatch ambiguity: "which agent handles this?" If that question cannot be answered by reading the routing artifact, the routing artifact is incomplete. Also fires when adding a new capability to the system — the new capability needs a routing entry before it needs an implementation.

## What it serves

CO §9 (Layer 1 — Intent) requires that the implementation "define at least two distinct agent specializations" where "each agent carries domain-specific institutional knowledge." CO §11 (Routing Rules) requires that "the implementation MUST define routing rules that determine which agent handles which task type" and that these rules "SHOULD be explicit and documented." Together, §9 and §11 demand that routing is a first-class authored artifact — not a byproduct of agent descriptions that the orchestrator reverse-engineers at runtime.

## Evidence walkthrough

- **kaizen-cli-rs 0007** (POSITIVE) — The capability development priorities entry explicitly designed routing for 25+ capabilities across 5 phases. Rather than building everything and letting the orchestrator sort it out, each capability was assigned to a phase with explicit prerequisites and a clear ownership path (e.g., "Bash tool with exec policy enforcement" is Phase 1; "Sub-agent spawning via kaizen-agents orchestration" is Phase 3). The routing was the plan; the implementation followed the routing. The entry also shows the routing decision as a strategic choice: "Ship What They Gate" determined which capabilities entered the system at all.
- **kaizen-cli-rs 0022** (POSITIVE) — The entry-point wiring session connected four subsystems (SystemPromptBuilder, MCP bridge, permission extraction, sub-agent protocol) into a production entry point. The sub-agent protocol used a BuiltinAgentRegistry with 5 named agents and two execution paths (spawn*sync, spawn_background). The routing was structural: each subsystem had a defined entry point and a defined dispatch path. The CompositeToolExecutor routed by `mcp*` prefix, making the routing rule explicit and inspectable rather than buried in conditional logic.
- **kailash-rs 0007** (POSITIVE) — The codification scope decision demonstrates routing at the knowledge level: 6 PRs across two domains (PACT (primitives) governance and Kaizen streaming) were routed to existing skill files rather than new ones, because the new features were "extensions of existing documented patterns, not new architectural concepts." This is specialist-routing applied to knowledge artifacts — the question "which skill file owns this PR's documentation?" has the same structure as "which agent owns this request?" and the same answer: explicit, upfront, authored.

## How to drill it

Give the practitioner a system with 4 agents (e.g., code-agent, test-agent, review-agent, deploy-agent) and 10 request types (e.g., "write a function," "run the test suite," "review this PR," "deploy to staging," "add a dependency," "fix a failing test," "refactor this module," "update documentation," "triage this issue," "create a release"). The drill: (a) write a routing table that maps each request type to exactly one agent, (b) identify any request types that do not cleanly map to one agent and propose either splitting the request or adding a specialist, (c) write the routing table as an authored artifact (agent description frontmatter or a dedicated routing rule file). Score on completeness (all 10 types covered), uniqueness (no type mapped to two agents), and the quality of the practitioner's handling of ambiguous cases (e.g., "fix a failing test" could route to code-agent or test-agent — the practitioner must decide and justify).

## Common failure mode

The practitioner writes detailed agent descriptions but no routing rules, assuming the orchestrator will infer routing from the descriptions. This works for obvious cases ("write code" goes to the code agent) but fails for ambiguous cases ("fix a failing test" — is that a code task or a test task?). The orchestrator guesses differently on different runs, producing non-deterministic specialist selection. The fix is the explicit routing table: every request type gets a row, even the obvious ones.

## Related atoms

- SC-B-004 (specialist delegation as required first move) — delegation is the runtime execution of routing; this atom covers the authoring of the routing artifact that delegation follows.
- SC-B-017 (verification-gradient zone assignment) — routing determines which specialist handles the work; zone assignment determines the trust level of that specialist's output.
- SC-A-009 (progressive disclosure architecture) — agent descriptions are index-level artifacts in the 4-level hierarchy; routing rules determine which agent's context gets loaded at the topic level.
- SC-A-012 (CLAUDE.md as Layer 2 context) — the master directive indexes agents and their specialties; the routing artifact is the detailed map that the directive points to.
