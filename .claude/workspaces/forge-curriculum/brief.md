# FORGE Curriculum Development

## Objective
Design and build the complete FORGE curriculum — 12 modules (4 foundation + 4 engineer + 4 consultant) teaching CO and COC skills to AI practitioners.

## Context
FORGE draws from three existing Terrene Open Academy programmes:

### From co-ai (COE) — Assessment & Pedagogy
- AI-resilient assessment design methodology
- Constructive alignment (Biggs) between outcomes, activities, assessments
- Three failure modes: Amnesia, Convention Drift, Safety Blindness
- Five-phase workflow gates with guardrail enforcement
- Transparent, criterion-referenced rubrics

### From co-ax (CO-DX) — DX Strategy & Executive Education
- MGMT6020 "Leading AI Transformation" (8 sessions x 3.5hrs, SMU MBA)
- 5 Domains of DX Strategy: Customer, Value, Competition, Data, Innovation
- 3x3 Matrix: Digitization/Digitalization/Transformation x Products/Platforms/Ecosystems x Automation/Augmentation/Amplification
- 3 Competitive Dimensions: Scale (exponential via platforms), Scope (variable scope, fixed timeline), Speed (learning speed beats first-mover advantage)
- Shadow cases with spec-derived narratives:
  - Zillow Offers ($569M write-down — Trust Plane suppression)
  - Nubank (financial inclusion — "doing better things")
  - Shopee (EATP trust posture mapping)
  - Boeing 737 MAX + DBS (governance failure vs enabling governance)
  - Booking.com (25K experiments/year — learning speed as moat)
  - Grab (super app integration)
- Constraint Theater Thesis: governance artifacts competing with judgment destroy value
- Dual Plane Model: Trust Plane (human judgment) vs Execution Plane (AI-shareable)
- Mirror Thesis: AI reveals what humans uniquely contribute (6 categories)
- 10-point spec-derived pitch rubric
- Assessment: participation (30%) + group pitch (30%) + individual report (40%)

### From ascent — ML Engineering
- 10 modules, 80 exercises, polars-native, Kailash framework-first
- Progressive scaffolding from 70% provided (M1) to 20% (M6+)
- Solution-first authoring: write complete solutions, strip to create exercises
- Three-format delivery: local .py, Jupyter .ipynb, Colab .ipynb
- Key patterns: DataExplorer, PreprocessingPipeline, TrainingPipeline, AutoMLEngine, ModelRegistry, InferenceServer, DriftMonitor, OnnxBridge
- Kaizen agents: Delegate, BaseAgent, Signature, Pipeline, ReActAgent
- PACT governance: GovernanceEngine, PactGovernedAgent, D/T/R addressing

### From loom/atelier — COC Architecture
- Five-layer architecture: Intent (agents), Context (CLAUDE.md + skills), Guardrails (rules + hooks), Instructions (commands + phases), Learning (observations + instincts)
- Variant system: global artifacts + language-specific variants (py/rs)
- Sync mechanics: atelier -> loom -> USE templates -> downstream repos
- Agent categories: analysis, frameworks, frontend, implementation, management, quality, release, testing
- Skill structure: SKILL.md index + sub-skill files, ~35 skill directories
- Rule scoping: path-scoped rules that load conditionally
- Hook types: session-start, pre-commit, post-tool-use (9 hooks total)
- /codify -> /sync proposal flow for knowledge capture

### Terrene Foundation Specifications (First Principles)
- **CARE v2.1**: Dual Plane Model, Mirror Thesis, Human-on-the-Loop, Constraint Envelopes, 8 principles
- **EATP v2.2**: 5 trust postures (Pseudo-Agent -> Delegated), lineage chain, enforcement verdicts (HELD/FLAGGED/CLEAR)
- **CO v1.1**: 5 layers, 8 principles, 6 phases, 3 failure modes (Amnesia, Convention Drift, Safety Blindness)
- **COC**: Five-layer implementation (Intent, Context, Guardrails, Instructions, Learning), variant architecture, sync protocol
- **PACT**: D/T/R accountability grammar, operating envelopes, knowledge clearance, verification gradient, monotonic tightening

## Target Audience

### AI Engineers
- Software engineers who want to build AI-powered systems using Kailash SDK
- Need: COC artifact development, ML pipeline engineering, agent systems, production deployment
- Background: Python proficiency, some ML exposure, no CO/COC knowledge assumed

### AI Business Consultants
- Business professionals who advise organizations on AI transformation
- Need: DX strategy frameworks, organizational assessment, governance, client engagement
- Background: Business/strategy experience, basic technical literacy, no coding required for C-track

## Deliverables per Module
- Learning outcomes (mapped to spec concepts)
- Slide deck (Reveal.js)
- Exercises (hands-on for F/E-track, case-based for C-track)
- Assessment rubric (spec-derived, AI-resilient)
- Instructor notes

## Open Questions
- [ ] Duration per module? (Suggest: 3-4 hours each, 12 modules = 36-48 hours total)
- [ ] Certification structure? (Foundation Certificate after F1-F4, Specialist Certificate after track)
- [ ] Case library: reuse co-ax shadow cases for C-track or create new ones?
- [ ] Exercise format: follow ASCENT three-format pattern or simpler for this audience?
- [ ] Assessment weighting across tracks
