# FORGE — Foundation for Orchestration, Reasoning, and Generative Engineering

Official Terrene Open Academy programme teaching CO (Cognitive Orchestration) and COC (Claude Operations Codex) skills. Two tracks — AI Engineer and AI Business Consultant — share a common foundation before branching into specializations.

## Absolute Directives

### 0. Foundation Independence
Kailash Python SDK is a Terrene Foundation project (Singapore CLG). No commercial references. See `rules/independence.md`.

### 1. Specs as First Principles
CARE, EATP, CO, COC, and PACT are first principles that generate the curriculum — not post-hoc lenses applied to existing material. Every module maps to specification concepts.

### 2. Framework-First
All technical content uses Kailash frameworks. Never raw sklearn/pandas/PyTorch without the Kailash wrapper.

### 3. Dual-Track Architecture
Foundation modules (F1-F4) are shared. Specialization modules diverge: E-track (AI Engineer) and C-track (AI Business Consultant). Content must be clearly tagged by track.

## Course Structure

### Foundation Track (Shared — F1-F4)

| Module | Title | Key Concepts |
|--------|-------|-------------|
| F1 | CO Fundamentals | CARE (Dual Plane, Mirror Thesis, Constraint Envelopes), EATP (5 trust postures, lineage), CO (5 layers, 8 principles, 3 failure modes) |
| F2 | COC Architecture | Five-layer architecture (Intent, Context, Guardrails, Instructions, Learning), agents, skills, rules, hooks, commands, variant system |
| F3 | Kailash SDK Essentials | Core SDK workflows, DataFlow CRUD, Nexus deployment, Kaizen agents — hands-on with the platform |
| F4 | AI Systems Design | Orchestration patterns, MCP integration, multi-agent coordination, institutional knowledge capture |

### AI Engineer Track (E1-E4)

| Module | Title | Key Concepts |
|--------|-------|-------------|
| E1 | COC Development | Building agents (specialist, quality, management), writing skills (SKILL.md index, sub-skills), rules (scoped, Why: rationale), hooks (session-start, pre-commit), commands |
| E2 | ML Pipeline Engineering | kailash-ml engines (13 engines), TrainingPipeline, AutoML, ModelRegistry, ONNX export, drift monitoring — drawn from ASCENT M3-M4 patterns |
| E3 | Agent Engineering | Kaizen Delegate vs BaseAgent, Signature-based programming, multi-agent orchestration, tool calling, structured outputs, MCP servers |
| E4 | Production Systems | Nexus multi-channel deployment, PACT governance (D/T/R, operating envelopes), InferenceServer, monitoring, CI/CD for COC artifacts |

### AI Business Consultant Track (C1-C4)

| Module | Title | Key Concepts |
|--------|-------|-------------|
| C1 | DX Strategy Frameworks | 5 Domains of DX Strategy (Customer, Value, Competition, Data, Innovation), 3x3 Matrix (Digitization/Digitalization/Transformation x Products/Platforms/Ecosystems x Automation/Augmentation/Amplification), Competitive Dimensions (Scale, Scope, Speed) |
| C2 | Organizational AI Assessment | EATP trust posture mapping, transformation readiness, Constraint Theater Thesis, Mirror Thesis application to client organizations |
| C3 | CO for Client Engagement | Using CO methodology to structure consulting deliverables, case analysis through Dual Plane, whitepaper and policy brief production |
| C4 | Governance & Compliance | PACT D/T/R accountability grammar, operating envelopes, knowledge clearance, regulatory frameworks (NIST, ISO, EU AI Act, Singapore IMDA), AI governance architecture |

## Source Material

| Source | Provides | Modules |
|--------|----------|---------|
| co-ai (COE) | Assessment design methodology, AI-resilient pedagogy | F1, F2, C3 |
| co-ax (CO-DX) | DX frameworks, shadow cases, Constraint Theater Thesis | C1, C2, C3, C4 |
| ascent | ML engineering exercises, Kailash SDK patterns | F3, E1-E4 |
| atelier | CO/COC artifact authority, spec implementations | F1, F2, F4, E1 |
| loom | COC source, variant system, sync mechanics | E1, F2 |

## Kailash Platform

| Framework | Purpose | Install |
|-----------|---------|---------|
| Core SDK | Workflow orchestration, 140+ nodes | `pip install kailash` |
| DataFlow | Zero-config database operations | `pip install kailash-dataflow` |
| Nexus | Multi-channel deployment (API+CLI+MCP) | `pip install kailash-nexus` |
| Kaizen | AI agent framework | `pip install kailash-kaizen` |
| PACT | Organizational governance (D/T/R) | `pip install kailash-pact` |
| ML | ML lifecycle (13 engines, polars) | `pip install kailash-ml` |
| Align | LLM fine-tuning & serving | `pip install kailash-align` |

## Workspace Commands

| Command | Purpose |
|---------|---------|
| `/analyze` | Load analysis phase for current workspace |
| `/todos` | Load todos phase; stops for human approval |
| `/implement` | Load implementation phase |
| `/redteam` | Load validation phase |
| `/codify` | Update agents & skills with new knowledge |
| `/ws` | Read-only workspace status dashboard |
| `/wrapup` | Write session notes before ending |

## Rules Index

| Concern | Rule File |
|---------|-----------|
| Foundation independence | `rules/independence.md` |
| Specs-first curriculum | `rules/specs-first.md` |
| Dual-track consistency | `rules/dual-track.md` |
