---
destination: both
spec: PACT
layer: spec-index
status: v1 enumeration — read-before-cite verified
date: 2026-04-07
authority_note: |
  PACT is the fourth Foundation standard by programme-owner declaration.
  Filesystem location reflects active graduation (workspace → 02-standards/)
  but FORGE treats PACT as a full peer of CARE/EATP/CO.
source_paths:
  - terrene/foundation/docs/02-standards/publications/PACT-Core-Thesis.md
  - terrene/foundation/workspaces/pact/01-analysis/01-pact-core-specification.md
  - terrene/foundation/workspaces/pact/01-analysis/adrs/ADR-001-pact-as-fourth-standard.md
  - terrene/foundation/workspaces/pact/01-analysis/adrs/ADR-002-pact-naming.md
  - terrene/foundation/workspaces/pact/01-analysis/adrs/ADR-003-pact-packaging-decisions.md
  - terrene/foundation/workspaces/pact/briefs/01-pact-specification-brief.md
  - terrene/foundation/workspaces/pact/01-analysis/02-cross-reference-analysis.md
concept_count: 67
contradictions_found: 0
wip_concepts:
  - "Shadow Agent Planning (Part 6) — earmarked for migration to CO per ADR-003"
---

# PACT Specification Concept Index

**PACT — Principled Architecture for Constrained Trust.** Enumerated concept list for the PACT (spec) layer. PACT has three layers of instantiation — PACT (spec), PACT (primitives), PACT (platform) — and this index covers the spec layer only. SDK primitives and reference platform architecture are enumerated elsewhere if and when needed.

**Authority note on filesystem vs peer status.** PACT (spec) currently lives under `workspaces/pact/` rather than `02-standards/pact/`. ADR-003 publishes it as "Working Architecture v0.1" pending external validation; ADR-001 establishes the decision to make it the fourth standard. Programme-owner authority declares PACT a peer of CARE/EATP/CO regardless of filesystem location — filesystem graduation is a publication timeline decision, not a status decision.

## Source materials

| Source                              | Type     | Status                           |
| ----------------------------------- | -------- | -------------------------------- |
| PACT-Core-Thesis.md                 | Thesis   | Published working draft          |
| 01-pact-core-specification.md       | Spec     | Working Draft v0.1 (seven parts) |
| 01-pact-specification-brief.md      | Brief    | Published                        |
| ADR-001-pact-as-fourth-standard.md  | ADR      | Proposed                         |
| ADR-002-pact-naming.md              | ADR      | Proposed                         |
| ADR-003-pact-packaging-decisions.md | ADR      | Accepted                         |
| 02-cross-reference-analysis.md      | Analysis | Working                          |

## Core concept categories (confirmed)

- **D/T/R Accountability Grammar** — formally defined
- **Operating Envelopes** — three-layer (Role / Task / Effective) with five constraint dimensions
- **Knowledge Clearance** — five levels, independent of authority
- **Verification Gradient** — four-zone calibrated intervention
- **Positional Addressing** — globally unique D/T/R path encoding
- **Monotonic Tightening Invariant** — core safety guarantee
- **Emergency Bypass Tiering** — time-bounded override mechanism
- **Bridges / Cross-functional Relationships** — standing bridges for matrix/shared services
- **Posture-Clearance Interaction** — effective clearance capped by posture
- **EATP Integration Layer** — governance-to-protocol mapping

## PACT-Core-Thesis.md

### Concept 1: Human-on-the-Loop Operationalization

- **Source**: PACT-Core-Thesis.md, Section 1 (Thesis framing)
- **Definition**: "CARE says 'Human-on-the-Loop.' PACT says 'here's the architecture that makes on-the-loop work at any scale, in any domain.'"
- **Normative**: MUST define operating envelopes at every role level. SHOULD enable agents to execute within envelopes without approval bottlenecks. MUST maintain human observation and refinement capacity.
- **Related**: CARE Dual Plane, Execution Plane, Trust Plane; Verification Gradient; Operating Envelope

### Concept 2: Domain Universality Claim

- **Source**: PACT-Core-Thesis.md, Section 1.4 (Universality Claim) and Brief Table
- **Definition**: "The architectural patterns in PACT are universal. They appear, in recognizable form, in military command structures, government ministries, hospital departments, university faculties, open source governance, and family households."
- **Normative**: MUST apply to any organized group that delegates authority. SHOULD be demonstrated through worked examples in multiple domains. Limits acknowledged: informal organizations, highly egalitarian structures, single-person operations, crisis collapse.
- **Related**: Organizational theory convergence; falsification conditions

### Concept 3: Dual-Use Risk (Surveillance/Accountability Tradeoff)

- **Source**: PACT-Core-Thesis.md, Section 12.8
- **Definition**: "The same mechanisms enable granular workplace surveillance... PACT trades organizational flexibility for accountability; the cost is that it encodes hierarchy as a prerequisite for governance."
- **Normative**: MUST acknowledge PACT's dual-use potential. SHOULD apply clearance framework reflexively to audit data itself. Mitigations: access controls on audit data, purpose limitation, data minimization, compliance with PDPA/GDPR.
- **Related**: Transparency Governance principle; Audit Anchors

## 01-pact-core-specification.md (Working Draft v0.1)

### Concept 4: Department (D) Node Type

- **Source**: Section 2.1 (lines 213-220), Three Node Types
- **Definition**: "Department (or Division, Domain, Directorate) — Persistent knowledge container. Organizational division with defined scope and boundary."
- **Normative**: MUST persist across personnel changes. MUST be immediately followed by exactly one R before further structure attaches. SHOULD have long-lived institutional memory.
- **Related**: Team (T), Role (R), grammar constraint, containment as knowledge boundary

### Concept 5: Team (T) Node Type

- **Source**: Section 2.1 (lines 213-220), Three Node Types
- **Definition**: "Team (or Task Force, Tiger Team, Table) — Fluid knowledge container. Working group, project team, or cross-functional unit."
- **Normative**: MAY be temporary or permanent, more fluid than D. MUST be immediately followed by exactly one R before further structure attaches. CAN cross normal departmental boundaries (cross-functional teams).
- **Related**: Department (D), Role (R), knowledge inheritance from parent D

### Concept 6: Role (R) Node Type

- **Source**: Section 2.1 (lines 242-253), R — The Accountability Anchor
- **Definition**: "The Role is PACT's most important primitive. Every accountability chain traces through roles. Every operating envelope is defined for a role by a role. Every clearance is granted to a role. Every agent in the system is attached to a role."
- **Normative**: Role is a POSITION, not a person (persists when occupant leaves). MUST have exactly one `reports_to_role_id`. MAY be vacant (satisfies grammar but cannot execute). MAY be external (board members). MUST be automatically created with explanation when D or T created (skeleton enforcement).
- **Related**: Accountability Grammar, Positional Addressing, Vacant Roles, Matrix Reporting via Bridges

### Concept 7: Core Grammar Constraint (D/T/R)

- **Source**: Section 2.2 (lines 256-332)
- **Definition**: "A containment node (D or T) MUST be immediately followed by exactly one R before any further D, T, or R can attach."
- **Normative**: MUST enforce at write time. Rejects invalid sequences: D-D, D-T, T-T, T-D. Allows D-R, D-R-R, D-R-D, D-R-T, T-R, etc. MUST auto-create mandatory head R when D or T created. Auto-created R marked `is_vacant: true` until occupied. Vacant roles cannot participate in trust chains.
- **Related**: Principle 2 (Accountability Through People), Formal Grammar Specification, Skeleton Enforcement

### Concept 8: Positional Addressing

- **Source**: Section 2.3 (lines 336-397)
- **Definition**: "Every node in the organizational tree receives a globally unique address. The address encodes both the containment path (which D/T the node is inside) and the accountability path (which R it reports to)."
- **Normative**: Address format `[TYPE][sequence]-[TYPE][sequence]-...` (e.g., `D1-R1-D1-R1-T1-R1`). MUST be globally unique, deterministic, materialized, indexed for prefix queries. MUST be recomputed atomically during reorganizations with old addresses preserved in `address_history`. BOD root has no address.
- **Related**: Organization Compilation, Address Properties, Knowledge scoping, Reorganization events

### Concept 9: Board of Directors (BOD) as Governance Root

- **Source**: Section 2.4 (lines 400-416)
- **Definition**: "The Board of Directors (BOD) is a special root node at level L0. It is not a D or T — it is the governance root, above the management hierarchy."
- **Normative**: BOD is NOT a management entity; it is a governance entity. BOD members are R nodes with `is_external: true`. External R nodes CANNOT have operational shadow agents. BOD is the Genesis Record authority for organization-level operating envelope. BOD member addresses: `R1`, `R2`, etc. (no D/T prefix).
- **Related**: Genesis Record (EATP), External Roles, Organization-level Envelope

### Concept 10: Monotonic Tightening Invariant

- **Source**: Section 3.4 (lines 691-730)
- **Definition**: "The Monotonic Tightening invariant is PACT's core safety guarantee: no operating envelope at any level can be wider than the envelope at any ancestor level. Operating envelopes can only tighten through delegation, never widen."
- **Formal**: For Role Envelope `E_child` for `R_child` reporting to `R_parent`: ∀ dimension d: E_child.d ≤ E_parent.d
- **Normative**: MUST be enforced at write time. Write rejected with error identifying violating dimension. Hard enforcement (no exceptions except organization-level envelope at Genesis). Prevents authority overflow in delegation chains.
- **Related**: Principle 1 (Constrained Delegation), Operating Envelope composition, Widening Veto

### Concept 11: Role Envelope (Standing)

- **Source**: Section 3.2 Layer 1 (lines 512-548)
- **Definition**: "A Role Envelope is a standing operating boundary defined by a supervisor for a direct report. Defined once, applies to all tasks delegated to that role. The expression of a supervisor's standing trust in a direct report."
- **Normative**: MANDATORY: every active role SHOULD have a Role Envelope defined by its supervisor. Supervisor = head of parent D/T. MUST be versioned with timestamp and authorizing role on changes. Scoped to DIRECT reports only (not two levels down).
- **Related**: Task Envelope, Effective Envelope, Five Constraint Dimensions

### Concept 12: Task Envelope (Ephemeral)

- **Source**: Section 3.2 Layer 2 (lines 549-576)
- **Definition**: "When delegating a specific task, the delegator may optionally narrow the Role Envelope for that task. Always tighter than or equal to the Role Envelope (cannot widen)."
- **Normative**: Task-specific and ephemeral. MUST be tighter than or equal to Role Envelope. OPTIONAL; if not provided, acknowledgment prompt ensures conscious choice. MAY narrow any or all five dimensions independently. Narrowing only applies to this task.
- **Related**: Role Envelope, Effective Envelope

### Concept 13: Effective Envelope (Computed Intersection)

- **Source**: Section 3.2 Layer 3 (lines 577-613)
- **Definition**: "The Effective Envelope is the intersection of ALL ancestor envelopes from the organizational root to the current role's position. Never stored directly — always computed on demand. The binding operating boundary for all agent actions."
- **Normative**: MUST be computed on demand, not stored. Computation: Organization env ∩ [all ancestor Role Envelopes] ∩ [current Task Envelope]. MUST guarantee tighter than or equal to any single ancestor. Validation invariant checked at write time.
- **Related**: Operating Envelope, Monotonic Tightening

### Concept 14: Financial Dimension

- **Source**: Section 3.3 (lines 620-634)
- **Definition**: "Controls monetary authority at every level."
- **Parameters**: max_per_action, daily_cumulative, monthly_cumulative, approval_threshold, flagging_threshold, vendor_caps, currency_restrictions, instrument_restrictions
- **Normative**: MUST be expressed in monetary units. Each parameter independently evaluated. Tightening comparison: smaller limits = tighter.
- **Related**: Operational, Temporal, Data Access, Communication dimensions

### Concept 15: Operational Dimension

- **Source**: Section 3.3 (lines 635-647)
- **Definition**: "Controls what actions the agent may take."
- **Parameters**: allowed_action_types, blocked_action_types, rate_limit, concurrency_limit, scope_restriction, retry_policy
- **Normative**: Whitelist + blacklist model. Can restrict to organizational scope (address prefix). Tightening: smaller allowed sets = tighter.
- **Related**: Five Constraint Dimensions

### Concept 16: Temporal Dimension

- **Source**: Section 3.3 (lines 648-660)
- **Definition**: "Controls when actions may occur."
- **Parameters**: operating_hours, blackout_periods, max_task_duration, deadline_behavior, urgency_override_conditions, task_expiry_default
- **Normative**: MUST respect organizational calendars. Tightening: shorter hours, more blackouts, shorter duration = tighter. Urgency override conditions MUST be pre-authorized, not ad hoc.
- **Related**: Emergency Bypass interaction

### Concept 17: Data Access Dimension

- **Source**: Section 3.3 (lines 661-674)
- **Definition**: "Controls what information the agent may access."
- **Parameters**: max_classification, compartments, allowed_data_scopes, excluded_data_scopes, pii_handling, data_retention, write_permissions
- **Normative**: Enforced via clearance framework. Tightening: lower classification ceiling, fewer scopes, more exclusions = tighter. Write permissions more restrictive than read.
- **Related**: Knowledge Clearance Framework, Classification Levels

### Concept 18: Communication Dimension

- **Source**: Section 3.3 (lines 675-688)
- **Definition**: "Controls who the agent may contact and how."
- **Parameters**: internal_channels, external_allowed, external_channels, allowed_recipients, external_recipients, review_required_triggers, tone_policy
- **Normative**: Channel restrictions, recipient whitelisting, keyword-triggered escalation. Tightening: fewer channels, fewer recipients, more restricted = tighter.
- **Related**: Five Constraint Dimensions

### Concept 19: Verification Gradient (4-Zone)

- **Source**: Section 3.5 (lines 733-790)
- **Definition**: "The Verification Gradient is the mechanism that makes Human-on-the-Loop concrete. Rather than a binary permitted/denied, the Gradient provides four zones."
- **Zones**:
  - **Auto-approved**: Proceeds immediately; action logged; no real-time human involvement; routine action within envelope
  - **Flagged**: Proceeds immediately; notification sent; human can intervene retroactively; within envelope but near soft boundary
  - **Held**: Pauses for human decision; waits up to timeout; bounded decision required; at/near envelope boundary
  - **Blocked**: Cannot proceed; action denied; explanation logged; no human involvement; clearly outside envelope
- **Normative**: MUST be configured per-dimension within each Role Envelope. Gradient can be narrowed per Task Envelope, cannot be widened. Human decision point for Held actions MUST include: action, why held, context, agent recommendation, alternatives, time limit (default 4 hours). All gradient decisions create Audit Anchor records.
- **Related**: Principle 4 (Calibrated Intervention), Operating Envelope, Audit Anchors

### Concept 20: Auto-approved Zone

- **Source**: Section 3.5 (Verification Gradient table)
- **Definition**: Proceeds immediately; action logged; no synchronous human approval.
- **Normative**: MUST log for audit trail. MUST NOT require synchronous human approval. SHOULD be used for actions well within envelope.
- **Related**: Verification Gradient

### Concept 21: Flagged Zone

- **Source**: Section 3.5 (Verification Gradient table)
- **Definition**: Proceeds immediately; notification sent; human can intervene retroactively; does not block.
- **Normative**: MUST send notification to supervising role. MUST NOT block action. SHOULD be used for near-boundary actions or anomalous patterns.
- **Related**: Verification Gradient

### Concept 22: Held Zone

- **Source**: Section 3.5 (Verification Gradient + Human Decision Point)
- **Definition**: Pauses for human decision; waits up to timeout; bounded decision required.
- **Normative**: MUST pause action pending human decision. MUST provide bounded decision request including: action (plain language), why held, context, agent recommendation, alternatives, time limit (default 4 hours). Timeout behavior configurable. Both paths create distinct audit records.
- **Related**: Verification Gradient, Human Touch Points, Timeout Behavior

### Concept 23: Blocked Zone

- **Source**: Section 3.5 (Verification Gradient table)
- **Definition**: Cannot proceed; action denied; explanation logged.
- **Normative**: MUST deny action with explanation. MUST NOT require human intervention. MUST log denial with reason. SHOULD be used for clear constraint violations.
- **Related**: Verification Gradient

### Concept 24: Default Envelope Profiles (Progressive Trust)

- **Source**: Section 3.6 (lines 796-806)
- **Definition**: "New roles start with conservative default envelopes. Trust expands based on demonstrated performance — consistent with CARE's Principle 7 (Evolutionary Trust)."
- **Default profiles by CARE posture**:
  - Pseudo (L1) → Minimal (observe only)
  - Supervised (L2) → Narrow (limited actions, every action flagged)
  - Shared Planning (L3) → Moderate (routine auto-approved, boundary held)
  - Continuous Insight (L4) → Broad (most auto-approved, anomalies flagged)
  - Delegated (L5) → Full (operates at Role Envelope limits)
- **Normative**: MUST be starting points only, not ceilings. Supervisor defines actual envelope within defaults.
- **Related**: CARE Trust Posture, Evolutionary Trust

### Concept 25: Five Classification Levels (Knowledge Clearance)

- **Source**: Section 4.2 (lines 915-947)
- **Definition**: "PACT adopts five classification levels, adapted from Singapore Government IM classification."
- **Levels**:
  - **C0 OFFICIAL** — day-to-day operations; auto-access within same D/T
  - **C1 SENSITIVE** — commercial/personnel data; auto-access within same D/T, KSP needed cross-boundary
  - **C2 CONFIDENTIAL** — strategic plans, M&A, board materials; pre-cleared roles only
  - **C3 SECRET** — legal privilege, regulatory investigation; pre-cleared individuals + NDA; compartments apply
  - **C4 TOP SECRET** — existential risk plans; BOD compartment access only; formal vetting
- **Normative**: MUST have five levels. OFFICIAL/SENSITIVE auto-access within same D/T. CONFIDENTIAL and above require pre-clearance. Classification is independent of authority/seniority. Clearance is independent of role level.
- **Related**: Principle 5 (Transparent Governance); separation of authority and clearance

### Concept 26: OFFICIAL Classification

- **Source**: Section 4.2
- **Definition**: Day-to-day operations; routine information.
- **Normative**: Default access for all members of same D/T. No pre-clearance required. NOT visible across D/T boundaries without KnowledgeSharePolicy or bridge.
- **Related**: Knowledge Cascade Rules

### Concept 27: SENSITIVE Classification

- **Source**: Section 4.2 (lines 920, 938-939)
- **Definition**: Commercial information, personnel data, internal strategy.
- **Normative**: Default access within same D/T. Cross-D/T access requires explicit KnowledgeSharePolicy. PACT tightens legacy "any employee can see" to unit-scoping.
- **Related**: KnowledgeSharePolicy, Knowledge Cascade Rules

### Concept 28: CONFIDENTIAL Classification

- **Source**: Section 4.2 (lines 920, 933-936)
- **Definition**: Strategic plans, M&A activity, board materials, regulatory positions.
- **Normative**: Pre-cleared roles only (even within same D/T). Role-level clearance required. Pre-clearance required WITHIN same team (clearance independent of seniority).
- **Related**: Clearance Assignment, Posture-Clearance Interaction

### Concept 29: SECRET Classification

- **Source**: Section 4.2 (lines 920, 933-936)
- **Definition**: Legal privilege, regulatory investigation, executive compensation.
- **Normative**: Pre-clearance at individual level. Individual clearance + NDA required. Compartments apply: not all SECRET items accessible with single clearance. Dual approval required.
- **Related**: Compartments, Clearance Assignment, Access Enforcement

### Concept 30: TOP SECRET Classification

- **Source**: Section 4.2 (lines 920, 937)
- **Definition**: Existential risk plans, crisis management playbooks, major acquisition targets.
- **Normative**: BOD compartment access only. Triple approval required (BOD + CEO + General Counsel). Formal vetting procedure. Compartments mandatory. Semi-annual review cycle.
- **Related**: Compartments, BOD governance, Clearance Assignment

### Concept 31: Clearance Assignment Authority

- **Source**: Section 4.3 (lines 949-975)
- **Definition**: "Clearance is stored in a `RoleClearance` record, independent of the role's authority level or position in the hierarchy."
- **Authority**:
  - OFFICIAL: automatic
  - SENSITIVE: unit head (L3+), single approval
  - CONFIDENTIAL: department head (L4+), single approval + justification
  - SECRET: C-Suite (L5), dual approval (requester + info owner)
  - TOP SECRET: BOD or CEO + General Counsel, triple approval + formal vetting
- **Normative**: Assignment NOT automatic from authority level. MUST be based on operational necessity (need-to-know). MUST include justification at CONFIDENTIAL+.
- **Related**: Classification Levels, Need-to-know principle

### Concept 32: Compartments (Need-to-Know Isolation)

- **Source**: Section 4.5 (lines 997-1020)
- **Definition**: "For SECRET and TOP SECRET classification levels, compartments provide need-to-know isolation within a clearance level. Holding SECRET clearance does not grant access to all SECRET information."
- **Normative**: Compartments ONLY apply to SECRET and TOP SECRET. Role may have SECRET clearance but access only a subset of SECRET compartments. Stored in `RoleClearance.compartments` array. Membership reviewed on same cycle as clearance.
- **Related**: SECRET, TOP SECRET, Clearance Assignment

### Concept 33: Knowledge Cascade Rules

- **Source**: Section 4.6 (lines 1023-1078)
- **Definition**: "Access to knowledge is governed by TWO independent checks: classification/clearance AND containment boundary. Both must pass."
- **Key rule**: "All cross-boundary access requires explicit KnowledgeSharePolicy (KSP) grants plus clearance checks. There are no implicit cross-boundary grants."
- **Scenarios**:
  - Within same D/T: OFFICIAL/SENSITIVE auto; CONFIDENTIAL auto with clearance; SECRET/TOP SECRET require pre-clearance + compartment check
  - Across D/T: requires KSP + clearance + compartment + conditions
  - Trust lineage downward (ancestor reading child): Pseudo/Supervised none; Shared Planning OFFICIAL/SENSITIVE; Continuous Insight up to CONFIDENTIAL; Delegated full
  - T inherits from parent D: team members see department knowledge (inheritance, not KSP)
- **Related**: KnowledgeSharePolicy, Bridges, Posture-Clearance Interaction

### Concept 34: KnowledgeSharePolicy (KSP)

- **Source**: Section 4.6 (lines 1039-1049)
- **Definition**: "All cross-boundary access requires explicit KnowledgeSharePolicy (KSP) grants plus clearance checks. There are no implicit cross-boundary grants, regardless of classification level."
- **Normative**: MUST be explicit (no blanket policies). FROM source D/T TO requesting D/T. Requires requesting role's clearance >= knowledge classification. MAY have conditions. Can be revoked. Distinct from Bridges.
- **Related**: Containment as boundary, Access Enforcement Algorithm

### Concept 35: Bridges (Standing Cross-functional Relationships)

- **Source**: Section 2.5 (lines 421-430)
- **Definition**: "Some organizational units serve multiple reporting lines — a shared legal team, a central IT function, a company-wide HR department. In PACT terms, these are not structural violations; they are bridge configurations."
- **Normative**: Bridges connect ROLES, not units. Primary reporting chain determines address. Secondary relationships modeled as standing bridges. For matrix reporting: effective envelope = intersection of all bridges' Role Envelopes. Bilateral establishment (both sides must agree). MAY have scope limits.
- **Related**: Matrix Reporting, Shared Services, KnowledgeSharePolicy

### Concept 36: Posture-Clearance Interaction

- **Source**: Section 4.4 (lines 977-994)
- **Definition**: "The CARE trust posture system interacts with the clearance framework: an agent's posture gates the maximum effective clearance it can exercise, regardless of the role's formal clearance."
- **Formula**: `Effective clearance = min(role.max_clearance, posture_ceiling[agent.posture])`
- **Posture ceilings**:
  - Pseudo (L1) → OFFICIAL only
  - Supervised (L2) → SENSITIVE
  - Shared Planning (L3) → CONFIDENTIAL
  - Continuous Insight (L4) → SECRET
  - Delegated (L5) → Role's full clearance (up to TOP SECRET)
- **Normative**: MUST apply even if role has higher clearance. Agent trust must be earned independently of role clearance.
- **Related**: CARE Trust Posture, Classification Levels

### Concept 37: Access Enforcement Algorithm

- **Source**: Section 4.7 (lines 1081-1142)
- **Definition**: Complete decision tree for "Can role R access knowledge item K?"
- **Steps**:
  1. Compute effective clearance: `min(R.max_clearance, posture_ceiling[P])`
  2. Classification check: if K.classification > effective_clearance, DENY
  3. Compartment check (SECRET/TOP SECRET): if not in clearance.compartments, DENY
  4. Containment check: same unit → ALLOW; K in child unit → check posture; R in child team → T-inherits-D; active KSP → check conditions; active bridge → check scope; else DENY
  5. All checks pass → ALLOW with audit log
- **Normative**: Audit log MUST record role, knowledge item, classification, access path, posture, timestamp.
- **Related**: All clearance concepts

### Concept 38: Pre-Clearance Workflow

- **Source**: Section 4.8 (lines 1146-1180)
- **Definition**: "For CONFIDENTIAL and above, clearance must be formally established before access is granted."
- **Process**: Request initiation → Justification → Approval chain → Vetting (SECRET+) → NDA/commitment (SECRET+) → Record creation → Activation → Review scheduling
- **Review cycles**:
  - OFFICIAL: automatic
  - SENSITIVE: every 2 years
  - CONFIDENTIAL: every 2 years
  - SECRET: every 1 year
  - TOP SECRET: every 6 months
- **Normative**: Revocation follows same approval chain. Emergency revocation permitted for SECRET/TOP SECRET. Expired clearance without review auto-downgrades.
- **Related**: Clearance Assignment, Compartments

### Concept 39: Emergency Bypass (Tiered Override)

- **Source**: Section 3.7 (lines 819-847)
- **Definition**: "Legitimate emergency situations require the ability to temporarily expand envelopes beyond normal bounds. PACT provides a tiered emergency bypass system with time bounds, approval requirements, and mandatory post-incident review."
- **Tiers**:
  - Tier 1: Up to 4 hours, immediate supervisor, up to supervisor's own envelope
  - Tier 2: 4-24 hours, two levels up, up to two-levels-up envelope
  - Tier 3: 24-72 hours, C-Suite (L4+), up to C-Suite envelope
  - Tier 4: Over 72 hours, not permitted via bypass, use BUILD cycle instead
- **Normative**: CANNOT widen beyond approving authority's envelope. Auto-expiry hard-enforced. Post-incident review MANDATORY within 7 days. Creates Audit Anchor with `subtype: emergency_bypass`. Rate limiting: max 2 Tier 1 per role per 30 days without Tier 2 approval.
- **Related**: Verification Gradient, Monotonic Tightening, Audit Anchors

### Concept 40: EATP Integration (Governance-Protocol Mapping)

- **Source**: Section 5 (lines 1183-1338)
- **Definition**: "PACT defines governance structures. EATP makes those structures tamper-evident. Every PACT governance action — creating an envelope, granting clearance, computing an address, activating a bypass — must create a corresponding EATP record."
- **EATP element mapping**:
  - Genesis Record → Organization root envelope; BOD accountability
  - Delegation Record → Role Envelope creation; task cascade; bridge establishment
  - Constraint Envelope → The PACT Operating Envelope itself (three layers)
  - Capability Attestation → Clearance grant; agent posture level
  - Audit Anchor → Verification Gradient decisions; emergency bypass; clearance events
- **Normative**: EVERY governance action creates an EATP record. Three-check model: EATP verification + PACT envelope + PACT clearance (all must pass).
- **Related**: All EATP elements

### Concept 41: Genesis Record (Organization-Level Commitment)

- **Source**: Section 5.1 (lines 1185-1199) + Section 2.4 (lines 400-416)
- **Definition**: "The Genesis Record in EATP corresponds to the BOD-level commitment: the collective human authority at the highest level of the organization accepting accountability for all autonomous operations within it."
- **Normative**: Created when organization establishes PACT structure. Signed by BOD. Defines root operating envelope. All downstream Delegation Records chain to Genesis Record.
- **Related**: BOD, Operating Envelope, Delegation Record

### Concept 42: Delegation Record (EATP Backing)

- **Source**: Section 5.2 (lines 1205-1227)
- **Definition**: "When a supervisor creates or modifies a Role Envelope for a direct report... Record type: Delegation Record."
- **Created for**: Standing Role Envelope, Task Envelope, Bridge establishment
- **Content**: Delegator role, delegate agent, constraints block (five-dimensional envelope), chain reference, reasoning trace (optional), duration (if Task Envelope)
- **Related**: Role Envelope, Task Envelope, Bridges

### Concept 43: Audit Anchor (EATP Backing)

- **Source**: Section 5.2 (lines 1261-1276) + Section 5.4 (lines 1304-1338)
- **Definition**: "When a Held action is resolved by a human... Record type: Audit Anchor with `subtype: 'gradient_held_resolved'`"
- **Created for**: Verification Gradient decisions, Emergency Bypass activation, Clearance grants/revocations, Knowledge access events, Task completion, Envelope modifications
- **Normative**: Content MUST include subtype, action proposed (if gradient), triggering dimension, human decision (if Held), time measurements, timestamp, authority identity.
- **Related**: Verification Gradient, Emergency Bypass, Transparent Governance

### Concept 44: Organizational Compilation

- **Source**: Section 2.3 (lines 390-397)
- **Definition**: "A structured pass that computes addresses, validates grammar constraints, computes posture ceilings, and validates monotonic tightening."
- **Responsibilities**: Compute addresses, validate grammar (no D-D/D-T/T-T/T-D), compute posture ceilings, validate monotonic tightening, materialize computed values, create EATP records.
- **Normative**: Atomic operation. Reorganizations recompute atomically; old addresses preserved in history.
- **Related**: Positional Addressing, Grammar Constraint, Skeleton Enforcement

### Concept 45: FM-1 Cross-Department Task Cascade

- **Source**: Section 3.8 (lines 852-859)
- **Definition**: "A task cascades from the CFO's agent to a Treasury action that requires Legal input. Legal is outside the Treasury envelope."
- **Mitigation**: Standing bridges between roles. Shadow agent planning identifies bridge dependencies before cascade. If bridges missing, task held and human notified.
- **Related**: Bridges, Cross-functional relationships

### Concept 46: FM-2 Envelope Too Restrictive

- **Source**: Section 3.8 (lines 860-867)
- **Definition**: "Legitimate operational tasks are consistently held or blocked because the Role Envelope is too tight."
- **Mitigation**: Evolutionary trust (CARE Principle 7). System surfaces patterns of held/blocked actions to supervising role. Gradient analytics dashboards.
- **Related**: Evolutionary Trust, Default Envelope Profiles

### Concept 47: FM-3 Envelope Too Loose

- **Source**: Section 3.8 (lines 868-873)
- **Definition**: "Legitimate concerns arise that the envelope is allowing too much autonomy — cumulative spending is near organizational-level limits, or communication patterns seem unusual."
- **Mitigation**: Cumulative tracking. Drift detection at configurable thresholds (e.g., 80% of monthly limit by day 20). Supervisor can tighten or investigate.
- **Related**: Envelope monitoring, Governance dashboards

### Concept 48: FM-4 Envelope Conflict Between Levels

- **Source**: Section 3.8 (lines 874-879)
- **Definition**: "Two different ancestors in the delegation chain have envelopes that appear to conflict."
- **Mitigation**: Monotonic tightening resolves automatically. Effective Envelope is most restrictive intersection. Architecture prevents conflict configuration from arising.
- **Related**: Monotonic Tightening Invariant, Effective Envelope

### Concept 49: FM-5 Emergency Bypass Abuse

- **Source**: Section 3.8 (lines 880-885)
- **Definition**: "Emergency bypass is being used as a routine workaround rather than for genuine emergencies."
- **Mitigation**: Rate limiting, mandatory post-incident review, pattern detection, audit visibility, automatic escalation of missing reviews.
- **Related**: Emergency Bypass Tiering

### Concept 50: FM-6 Infinite Task Decomposition

- **Source**: Section 3.8 (lines 886-891)
- **Definition**: "A task cascade recurses so deeply that the effective envelope at the bottom level is effectively zero."
- **Mitigation**: Degenerate envelope detection. System flags when Effective Envelope becomes operationally degenerate. Task held and human notified.
- **Related**: Effective Envelope

### Concept 51: Edge Case — Vacant Roles

- **Source**: Section 2.5 (lines 442-452)
- **Definition**: "A vacant role satisfies the grammatical constraint but cannot execute."
- **Normative**: Cannot execute autonomously or participate in trust chains. When agent cascades task to vacant role: check for acting role; if none, hold task and notify parent.
- **Related**: Grammar Constraint, Acting in Two Roles

### Concept 52: Edge Case — Acting in Two Roles

- **Source**: Section 2.5 (lines 453-462)
- **Definition**: "An individual may temporarily hold two positions (acting in a higher role while their own position is vacant, or serving as interim)."
- **Normative**: Each role remains separate R node with own address, envelope, clearance. Individual occupies both simultaneously. More restrictive envelope applies when acting in primary role. Clearances stack, capped by posture.
- **Related**: Vacant Roles, Envelope intersection

### Concept 53: Edge Case — Matrix Reporting

- **Source**: Section 2.5 (lines 431-441)
- **Definition**: "In matrix organizations, individuals may report to two or more managers."
- **Normative**: Primary reporting = `reports_to_role_id` (determines address). Secondary relationships = standing bridges. Effective envelope = intersection of primary Role Envelope AND all secondary bridge envelopes. Both principals must agree to bridge establishment.
- **Related**: Bridges, Primary reporting chain

### Concept 54: Edge Case — Shared Services (Bridges)

- **Source**: Section 2.5 (lines 421-430)
- **Definition**: "Some organizational units serve multiple reporting lines — a shared legal team, a central IT function, a company-wide HR department."
- **Normative**: Primary reporting chain determines main address. Secondary relationships = bridges. Bridges connect to roles, not units. Enables centralized shared services without structural ambiguity.
- **Related**: Bridges, Primary reporting chain

### Concept 55: Edge Case — Reorganizations

- **Source**: Section 2.5 (lines 463-473)
- **Definition**: "When an organizational unit moves in the hierarchy, addresses must be recomputed for all affected nodes."
- **Normative**: MUST preserve old address in `address_history`. MUST recompute atomically. MUST update all EATP records referencing old address. All downstream addresses cascade-recomputed. Envelope recomputation validates monotonic tightening with new parent.
- **Related**: Positional Addressing, Address history, Organizational Compilation

## briefs/01-pact-specification-brief.md

### Concept 56: Accountability Grammar (Brief Summary)

- **Source**: Brief (lines 36-43), Section 1
- **Definition**: "Three node types for modeling any organization: D, T, R. Core constraint: A containment (D or T) MUST be immediately followed by exactly one R."
- **Related**: Full specification in spec file

### Concept 57: Operating Envelope (Brief Summary)

- **Source**: Brief (lines 45-51), Section 2
- **Definition**: "Recursive delegation of bounded autonomy: Each R defines the operating envelope for its DIRECT reports only; Envelopes compose through intersection; Monotonic Tightening; Three layers."
- **Related**: Role Envelope, Task Envelope, Effective Envelope

### Concept 58: Knowledge Clearance Framework (Brief Summary)

- **Source**: Brief (lines 53-59), Section 3
- **Definition**: "Classification-based knowledge governance: Five levels; Clearance independent of authority; Trust posture gates effective clearance; Compartments for need-to-know isolation."
- **Related**: Classification Levels, Posture-Clearance Interaction

### Concept 59: Verification Gradient (Brief Summary)

- **Source**: Brief (lines 61-67), Section 4
- **Definition**: "Calibrated human intervention (not binary): Auto-approved, Flagged, Held, Blocked."
- **Related**: Four-zone gradient

### Concept 60: Positional Addressing (Brief Summary)

- **Source**: Brief (lines 69-72), Section 5
- **Definition**: "Every entity gets a globally unique address encoding both containment and accountability chain: `D1-R1-D1-R1-T1-R1`."
- **Related**: Address encoding rules

### Concept 61: Domain Universality (Brief Evidence)

- **Source**: Brief (lines 74-86)
- **Definition**: "The patterns apply to ANY organized group." Table shows Enterprise, Government, Military, Healthcare, Education, Family, Open Source all using D/T/R and operating envelopes.
- **Related**: Universality claim, Falsification conditions

## ADRs

### Concept 62: PACT as Fourth Standard (Decision)

- **Source**: ADR-001-pact-as-fourth-standard.md (lines 38-40)
- **Definition**: "Create PACT (Principled Architecture for Constrained Trust) as a fourth, peer standard in the Terrene Foundation standards family."
- **Rationale**: Architectural patterns don't fit cleanly in CARE, EATP, or CO. Patterns are domain-universal. Fills gap between CARE philosophy and implementation.
- **Consequences**: Trinity becomes Quartet (CARE/PACT/EATP/CO). Adoption has two paths. CARE and EATP need cross-references to PACT.
- **Related**: ADR-003 (graduation timeline)

### Concept 63: Alternatives Considered and Rejected

- **Source**: ADR-001 (lines 44-68)
- **Rejected alternatives**:
  - Extend CARE — would blur CARE's philosophical focus
  - Make part of EATP — EATP is protocol layer; PACT is organizational architecture layer
  - Make part of CO — CO is methodology; PACT is structure
  - Keep as implementation detail — patterns are domain-universal
- **Related**: Standards layering, Separation of concerns

### Concept 64: PACT Acronym Justification

- **Source**: ADR-002-pact-naming.md (lines 52-95)
- **Definition**: "P: Principled, A: Architecture, C: Constrained, T: Trust."
- **Metaphorical grounding**: "A pact is an agreement between parties. Operating envelope IS a pact (bilateral commitment). Not restriction imposed top-down; agreement on bounds."
- **Name family**: CARE, PACT, EATP, CO form stylistically coherent family of short meaningful words + precise acronyms.
- **Related**: Standards naming

### Concept 65: Working Architecture Status (Publication Model)

- **Source**: ADR-003-pact-packaging-decisions.md (lines 10-20)
- **Definition**: "Publish PACT as 'PACT Working Architecture v0.1' under CC BY 4.0. Not a fourth standard (published); a technical report."
- **Rationale**: Content strong but zero published theses and zero external adopters. Standardizing from single implementation before external validation = known anti-pattern. Allows external feedback cycle before "standard" commitment.
- **Promotion criteria**: 2 independent implementations + 3 adopter feedback + CARE/EATP/CO theses published + boundary map published.
- **Note (FORGE authority override)**: ADR-003 defers _publication_ label; ADR-001 makes PACT the fourth standard. The decision is final; the publication timeline is separate. FORGE treats PACT as a standard.
- **Related**: Publication timeline, Promotion criteria

### Concept 66: Publication Timeline

- **Source**: ADR-003 (lines 23-36)
- **Definition**: "PACT goes after CARE, EATP, CO, and Constrained Org are submitted."
- **Sequence**: EATP → arXiv → CO → arXiv → CARE → SSRN → Constrained Org → AIES 2026 → PACT Working Architecture → GitHub
- **Rationale**: Four theses further along with defined venues; PACT on own timeline without venue pressure.
- **Related**: Foundation publication schedule

### Concept 67: Shadow Agent Planning Removal (to CO)

- **Source**: ADR-003 (lines 38-42)
- **Definition**: "Remove Part 6 (Shadow Agent Planning) from PACT. Move it to CO."
- **Rationale**: Shadow agent planning is methodology (how agents plan within envelopes), not architecture (how envelopes structured). Belongs in CO.
- **Current status**: Retained in Part 6 for reference only; marked `[spec stability: draft]`; will migrate to CO specification.
- **Related**: PACT scope, CO relationship

---

## Cross-standard relationships

### PACT ↔ CARE

- CARE provides: Philosophical foundation (why Human-on-the-Loop)
- PACT provides: Structural patterns HOW to implement Human-on-the-Loop at scale
- PACT instantiates: CARE's Dual Plane (D/T/R is Trust Plane structure; envelopes are Trust Plane configuration)
- PACT operationalises: CARE Principle 7 (Evolutionary Trust via default envelope profiles)
- Mutual requirement: PACT without CARE = architecture without moral grounding

### PACT ↔ EATP

- EATP provides: Cryptographic protocol backing every governance action
- PACT provides: Organizational structure that EATP records
- Mapping: Every PACT action creates EATP record
- Three-check model: EATP verification + PACT envelope + PACT clearance (all must pass)

### PACT ↔ CO

- CO provides: Knowledge methodology (five layers, task decomposition)
- PACT provides: Organizational structure scoping CO's knowledge operations
- Shadow Agent Planning: scope boundary — currently in PACT Part 6, earmarked for CO
- Five layers distinction: PACT's five constraint dimensions ≠ CO's five knowledge layers (orthogonal)

## Stability audit

| Component                         | Stability | Notes                                        |
| --------------------------------- | --------- | -------------------------------------------- |
| D/T/R Grammar                     | Stable    | Normative in thesis and spec                 |
| Operating Envelopes (3-layer)     | Stable    | Core innovation                              |
| Five Constraint Dimensions        | Stable    | Directly from EATP schema; no contradictions |
| Knowledge Clearance               | Stable    | SG Gov IM attribution clear                  |
| Verification Gradient             | Stable    | Four zones consistent in thesis and spec     |
| Positional Addressing             | Stable    | Algorithm detailed                           |
| Monotonic Tightening              | Stable    | Foundational; enforced at write time         |
| Emergency Bypass                  | Stable    | Tiered system fully specified                |
| EATP Integration                  | Stable    | Clear mapping in Section 5                   |
| Shadow Agent Planning             | Draft     | Marked for migration to CO (ADR-003)         |
| Clearance-authority orthogonality | Stable    | Core principle                               |

**Contradictions between thesis and spec:** None detected.

## Summary

**Total concepts enumerated:** 67
**Contradictions:** 0
**WIP markers:** 1 (Shadow Agent Planning, earmarked for CO migration)

**Confirmed core concepts:** D/T/R accountability grammar, operating envelopes (three-layer + five dimensions), knowledge clearance (five levels + compartments), verification gradient (four zones), positional addressing, monotonic tightening invariant, emergency bypass tiering, bridges, posture-clearance interaction, EATP integration layer.

**Novel content (per cross-reference analysis):** ~17 pages of genuinely novel content distinct from CARE/EATP — primarily D/T/R grammar, positional addressing, three-layer envelope composition, clearance-authority orthogonality, emergency bypass. Rest of the spec elaborates patterns that already exist in CARE/EATP but gives them concrete structural form.
