---
destination: both
spec: CARE
layer: spec-index
status: v1 enumeration — read-before-cite verified
date: 2026-04-07
source_paths:
  - terrene/foundation/docs/02-standards/care/00-overview.md
  - terrene/foundation/docs/02-standards/care/01-philosophy/*.md
  - terrene/foundation/docs/02-standards/care/02-architecture/*.md
  - terrene/foundation/docs/02-standards/care/03-human-competency/*.md
  - terrene/foundation/docs/02-standards/care/04-governance/*.md
  - terrene/foundation/docs/02-standards/care/05-implementation/*.md
concept_count: 69
contradictions_found: 0
gaps_flagged: 3
---

# CARE Framework: Canonical Concept Index

Enumerated concept list for the CARE standard — one of the four Foundation standards alongside EATP, CO, and PACT. Every concept below is traceable to a specific file and section. This index is used to map journal evidence to spec concepts for the FORGE skill catalog.

**Source path root:** `terrene/foundation/docs/02-standards/care/`

## 00-Overview (Foundational Framework)

### Concept 1: Core Thesis

- **Source**: `00-overview.md`, Section: Core Thesis
- **Definition**: "Trust is human. Execution is shared. The system reveals what only humans can provide."
- **Normative**: CARE answers "what is the human actually for?" by building a shadow enterprise where AI agents execute every role autonomously, not to replace humans but to create a mirror. Organizing principle from which all else derives.
- **Related**: Dual Plane Model, Mirror Thesis, Human-on-the-Loop

### Concept 2: Dual Plane Model

- **Source**: `00-overview.md`, Section: Three Foundational Principles → The Dual Plane Model
- **Definition**: "We propose treating organizations as operating on two conceptually separable planes: the Trust Plane (accountability, authority delegation, values, boundaries)—inherently human—and the Execution Plane (task completion, information processing, coordination)—shareable with AI within human-defined boundaries."
- **Normative**: MUST separate trust decisions from execution structurally to preserve human authority while enabling machine-speed execution. Normative for governance design, not an ontological claim.
- **Related**: Core Thesis, Mirror Thesis, Trust Verification Bridge

### Concept 3: Mirror Thesis

- **Source**: `00-overview.md`, Section: Three Foundational Principles → The Mirror Thesis
- **Definition**: "When you build an AI that can execute a role's tasks, you create a reflection that asks: What does the human in this role contribute that the AI cannot?"
- **Normative**: MUST be used to invest in human development, not merely identify displacement. The six human competency categories are _current_ AI limitations, not principled impossibilities; require empirical validation.
- **Related**: Core Thesis, Six Human Competency Categories

### Concept 4: Human-on-the-Loop (Framework Model)

- **Source**: `00-overview.md`, Section: Three Foundational Principles → Human-on-the-Loop
- **Definition**: "Neither human-in-the-loop (bottleneck) nor human-out-of-the-loop (no accountability): Humans define the operating envelope (trust, constraints, values), AI executes within that envelope at machine speed, humans observe patterns, develop meta-cognitive insight, and refine boundaries."
- **Normative**: MUST provide continuous observation producing continuous refinement. Is aspirational architecture, not guaranteed control.
- **Related**: Core Thesis, Dual Plane, Observation Advantage

### Concept 5: Observation Advantage

- **Source**: `01-philosophy/02-human-on-the-loop.md`, Section: The Observation Advantage
- **Definition**: "Humans freed from process work develop deeper insight than those buried in execution. Observation at distance combined with visibility gives strategic knowledge—why things happen, what patterns emerge across many executions, where system assumptions diverge from reality."
- **Normative**: Pattern recognition across large-scale execution available only to humans-on-the-loop. Not passive observation but active analytical engagement with dataset larger than any human could personally process.
- **Related**: Human-on-the-Loop, Refinement Loop, Elevation Effect

### Concept 6: Trust Verification Bridge

- **Source**: `02-architecture/01-dual-plane.md`, Section: The Trust Verification Bridge
- **Definition**: "Between the two planes sits the Trust Verification Bridge. This is the mechanism that keeps execution anchored to trust. Every significant action in the Execution Plane passes through verification before it takes effect."
- **Normative**: MUST verify every action through gradient: auto-approved (within bounds), flagged (near boundary), held (soft limit exceeded), blocked (hard constraint violated). Not a bottleneck but pre-computed authorization check.
- **Related**: Dual Plane, Constraint Envelopes

## 01-Philosophy

### Concept 7: Principle 1 — Full Autonomy as Baseline

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 1
- **Definition**: "Full autonomy is the default operating mode—not because humans are unnecessary, but because starting from full autonomy reveals what humans uniquely contribute by handling everything else."
- **Normative**: MUST avoid rubber-stamping, competence theater, autonomy absolutism, and gradual creep. Autonomy operates within trust boundaries humans set and maintain.
- **Related**: Principle 2, Human-on-the-Loop

### Concept 8: Principle 2 — Human Choice of Engagement

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 2
- **Definition**: "Humans choose when and how they engage with AI execution, elevating their involvement from rubber-stamping to deliberate judgment. The human is an authority—someone whose engagement is valuable precisely because it is chosen, not compelled."
- **Normative**: MUST avoid notification storms, passive observer, micromanager, and abdication. Choice requires understanding what AI is doing well enough to know when engagement adds value.
- **Related**: Principle 1, Observation Advantage

### Concept 9: Principle 3 — Transparency as Foundation

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 3
- **Definition**: "Every AI action, decision, and reasoning chain is fully visible—not because humans must watch everything, but because the ability to look is what makes the choice not to look an informed one."
- **Normative**: MUST avoid data dumps, explanation theater, selective reveals, and complexity shields. Transparency must be accessible to humans who need it, not raw logs.
- **Related**: Principle 2, Audit Trail

### Concept 10: Principle 4 — Continuous Operation

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 4
- **Definition**: "AI operates continuously within its trust boundaries, providing the sustained attention that humans cannot maintain—and should not have to."
- **Normative**: MUST avoid always-on humans, unsupervised machines, efficiency traps, burnout bypass. Continuity within trust boundaries, not without oversight.
- **Related**: Human-on-the-Loop, Principle 5

### Concept 11: Principle 5 — Human Accountability Preserved

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 5
- **Definition**: "Human accountability is preserved and strengthened, not diluted—because the Trust Plane ensures that every consequential decision traces back to human authority."
- **Normative**: MUST avoid blame machines, accountability voids, phantom authority, committee diffusion. Every AI action must trace to human authority.
- **Related**: Responsibility Model, Accountability Chains

### Concept 12: Principle 6 — Graceful Degradation

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 6
- **Definition**: "When AI encounters the boundaries of its competence, it degrades gracefully—preserving human agency by ensuring the human always has the information and time to exercise judgment."
- **Normative**: MUST avoid cliff edges, cry wolf, silent fade, panic mode. Applies to human unavailability, not just AI competence. Auto-approval of HELD actions is Trust Plane decision with visible audit trail.
- **Related**: Uncertainty Handling, Three Levels of Uncertainty

### Concept 13: Principle 7 — Evolutionary Trust

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 7
- **Definition**: "Trust boundaries evolve based on demonstrated performance, creating a relationship that deepens through evidence rather than assumption."
- **Normative**: MUST avoid trust assumptions, frozen boundaries, ratchet effects, metrics traps. Evolution is ongoing conversation of partnership; each expansion represents human judgment based on evidence.
- **Related**: Principle 1, Trust Chains

### Concept 14: Principle 8 — Purpose Alignment

- **Source**: `01-philosophy/01-first-principles.md`, Section: Principle 8
- **Definition**: "AI execution serves the human-defined purpose of the organization, ensuring that autonomy amplifies human intent rather than replacing it with optimization."
- **Normative**: MUST avoid Goodhart traps, purpose atrophy, values wash, autonomous drift. Purpose defines what gets optimized; values govern constraints on optimization.
- **Related**: Human-on-the-Loop, Workspace as Container

### Concept 15: Human-on-the-Loop (Detailed)

- **Source**: `01-philosophy/02-human-on-the-loop.md`, Section: Model 3 — Human-on-the-Loop (The CARE Model)
- **Definition**: "In human-on-the-loop systems, AI executes continuously within human-defined trust boundaries while humans observe, refine, and intervene based on their judgment. The human is not in the execution chain (not a bottleneck) but is on the loop (an authority with full visibility and intervention capability)."
- **Normative**: MUST preserve human authority, accountability, judgment from human-in-the-loop while achieving operational efficiency. The human is authority over execution chain, not component in it.
- **Related**: Human-on-the-Loop Model, Pilot and Autopilot Metaphor

### Concept 16: Pilot and Autopilot Metaphor

- **Source**: `01-philosophy/02-human-on-the-loop.md`, Section: The Pilot and Autopilot — A Metaphor Expanded
- **Definition**: "The relationship between a pilot and an autopilot system is the closest existing analogy to human-on-the-loop. Pilot does not hand-fly every inch; instead sets flight plan, configures autopilot, monitors systems, makes judgment calls, prepared to take control when judgment demands."
- **Normative**: Pilot's value is not holding yoke steady but recognizing when autopilot's parameters no longer match reality. Intervention timing is itself a valuable skill requiring experience.
- **Related**: Human-on-the-Loop, Observation Advantage

### Concept 17: Refinement Loop

- **Source**: `01-philosophy/02-human-on-the-loop.md`, Section: The Refinement Loop
- **Definition**: "Human observation of AI execution produces insights that improve trust boundaries, which improves AI execution, which produces new insights—ongoing cycle by which human-AI partnership improves through continuous refinement."
- **Normative**: MUST complete cycle: observe, form insight, adjust boundaries, observe new patterns. Compounds over time. Organizations investing in refinement loops develop partnerships extraordinarily difficult for competitors to replicate.
- **Related**: Observation Advantage, Evolutionary Trust

### Concept 18: Elevation Effect

- **Source**: `01-philosophy/02-human-on-the-loop.md`, Section: The Elevation Effect
- **Definition**: "Freed from process work, humans focus on higher-order contributions: from process to purpose, execution to judgment, routine to relationship, individual to systemic."
- **Normative**: Improves both organizational outcomes and human experience of work.
- **Related**: Redefining Work, Human Competency Map

### Concept 19: Trust is Human

- **Source**: `01-philosophy/03-trust-is-human.md`, Section: The Nature of Trust
- **Definition**: "Trust is a human social technology—perhaps the most fundamental—that enables cooperation, delegation, and shared endeavor. Trust belongs to humans not because machines are insufficiently advanced but because trust is constituted by properties that are inherently, irreducibly human."
- **Normative**: MUST NOT attempt to automate trust creation. Requires lived experience, shared vulnerability, social contract participation, cultural context, ethical reasoning. Properties of moral agency, not computational capability.
- **Related**: Verification Distinction, Trust Plane Permanence

### Concept 20: Verification Distinction

- **Source**: `01-philosophy/03-trust-is-human.md`, Section: The Verification Distinction
- **Definition**: "AI can verify trust chains, audit compliance, detect anomalies, maintain trails, report metrics, flag escalations. But verification is not creation—auditing trust compliance is not the same as establishing what constitutes trustworthy behavior."
- **Normative**: MUST distinguish what AI can do (verify, detect, maintain, report, flag, track) from what AI cannot (define, interpret, establish, evaluate, navigate, adapt). Verification is technical; creation is governance.
- **Related**: Trust is Human, Audit Trail

### Concept 21: Trust Plane Permanence

- **Source**: `01-philosophy/03-trust-is-human.md`, Section: The Trust Plane — Where Human Authority Lives
- **Definition**: "The Trust Plane is permanently human because the decisions it contains—who to trust, with what, under what conditions, based on what values—require properties only humans possess."
- **Normative**: MUST remain human even if future AI achieves general intelligence. AI is instrument through which trust is enacted, not party to trust relationship.
- **Related**: Dual Plane, Trust is Human

### Concept 22: Mirror Thesis (Detailed)

- **Source**: `01-philosophy/04-the-mirror-thesis.md`, Section: The Discovery & The Mirror in Action
- **Definition**: "Building an AI system capable of executing all measurable tasks of a human role creates a mirror reflecting back irreducibly human dimensions of work. Before AI, human contributions were invisible—entangled with task execution. AI disentangles them, making visible what was always there but never seen."
- **Normative**: MUST use mirror to invest in human development, not identify displacement. Power asymmetries exist; must implement safeguards: worker consent, data transparency, democratic governance, mirroring managers, equity impact assessment.
- **Related**: Core Thesis, Six Competency Categories, Mirror Power Dynamics

### Concept 23: Ethical Judgment (Competency)

- **Source**: `01-philosophy/04-the-mirror-thesis.md` + `03-human-competency/01-competency-map.md` §1
- **Definition**: "Ethical judgment is the capacity to recognize ethical dimensions of situations, reason about competing moral claims, and make decisions that reflect deeply held values—fairness, dignity, sustainability, community, justice—even when those values conflict with measurable optimization targets."
- **Normative**: AI can be constrained to avoid harmful outputs but cannot experience moral weight of decisions. Is irreducibly human.
- **Related**: Mirror Thesis, Establishing Values

### Concept 24: Relationship Capital (Competency)

- **Source**: `01-philosophy/04-the-mirror-thesis.md` + `03-human-competency/01-competency-map.md` §2
- **Definition**: "Relationship Capital is the accumulated trust, rapport, and mutual understanding built through sustained human-to-human interaction. Knowledge that lives in shared memories, mutual respect, and proven reliability."
- **Normative**: Relationships require reciprocity and vulnerability. Clients form relationships with humans, not systems.
- **Related**: Building Relationships, Mirror Thesis

### Concept 25: Contextual Wisdom (Competency)

- **Source**: `01-philosophy/04-the-mirror-thesis.md` + `03-human-competency/01-competency-map.md` §3
- **Definition**: "Contextual wisdom is the deep understanding of a specific context—its history, politics, unwritten rules, hidden dynamics—that comes from extended immersion and cannot be fully captured in data."
- **Normative**: Experiential knowledge integrated with understanding, colored by experience, refined by reflection.
- **Related**: Mirror Thesis, Navigating Ambiguity

### Concept 26: Creative Synthesis (Competency)

- **Source**: `01-philosophy/04-the-mirror-thesis.md` + `03-human-competency/01-competency-map.md` §4
- **Definition**: "Creative synthesis is the ability to combine ideas from disparate domains in novel ways, generating solutions that could not be reached by extending any single line of reasoning."
- **Normative**: AI can recombine training patterns but cannot make genuinely novel connections between never-before-linked domains. Creates discontinuities, not optimizations.
- **Related**: Creating Novel Solutions, Mirror Thesis

### Concept 27: Emotional Intelligence (Competency)

- **Source**: `01-philosophy/04-the-mirror-thesis.md` + `03-human-competency/01-competency-map.md` §5
- **Definition**: "Emotional Intelligence is the capacity to perceive, understand, and respond to the emotional states of others—and to manage one's own emotional states in service of effective interaction."
- **Normative**: AI can detect emotional signals and respond in trained ways but cannot feel emotions. Absence fundamentally limits genuine empathy.
- **Related**: Caring for People, Mirror Thesis

### Concept 28: Cultural Navigation (Competency)

- **Source**: `01-philosophy/04-the-mirror-thesis.md` + `03-human-competency/01-competency-map.md` §6
- **Definition**: "Cultural Navigation is the capacity to understand, respect, and operate effectively across different cultural contexts—organizational, national, professional, community."
- **Normative**: Culture is water humans swim in, invisible to those inside and incomprehensible to those outside. AI can process cultural data but not inhabit culture.
- **Related**: Mirror Thesis, Navigating Ambiguity

### Concept 29: Mirror Power Dynamics

- **Source**: `01-philosophy/04-the-mirror-thesis.md`, Section: Power Dynamics — Who Controls the Mirror?
- **Definition**: "Management deploys the mirror onto workers, not the reverse, creating power asymmetries: deployment control, category definition, data access, interpretation control."
- **Normative**: MUST implement safeguards: worker consent, data transparency, democratic governance, mirror the managers, equity impact assessment.
- **Related**: Mirror Thesis, Equity Across Dimensions

## 02-Architecture

### Concept 30: Constraint Envelopes (Definition)

- **Source**: `02-architecture/02-constraint-envelopes.md`, Section: What Is a Constraint Envelope
- **Definition**: "A constraint envelope defines the operating boundaries for an AI agent or workspace. It is a multi-dimensional space within which the agent can act autonomously. Actions inside the envelope are pre-authorized. Actions outside require human approval."
- **Normative**: MUST encode human judgment about monetary risk, process boundaries, timing/urgency, information sensitivity, and external interactions. Five dimensions (Financial, Operational, Temporal, Data Access, Communication) checked at execution time via gradient verification.
- **Related**: Trust Verification Bridge, Constraint Inheritance

### Concept 31: Constraint Inheritance

- **Source**: `02-architecture/02-constraint-envelopes.md`, Section: Inheritance Modes
- **Definition**: "Child envelopes inherit from parent envelopes through three modes: Strict (default—child only more restrictive), Additive (child inherits and can add new), Override (child can override with explicit authorization, creating audit trail)."
- **Normative**: MUST enforce strict by default; delegation never accidentally expands authority. Manager cannot grant agent more authority than director granted manager.
- **Related**: Constraint Envelopes, Trust Chains

### Concept 32: Cross-Functional Bridges

- **Source**: `02-architecture/03-cross-functional-bridges.md`, Section: Bridge Types
- **Definition**: "Three types of bridges enable coordination across organizational boundaries: Standing (permanent channels between regularly-interacting units), Scoped (time-limited for specific initiatives), Ad-Hoc (one-time requests)."
- **Normative**: MUST encode organizational wisdom—every standing bridge exists because someone learned what happens when departments act without coordination.
- **Related**: Information Boundaries, Bridge Governance

### Concept 33: Information Boundaries (Bridges)

- **Source**: `02-architecture/03-cross-functional-bridges.md`, Section: Information Boundaries
- **Definition**: "Three sharing modes govern information crossing bridges: Auto-share (crosses automatically when relevant, non-sensitive), Request-share (available but explicitly requested), Never-share (cannot cross under any circumstances)."
- **Normative**: MUST enforce more restrictive rule when bridge rules conflict with constraint envelopes. Errs on side of caution.
- **Related**: Cross-Functional Bridges, Knowledge Ledger

### Concept 34: Bridge Governance and Verification

- **Source**: `02-architecture/03-cross-functional-bridges.md`, Sections: Bridge Verification & Bridge Governance
- **Definition**: "Every bridge crossing verified: bridge exists, action type allowed, information sharing rules followed, approvals required, SLA expectations met. Creation authority varies by type. Bridges reviewed per cadence; dormant bridges retired."
- **Normative**: MUST validate before executing. If conflicts with constraints, more restrictive wins.
- **Related**: Cross-Functional Bridges, Information Boundaries

### Concept 35: Knowledge Ledger (Architecture & Access)

- **Source**: `02-architecture/04-knowledge-ledger.md`, Sections: Flat Access, Hierarchical Authority & What the Knowledge Ledger Contains
- **Definition**: "The Knowledge Ledger is shared knowledge infrastructure implementing: Knowledge is flat (all agents access what they need regardless of origin), Authority is hierarchical (access does not confer authority to act). Contains explicit knowledge and tacit knowledge traces."
- **Normative**: MUST separate knowledge access from decision authority. Best insights come from unexpected connections between disparate information; silos prevent these.
- **Related**: Workspaces, Cross-Functional Bridges

### Concept 36: Knowledge Ledger Provenance and Evolution

- **Source**: `02-architecture/04-knowledge-ledger.md`, Sections: Provenance and Context & Knowledge Evolution
- **Definition**: "Every entry carries provenance: contributor, timestamp, context, basis, confidence, evolution. Knowledge is a living thing; evolves through contribution, corroboration, integration, application, reflection, maturation. Supports contested knowledge—disagreement visible rather than suppressed."
- **Normative**: MUST track versions immutably. Contested knowledge identifies frontiers where human judgment is most needed.
- **Related**: Knowledge Ledger, Workspace Evolution

### Concept 37: Workspaces (Definition and Content)

- **Source**: `02-architecture/05-workspaces.md`, Section: What a Workspace Contains
- **Definition**: "Workspace is an objective container that preserves the connection between human intent and every action taken in service of intent. Contains: objective, constraint envelope, trust chain, execution history, knowledge contributions, decisions and outcomes."
- **Normative**: MUST preserve objective in full human-articulated form. AI can reference, decompose, measure progress against objective but cannot modify it. Objective belongs to Trust Plane.
- **Related**: Dual Plane, Workspace Evolution

### Concept 38: Workspace Evolution

- **Source**: `02-architecture/05-workspaces.md`, Section: Workspace Evolution
- **Definition**: "Five phases: articulation (human defines objective), decomposition (AI analyzes, humans review), execution and learning (work proceeds, assumptions refined), refinement (humans adjust), resolution (achieved/abandoned/transformed)."
- **Normative**: MUST support emergent understanding. Full history preserved for institutional memory.
- **Related**: Workspaces, Refinement Loop

### Concept 39: Uncertainty Handling — Three Levels

- **Source**: `02-architecture/06-uncertainty-handling.md`, Section: Three Levels of Uncertainty
- **Definition**: "Operational Uncertainty (ambiguity within constraint envelope, resolved autonomously), Boundary Uncertainty (approaches constraint edges, executed with flag), Fundamental Uncertainty (outside envelope or insufficient confidence, escalated with full context)."
- **Normative**: MUST distinguish three levels. Different response patterns; different value to humans.
- **Related**: Graceful Degradation, Structured Pipeline

### Concept 40: Uncertainty Structured Pipeline

- **Source**: `02-architecture/06-uncertainty-handling.md`, Section: The CARE Approach
- **Definition**: "Rather than halting, guessing, or ignoring uncertainty, CARE treats it as signal inviting collaboration: gather information autonomously, analyze options, formulate recommendation, check authority, execute or escalate with full context."
- **Normative**: MUST follow pipeline. Human receives contextualized recommendations, not bare questions.
- **Related**: Uncertainty Handling, Graceful Degradation

### Concept 41: Uncertainty as Mirror

- **Source**: `02-architecture/06-uncertainty-handling.md`, Section: Uncertainty as Mirror
- **Definition**: "Aggregate pattern of uncertainty events across organization is a map showing with precision where human judgment is most needed—not where org chart says but where demonstrably, in practice, required."
- **Normative**: MUST surface patterns to humans. Maps to Mirror Thesis; identifies where human-irreplaceable value concentrates.
- **Related**: Mirror Thesis, Uncertainty Handling

### Concept 42: Constraint Resolution (Formal Specification)

- **Source**: `02-architecture/07-formal-specifications.md`, Section: Constraint Resolution Algorithm
- **Definition**: "Constraints collected from multiple sources, merged via priority ordering: explicit deny (highest), regulatory, workspace, role, organizational, permissive default (lowest). Outcome at four levels: auto-approved, flagged, held, blocked."
- **Normative**: MUST use priority resolution when conflicts exist. MUST fail closed if no constraints match. MUST track cumulative state. Drift detection surfaces boundary-testing patterns.
- **Related**: Constraint Envelopes, Trust Verification Bridge

### Concept 43: State Machines (Formal Specification)

- **Source**: `02-architecture/07-formal-specifications.md`, Section: State Machines
- **Definition**: "Specifies state transitions for trust chains (DRAFT→PENDING→ACTIVE↔SUSPENDED→REVOKED/EXPIRED), bridges (PROPOSED→NEGOTIATING→ACTIVE↔SUSPENDED→CLOSED), workspaces (PLANNING→ACTIVE↔BLOCKED→COMPLETED/ABANDONED)."
- **Normative**: MUST log every transition with timestamp, actor, reason. Transitions to REVOKED/SUSPENDED MUST include justification. No transitions from terminal states.
- **Related**: Constraint Envelopes, Workspaces

### Concept 44: Failure Modes (Architectural Resilience)

- **Source**: `02-architecture/08-failure-modes.md`, Section: Failure Modes
- **Definition**: "Five failure modes: Bridge unavailability, Constraint service failure, Trust chain corruption, Workspace inconsistency, Knowledge ledger partition."
- **Normative**: MUST detect within specified latency (<30s bridge, <10s constraints, <1m trust chain, <5m workspaces, <1m partitions). MUST mitigate via design-time (redundancy, graceful degradation, circuit breakers) and runtime (queueing, conservative fallbacks, alerts).
- **Related**: All architectural concepts

## 03-Human Competency

### Concept 45: Human Competency Map (Framework)

- **Source**: `03-human-competency/01-competency-map.md`, Section: Introduction & The Six Categories
- **Definition**: "The Mirror reveals six categories of uniquely human competency: ethical judgment, relationship capital, contextual wisdom, creative synthesis, emotional intelligence, cultural navigation. These are current AI limitations, not permanent boundaries."
- **Normative**: MUST validate empirically. Categories require ongoing validation. Some may prove automatable. Snapshot, not permanent boundary.
- **Related**: Mirror Thesis, Redefining Work

### Concept 46: Role Evolution (Framework)

- **Source**: `03-human-competency/02-role-evolution.md`, Sections: Introduction & The Pattern Across All Roles
- **Definition**: "For professional roles (CEO, Finance Manager, HR, Sales, Engineer), the mirror reveals role deepening. For operational roles (retail, care, manufacturing, driver, admin), the mirror reflection is thinner but not empty. Honest pattern: Mirror shows less in process-execution roles."
- **Normative**: MUST acknowledge uncomfortable finding. Mirror does not guarantee every role can be redesigned. Some roles may have minimal uniquely human contribution.
- **Related**: Mirror Thesis, Competency Map

### Concept 47: Skills Framework — Tier 1 Foundation

- **Source**: `03-human-competency/03-skills-framework.md`, Section: Tier 1
- **Definition**: "Tier 1 capabilities are internal: critical thinking, ethical reasoning, self-awareness."
- **Normative**: MUST be foundation; without it everything above is unstable. Judgment without critical thinking is opinion. Without ethical reasoning is preference. Without self-awareness is projection.
- **Related**: Skills Framework Tiers 2-4

### Concept 48: Skills Framework — Tier 2 Relational

- **Source**: `03-human-competency/03-skills-framework.md`, Section: Tier 2
- **Definition**: "Tier 2 capabilities are interpersonal: empathy, communication, conflict resolution, trust-building."
- **Normative**: Trust is a skill that can be developed and practiced. Creates relationship capital.
- **Related**: Skills Framework Tier 1, Relationship Capital

### Concept 49: Skills Framework — Tier 3 Strategic

- **Source**: `03-human-competency/03-skills-framework.md`, Section: Tier 3
- **Definition**: "Tier 3 capabilities are about pattern recognition: vision-setting, systems thinking, cross-domain synthesis."
- **Normative**: AI excels at optimization within bounds. Humans needed to define bounds, see whole system, imagine transformation.
- **Related**: Skills Framework Tier 1, Creative Synthesis

### Concept 50: Skills Framework — Tier 4 Leadership

- **Source**: `03-human-competency/03-skills-framework.md`, Section: Tier 4
- **Definition**: "Tier 4 capabilities create conditions for others to contribute best: values articulation, accountability modeling, cultural stewardship."
- **Normative**: MUST model values before holding others accountable.
- **Related**: Skills Framework Tiers 1-3, Establishing Values

### Concept 51: Training Implications

- **Source**: `03-human-competency/04-training-implications.md`, Sections: What Schools Should Teach & What Corporate Training Should Focus On
- **Definition**: "MUST shift from knowledge transfer to judgment development. Shift assessment from knowledge mastery to judgment quality—reasoning not correctness, ambiguity tolerance, perspective-taking, revision willingness, ethical dimensions."
- **Normative**: Knowledge abundant in AI age. Bottleneck shifted from knowledge to judgment.
- **Related**: Skills Framework

### Concept 52: Apprenticeship Evolution

- **Source**: `03-human-competency/04-training-implications.md`, Section: How Apprenticeship Models Evolve
- **Definition**: "Traditional progression (observe, assist, perform, master) evolves: observe (AI execution), evaluate (assess outputs), judge (decisions AI cannot), intervene (when/how), design (architect trust plane). At mastery, apprentice becomes architect of governance."
- **Normative**: AI execution sharpens human judgment. Refinement loop creates continuous learning.
- **Related**: Refinement Loop, Observation Advantage

## 04-Governance

### Concept 53: Responsibility Model (Three Domains)

- **Source**: `04-governance/01-responsibility-model.md`, Section: The Three Domains of Responsibility
- **Definition**: "Platform provides capability (infrastructure, audit trails, configuration tools, security, transparency). Organization provides governance (structures, constraints, compliance, training, monitoring). Humans retain accountability (configuration, overrides, escalation responses, governance design)."
- **Normative**: MUST separate clearly. 'AI did it' never acceptable. Neither party can transfer responsibility to other.
- **Related**: Trust Contract, Accountability Chains

### Concept 54: Trust Contract (Responsibility)

- **Source**: `04-governance/01-responsibility-model.md`, Section: The Responsibility Contract
- **Definition**: "Organization accepts: agents act on their behalf, configuration defines behavior, monitoring is duty, consequences are theirs, humans remain in loop. Platform accepts: reliability non-negotiable, transparency mandatory, security foundational, audit integrity absolute, graceful degradation required."
- **Normative**: MUST be honored by both parties. Creates liability boundaries.
- **Related**: Responsibility Model

### Concept 55: Policy Framework (CARE Principles)

- **Source**: `04-governance/02-policy-framework.md`, Section: The CARE Principles as Policy Foundation
- **Definition**: "Key policy argument: regulate the Trust Plane, not Execution Plane. Governance feasible and effective; specific AI capability regulation futile and counterproductive."
- **Normative**: SHOULD regulate Trust Plane (governance, accountability, transparency, oversight). SHOULD NOT regulate specific algorithms or technology stack.
- **Related**: Dual Plane, Regulatory Categories

### Concept 56: Regulatory Categories (Policy)

- **Source**: `04-governance/02-policy-framework.md`, Section: Regulatory Categories
- **Definition**: "SHOULD require: agent registration/disclosure, constraint requirements, audit mandates, transparency obligations."
- **Normative**: SHOULD NOT regulate specific algorithms, technical implementation, execution speed/scale, technology stack.
- **Related**: Policy Framework

### Concept 57: EU AI Act Alignment

- **Source**: `04-governance/03-regulatory-considerations.md`, Sections: The EU AI Act and CARE Alignment & Article-Level Mapping
- **Definition**: "EU AI Act risk-based framework maps to CARE: risk management (constraint envelopes), transparency (audit trails), human oversight (trust plane), conformity assessment (governance documentation). Article 14 (human oversight) aligns deeply."
- **Normative**: MUST implement Articles 9-17 mapping (risk, data, documentation, records, transparency, oversight, robustness, quality).
- **Related**: Regulatory Alignment (NIST, OECD, ISO)

### Concept 58: Ethical Framework — Human Dignity

- **Source**: `04-governance/04-ethical-framework.md`, Sections: The Dignity of Human Work & The Ethics of the Mirror
- **Definition**: "Human beings derive meaning, identity, and social connection from work. Ethical imperative: AI should elevate human contribution, not diminish it. When mirror reveals minimal human contribution, ethical answer is role redesign, not elimination."
- **Normative**: MUST invest in human elevation over replacement. Mirror does not guarantee every role can be redesigned; does guarantee clear seeing.
- **Related**: Mirror Thesis, Role Evolution

## 05-Implementation

### Concept 59: Organization Builder (Purpose)

- **Source**: `05-implementation/01-builder-module.md`, Sections: The Design Exercise & The Builder as Mirror
- **Definition**: "Organization Builder is not setup wizard but design exercise forcing organizations to articulate beliefs about governance, trust, collaboration. Process surfaces gaps, contradictions, inherited assumptions."
- **Normative**: MUST require honesty. Map actual structure, not aspirational. Produces Shadow Enterprise AND organizational self-knowledge.
- **Related**: Constraint Envelopes, Trust Chains

### Concept 60: Builder Process (Five Stages)

- **Source**: `05-implementation/01-builder-module.md`, Section: The Five-Stage Builder Process
- **Definition**: "Stage 1: Map organizational structure. Stage 2: Define roles and responsibilities. Stage 3: Configure trust chains. Stage 4: Set constraint envelopes. Stage 5: Establish bridges."
- **Normative**: Each stage requires human wisdom. All outputs versioned and auditable.
- **Related**: Constraint Envelopes, Cross-Functional Bridges

### Concept 61: Builder Compilation and Deployment

- **Source**: `05-implementation/01-builder-module.md`, Section: Compilation — From Configuration to Shadow Enterprise
- **Definition**: "Compilation produces: shadow agents, trust chains, constraint envelopes, knowledge access policies, bridge configurations. Validates structural integrity before creating. Supports preview and rollback."
- **Normative**: MUST validate before committing. Reversibility respects human authority.
- **Related**: Builder Process

### Concept 62: EATP — Five Elements (from CARE's perspective)

- **Source**: `05-implementation/02-eatp-integration.md`, Section: The Five-Element Trust Lineage
- **Definition**: "Genesis Record, Capability Attestation, Delegation Record, Constraint Envelope, Audit Anchor. Each link verifiable; every link traces to human decision."
- **Normative**: Each element serves specific function. Together create complete trust lineage.
- **Related**: Trust Chains, Constraint Envelopes, EATP spec index

### Concept 63: EATP — Trust Verification Speed

- **Source**: `05-implementation/02-eatp-integration.md`, Section: Trust Verification — Instant Because Human
- **Definition**: "EATP: trust established once (human speed) → action proposed → verification (milliseconds) → executed. Human cognition invested upfront, not at decision time."
- **Normative**: Total verification <35ms for complete trust lineage validation. Not shortcut but reallocation.
- **Related**: EATP Elements, Trust Verification Bridge

### Concept 64: Technical Blueprint — Two-Plane Architecture

- **Source**: `05-implementation/03-technical-blueprint.md`, Section: The Dual Plane Implementation
- **Definition**: "Trust Plane manages configuration, governance, oversight. Execution Plane manages execution, workflows, verification, enforcement, coordination, uncertainty, knowledge."
- **Normative**: MUST maintain separation. Agents cannot modify Trust Plane. Every action in Execution Plane passes verification. Observable by default.
- **Related**: Dual Plane, Technical Patterns

### Concept 65: Technical Pattern 1 — Pre-Established Trust

- **Source**: `05-implementation/03-technical-blueprint.md`, Section: Pattern 1
- **Definition**: "Compile trust artifacts ahead: Builder (minutes to hours) → Compiled artifacts → Runtime verification (milliseconds). Human judgment invested upfront."
- **Normative**: Supports instant verification because trust is pre-established.
- **Related**: Builder Process, EATP

### Concept 66: Technical Pattern 2 — Constraint-Bounded Execution

- **Source**: `05-implementation/03-technical-blueprint.md`, Section: Pattern 2
- **Definition**: "Every action passes four-level verification: auto-approved, flagged, held, blocked. Five constraint dimensions enforced. Cumulative tracking required."
- **Normative**: MUST avoid Goodhart trap. Enforcement tiers available per assurance level needed.
- **Related**: Constraint Envelopes, Technical Pattern 3

### Concept 67: Technical Pattern 3 — Observable Execution

- **Source**: `05-implementation/03-technical-blueprint.md`, Section: Pattern 3
- **Definition**: "Three levels: action logging (full context), execution streaming (real-time view), aggregate analytics (patterns across time/agents/objectives)."
- **Normative**: MUST support all three. Observability architectural, not optional.
- **Related**: Observation Advantage, Competency Map

### Concept 68: Technical Pattern 4 — Knowledge-Flat Architecture

- **Source**: `05-implementation/03-technical-blueprint.md`, Section: Pattern 4
- **Definition**: "Shared knowledge layer all agents query, access controlled by policies: organization-level, unit-level, role-level, cross-unit (through bridges). Flat access; hierarchical authority."
- **Normative**: MUST ensure agents access organizational context without creating silos.
- **Related**: Knowledge Ledger, Cross-Functional Bridges

### Concept 69: Technical Pattern 5 — Workspace-Centric Design

- **Source**: `05-implementation/03-technical-blueprint.md`, Section: Pattern 5
- **Definition**: "All work organized around objectives in workspaces. Contains constraint inheritance, knowledge access, agent assignments, memory/artifacts, progress tracking. Supports nesting."
- **Normative**: MUST NOT allow ambient execution. Everything in workspace context.
- **Related**: Workspaces, Workspace Evolution

---

## Summary

**Total concepts enumerated:** 69
**Contradictions found:** 0 (CARE is internally consistent across all 25+ files)

### Gaps flagged (for future spec work, not FORGE v1)

1. **Operational metrics for human contribution** — CARE identifies what humans should contribute but provides limited guidance on measuring relationship depth, creative contribution, or ethical leadership operationally. FORGE will need assessment rubrics.
2. **Organizational-level transition economics** — Framework acknowledges displacement is real and reskilling expensive but underspecifies how organizations budget and plan capability development during deployment.
3. **Role-specific constraint templates** — While constraint templates are mentioned, no concrete calibrated envelopes provided for specific roles.

### Load-bearing foundational concepts

- **Mirror Thesis** (Concept 3, 22) — the central claim; everything else elaborates or operationalizes
- **Dual Plane Model** (Concept 2) — the structural separation that enables everything else
- **Responsibility Model** (Concept 53-54) — the linchpin; if governance is weak, all CARE promises fail
- **Trust is Human** (Concept 19, 21) — the claim that prevents any drift toward automating trust itself
