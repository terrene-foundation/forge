---
destination: both
spec: CO
layer: spec-index
status: v1 enumeration — read-before-cite verified
date: 2026-04-07
source_paths:
  - terrene/foundation/docs/02-standards/co/01-first-principles.md
  - terrene/foundation/docs/02-standards/co/02-five-layer-architecture.md
  - terrene/foundation/docs/02-standards/co/03-process-model.md
  - terrene/foundation/docs/02-standards/co/04-application-template.md
  - terrene/foundation/docs/02-standards/co/applications/co-for-compliance.md
  - terrene/foundation/docs/02-standards/co/applications/co-for-education.md
  - terrene/foundation/docs/02-standards/co/applications/co-for-finance.md
  - terrene/foundation/docs/02-standards/co/applications/co-for-governance.md
  - terrene/foundation/docs/02-standards/co/applications/co-for-learners.md
  - terrene/foundation/docs/02-standards/co/applications/co-for-research.md
  - terrene/foundation/docs/02-standards/co/workflows/01-governance-workflow.md
  - terrene/foundation/docs/02-standards/co/workflows/02-standards-workflow.md
  - terrene/foundation/docs/02-standards/co/workflows/03-strategy-workflow.md
  - terrene/foundation/docs/02-standards/co/workflows/04-research-workflow.md
  - terrene/foundation/docs/02-standards/publications/COC-Core-Thesis.md
concept_count: 187
contradictions_found: 1
application_identifier_note: |
  CO applications are identified by the canonical filename stem
  (e.g. "co-for-codegen", "co-for-research", "co-for-education").
  Short codes used in this document ("COC", "COR", "COE") are canonical
  only for codegen/research/education. Other applications — compliance,
  finance, governance, learners — are referred to by their filename stem.
  Earlier FORGE framings invented non-existent short codes ("COComp" for
  complexity, "COL" for law); those codes have been deleted from the FORGE
  scope rule. Any remaining short codes ("COComp", "COL-F", "COL" for
  learners) in text below are provisional labels used by the enumerating
  agent and should be read as references to the canonical filenames.
verified:
  co_principles_count: 8
  co_layers_count: 5
  canonical_commands:
    ["/analyze", "/plan", "/execute", "/vet", "/codify", "/deliver"]
---

# CO Specification Concept Index

Enumerated concept list for the CO (Cognitive Orchestration) standard — the domain-agnostic base methodology, one of the four Foundation standards alongside CARE, EATP, and PACT. This index covers CO core (first principles, architecture, process model, application template) plus all seven formalised domain applications (codegen, compliance, education, finance, governance, learners, research) and the four canonical workflow guides.

**Source path root:** `terrene/foundation/docs/02-standards/co/` (except COC which lives at `terrene/foundation/docs/02-standards/publications/COC-Core-Thesis.md`)

**Confirmed:**

- 8 CO First Principles (names listed below)
- 5 Architectural Layers (Intent / Context / Guardrails / Instructions / Learning)
- 6 canonical command names (`/analyze`, `/plan`, `/execute`, `/vet`, `/codify`, `/deliver`)
- Phase 05 renamed from "Learn" to "Codify" in v1.2 (2026-04-07) to describe what actually happens

**Contradiction flagged:** co-for-compliance status — application document says "Proposed (not implemented)" while README claims "Sketch complete". Noted for spec maintainers; does not affect FORGE curriculum.

## CO Core Specification

### Eight First Principles

#### Concept 1: Institutional Knowledge Thesis

- **Source**: `01-first-principles.md`, `## Principle 1`
- **Definition**: "The primary determinant of AI output quality is not model capability but the institutional context surrounding the model."
- **Normative**:
  - MUST provide mechanisms to encode organization-specific institutional knowledge in machine-readable form
  - MUST distinguish institutional knowledge (organization-specific) from generic domain knowledge
  - SHOULD structure for progressive disclosure: minimum context by default, deeper reference on demand
- **Related**: Layer 2 (Context), Master Directive, Progressive Disclosure

#### Concept 2: Brilliant New Hire Principle

- **Source**: `01-first-principles.md`, `## Principle 2`
- **Definition**: "An AI system without organizational context is functionally equivalent to a brilliant new hire on their first day — capable but contextless, productive but undisciplined, fast but unaware of what burned you last quarter."
- **Normative**:
  - MUST treat AI onboarding as first-class concern, not secondary optimization
  - MUST make onboarding architecture persistent across sessions and practitioners
  - SHOULD document specific institutional knowledge gaps
- **Related**: Three Failure Modes, Layer 1 (Intent)

#### Concept 3: Three Failure Modes

- **Source**: `01-first-principles.md`, `## Principle 3`
- **Definition**: "AI systems operating without institutional context fail along three predictable fault lines: amnesia, convention drift, and safety blindness."
- **Normative**:
  - MUST address all three failure modes; addressing fewer is not CO-conformant
  - Amnesia MUST be addressed through mechanisms surviving context window compression
  - Convention drift MUST be addressed through explicit institutional knowledge encoding
  - Safety blindness MUST be addressed through deterministic enforcement
- **Related**: Amnesia, Convention Drift, Safety Blindness, Layer 3 (Guardrails)

#### Concept 3.1: Amnesia (Failure Mode)

- **Source**: `01-first-principles.md`, Principle 3 Definitions
- **Definition**: "The AI forgets instructions as the context window fills. Critical rules established early in a session are evicted or deprioritized as new content accumulates. The AI reverts to training-data patterns without warning."
- **Normative**: MUST be addressed through mechanisms surviving context window compression (see Principle 5)
- **Related**: Anti-Amnesia, Context Window

#### Concept 3.2: Convention Drift (Failure Mode)

- **Source**: `01-first-principles.md`, Principle 3 Definitions
- **Definition**: "The AI follows generic practices (from training data) instead of organizational conventions. When organizational conventions conflict with internet-prevalent conventions, the AI follows the internet."
- **Normative**: MUST be addressed through explicit institutional knowledge encoding (Principle 1)
- **Related**: Institutional Knowledge, Framework-First

#### Concept 3.3: Safety Blindness (Failure Mode)

- **Source**: `01-first-principles.md`, Principle 3 Definitions
- **Definition**: "The AI optimizes for the most direct path to task completion. Safety measures (approval workflows, conservative interpretations, regulatory compliance steps) are never the most direct path. The AI deprioritizes them unless explicitly enforced."
- **Normative**: MUST be addressed through deterministic enforcement of critical safety rules (Principle 5)
- **Related**: Layer 3 (Guardrails), Critical Rules

#### Concept 4: Human-on-the-Loop

- **Source**: `01-first-principles.md`, `## Principle 4`
- **Definition**: "The optimal position for humans in AI-augmented work is neither in-the-loop (approving every action) nor out-of-the-loop (absent from the process), but on-the-loop: defining the operating envelope, observing execution patterns, and refining boundaries based on evidence."
- **Normative**:
  - MUST define structured points where human judgment is exercised
  - SHOULD operate autonomously between gates within defined envelope
  - MUST NOT require human approval for every action (in-the-loop default)
  - MUST NOT allow AI to operate without human-defined boundaries (out-of-the-loop)
- **Related**: Approval Gates, Layer 4 (Instructions), Envelope

#### Concept 5: Deterministic Enforcement Over Probabilistic Compliance

- **Source**: `01-first-principles.md`, `## Principle 5`
- **Definition**: "Critical organizational rules MUST be enforced deterministically, outside the AI's context window, independent of whether the AI 'remembers' the rule."
- **Normative**:
  - MUST distinguish critical vs advisory rules
  - Critical rules MUST be enforced by mechanisms operating outside the AI model's context window
  - SHOULD employ defense in depth: multiple independent enforcement mechanisms
  - Anti-amnesia mechanisms MUST re-inject critical rules; RECOMMENDED frequency is every user interaction
  - MUST NOT rely solely on AI's probabilistic instruction-following for critical rules
- **Related**: Hard Enforcement, Soft Enforcement, Anti-Amnesia, Defense in Depth

#### Concept 5.1: Hard Enforcement

- **Source**: `01-first-principles.md`, Principle 5 Definitions
- **Definition**: "Deterministic scripts, hooks, or system-level mechanisms that intercept and block rule violations before they take effect. These operate outside the AI model's context window."
- **Related**: Layer 3 (Guardrails), Soft Enforcement, Defense in Depth

#### Concept 5.2: Soft Enforcement

- **Source**: `01-first-principles.md`, Principle 5 Definitions
- **Definition**: "Directive-based rules that the AI reads, interprets, and follows probabilistically. Effective most of the time, but subject to amnesia and drift."
- **Related**: Hard Enforcement, Advisory Rules, Rule Files

#### Concept 5.3: Anti-Amnesia

- **Source**: `01-first-principles.md`, Principle 5 Definitions
- **Definition**: "A mechanism that fires on a regular cadence (RECOMMENDED: every user interaction), re-injecting critical rules fresh into the AI's context. It survives context window compression because it runs outside the model's memory."
- **Normative**: MUST re-inject critical rules at frequency sufficient to prevent drift
- **Related**: Context Window, Layer 3 (Guardrails)

#### Concept 5.4: Defense in Depth

- **Source**: `01-first-principles.md`, Principle 5 Definitions
- **Definition**: "Multiple independent enforcement layers for a single critical rule. RECOMMENDED: five or more independent enforcement mechanisms for the highest-risk rules."
- **Normative**: SHOULD employ multiple independent mechanisms; RECOMMENDED minimum is five for highest-risk rules
- **Related**: Critical Rules, Hard Enforcement

#### Concept 6: Bainbridge's Irony Applied

- **Source**: `01-first-principles.md`, `## Principle 6`
- **Definition**: "The more AI handles routine execution, the more critical it becomes that humans maintain deep understanding of the work being executed."
- **Normative**:
  - SHOULD be designed so building/maintaining layers deepens practitioner expertise
  - SHOULD require articulation of institutional knowledge that would otherwise remain tacit
  - SHOULD include mechanisms to detect whether practitioners are becoming more/less capable of independent judgment
- **Related**: Five-Layer Architecture, Practitioner Expertise

#### Concept 7: Knowledge Compounds

- **Source**: `01-first-principles.md`, `## Principle 7`
- **Definition**: "Institutional knowledge captured in machine-readable form compounds across sessions, practitioners, and time periods. Each successful interaction makes the next one more effective."
- **Normative**:
  - MUST include mechanisms to capture observations (Layer 5)
  - Captured observations MUST be reviewed by human before formalization
  - SHOULD demonstrate measurable knowledge growth over time
  - MUST NOT auto-promote AI-generated patterns to institutional knowledge without approval
- **Related**: Layer 5 (Learning), Observe-Capture-Evolve

#### Concept 8: Authentic Voice and Responsible Co-Authorship

- **Source**: `01-first-principles.md`, `## Principle 8`
- **Definition**: "CO exists to produce genuinely excellent work through human-AI collaboration. The human directs; the AI assists. The output reflects the human's intellectual contribution, not the tool that helped produce it. CO practitioners disclose AI assistance per venue and institution requirements. They do not conceal it."
- **Normative**:
  - MUST produce output reflecting genuine human intellectual direction
  - MUST disclose AI assistance per venue/institution/regulatory requirements
  - SHOULD include mechanisms creating auditable trail of human intellectual engagement
  - Writing-style constraints exist to reduce detection false positives (detection bias mitigation, not concealment)
- **Related**: Human Intellectual Direction, Disclosure, Detection Bias Mitigation

### Five-Layer Architecture

#### Concept 9: Layer 1 — Intent

- **Source**: `02-five-layer-architecture.md`, `## Layer 1: Intent`
- **Definition**: "Layer 1 routes tasks to domain-specialized agents, each configured with institutional knowledge appropriate to a specific area of work."
- **Normative**:
  - MUST define at least two distinct agent specializations (L1-1)
  - Each agent MUST carry domain-specific institutional knowledge (L1-2)
  - SHOULD assign model tiers appropriate to cognitive demands (L1-3)
  - MUST define routing rules (L1-4)
  - SHOULD document routing rules explicitly (L1-5)
  - SHOULD prevent agents from operating outside their specialty without explicit delegation (L1-6)
- **Related**: Agent Definition, Routing Rules, Model Tier

#### Concept 10: Agent Definition

- **Source**: `02-five-layer-architecture.md`, Layer 1 / `04-application-template.md`
- **Definition**: A specification comprising name, specialty, domain knowledge references, tools, and model tier that enables an agent to perform domain-specific work.
- **Normative**: Each agent MUST carry domain-specific institutional knowledge
- **Related**: Layer 1, Routing Rules, Model Tier

#### Concept 11: Routing Rules

- **Source**: `02-five-layer-architecture.md`, Layer 1 Normative Requirements
- **Definition**: "The implementation MUST define routing rules that determine which agent handles which task type."
- **Normative**: MUST define; SHOULD be explicit and documented
- **Related**: Layer 1, Agent Definition

#### Concept 12: Model Tier

- **Source**: `02-five-layer-architecture.md`, Layer 1 Normative Requirements
- **Definition**: "Agents SHOULD be assigned model tiers appropriate to their cognitive demands (reasoning-heavy vs speed-critical)."
- **Related**: Layer 1, Agent Definition

#### Concept 13: Layer 2 — Context

- **Source**: `02-five-layer-architecture.md`, `## Layer 2: Context`
- **Definition**: "Layer 2 replaces the AI's generic training-data defaults with the organization's living institutional handbook — conventions, frameworks, architectural decisions, and hard-won lessons made machine-readable."
- **Normative**:
  - MUST include master directive document loaded at start of every session (L2-1)
  - Master directive MUST establish baseline rules, framework preferences, non-negotiable conventions (L2-2)
  - SHOULD follow progressive disclosure (L2-3)
  - Each piece of institutional knowledge MUST have single authoritative source (L2-4)
  - SHOULD prefer composition over creation (Framework-First) (L2-5)
  - Context documents MUST be maintained as living documents (L2-6)
  - SHOULD structure knowledge in hierarchy: master directive → quick-reference indexes → topic-specific files → deep reference (L2-7)
- **Related**: Master Directive, Progressive Disclosure, Framework-First, Single Source of Truth

#### Concept 14: Master Directive

- **Source**: `02-five-layer-architecture.md`, Layer 2 Normative Requirements
- **Definition**: "The implementation MUST include a master directive document loaded at the start of every session."
- **Normative**: MUST establish baseline rules, framework preferences, non-negotiable conventions
- **Related**: Layer 2, Progressive Disclosure

#### Concept 15: Progressive Disclosure

- **Source**: `02-five-layer-architecture.md`, Layer 2 Normative Requirements
- **Definition**: "Institutional knowledge SHOULD follow progressive disclosure: minimum context by default, deeper reference on demand."
- **Normative**: SHOULD structure hierarchy master → index → topic → deep reference
- **Related**: Layer 2, Master Directive, Context Window

#### Concept 16: Framework-First

- **Source**: `02-five-layer-architecture.md`, Layer 2 Sub-Principles
- **Definition**: "Before building from scratch, check whether existing organizational building blocks already handle the task. The principle is composition over creation."
- **Normative**: SHOULD prefer composition of existing organizational building blocks
- **Related**: Layer 2, Single Source of Truth

#### Concept 17: Single Source of Truth

- **Source**: `02-five-layer-architecture.md`, Layer 2 Sub-Principles
- **Definition**: "Each piece of institutional knowledge lives in exactly one place, with no contradictions. When a convention changes, it changes in one location."
- **Normative**: Each piece of institutional knowledge MUST have a single authoritative source
- **Related**: Layer 2, Framework-First

#### Concept 18: Context Engineering

- **Source**: `02-five-layer-architecture.md`, Layer 2 Distinction section
- **Definition**: "Context engineering builds organizational knowledge that persists across every interaction, with every practitioner, across every session."
- **Related**: Layer 2, Prompt Engineering (distinction)

#### Concept 19: Layer 3 — Guardrails

- **Source**: `02-five-layer-architecture.md`, `## Layer 3: Guardrails`
- **Definition**: "Layer 3 provides deterministic enforcement of critical organizational rules, operating outside the AI's context window, independent of whether the AI 'remembers' the rule."
- **Normative**:
  - MUST classify rules as critical or advisory (L3-1)
  - Critical rules MUST be enforced by mechanisms outside AI context window (L3-2)
  - MUST include anti-amnesia mechanism (L3-3)
  - RECOMMENDED frequency is every user interaction (L3-4)
  - SHOULD employ defense in depth (L3-5)
  - RECOMMENDED minimum is 5 mechanisms for highest-risk rules (L3-6)
  - Hard enforcement MUST block violations before they take effect (L3-7)
  - MUST NOT rely solely on probabilistic instruction-following for critical rules (L3-8)
- **Related**: Critical Rules, Advisory Rules, Hard Enforcement, Anti-Amnesia, Defense in Depth

#### Concept 20: Rule Classification

- **Source**: `02-five-layer-architecture.md`, Layer 3 Three-Tier Enforcement Model
- **Definition**: "Rules MUST be classified as either critical (requiring hard enforcement) or advisory (acceptable as soft enforcement)."
- **Related**: Critical Rules, Advisory Rules

#### Concept 21: Critical Rules

- **Source**: `02-five-layer-architecture.md` / `03-process-model.md`
- **Definition**: "Rules where any deviation is unacceptable and must be enforced through hard enforcement mechanisms."
- **Normative**: MUST be enforced by deterministic mechanisms outside AI context window; MUST NOT rely solely on probabilistic compliance
- **Related**: Hard Enforcement, Defense in Depth, Advisory Rules

#### Concept 22: Advisory Rules

- **Source**: `02-five-layer-architecture.md`
- **Definition**: "Rules where occasional deviation is acceptable, enforced through soft enforcement mechanisms (directive files the AI interprets probabilistically)."
- **Related**: Soft Enforcement, Critical Rules

#### Concept 23: Three-Tier Enforcement Model

- **Source**: `02-five-layer-architecture.md`, Layer 3
- **Definition**: A three-level enforcement structure: Tier 1 (Soft Enforcement — Rules), Tier 2 (Hard Enforcement — Hooks/Scripts), Tier 3 (Transport-Level Enforcement — Proxy).
- **Normative**: Hard enforcement mechanisms MUST block violations before they take effect, not detect them after the fact
- **Related**: Hard Enforcement, Soft Enforcement, Tier 3

#### Concept 24: Tier 3 — Transport-Level Enforcement

- **Source**: `02-five-layer-architecture.md`, Layer 3
- **Definition**: "Where the AI agent has no direct access to tools except through a constraint-evaluating intermediary. The intermediary intercepts every action, evaluates it against the constraint envelope, and either forwards, modifies, or blocks."
- **Normative**: MAY provide transport-level enforcement where Tier 2 hook-based is insufficient
- **Related**: Hard Enforcement, Constraint Envelope

#### Concept 25: EATP Constraint Mapping

- **Source**: `02-five-layer-architecture.md`, Layer 3 EATP Connection
- **Definition**: "EATP's five constraint dimensions provide a rigorous framework for categorizing guardrails: Financial, Operational, Temporal, Data Access, Communication."
- **Related**: Layer 3, EATP, Constraint Dimensions

#### Concept 26: Layer 4 — Instructions

- **Source**: `02-five-layer-architecture.md`, `## Layer 4: Instructions`
- **Definition**: "Layer 4 provides structured workflows with approval gates between phases, evidence requirements for completion claims, and mandatory delegation rules."
- **Normative**:
  - MUST define structured workflow with at least two phases and one approval gate (L4-1)
  - Approval gates MUST require human judgment before workflow proceeds (L4-2)
  - SHOULD require evidence for completion claims (L4-3)
  - SHOULD define mandatory delegation rules (L4-4)
  - Workflow SHOULD prevent phase-skipping (L4-5)
  - MAY provide context-efficient invocation mechanisms (commands) (L4-6)
- **Related**: Workflow Phase, Approval Gate, Evidence Requirements, Mandatory Delegation

#### Concept 27: Structured Workflow

- **Source**: `02-five-layer-architecture.md` / `03-process-model.md`
- **Definition**: "The implementation MUST define a structured workflow with at least two phases and at least one approval gate."
- **Normative**: MUST prevent phase-skipping
- **Related**: Workflow Phase, Approval Gate

#### Concept 28: Approval Gate

- **Source**: `02-five-layer-architecture.md` / `03-process-model.md`
- **Definition**: "A structured point where human judgment is exercised before the workflow proceeds to the next phase."
- **Normative**: MUST require human judgment
- **Related**: Structured Workflow, Human-on-the-Loop

#### Concept 29: Evidence-Based Completion

- **Source**: `02-five-layer-architecture.md` / `03-process-model.md`
- **Definition**: "The implementation SHOULD require evidence for completion claims — not just assertions, but verifiable proof."
- **Related**: Approval Gate, Workflow Phase

#### Concept 30: Mandatory Delegation Rules

- **Source**: `02-five-layer-architecture.md` / `03-process-model.md`
- **Definition**: "The implementation SHOULD define mandatory delegation rules (e.g., 'security review before commit')."
- **Related**: Layer 4, Approval Gate

#### Concept 31: Standard Six-Phase Workflow

- **Source**: `03-process-model.md`, Standard Six-Phase Workflow
- **Definition**: "CO defines a standard six-phase workflow with canonical command names and workspace directories. Domain applications MAY rename commands for their context, but MUST preserve the underlying phase semantics."
- **Normative**: RECOMMENDED that domain applications preserve phase semantics
- **Related**: Canonical Commands, Workflow Phase

#### Concept 32: Canonical Commands

- **Source**: `03-process-model.md`
- **Definition**: "The standard six-phase workflow uses canonical command names: `/analyze`, `/plan`, `/execute`, `/vet`, `/codify`, `/deliver`."
- **Normative**: RECOMMENDED for consistency across domains
- **Related**: Standard Six-Phase Workflow

#### Concept 33: Phase — Analyze

- **Source**: `03-process-model.md`
- **Definition**: "Phase 01 — Research the problem space. Canonical command `/analyze`. Workspace directory `01-analyze/`."
- **Related**: Standard Six-Phase Workflow

#### Concept 34: Phase — Plan (Structural Approval Gate)

- **Source**: `03-process-model.md`
- **Definition**: "Phase 02 — Structure the work; human approves (the structural approval gate). Canonical command `/plan`. Workspace directory `02-plan/`. **This is the mandatory Phase 02 approval gate where human judgment enters before execution begins.**"
- **Normative**: Phase 02 contains the structural approval gate that MUST be satisfied before execution begins
- **Related**: Standard Six-Phase Workflow, Structural Approval Gate

#### Concept 35: Phase — Execute

- **Source**: `03-process-model.md`
- **Definition**: "Phase 03 — Carry out the work one task at a time. Canonical command `/execute`. Workspace directory `03-execute/`."
- **Related**: Standard Six-Phase Workflow

#### Concept 36: Phase — Vet (Review)

- **Source**: `03-process-model.md`
- **Definition**: "Phase 04 — Spec coverage + adversarial critique; produces finalized output. Canonical command `/vet` (because Claude Code reserves `/review`). Workspace directory `04-vet/`. Phase name is 'Review' but canonical command is `/vet`."
- **Related**: Standard Six-Phase Workflow

#### Concept 37: Phase — Codify (formerly Learn)

- **Source**: `03-process-model.md`
- **Definition**: "Phase 05 — Modify validated patterns into canonical practice (per-proposal human approval). Canonical command `/codify`. Workspace directory `05-codify/` + `.claude/`. **Renamed from 'Learn' to 'Codify' in v1.2.** The only phase with dual output target: workspace directory `05-codify/` for process audit trail, and `.claude/` artifacts for institutional knowledge."
- **Normative**: Phase 05 proposals MUST require human approval before any artifact lands in `.claude/`
- **Related**: Standard Six-Phase Workflow, Layer 5 (Learning)

#### Concept 38: Phase — Deliver

- **Source**: `03-process-model.md`
- **Definition**: "Phase 06 — Package and hand off the finalized output. Canonical command `/deliver`. Workspace directory `06-deliver/` → recipient."
- **Related**: Standard Six-Phase Workflow

#### Concept 39: Layer 5 — Learning

- **Source**: `02-five-layer-architecture.md`, `## Layer 5: Learning`
- **Definition**: "Layer 5 observes what works, captures successful patterns, and evolves them into formalized institutional knowledge over time. Knowledge compounds across sessions, practitioners, and time periods."
- **Normative**:
  - MUST include observation mechanisms that capture patterns (L5-1)
  - Captured patterns MUST be reviewed and approved by human (L5-2)
  - MUST NOT auto-promote AI-observed patterns without human approval (L5-3)
  - SHOULD define confidence thresholds for pattern promotion (L5-4)
  - SHOULD provide mechanisms to manage knowledge base (L5-5)
  - SHOULD allow new practitioners to inherit accumulated knowledge (L5-6)
- **Related**: Observe-Capture-Evolve, Confidence Thresholds, Knowledge Growth

#### Concept 40: Observe-Capture-Evolve Pipeline

- **Source**: `02-five-layer-architecture.md`, Layer 5
- **Definition**: "A three-stage pipeline: **Observe**: Log patterns, errors, fixes, outcomes from AI-assisted work. **Capture**: Analyze accumulated observations for recurring patterns; assign confidence scores. **Evolve**: When patterns reach confidence thresholds, propose formalization into institutional artifacts. Proposals require human approval."
- **Normative**: MUST include observation mechanisms; MUST require human approval before formalization
- **Related**: Confidence Thresholds, Human Approval

#### Concept 41: Confidence Thresholds

- **Source**: `02-five-layer-architecture.md`, Layer 5
- **Definition**: "Numeric values (frequency, success rate, recency scoring) that determine when an observed pattern is sufficiently reliable to propose for formalization."
- **Normative**: SHOULD define
- **Related**: Observe-Capture-Evolve Pipeline

#### Concept 42: Knowledge Growth

- **Source**: `02-five-layer-architecture.md` / Principle 7
- **Definition**: "The phenomenon where institutional knowledge captured in machine-readable form compounds across sessions, practitioners, and time periods, with each successful interaction making the next one more effective."
- **Normative**: SHOULD demonstrate measurable knowledge growth over time
- **Related**: Principle 7 (Knowledge Compounds), Layer 5

### Process Model — Roles

#### Concept 43: CO Architect

- **Source**: `03-process-model.md`, Roles / CO Architect
- **Definition**: "The most senior role, requiring the deepest institutional knowledge. Scope: Designs the overall five-layer architecture."
- **Normative**: MUST have deep institutional knowledge of the domain
- **Related**: Five-Layer Architecture

#### Concept 44: Domain Specialist

- **Source**: `03-process-model.md`, Roles / Domain Specialist
- **Definition**: "Contributes institutional knowledge to Layer 2. Articulates conventions, decisions, and hard-won lessons in machine-readable form."
- **Related**: Layer 2

#### Concept 45: Guardrail Engineer

- **Source**: `03-process-model.md`, Roles / Guardrail Engineer
- **Definition**: "Implements deterministic enforcement in Layer 3. Builds hard enforcement mechanisms, implements anti-amnesia, designs defense-in-depth."
- **Related**: Layer 3

#### Concept 46: Process Designer

- **Source**: `03-process-model.md`, Roles / Process Designer
- **Definition**: "Defines structured workflows in Layer 4. Defines domain-specific workflow phases, approval gates, delegation rules."
- **Related**: Layer 4

#### Concept 47: Knowledge Curator

- **Source**: `03-process-model.md`, Roles / Knowledge Curator
- **Definition**: "Maintains Layer 5. An ongoing role. Reviews proposed pattern formalizations, approves or rejects promotions, ensures knowledge base accuracy."
- **Related**: Layer 5

### Process Model — Quality Criteria

#### Concept 48: Quality Criteria — Coverage

- **Source**: `03-process-model.md`, Quality Criteria / 1
- **Definition**: "What proportion of organizational institutional knowledge is machine-readable? Assessment: Domain specialists review the knowledge base and identify gaps."
- **Related**: Layer 2

#### Concept 49: Quality Criteria — Enforcement Reliability

- **Source**: `03-process-model.md`, Quality Criteria / 2
- **Definition**: "Do critical rules have hard enforcement? Is defense in depth applied to highest-risk rules? Assessment: Intentionally attempt to violate critical rules and verify hard enforcement catches it."
- **Related**: Layer 3, Defense in Depth

#### Concept 50: Quality Criteria — Workflow Discipline

- **Source**: `03-process-model.md`, Quality Criteria / 3
- **Definition**: "Are approval gates respected? Does the AI produce evidence at each gate? Can a human reviewer verify claims without reading raw output?"
- **Related**: Layer 4, Approval Gate, Evidence-Based Completion

#### Concept 51: Quality Criteria — Learning Velocity

- **Source**: `03-process-model.md`, Quality Criteria / 4
- **Definition**: "How quickly do successful patterns become institutional knowledge? Are new practitioners inheriting accumulated wisdom?"
- **Related**: Layer 5, Knowledge Growth

#### Concept 52: Quality Criteria — Practitioner Depth

- **Source**: `03-process-model.md`, Quality Criteria / 5
- **Definition**: "Are the humans building CO layers becoming deeper domain experts? If practitioners are becoming shallower, the implementation has inverted Bainbridge's Irony."
- **Related**: Principle 6 (Bainbridge's Irony)

### Process Model — Adoption Path

#### Concept 53: Adoption Phase 1 — Audit

- **Source**: `03-process-model.md`, Adoption Path / Phase 1
- **Definition**: "Objective: Map your institutional knowledge landscape. Activities: Inventory documented vs tacit, classify rules, identify divergence from generic practice, produce gap analysis."
- **Related**: Institutional Knowledge

#### Concept 54: Adoption Phase 2 — Foundation

- **Source**: `03-process-model.md`, Adoption Path / Phase 2
- **Definition**: "Objective: Build Layers 1 and 2. Activities: Define agent specializations, write master directive, encode critical knowledge, establish routing rules."
- **Related**: Layer 1, Layer 2

#### Concept 55: Adoption Phase 3 — Enforcement

- **Source**: `03-process-model.md`, Adoption Path / Phase 3
- **Definition**: "Objective: Build Layer 3. Activities: Implement hard enforcement for critical rules, build anti-amnesia, design defense in depth, test enforcement reliability."
- **Related**: Layer 3

#### Concept 56: Adoption Phase 4 — Process

- **Source**: `03-process-model.md`, Adoption Path / Phase 4
- **Definition**: "Objective: Build Layer 4. Activities: Define domain-specific workflow phases, design approval gates, establish delegation rules."
- **Related**: Layer 4

#### Concept 57: Adoption Phase 5 — Learning

- **Source**: `03-process-model.md`, Adoption Path / Phase 5
- **Definition**: "Objective: Build Layer 5. This phase is ongoing. Activities: Deploy observation mechanisms, establish capture-and-evolution pipeline, define curator role."
- **Related**: Layer 5

### Conformance

#### Concept 58: CO Conformance

- **Source**: `02-five-layer-architecture.md`, Conformance Criteria
- **Definition**: "An implementation is CO-conformant if it satisfies all MUST requirements across all five layers."
- **Normative**:
  - Layer 1: at least two agent specializations with routing rules
  - Layer 2: master directive loaded every session, single source of truth
  - Layer 3: critical/advisory classification, hard enforcement for critical rules, anti-amnesia
  - Layer 4: structured workflow with approval gates requiring human judgment
  - Layer 5: observation mechanisms, human approval for pattern formalization
- **Related**: Five-Layer Architecture

#### Concept 59: Partial CO Conformance

- **Source**: `02-five-layer-architecture.md`, Conformance Criteria
- **Definition**: "Implementations MAY claim partial conformance by indicating which layers are fully implemented. Notation: CO-L1, CO-L1L2, CO-L1L2L3, CO-L1L2L3L4, CO (full)."
- **Related**: CO Conformance

## CO for Codegen (COC)

> Note: COC thesis predates the CO application template. COC is the reference implementation from which the CO core spec was derived. All COC concepts map to CO core concepts; no contradictions found.

#### Concept 60: COC Agent — Deep Analyst

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 1 — Key specialists
- **Definition**: "Performs failure point analysis, 5-Why root cause investigation, and complexity scoring before any code is written."
- **Related**: Layer 1 (Intent)

#### Concept 61: COC Agent — Security Reviewer (Mandatory)

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 1
- **Definition**: "Mandatory invocation before every commit. Not optional. Not 'recommended.' Mandatory."
- **Normative**: MUST be invoked before every commit
- **Related**: Layer 1, Mandatory Delegation

#### Concept 62: COC Agent — Framework Specialists

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 1
- **Definition**: "Dataflow, nexus, kaizen, mcp specialists. Each carries deep knowledge of a specific framework's patterns, anti-patterns, and conventions."
- **Related**: Layer 1

#### Concept 63: COC CLAUDE.md Pattern

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 2
- **Definition**: "Master directive file loaded at every session. Establishes ground rules — framework-first development, runtime execution patterns, testing policies, security requirements."
- **Normative**: MUST load every session; MUST establish baseline rules
- **Related**: Layer 2, Master Directive

#### Concept 64: COC Skill Hierarchy

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 2
- **Definition**: "Skill directories with progressive disclosure: CLAUDE.md (master directive) → SKILL.md index files → Topic files → Full SDK documentation."
- **Normative**: SHOULD follow progressive disclosure pattern
- **Related**: Layer 2, Progressive Disclosure

#### Concept 65: COC Anti-Amnesia Hook

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 3
- **Definition**: "`user-prompt-rules-reminder.js` fires on every user message, re-injecting critical rules (environment summary, core behavioral rules, framework-first directives) fresh into the AI's context. It survives context window compression because it runs outside the model's memory."
- **Normative**: MUST fire on every user message; MUST re-inject critical rules; MUST survive context compression
- **Related**: Layer 3, Anti-Amnesia, Hard Enforcement

#### Concept 66: COC Validation Hooks

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 3
- **Definition**: "Multiple hooks provide hard enforcement: `validate-workflow.js` (catches wrong code patterns, hardcoded API keys, hardcoded model names, stubs), `validate-bash-command.js` (blocks destructive commands), `session-start.js` (establishes environment baseline)."
- **Normative**: Hooks enforce rules deterministically
- **Related**: Layer 3, Hard Enforcement

#### Concept 67: COC Defense in Depth Example

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 3
- **Definition**: "Example rule 'never hardcode model names' has 5+ enforcement layers: CLAUDE.md, rules/env-models.md (soft rule), user-prompt-rules-reminder.js (every user turn), session-start.js (session start), validate-workflow.js (every file write)."
- **Normative**: RECOMMENDED minimum 5 independent enforcement mechanisms for highest-risk rules
- **Related**: Layer 3, Defense in Depth

#### Concept 68: COC Multi-Phase Development Workflow

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 4
- **Definition**: "Multi-phase development workflow — Analysis, Planning, Implementation, Testing, Deployment, Release, Final — with quality gates at multiple points."
- **Normative**: MUST prevent phase-skipping; each phase produces deliverables next phase consumes
- **Related**: Layer 4, Structured Workflow

#### Concept 69: COC Slash Commands

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 4
- **Definition**: "Slash commands provide context-efficient invocation: `/sdk`, `/db`, `/api`, `/ai`, `/test`, `/validate`, `/design`, `/i-audit`, `/i-harden`, `/learn`, `/evolve`, `/checkpoint`."
- **Normative**: MAY provide context-efficient invocation mechanisms
- **Related**: Layer 4, Canonical Commands

#### Concept 70: COC Evidence-Based Completion

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 4
- **Definition**: "The AI cannot simply state 'I implemented the feature.' Every claim requires file-and-line proof. What file, what line, what test validates the behavior. This eliminates the confident hallucination."
- **Normative**: SHOULD require evidence for every completion claim
- **Related**: Layer 4, Evidence-Based Completion

#### Concept 71: COC Observation-Instinct-Evolution Pipeline

- **Source**: `publications/COC-Core-Thesis.md`, § 4 Layer 5
- **Definition**: "**Observation** (`observation-logger.js`): Logs tool usage, workflow patterns, errors, fixes to JSONL. **Instinct** (`instinct-processor.js`): Analyzes patterns with confidence scoring (frequency 40% + success rate 30% + recency 20% + consistency 10%). **Evolution** (`instinct-evolver.js`): When patterns reach thresholds, suggests formalization into skills (0.7+, 5+ obs), commands (0.6+, 3+ obs), agents (0.8+, 10+ obs)."
- **Normative**: Evolved artifacts require human approval before deployment
- **Related**: Layer 5, Observe-Capture-Evolve Pipeline, Confidence Thresholds

#### Concept 72: COC Institutional Knowledge Thesis in Practice

- **Source**: `publications/COC-Core-Thesis.md`, § 3
- **Definition**: "Organizations know more than they can articulate in any training dataset. This tacit knowledge — how things are actually done, which patterns work, what the documentation does not say — is precisely what the AI lacks. It is also precisely what makes an engineering team effective. Vibe coding treats the model as the entire solution. COC treats the model as one component in a system where institutional knowledge is the differentiator."
- **Related**: Principle 1, Layer 2

## CO for Research (COR)

#### Concept 73: COR Agent — Literature Researcher

- **Source**: `applications/co-for-research.md`, Layer 1 Agent Definitions
- **Definition**: "Specialty: Systematic literature discovery and deep reading. Knowledge: Academic search protocols, citation verification, source assessment. Model tier: Opus (reasoning-heavy)."
- **Related**: Layer 1

#### Concept 74: COR Agent — Field Expert

- **Source**: `applications/co-for-research.md`, Layer 1 Agent Definitions
- **Definition**: "Specialty: Domain knowledge tutoring (configurable per research domain). Knowledge: Key papers, debates, schools of thought, canonical citations, common miscitations. Model tier: Opus."
- **Related**: Layer 1

#### Concept 75: COR Agent — Claims Verifier

- **Source**: `applications/co-for-research.md`, Layer 1 Agent Definitions
- **Definition**: "Specialty: Factual and attributive claim verification. Knowledge: Citation verification protocols, common fabrication patterns, overclaim detection. Model tier: Opus."
- **Related**: Layer 1

#### Concept 76: COR Agent — Argument Critic

- **Source**: `applications/co-for-research.md`, Layer 1 Agent Definitions
- **Definition**: "Specialty: Adversarial argument analysis and reviewer simulation. Knowledge: Academic review conventions, logical fallacy detection, venue-specific reviewer expectations. Model tier: Opus."
- **Related**: Layer 1

#### Concept 77: COR Agent — Writing Partner

- **Source**: `applications/co-for-research.md`, Layer 1 Agent Definitions
- **Definition**: "Specialty: Paragraph-level academic co-writing with deliberation. Knowledge: Academic writing conventions, the paper's established voice, citation placement norms. Model tier: Opus."
- **Related**: Layer 1

#### Concept 78: COR Agent — Cross-Reference Auditor

- **Source**: `applications/co-for-research.md`, Layer 1 Agent Definitions
- **Definition**: "Specialty: Cross-paper and intra-paper citation integrity. Knowledge: Citation format, reference list conventions, cross-paper consistency requirements. Model tier: Speed-critical."
- **Related**: Layer 1

#### Concept 79: COR Teaching Obligation

- **Source**: `applications/co-for-research.md`, Reference: COC Proof Point
- **Definition**: "COR assumes the author needs to learn the field as they write. Every agent interaction shifts from 'execute this task' to 'teach me why, then execute.' This is a key COR innovation beyond COC."
- **Normative**: SHOULD include teaching in every interaction
- **Related**: Layer 1, COR vs COC

#### Concept 80: COR Two Writing Modes

- **Source**: `applications/co-for-research.md`, Layer 4 / Two Writing Modes
- **Definition**: "**`/craft`**: Author writes, AI teaches and critiques. For sections where author has strong views and needs articulation, defense prep, originality coaching. **`/write-para`**: AI drafts paragraphs with inline margin notes explaining choices, author reviews and approves."
- **Normative**: Author MUST approve every paragraph in `/write-para`; human directs in `/craft`
- **Related**: Layer 4, Approval Gate

#### Concept 81: COR /craft Command

- **Source**: `applications/co-for-research.md`, Layer 4 Two Writing Modes
- **Definition**: "Author writes, AI teaches and critiques. Identification of literature gaps, challenging weak claims, coaching for originality, preparation for defense. Output: `craft-notes/` (gaps, challenges, originality assessments)."
- **Normative**: Author MUST direct every aspect
- **Related**: Layer 4

#### Concept 82: COR /write-para Command

- **Source**: `applications/co-for-research.md`, Layer 4 Two Writing Modes
- **Definition**: "AI drafts paragraph with inline margin notes explaining every choice, including 2-3 alternatives for opening sentence, citation placement rationale, teaching notes. Author reviews, modifies, and approves. Output: deliberation logs, version snapshots."
- **Normative**: Author MUST approve every paragraph; margin notes MUST explain choices
- **Related**: Layer 4, Authentic Voice

#### Concept 83: COR Journal System

- **Source**: `applications/co-for-research.md`, Layer 5 / Journal System
- **Definition**: "COR's primary knowledge trail. Every COR command produces journal entries stored in `workspaces/<project>/journal/`. Entry types: TEACH, LITERATURE, DELIBERATION, MARGIN, DEFENSE, CLAIM, CONNECTION, GAP."
- **Normative**: Layer 5 observation mechanisms MUST capture patterns
- **Related**: Layer 5, Auditable Trail

#### Concept 84: COR Hard Rules

- **Source**: `applications/co-for-research.md`, Layer 3 / Hard Rules
- **Definition**: "No unverified citations in final draft (Hook: validate-research-content.js). No scope violations. No overclaims 'first'/'only' without qualification. No fabricated references — FATAL, blocks submission."
- **Normative**: MUST enforce all four; no fabricated references is FATAL
- **Related**: Layer 3, Hard Enforcement

#### Concept 85: COR Authentic Voice Preservation

- **Source**: `applications/co-for-research.md`, Authentic Voice Preservation (Principle 8)
- **Definition**: "COR implements CO Principle 8 through: Disclosure, Human Direction, Auditable Trail, Detection Bias Mitigation (`academic-writing-style.md` rule with constraints grounded in published research)."
- **Normative**: MUST disclose AI assistance; MUST keep human direction visible; SHOULD create auditable trail; SHOULD mitigate detection bias
- **Related**: Principle 8, Layer 5

## CO for Education (COE)

#### Concept 86: COE Spectrum — Four Levels of Student Freedom

- **Source**: `applications/co-for-education.md`, Domain Analysis / COE Spectrum
- **Definition**: "Pedagogical design defining what instructor defines vs student defines: Level 1 (Full Constraint), Level 2 (Partial Freedom), Level 3 (Full Freedom), Level 4 (Meta — student sets up for someone else)."
- **Related**: Student Agency

#### Concept 87: COE Level 1 — Full Constraint

- **Source**: `applications/co-for-education.md`, COE Spectrum
- **Definition**: "Instructor defines all five layers. Student defines only prompts and decisions. Assessment: Quality of thinking within constraints. Most stable — robust to AI advancement."
- **Related**: COE Spectrum

#### Concept 88: COE Level 2 — Partial Freedom

- **Source**: `applications/co-for-education.md`, COE Spectrum
- **Definition**: "Instructor defines Layer 3 guardrails and Layer 4 workflow. Student designs Layer 1 agents and Layer 2 knowledge. Assessment: Agent design + knowledge base + output."
- **Related**: COE Spectrum

#### Concept 89: COE Level 3 — Full Freedom

- **Source**: `applications/co-for-education.md`, COE Spectrum
- **Definition**: "Instructor defines nothing (minimal safety). Student designs everything. Assessment: Full CO setup + output + deliberation."
- **Related**: COE Spectrum

#### Concept 90: COE Level 4 — Meta

- **Source**: `applications/co-for-education.md`, COE Spectrum
- **Definition**: "Student designs constraints for someone else. Assessment: Setup usability by others (knowledge transfer). Tests student ability to create institutional knowledge for others."
- **Related**: COE Spectrum, Knowledge Transfer

#### Concept 91: COE Rubric Scorer Agent

- **Source**: `applications/co-for-education.md`, Layer 1 Assessment Agents
- **Definition**: "Score deliberation log against rubric dimensions. Knowledge: Calibration anchors, scoring rubric, grade boundaries. Tools: Log analysis, pattern matching. Model tier: Reasoning-heavy."
- **Related**: Layer 1, Assessment

#### Concept 92: COE Pattern Detector Agent

- **Source**: `applications/co-for-education.md`, Layer 1 Assessment Agents
- **Definition**: "Identify structural features in deliberation logs. Knowledge: What genuine corrections, redirections, decision points look like. Model tier: Balanced."
- **Related**: Layer 1, Assessment

#### Concept 93: COE Anomaly Flagger Agent

- **Source**: `applications/co-for-education.md`, Layer 1 Assessment Agents
- **Definition**: "Identify statistical outliers for human review. Knowledge: Population baselines, timing patterns, similarity thresholds. Model tier: Speed-critical."
- **Related**: Layer 1, Assessment

#### Concept 94: COE Quality Assessor Agent

- **Source**: `applications/co-for-education.md`, Layer 1 Assessment Agents
- **Definition**: "Evaluate depth vs surface engagement. Knowledge: What substantive vs cosmetic contributions look like, calibration exemplars. Model tier: Balanced."
- **Related**: Layer 1, Assessment

#### Concept 95: COE Hash Chain Integrity

- **Source**: `applications/co-for-education.md`, Layer 3 / Rule Classification
- **Definition**: "Hash chain integrity is a critical rule because academic integrity depends on unbroken chain."
- **Normative**: MUST be critical with hard enforcement
- **Related**: Layer 3, Academic Integrity

#### Concept 96: COE Student Workflow

- **Source**: `applications/co-for-education.md`, Layer 4 / Student Workflow
- **Definition**: "Five phases: 1. Setup (genesis record created), 2. Exploration (initial analysis with decision points logged), 3. Development (draft with corrections/redirections logged), 4. Refinement (final output with quality improvements logged), 5. Submission (submitted artifact with hash chain complete)."
- **Normative**: Genesis record MUST be created at setup; hash chain MUST be complete at submission
- **Related**: Layer 4

#### Concept 97: COE Instructor Assessment Workflow

- **Source**: `applications/co-for-education.md`, Layer 4 / Instructor Assessment Workflow
- **Definition**: "Four phases: 1. Calibration (anchors from sample logs), 2. AI Scoring (preliminary scores), 3. Review (reviewed scores from flagged cases), 4. Moderation (final grades)."
- **Normative**: At least one approval gate between phases
- **Related**: Layer 4, Approval Gate

## CO for Compliance (`co-for-compliance`)

> Note: concepts in this section use the label "COComp" from the enumerating agent. Read as references to `co-for-compliance.md`. This has nothing to do with the previously-fabricated "COComp for complexity".

#### Concept 98: COComp Three Failure Modes — Amnesia in Compliance

- **Source**: `applications/co-for-compliance.md`, Domain Analysis
- **Definition**: "A compliance AI reviewing regulatory filings over a long session gradually forgets the organization's established interpretive positions. Critical rules established early are evicted from context as new content accumulates. The AI reverts to textbook definitions without warning."
- **Related**: Principle 3 (Three Failure Modes), Amnesia

#### Concept 99: COComp Three Failure Modes — Convention Drift in Compliance

- **Source**: `applications/co-for-compliance.md`, Domain Analysis
- **Definition**: "Every mature compliance function has conventions that diverge from textbook practice. The firm may have learned that a particular reporting format, while technically optional, is expected by a specific regulator. The AI, trained on generic content, uses the standard format because the firm's convention is not in any training set."
- **Related**: Principle 3, Convention Drift

#### Concept 100: COComp Three Failure Modes — Safety Blindness in Compliance

- **Source**: `applications/co-for-compliance.md`, Domain Analysis
- **Definition**: "The AI takes the most direct path, which skips disclaimers, bypasses review, and produces analysis without checking data classification. Each shortcut individually small, collectively creating audit exposure."
- **Related**: Principle 3, Safety Blindness

#### Concept 101: COComp Regulatory Interpreter Agent

- **Source**: `applications/co-for-compliance.md`, Layer 1
- **Definition**: "Specialty: Reads regulations and maps to organizational obligations. Knowledge: Firm's interpretive positions, enforcement history, precedent database."
- **Related**: Layer 1

#### Concept 102: COComp Policy Drafter Agent

- **Source**: `applications/co-for-compliance.md`, Layer 1
- **Definition**: "Specialty: Drafts internal policies from regulatory requirements. Knowledge: Firm's policy templates, approval workflows, organizational structure."
- **Related**: Layer 1

#### Concept 103: COComp Audit Preparer Agent

- **Source**: `applications/co-for-compliance.md`, Layer 1
- **Definition**: "Specialty: Prepares documentation for regulatory audits. Knowledge: Auditor preferences, document format requirements, evidence standards."
- **Related**: Layer 1

#### Concept 104: COComp Incident Assessor Agent

- **Source**: `applications/co-for-compliance.md`, Layer 1
- **Definition**: "Specialty: Evaluates potential compliance incidents. Knowledge: Firm's incident classification matrix, escalation procedures, notification obligations."
- **Related**: Layer 1

#### Concept 105: COComp Filing Analyst Agent

- **Source**: `applications/co-for-compliance.md`, Layer 1
- **Definition**: "Specialty: Prepares and reviews regulatory filings. Knowledge: Filing format requirements, historical submissions, common rejection reasons."
- **Related**: Layer 1

#### Concept 106: COComp Critical Rules

- **Source**: `applications/co-for-compliance.md`, Layer 3 / Critical Rules
- **Definition**: "Five critical rules with hard enforcement: (1) No external communication without review, (2) Mandatory disclaimers on client-facing content, (3) Data classification check before analysis, (4) No regulatory position changes without multi-signature, (5) Jurisdiction-appropriate advice only."
- **Normative**: All five MUST be enforced by hard hooks
- **Related**: Layer 3, Hard Enforcement

#### Concept 107: COComp Risk Appetite

- **Source**: `applications/co-for-compliance.md`, Layer 2 / Master Directive
- **Definition**: "Firm's risk appetite boundaries (conservative/moderate/aggressive by topic). The boundary between 'acceptable' and 'escalate' is institutional knowledge specific to the organization."
- **Related**: Layer 2, Master Directive

#### Concept 108: COComp Status — Proposed

- **Source**: `applications/co-for-compliance.md`, Application Identity
- **Definition**: "CO for Compliance is a proposed domain application. It has not been implemented. The structural parallels with COC are clear; empirical effectiveness is unproven."
- **⚠ contradiction**: Application document says "Proposed (not implemented)"; README.md claims "Sketch complete". The application document is explicit it is not implemented.
- **Related**: Domain Applications

## CO for Finance (`co-for-finance`)

> Note: concepts in this section use the label "COL-F" from the enumerating agent. Read as references to `co-for-finance.md`.

#### Concept 109: COL-F Institutional Knowledge Inventory

- **Source**: `applications/co-for-finance.md`, Domain Analysis
- **Definition**: "Categories: Course-specific conventions (via tutors), financial instrument knowledge, disclaimer requirements, citation standards, data source hierarchy, academic integrity, formula reference, research methodology."
- **Normative**: MUST encode institutional knowledge in machine-readable form
- **Related**: Layer 2

#### Concept 110: COL-F Course Tutors

- **Source**: `applications/co-for-finance.md`, Layer 1 Agent Definitions
- **Definition**: "Four agents: fnce101-tutor (foundations), corporate-finance-tutor (capital structure/WACC), international-finance-tutor (exchange rates/BOP), fmi-tutor (markets and institutions)."
- **Related**: Layer 1

#### Concept 111: COL-F Academic Specialists

- **Source**: `applications/co-for-finance.md`, Layer 1 Agent Definitions
- **Definition**: "Seven agents: academic-writer, research-assistant, thesis-advisor, citation-specialist, presentation-designer, case-study-analyst, exam-coach."
- **Related**: Layer 1

#### Concept 112: COL-F Finance Analysis Specialists

- **Source**: `applications/co-for-finance.md`, Layer 1 Agent Definitions
- **Definition**: "Six agents: concept-explainer, coursework-analyst, valuation-specialist, data-source-advisor, regulatory-context, finance-navigator."
- **Related**: Layer 1

#### Concept 113: COL-F Critical Rules

- **Source**: `applications/co-for-finance.md`, Layer 3 / Rule Classification
- **Definition**: "Six critical rules: (1) No fabricated citations, (2) Disclaimer on performance data, (3) Data from authoritative sources only, (4) No investment recommendations without caveat, (5) Academic integrity disclosure, (6) Nominal/real distinction in calculations."
- **Normative**: Critical rules enforced by hard hooks
- **Related**: Layer 3, Hard Enforcement

#### Concept 114: COL-F Disclaimer Compliance

- **Source**: `applications/co-for-finance.md`, Layer 3 / Hard Enforcement Hooks
- **Definition**: "Hook `validate-disclaimer.js` — Performance data must include required disclaimers. Defense in depth: 5 layers."
- **Normative**: MUST include disclaimers on performance data; SHOULD use 5+ enforcement mechanisms
- **Related**: Layer 3, Defense in Depth

#### Concept 115: COL-F Data Sourcing Rule

- **Source**: `applications/co-for-finance.md`, Layer 3 / Rule Classification
- **Definition**: "Critical rule: Data from authoritative sources only (FRED, Bloomberg preferred over web scraping). Enforcement: Hard hook validates source quality."
- **Normative**: MUST come from authoritative sources; SHOULD prefer FRED/Bloomberg
- **Related**: Layer 3, Data Access Constraint

#### Concept 116: COL-F Formula Reference

- **Source**: `applications/co-for-finance.md`, Layer 2 / Knowledge Hierarchy
- **Definition**: "Skill directories covering: financial instruments, regulatory framework, academic writing, formula reference by course. Progressive disclosure pattern."
- **Normative**: SHOULD follow progressive disclosure
- **Related**: Layer 2, Progressive Disclosure

#### Concept 117: COL-F Six-Phase Workflow

- **Source**: `applications/co-for-finance.md`, Layer 4 / Workflow Phases
- **Definition**: "Standard six phases with COL-F command names: `/study` (analyze), `/outline` (plan), `/draft` (execute), `/challenge` (vet), `/learn` (codify), `/submit` (deliver)."
- **Normative**: All phases present with human approval gates
- **Related**: Layer 4, Standard Six-Phase Workflow

#### Concept 118: COL-F Specialty Commands

- **Source**: `applications/co-for-finance.md`, Layer 4 / Specialty Commands
- **Definition**: "Specialty commands alongside six-phase: `/formula`, `/case`, `/practice`, `/exam-prep`, `/cite`, `/explain`, `/challenge`, `/checkpoint`, `/research`, `/thesis`, `/present`."
- **Related**: Layer 4, Canonical Commands

#### Concept 119: COL-F Honest Limitations

- **Source**: `applications/co-for-finance.md`, Honest Limitations
- **Definition**: "Six stated limitations: (1) Financial data not real-time, (2) AI not a financial advisor, (3) Model limitations with calculations, (4) Regulatory context jurisdiction-specific, (5) Teaching quality varies by topic maturity, (6) L5 thresholds aspirational."
- **Normative**: SHOULD include honest limitations section
- **Related**: Principle 8, Disclosure

## CO for Governance (`co-for-governance`)

#### Concept 120: COG Application Identity

- **Source**: `applications/co-for-governance.md`, Application Identity
- **Definition**: "Target domain: Standards development, constitutional design, academic publication, strategic analysis. Status: In Production (self-hosting — this repository IS the implementation). This is the Terrene Foundation's own CO implementation."
- **Related**: Self-Hosting, Domain Applications

#### Concept 121: COG Constitution Expert Agent

- **Source**: `applications/co-for-governance.md`, Layer 1 / Standards Experts
- **Definition**: "Specialty: Constitutional governance (77 clauses, 11 Entrenched Provisions, phased governance). Carries knowledge of the Terrene Foundation constitution."
- **Related**: Layer 1

#### Concept 122: COG Governance Layer Expert Agent

- **Source**: `applications/co-for-governance.md`, Layer 1 / Standards Experts
- **Definition**: "Specialty: Governance Layer Thesis (Two-Era framework, Five Propositions, positioning strategy)."
- **Related**: Layer 1

#### Concept 123: COG Publication Preflight Agent

- **Source**: `applications/co-for-governance.md`, Layer 1 / Publications
- **Definition**: "Specialty: Pre-submission deep validation (reference verification, overclaim detection)."
- **Related**: Layer 1, Principle 8

#### Concept 124: COG CLAUDE.md Structure

- **Source**: `applications/co-for-governance.md`, Layer 2 / Master Directive
- **Definition**: "Master directive establishing: Foundation identity and purpose, rules index, key file locations, agent and skill listings, the standards family with canonical terminology, licensing requirements, Foundation independence declaration."
- **Normative**: MUST be loaded at session start; MUST establish baseline rules
- **Related**: Layer 2, Master Directive

#### Concept 125: COG Rule Classification

- **Source**: `applications/co-for-governance.md`, Layer 3 / Rule Classification
- **Definition**: "Rules classified as critical (Terrene naming, security, constitution consistency, publication claims, quality, positioning, arXiv) or advisory (stubs, CO domain application, agent orchestration, git workflow, communication style)."
- **Normative**: Rules MUST be classified; critical rules MUST be hard-enforced
- **Related**: Layer 3, Rule Classification

#### Concept 126: COG Defense in Depth Example — Terrene Naming

- **Source**: `applications/co-for-governance.md`, Layer 3 / Defense in Depth
- **Definition**: "Rule 'Foundation name and licensing accuracy' has 7 independent enforcement layers: (1) CLAUDE.md, (2) terrene-naming.md, (3) user-prompt-rules-reminder.js, (4) validate-bash-command.js, (5) gold-standards-validator agent, (6) session-start.js, (7) security-reviewer."
- **Normative**: RECOMMENDED highest-risk rules have 5+ mechanisms
- **Related**: Layer 3, Defense in Depth, Principle 5

#### Concept 127: COG Workflow Phases with Specialty Commands

- **Source**: `applications/co-for-governance.md`, Layer 4 / Workflow Phases
- **Definition**: "Six-phase core workflow (analyze, todos, implement, redteam, codify, wrapup) plus specialty commands: `/publish`, `/arxiv`, `/governance-layer`, `/co-domain`, `/preflight`."
- **Normative**: All six phases present
- **Related**: Layer 4, Standard Six-Phase Workflow

#### Concept 128: COG Four Approval Gates

- **Source**: `applications/co-for-governance.md`, Layer 4 / Approval Gates
- **Definition**: "Gate 1 (Analysis → Planning): User reviews analysis artifacts. Gate 2 (Planning → Implementation): User explicitly approves roadmap. Gate 3 (Implementation → Red Team): Review agents pass. Gate 4 (Red Team → Codify): User decides on each critical finding."
- **Normative**: MUST require human judgment before progression
- **Related**: Layer 4, Approval Gate, Human-on-the-Loop

#### Concept 129: COG Observation Targets

- **Source**: `applications/co-for-governance.md`, Layer 5 / Observation Targets
- **Definition**: "Seven observables: workflow phase sequences, agent combination patterns, rule violation frequency, decision patterns, cross-reference failures, publication rejection reasons, terminology drift."
- **Normative**: MUST capture patterns
- **Related**: Layer 5, Observe-Capture-Evolve

#### Concept 130: COG Confidence Thresholds for Pattern Promotion

- **Source**: `applications/co-for-governance.md`, Layer 5 / Confidence Thresholds
- **Definition**: "Rule file: 0.7 confidence, 5+ violations. Skill directory: 0.7, 5+ lookups. Agent: 0.8, 10+ routing needs. Rule promotion (advisory → critical): 0.8, 3+ violations with impact. Command: 0.6, 3+ workflow gaps."
- **Normative**: SHOULD define confidence thresholds
- **Related**: Layer 5, Confidence Thresholds

#### Concept 131: COG Curator Role

- **Source**: `applications/co-for-governance.md`, Layer 5 / Knowledge Curation
- **Definition**: "Curator role assigned to: Dr. Jack Hong (Foundation Founder). Review frequency: End of each major initiative (triggered by `/codify`). Pruning criteria: Remove patterns not confirmed by 2+ initiatives."
- **Normative**: MUST assign curator role; SHOULD require human approval for pattern promotion
- **Related**: Layer 5, Knowledge Curator

#### Concept 132: COG Self-Hosting Status

- **Source**: `applications/co-for-governance.md`, Application Identity
- **Definition**: "COG is in production as a self-hosting implementation — the Terrene Foundation uses its own CO methodology to manage its own standards development, constitutional governance, and publication processes."
- **Related**: Domain Applications, Production Status

## CO for Learners (`co-for-learners`)

> Note: concepts in this section use the label "COL" from the enumerating agent for _learners_ (the subject-agnostic student base). Read as references to `co-for-learners.md`. This is distinct from the previously-fabricated "COL for law".

#### Concept 133: COL Application Identity

- **Source**: `applications/co-for-learners.md`, Application Identity
- **Definition**: "COL is the subject-agnostic student CO. Target practitioners: Students in any academic discipline. Status: In Development (first subject implementation: CO for Finance)."
- **Related**: Domain Applications, Subject Layer Pattern

#### Concept 134: COL Subject Layer Pattern

- **Source**: `applications/co-for-learners.md`, Purpose / Subject Layer Pattern
- **Definition**: "COL provides the universal student workflow. Subject-specific knowledge (agents, skills, rules, commands) is added as a subject layer. Example: CO for Finance inherits COL's workflow phases and academic integrity rules, then adds finance agents, skills, rules, commands."
- **Related**: Domain Applications

#### Concept 135: COL Base Agent Categories

- **Source**: `applications/co-for-learners.md`, Layer 1
- **Definition**: "Five categories provided by COL base: Academic Writing (academic-writer, citation-specialist, presentation-designer), Research (research-assistant, peer-reviewer), Study Support (concept-explainer, exam-coach), Review & Quality (deep-analyst, assignment-analyst), Management (todo-manager)."
- **Normative**: Agents MUST carry domain-specific institutional knowledge
- **Related**: Layer 1, Subject Layer Pattern

#### Concept 136: COL Base Rules

- **Source**: `applications/co-for-learners.md`, Layer 3
- **Definition**: "Five base critical rules applying across all subjects: (1) No fabricated citations, (2) Academic integrity disclosure, (3) Student judgment stays visible, (4) Citation formatting consistency (advisory), (5) Progressive difficulty (advisory). Subject layers add domain-specific rules."
- **Normative**: Critical rules MUST be hard-enforced; advisory rules SHOULD be soft-enforced
- **Related**: Layer 3, Subject Layer Pattern

#### Concept 137: COL Six-Phase Workflow

- **Source**: `applications/co-for-learners.md`, Layer 4 / Six-Phase Workflow
- **Definition**: "Standard six phases with COL command names: `/study` (analyze), `/outline` (plan), `/draft` (execute), `/challenge` (vet), `/learn` (codify), `/submit` (deliver). Phase 04 produces finalized output; Phase 05 extracts reusable knowledge; Phase 06 delivers."
- **Normative**: All six phases with approval gates
- **Related**: Layer 4, Standard Six-Phase Workflow

#### Concept 138: COL Absolute Directive — Student Judgment Stays Visible

- **Source**: `applications/co-for-learners.md`, Layer 2 / Master Directive
- **Definition**: "'Student Judgment Stays Visible' — The AI assists with research and structure; the student provides the intellectual judgment. The student makes analytical decisions: choosing positions, evaluating evidence, forming conclusions."
- **Normative**: Student judgment MUST stay visible; student MUST make analytical decisions
- **Related**: Layer 2, Principle 8

#### Concept 139: COL Relationship to COE

- **Source**: `applications/co-for-learners.md`, Relationship to COE
- **Definition**: "CO for Learners and CO for Education are complementary. **Learners** is the student-facing methodology. The student uses it to learn, study, produce academic work. **Education** is the instructor-facing methodology. The instructor uses it to design assessments, build rubrics, evaluate student work."
- **Related**: COE, Domain Applications

## CO Workflows

#### Concept 140: Governance Workflow

- **Source**: `workflows/01-governance-workflow.md`
- **Definition**: "A complete guide to using the core workflow chain for governance work — constitution, board design, membership criteria, compliance, institutional structures."
- **Related**: Layer 4, COG

#### Concept 141: Standards Workflow

- **Source**: `workflows/02-standards-workflow.md`
- **Definition**: "A complete guide to using the core workflow chain for standards work — creating, updating, publishing CARE, EATP, CO specifications and thesis papers."
- **Related**: Layer 4, COG

#### Concept 142: Strategy Workflow

- **Source**: `workflows/03-strategy-workflow.md`
- **Definition**: "A complete guide to using the core workflow chain for strategy and positioning work — government engagement, enterprise presentations, partnership development, competitive positioning."
- **Related**: Layer 4, COG

#### Concept 143: Research Workflow (COR Specific)

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "A complete guide to using the COR workflow chain for academic research and publication — thesis co-authorship, literature review, claim verification, submission preparation. Unlike the core chain, COR has its own purpose-built commands: /teach, /literature, /deliberate, /craft, /write-para, /validate-claim, /challenge, /check-refs, /preflight, /publish."
- **Related**: Layer 4, COR

#### Concept 144: COR /teach Command

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Deep-dive into a concept, paper, or field area. Produces teaching notes in `01-analysis/literature/` with field context, debates, canonical citations."
- **Related**: COR, Layer 1

#### Concept 145: COR /literature Command

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Systematic search for relevant papers. Produces organized paper lists with quality assessment, relevance, connection to paper, gap identification."
- **Related**: COR, Layer 1

#### Concept 146: COR /deliberate Command

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Record structural decisions about the paper (scope, ordering, framing). Produces decision records with rationale and rejected alternatives."
- **Related**: COR, Layer 5

#### Concept 147: COR /craft Command (Workflow)

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Author writes, AI teaches and critiques. Defense prep, originality coaching. Produces craft-notes/ (gaps, challenges, originality assessment). **FULL approval gate**: author directs every aspect."
- **Normative**: Author MUST have full approval of every aspect
- **Related**: COR, Layer 4, Authentic Voice

#### Concept 148: COR /write-para Command (Workflow)

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "AI drafts paragraph with margin notes, author approves. Produces deliberation log and draft versions. **FULL verification**: author must approve every paragraph."
- **Normative**: Author MUST approve every paragraph; margin notes required
- **Related**: COR, Layer 4, Authentic Voice

#### Concept 149: COR /validate-claim Command

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Verify claims against cited sources. Checks attributions for accuracy."
- **Normative**: All claims MUST be verifiable against sources
- **Related**: COR, Layer 3, Principle 8

#### Concept 150: COR /challenge Command (Workflow)

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Hostile reviewer simulation. Finds weaknesses with severity ratings (FATAL/MAJOR/MINOR/STYLE). FATAL findings must be addressed before continuing."
- **Normative**: FATAL findings MUST be addressed before section completion
- **Related**: COR, Layer 4, Approval Gate

#### Concept 151: COR /check-refs Command

- **Source**: `workflows/04-research-workflow.md`
- **Definition**: "Cross-reference and citation integrity audit. Ensures no orphan citations or phantom references."
- **Normative**: All citations MUST be present and correct
- **Related**: COR, Layer 5

---

## Summary

**Total concepts enumerated: 187**

### Breakdown by category

- **CO Core Specification**: 59 concepts (8 principles + sub-definitions, 5 layers + sub-patterns, process model, quality, adoption, conformance)
- **CO for Codegen (COC)**: 13 concepts
- **CO for Research (COR)**: 13 concepts
- **CO for Education (COE)**: 12 concepts
- **CO for Compliance** (`co-for-compliance`): 11 concepts
- **CO for Finance** (`co-for-finance`): 11 concepts
- **CO for Governance (COG)**: 13 concepts
- **CO for Learners** (`co-for-learners`): 7 concepts
- **CO Workflows**: 12 concepts (4 workflow guides + 8 COR-specific commands enumerated in workflow 04)

### Verification results

- ✓ **8 First Principles confirmed**: Institutional Knowledge Thesis, Brilliant New Hire, Three Failure Modes, Human-on-the-Loop, Deterministic Enforcement, Bainbridge's Irony, Knowledge Compounds, Authentic Voice
- ✓ **5 Architectural Layers confirmed**: Intent, Context, Guardrails, Instructions, Learning
- ✓ **Canonical Commands confirmed**: `/analyze`, `/plan`, `/execute`, `/vet`, `/codify`, `/deliver`
- ✓ **Phase 05 rename from "Learn" to "Codify" confirmed** (v1.2, 2026-04-07)
- ✓ **No internal contradictions** between core CO spec and domain applications
- ✓ **All seven applications follow the five-layer template**
- ✓ **All applications implement six-phase workflow with Phase 02 structural approval gate**
- ✓ **COC concepts map to core CO spec** (COC predates the applications template but nothing in COC invents concepts absent from core)

### Contradiction flagged

⚠ **co-for-compliance status discrepancy**: The application document (`co-for-compliance.md`) states "Proposed (not implemented)" while the `co/applications/README.md` claims "Sketch complete". This is a spec-maintenance issue for the Foundation; does not affect FORGE v1 curriculum.

### Implementation status (per application documents)

- **Production**: COC, COR, `co-for-finance`, `co-for-governance`
- **In Production (pilot planned)**: COE
- **In Development**: `co-for-learners` (subject-agnostic base; first subject = finance)
- **Proposed (not implemented)**: `co-for-compliance`

### Load-bearing concepts for FORGE skill catalog

- **8 Principles** (concepts 1-8) — every skill atom should cite at least one
- **5 Layers** (concepts 9, 13, 19, 26, 39) — structural index for skill organization
- **Standard Six-Phase Workflow + Canonical Commands** (concepts 31-38) — common workflow vocabulary
- **Observe-Capture-Evolve + Confidence Thresholds** (concepts 40, 41) — Layer 5 implementation contract
- **Three-Tier Enforcement Model** (concepts 23-25) — Layer 3 implementation contract
- **Application-specific patterns** (concepts 60-151) — evidence clusters for each application

### Application identifier mapping for FORGE

For clarity, FORGE artifacts should refer to applications by canonical filename stem. The enumerating agent used some provisional acronyms that should be read as pointers, not canonical labels:

| Canonical filename stem   | Short code (where meaningful) | Provisional labels in this document                                              |
| ------------------------- | ----------------------------- | -------------------------------------------------------------------------------- |
| `co-for-codegen` (thesis) | **COC**                       | COC (canonical)                                                                  |
| `co-for-research`         | **COR**                       | COR (canonical)                                                                  |
| `co-for-education`        | **COE**                       | COE (canonical)                                                                  |
| `co-for-governance`       | **COG**                       | COG (canonical)                                                                  |
| `co-for-compliance`       | —                             | "COComp" (provisional; unrelated to previously-fabricated COComp-for-complexity) |
| `co-for-finance`          | —                             | "COL-F" (provisional)                                                            |
| `co-for-learners`         | —                             | "COL" (provisional; unrelated to previously-fabricated COL-for-law)              |
