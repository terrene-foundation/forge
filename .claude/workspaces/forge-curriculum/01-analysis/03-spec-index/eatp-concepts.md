---
destination: both
spec: EATP
layer: spec-index
status: v1 enumeration — read-before-cite verified
date: 2026-04-07
source_paths:
  - terrene/foundation/docs/02-standards/eatp/README.md
  - terrene/foundation/docs/02-standards/eatp/01-first-principles.md
  - terrene/foundation/docs/02-standards/eatp/02-trust-lineage-chain.md
  - terrene/foundation/docs/02-standards/eatp/03-operations.md
  - terrene/foundation/docs/02-standards/eatp/04-integration.md
  - terrene/foundation/docs/02-standards/eatp/05-standalone-sdk.md
  - terrene/foundation/docs/02-standards/eatp/06-interoperability.md
  - terrene/foundation/docs/02-standards/eatp/07-conformance.md
concept_count: 94
---

# EATP Specification Concept Index

Enumerated concept list for the EATP (Enterprise Agent Trust Protocol) standard — one of the four Foundation standards alongside CARE, CO, and PACT. Every concept below is traceable to an exact file, section anchor, and quoted definition. This index will be used to map journal evidence to spec concepts for the FORGE skill catalog.

**Source path root:** `terrene/foundation/docs/02-standards/eatp/`

## README.md

### Concept 1: Trust Infrastructure Layer

- **Name**: Trust Infrastructure Layer (EATP)
- **Source**: README.md (Line 15), Section: Position in Architecture
- **Definition**: "EATP defines the trust infrastructure — cryptographic trust chains, constraint envelopes, and audit anchors — that enables verifiable traceability for AI agent actions."
- **Normative requirements**: None at MUST level; README is descriptive
- **Related**: Cryptographic trust chains, Constraint Envelope, Audit Anchor, Trust Lineage Chain

### Concept 2: Five Elements of Trust Lineage

- **Source**: README.md (Line 20), Section: Contents
- **Definition**: "The five elements of Trust Lineage" [Cross-reference to 02-trust-lineage-chain.md]
- **Normative**: Listed as core content structure
- **Related**: Genesis Record, Delegation Record, Constraint Envelope, Capability Attestation, Audit Anchor

### Concept 3: Four Operations (ESTABLISH, DELEGATE, VERIFY, AUDIT)

- **Source**: README.md (Line 21), Section: Contents
- **Definition**: "ESTABLISH, DELEGATE, VERIFY, AUDIT operations"
- **Normative**: Listed as specification requirement
- **Related**: Trust Lineage Chain, Constraint Envelope

### Concept 4: Constraint Envelope Schema (Five-Dimensional)

- **Source**: README.md (Line 31), Section: Shared Schemas
- **Definition**: "Five-dimensional constraint envelope (EATP Element 3)" — canonical shared schema
- **Normative**: Canonical JSON Schema shared across CARE, EATP, and CO
- **Related**: Financial, Operational, Temporal, Data Access, Communication dimensions

### Concept 5: Reasoning Trace Schema (Structured)

- **Source**: README.md (Line 32), Section: Shared Schemas
- **Definition**: "Structured reasoning trace (EATP Section 6)" — optional structured records of WHY decisions
- **Normative**: v2.2 feature; referenced canonical schema
- **Related**: Delegation Record, Audit Anchor, Confidentiality Classification

### Concept 6: Standalone SDK

- **Source**: README.md (Lines 36-42), Section: Standalone SDK
- **Definition**: "EATP has a standalone SDK independent of the full Kailash platform, enabling lightweight adoption without learning the entire framework"
- **Normative**: MUST implement all 5 elements, all 4 operations, 5 constraint dimensions, 6 credential formats, 3 storage backends, 15 CLI commands
- **Related**: MCP server, Interoperability formats, Storage backends

### Concept 7: Trust as Primary Barrier (Design Philosophy)

- **Source**: README.md (Line 50), Section: Key Concepts
- **Definition**: "Enterprise AI adoption is blocked by trust, not technology"
- **Normative**: Design philosophy; shapes all subsequent protocol design
- **Related**: Trust Lineage Chain, Verification, Audit

### Concept 8: Five Trust Postures

- **Source**: README.md (Line 53), Section: Key Concepts
- **Definition**: "Graduated autonomy from Pseudo-Agent (human does everything) through Delegated (remote monitoring)"
- **Normative**: Named set: PSEUDO_AGENT, SUPERVISED, SHARED_PLANNING, CONTINUOUS_INSIGHT, DELEGATED
- **Related**: VERIFY operation, Posture Transition Triggers, Enforcement Model

### Concept 9: Verification Gradient (Three Levels)

- **Source**: README.md (Line 54), Section: Key Concepts
- **Definition**: "Three levels (QUICK, STANDARD, FULL) trading off speed for thoroughness"
- **Normative**: QUICK ~1ms (hash/expiry), STANDARD ~5ms (capabilities/constraints), FULL ~50ms (cryptographic verification)
- **Related**: VERIFY operation, Trust Scoring, Reasoning Trace Verification

### Concept 10: Graceful Degradation

- **Source**: README.md (Line 56), Section: Key Concepts
- **Definition**: "Human fallback when trust cannot be established"
- **Normative**: When VERIFY fails, system MUST provide human override mechanism
- **Related**: VERIFY operation, Environmental Degradation, HELD verdict

## 01-first-principles.md

### Concept 11: First Principle 1 — Trust is the Primary Barrier

- **Source**: 01-first-principles.md (Line 3), Section: First Principle 1
- **Definition**: "Enterprise AI agent adoption is currently blocked not by technology capabilities, but by the inability to establish appropriate trust levels."
- **Normative**: Shapes framework requirement: "any framework for enterprise agents must prioritize trust architecture over feature capabilities" (line 28)
- **Related**: Trust architecture, Accountability, Agent governance

### Concept 12: First Principle 2 — Legacy Systems Have Embedded Trust

- **Source**: 01-first-principles.md (Line 30), Section: First Principle 2
- **Definition**: "Legacy enterprise systems have implicit trust relationships that must be preserved or explicitly translated when agents are introduced."
- **Normative**: "Agent trust frameworks must explicitly translate legacy trust models, not bypass them" (line 65)
- **Related**: ACLs, RBAC, Segregation of Duties, Trust translation

### Concept 13: First Principle 3 — Value Drives Adoption

- **Source**: 01-first-principles.md (Line 67), Section: First Principle 3
- **Definition**: "Enterprise adoption of any new system requires clear value demonstration, with trust infrastructure being the enabler rather than the goal."
- **Normative**: "Trust frameworks must be introduced as enablers of value, not as governance overhead" (line 88)
- **Related**: Value demonstration, Trust infrastructure enablement, Adoption sequencing

## 02-trust-lineage-chain.md

### Concept 14: Trust Lineage Chain (Overview)

- **Source**: 02-trust-lineage-chain.md (Line 5), Section: Overview
- **Definition**: "EATP's core mechanism for establishing, tracking, and verifying trust across agent interactions. It consists of five cryptographically linked elements that answer WHO authorized an action, WHAT was authorized, and WHEN it occurred."
- **Normative**: MUST consist of five cryptographically linked elements; optional reasoning traces answer WHY
- **Related**: All 5 Elements

### Concept 15: Element 1 — Genesis Record

- **Source**: 02-trust-lineage-chain.md (Line 9), Section: Element 1
- **Definition**: "The foundational trust anchor that establishes the root of authority for an agent ecosystem."
- **Normative**: MUST include Authority Identifier, Public Key, Policy Reference, Timestamp with tamper-evident properties, self-signed root credential (lines 15-21)
- **Related**: Root CA analogy, Authority verification, Delegation chain root

### Concept 16: Element 2 — Delegation Record

- **Source**: 02-trust-lineage-chain.md (Line 42), Section: Element 2
- **Definition**: "A signed record of trust delegation from one entity to another, enabling hierarchical trust models."
- **Normative**: MUST include Delegator, Delegate, Scope, Constraints, Delegation Depth, Chain Reference, Signature; MAY include Dimension Scope (v2.2+), reasoning_trace, reasoning_trace_hash, reasoning_signature (lines 48-61)
- **Related**: Hierarchical authority, Monotonic constraint tightening, Delegation depth limits, Cascade revocation

### Concept 17: Element 3 — Constraint Envelope

- **Source**: 02-trust-lineage-chain.md (Line 126), Section: Element 3
- **Definition**: "A wrapper that adds runtime constraints to an operation, enabling dynamic trust adjustment."
- **Normative**: MUST include Operation Reference, Runtime Constraints, Context, Expiry, Signature (lines 133-138); MUST implement six constraint types (lines 140-149)
- **Related**: RESOURCE_LIMIT, TIME_WINDOW, DATA_SCOPE, ACTION_RESTRICTION, AUDIT_REQUIREMENT, REASONING_REQUIRED

### Concept 18: Element 4 — Capability Attestation

- **Source**: 02-trust-lineage-chain.md (Line 177), Section: Element 4
- **Definition**: "A signed declaration of what capabilities an agent possesses and what constraints govern those capabilities."
- **Normative**: MUST include Agent Identifier, Capability List, Constraint List, Issuer Reference, Validity Period, Signature (lines 184-190)
- **Related**: Capability declaration, Agent behavior bounds, Verifiable capability claims

### Concept 19: Element 5 — Audit Anchor

- **Source**: 02-trust-lineage-chain.md (Line 217), Section: Element 5
- **Definition**: "An immutable record that anchors a complete trust lineage to a tamper-evident log, enabling post-hoc verification."
- **Normative**: MUST include Transaction ID, Lineage Hash, Outcome, Timestamp, Log Reference; MAY include Witnesses, reasoning_trace, reasoning_trace_hash, reasoning_signature (lines 223-233)
- **Related**: Post-hoc audit, Non-repudiation, Compliance, Hash-linked chaining

### Concept 20: Reasoning Trace (Extended Trust Chain)

- **Source**: 02-trust-lineage-chain.md (Line 297), Section: Section 6 — Reasoning Traces
- **Definition**: "Optional structured records of decision rationale that extend the chain with WHY — a structured, optionally signed record of decision rationale."
- **Normative**: OPTIONAL; attach to Delegation Records and Audit Anchors only; NOT Genesis/Constraint Envelope/Capability Attestation (line 312); MUST use dual-binding signing model (line 365)
- **Related**: Decision field, Rationale field, Confidentiality classification, Alternatives, Evidence refs

### Concept 21: Confidentiality Level (Five-Level Classification)

- **Source**: 02-trust-lineage-chain.md (Line 344), Section: ConfidentialityLevel
- **Definition**: "Reasoning traces contain the most privacy-sensitive information in the trust chain — they reveal thought processes, decision criteria, and business logic. A first-class confidentiality classification governs disclosure, encryption, and retention."
- **Normative**: MUST implement five levels: PUBLIC, RESTRICTED, CONFIDENTIAL, SECRET, TOP_SECRET (lines 349-354); MUST NOT be downgraded after signing (line 359)
- **Related**: SD-JWT selective disclosure, Encryption, Redaction, Retention policy

### Concept 22: ReasoningTrace Structure

- **Source**: 02-trust-lineage-chain.md (Line 314), Section: ReasoningTrace Structure
- **Definition**: "A ReasoningTrace contains: `decision` (what was decided), `rationale` (why), `confidentiality` (classification level), plus optional `alternatives_considered`, `evidence`, `methodology`, `confidence`, and `timestamp`."
- **Normative**: MUST include decision, rationale, confidentiality, timestamp (lines 320-327); confidence MUST be in [0.0, 1.0] (line 326)
- **Related**: EvidenceRef structure, Timestamp field, Confidence scoring

### Concept 23: Dual-Binding Signing Model (Reasoning Traces)

- **Source**: 02-trust-lineage-chain.md (Line 363), Section: Signing Strategy
- **Definition**: "Reasoning traces use a dual-binding signing model: the `reasoning_trace_hash` is included in the parent record's signing payload to bind the reasoning to the delegation, while the `reasoning_signature` provides independent verification of the reasoning trace content."
- **Normative**: reasoning_trace_hash MUST always be present in signing payload (either hash value or null); reasoning_signature MUST use JCS canonical serialization (RFC 8785) (lines 370-382)
- **Related**: Reasoning trace hash, Reasoning signature, JCS canonicalization, Integrity verification

### Concept 24: Dimension-Scoped Delegation

- **Source**: 02-trust-lineage-chain.md (Line 54), Section: Element 2
- **Definition**: "Which of the five constraint dimensions (Financial, Operational, Temporal, Data Access, Communication) this delegation covers. When present, the delegate's authority is limited to the listed dimensions."
- **Normative**: MAY include dimension_scope; delegations without explicit dimension scope inherit delegator's full dimension scope (lines 54-55)
- **Related**: Five constraint dimensions, Monotonic tightening, Multi-stakeholder oversight

### Concept 25: Delegation Depth

- **Source**: 02-trust-lineage-chain.md (Line 55), Section: Element 2
- **Definition**: "Distance from the genesis authority (0 = delegated directly by genesis authority, 1 = sub-delegated from a depth-0 delegate, and so on)."
- **Normative**: SHOULD enforce configurable maximum depth; default maximum 10 in reference implementation (line 55, 03-operations.md line 124)
- **Related**: Chain depth tracking, Verification latency, Attack surface expansion

### Concept 26: Chain Linkage (Five Elements)

- **Source**: 02-trust-lineage-chain.md (Line 427), Section: Chain Linkage
- **Definition**: "The five elements link together to form verifiable trust chains. Reasoning traces optionally attach to Delegation Records and Audit Anchors."
- **Normative**: MUST follow sequence: Genesis → Delegation(s) → Constraint Envelope → Capability Attestation → Audit Anchor; Reasoning traces optional on Delegation and Audit only (lines 431-454)
- **Related**: Verification process, Chain integrity, Element ordering

### Concept 27: Verification Process (Chain Resolution)

- **Source**: 02-trust-lineage-chain.md (Line 456), Section: Verification Process
- **Definition**: "Multi-step process: Resolve Genesis → Walk Chain → Validate Signatures → Check Constraints → Verify Reasoning → Record Audit."
- **Normative**: MUST perform all six steps in order; reasoning verification escalates with verification level (QUICK ignores, STANDARD warns if missing, FULL fails if missing) (lines 458-463)
- **Related**: Cryptographic verification, Constraint evaluation, Audit anchoring

## 03-operations.md

### Concept 28: Operation 1 — ESTABLISH

- **Source**: 03-operations.md (Line 7), Section: Operation 1
- **Definition**: "Create the foundational trust infrastructure for an agent ecosystem."
- **Normative**: MUST generate key pair, define governance policies, create Genesis Record, self-sign, publish to registry, create initial Capability Attestations, distribute trust anchors (lines 30-38)
- **Related**: Genesis Record, Authority registration, Initial capability definitions

### Concept 29: Operation 2 — DELEGATE

- **Source**: 03-operations.md (Line 53), Section: Operation 2
- **Definition**: "Transfer trust from one entity to another within defined bounds."
- **Normative**: MUST verify delegator authority, create Delegation Record with signature, update delegate's capability set; If reasoning_trace provided: compute hash (SHA-256 JCS), create reasoning_signature, attach both (lines 78-93); MUST enforce cascade revocation (lines 130-146)
- **Related**: Delegation Record, Constraint specification, Cascade revocation, Reasoning trace

### Concept 30: Constraint Types (Six Types)

- **Source**: 03-operations.md (Lines 97-105), Section: Constraint Types
- **Definition**: "Constraint types classify individual constraint rules. The six types are: RESOURCE_LIMIT, TIME_WINDOW, DATA_CLASSIFICATION, ACTION_RESTRICTION, AUDIT_REQUIREMENT, REASONING_REQUIRED."
- **Normative**: MUST implement all six; REASONING_REQUIRED mandates reasoning trace presence, inherited by all children, enforcement escalates with verification level
- **Related**: Five constraint dimensions, Constraint envelope, Enforcement model

### Concept 31: Dimension-Scoped Delegation (Operational)

- **Source**: 03-operations.md (Lines 107-118), Section: Dimension-Scoped Delegation
- **Definition**: "Delegations MAY be scoped to specific constraint dimensions. A delegator with authority across all five dimensions MAY delegate authority over only a subset."
- **Normative**: Sub-delegate MUST receive subset of parent delegate's dimensions, never superset (line 118); monotonic tightening enforced
- **Related**: Five constraint dimensions, Monotonic constraint tightening, Multi-stakeholder oversight

### Concept 32: Delegation Depth Limits (Enforcement)

- **Source**: 03-operations.md (Lines 120-126), Section: Delegation Depth Limits
- **Definition**: "Implementations SHOULD enforce a maximum delegation chain depth to prevent unbounded delegation chains. Deep chains increase verification latency, expand the attack surface, and make trust reasoning harder for humans."
- **Normative**: SHOULD enforce configurable maximum; reference implementation defaults to 10; delegation exceeding limit MUST be rejected
- **Related**: Chain depth, Verification latency, Attack surface

### Concept 33: Cascade Revocation

- **Source**: 03-operations.md (Lines 128-146), Section: Cascade Revocation
- **Definition**: "When trust is revoked at any delegation level, all downstream delegations MUST be automatically invalidated. No orphaned agents may continue operating after their authority source is removed."
- **Normative**: MUST propagate recursively; implementations MUST invalidate all downstream delegations within bounded propagation window; SHOULD mitigate via short-lived credentials and push-based notification
- **Related**: Revocation propagation, Immediateness, Audit anchoring of revocation

### Concept 34: Operation 3 — VERIFY

- **Source**: 03-operations.md (Line 163), Section: Operation 3
- **Definition**: "Confirm that an agent has authority to perform a specific action at runtime."
- **Normative**: MUST extract credentials, retrieve capability chain, walk to Genesis, verify all signatures, check all constraints, evaluate reasoning, generate Constraint Envelope, return authorization decision (lines 192-203); MUST implement four-level gradient (Auto-approved, Flagged, Held, Blocked) (lines 184-188)
- **Related**: Verification layers, Graceful degradation, Trust scoring, Posture transitions

### Concept 35: Verification Layers (Five Orthogonal Layers)

- **Source**: 03-operations.md (Lines 206-212), Section: Verification Layers
- **Definition**: "VERIFY checks across five orthogonal layers: Cryptographic (signatures valid/unexpired), Semantic (action within scope), Constraint (bounds satisfied), Context (runtime conditions), Reasoning (trace presence/integrity)."
- **Normative**: All five layers MUST be checked; Reasoning layer escalates with verification level (STANDARD: presence check with warning if missing; FULL: presence required, hash and signature verification)
- **Related**: Verification gradient, Signature verification, Constraint evaluation

### Concept 36: Graceful Degradation (VERIFY Failure Handling)

- **Source**: 03-operations.md (Lines 227-237), Section: Graceful Degradation
- **Definition**: "When VERIFY fails, the framework must provide human fallback: log failure reason, notify appropriate humans, provide human override mechanism with separate audit, queue action for later retry if appropriate."
- **Normative**: MUST provide human override with audit trail when VERIFY fails; fallback behavior MUST be documented
- **Related**: Human-in-the-loop, Audit trail, Override mechanisms

### Concept 37: Posture Transition Triggers (Three Categories)

- **Source**: 03-operations.md (Lines 239-250), Section: Posture Transition Triggers
- **Definition**: "Three categories of trigger may cause trust posture transitions: (1) Trust violation (constraint breach, verification failure, anomalous behavior); (2) Manual request by authorized human; (3) Environmental condition (reviewer unavailability, network partition, time-critical context)."
- **Normative**: Trust violation SHOULD trigger immediate downgrade; Environmental degradation is a valid trigger category
- **Related**: Trust violation, Human authorization, Environmental degradation, Posture state machine

### Concept 38: Verification Gradient (Operational Detail)

- **Source**: 03-operations.md (Lines 252-262), Section: Verification Levels
- **Definition**: "QUICK (~1ms): hash/expiration only, reasoning ignored. STANDARD (~5ms): capability/constraint validation, reasoning presence check. FULL (~50ms): full cryptographic verification, reasoning hash/signature verification when REASONING_REQUIRED active."
- **Normative**: QUICK ignores reasoning traces; STANDARD checks presence if REASONING_REQUIRED (valid with warning if absent); FULL fails if REASONING_REQUIRED and no trace; all levels include reasoning_trace_hash in signature verification
- **Related**: Latency, Reasoning trace verification, Trust scoring

### Concept 39: Trust Scoring (Five-Factor Quantitative)

- **Source**: 03-operations.md (Lines 264-276), Section: Trust Scoring
- **Definition**: "VERIFY output includes a trust score (0-100) computed across five weighted factors: Chain completeness (30%), Delegation depth (15%), Constraint coverage (25%), Posture level (20%), Chain recency (10%)."
- **Normative**: Scores MUST map to letter grades (A 90-100, B 80-89, C 70-79, D 60-69, F 0-59); score provides quantitative input for policy decisions
- **Related**: Quantitative risk assessment, Policy decision points, Grade mapping

### Concept 40: Enforcement Model (PDP/PEP Separation)

- **Source**: 03-operations.md (Lines 278-304), Section: Enforcement Model
- **Definition**: "VERIFY produces a VerificationResult (PDP); enforcement is separate concern (PEP). Two enforcement modes: StrictEnforcer (production, blocks unauthorized actions) and ShadowEnforcer (observation, never blocks)."
- **Normative**: StrictEnforcer MUST classify results into four verdicts; HELD SHOULD NOT auto-approve after timeout; ShadowEnforcer MUST run same classification but never block
- **Related**: StrictEnforcer, ShadowEnforcer, Verdicts, HELD timeout guidance

### Concept 41: StrictEnforcer Verdict Classification

- **Source**: 03-operations.md (Lines 295-300), Section: Enforcement Model
- **Definition**: "Four verdicts based on verification result: AUTO_APPROVED (valid, no violations), FLAGGED (valid, minor violations), HELD (violation count ≥ threshold), BLOCKED (verification failed)."
- **Normative**: AUTO_APPROVED and FLAGGED proceed; FLAGGED highlights for review; HELD queues for human review; BLOCKED rejects with EATPBlockedError
- **Related**: Enforcement outcomes, Human review queue, Exception handling

### Concept 42: ShadowEnforcer (Observation Mode)

- **Source**: 03-operations.md (Lines 306-312), Section: Enforcement Model
- **Definition**: "Observation enforcement mode that runs same classification logic as StrictEnforcer but never blocks execution. Logs all verdicts and collects metrics for gradual rollout."
- **Normative**: Never blocks execution; logs verdicts for all actions; collects metrics (block_rate, hold_rate, pass_rate, per-agent breakdown)
- **Related**: Gradual rollout, Metrics collection, Phase-based deployment

### Concept 43: Decorator Integration (@verified, @audited, @shadow)

- **Source**: 03-operations.md (Lines 314-337), Section: Decorator Integration
- **Definition**: "EATP provides decorators that wrap VERIFY and AUDIT into function annotations for framework integration."
- **Normative**: @verified wraps pre-action verification; @audited wraps post-action audit trail; @shadow runs in observation mode; both sync and async functions supported
- **Related**: Framework integration, Function wrapping, Deferred TrustOperations binding

### Concept 44: Operation 4 — AUDIT

- **Source**: 03-operations.md (Line 339), Section: Operation 4
- **Definition**: "Create immutable records of trust chain operations for compliance and investigation."
- **Normative**: MUST collect complete trust lineage, hash lineage data, create Audit Anchor, sign; If reasoning_trace provided: compute hash (SHA-256 JCS), create reasoning_signature, attach both; set reasoning_trace_hash to null if absent; sign with reasoning_trace_hash always in payload; submit to immutable log
- **Related**: Audit Anchor, Immutable storage, Audit query types, Compliance

### Concept 45: Audit Query Types (Five)

- **Source**: 03-operations.md (Lines 382-388), Section: Audit Query Types
- **Definition**: "Five query types for auditing: Lineage Query (complete trust chain), Actor Query (all actions by agent), Time Range Query (actions within period), Exception Query (failed/escalated actions), Compliance Query (regulatory criteria matches)."
- **Normative**: Listed as supported query types; no MUST enforcement level specified
- **Related**: Audit anchors, Compliance reporting, Investigation support

### Concept 46: Audit Retention Requirements

- **Source**: 03-operations.md (Lines 391-394), Section: Retention Requirements
- **Definition**: "Retention driven by enterprise policies, regulatory requirements, and legal hold considerations."
- **Normative**: Deployment-dependent; no protocol-level MUST specified
- **Related**: Compliance requirements, Legal discovery, Audit trail lifecycle

### Concept 47: Operation Lifecycle (Four Operations)

- **Source**: 03-operations.md (Lines 410-439), Section: Operation Interactions
- **Definition**: "Four operations link in sequence: ESTABLISH creates foundation → DELEGATE extends trust (feeds to VERIFY) → VERIFY produces decisions (feeds to AUDIT) → AUDIT records operations."
- **Normative**: Lifecycle diagram shows dependencies and data flows between operations
- **Related**: Trust lineage chain, Operation semantics, Data flow

## 04-integration.md

### Concept 48: Protocol Landscape (MCP/A2A/EATP)

- **Source**: 04-integration.md (Lines 5-21), Section: Protocol Landscape
- **Definition**: "Three complementary protocols: MCP (context retrieval), A2A (agent coordination), EATP (trust/authorization) form a protocol stack with distinct roles."
- **Normative**: Positions EATP as trust layer in broader protocol ecosystem
- **Related**: Model Context Protocol, Agent2Agent Protocol, Protocol stack architecture

### Concept 49: MCP + EATP Integration Pattern

- **Source**: 04-integration.md (Lines 58-107), Section: Integration Pattern: MCP + EATP
- **Definition**: "MCP requests carry EATP credentials; MCP servers verify authorization via EATP before context access; context response includes EATP audit anchor."
- **Normative**: MCP tool execution MUST verify EATP authorization; MCP resource access requires EATP capability attestation; MCP servers register with capability attestations; every MCP operation generates audit anchor
- **Related**: MCP tools, Context access control, Audit trail integration

### Concept 50: A2A + EATP Integration Pattern

- **Source**: 04-integration.md (Lines 109-159), Section: Integration Pattern: A2A + EATP
- **Definition**: "A2A task requests carry EATP delegation records; receiving agent verifies EATP chain before acting; cross-agent actions produce linked EATP audit chains."
- **Normative**: A2A agent cards include EATP capability attestations; A2A task requests carry delegation records; receiving agent MUST verify chain before execution; interactions produce composite audit chains
- **Related**: Agent discovery, Task delegation, Cross-agent authorization

### Concept 51: Combined Integration (MCP + A2A + EATP)

- **Source**: 04-integration.md (Lines 161-214), Section: Combined Integration
- **Definition**: "Multi-agent enterprise workflow where orchestrator delegates to data/analysis/report agents; each uses MCP for tool access with EATP verification; every operation audited with complete chain."
- **Normative**: Shows composition of all three protocols with complete audit trail
- **Related**: Multi-agent orchestration, Distributed trust chains, Audit aggregation

### Concept 52: EATP MCP Server (Reference)

- **Source**: 04-integration.md (Lines 216-426), Section: EATP MCP Server
- **Definition**: "The EATP standalone SDK ships an MCP server (`eatp.mcp.server`) that exposes EATP trust operations as MCP tools and trust data as MCP resources."
- **Normative**: Implements 5 tools (eatp_verify, eatp_status, eatp_audit_query, eatp_delegate, eatp_revoke) and 4 resources; implements MCP JSON-RPC protocol over stdio without external SDK dependency
- **Related**: MCP tools, MCP resources, Reference implementation

### Concept 53: Standalone SDK Integration Patterns (Four)

- **Source**: 04-integration.md (Lines 428-509), Section: Standalone SDK Integration Patterns
- **Definition**: "Four patterns for integrating standalone SDK: (1) Decorator Integration, (2) Middleware Integration, (3) Shadow Mode Rollout, (4) Interop Token Exchange."
- **Normative**: Each pattern has distinct use case; shadow mode enables observation before enforcement; token exchange enables cross-system integration
- **Related**: Framework integration, Gradual rollout, Token formats

### Concept 54: CARE Integration (Accountability Chain)

- **Source**: 04-integration.md (Lines 538-553), Section: CARE Integration
- **Definition**: "EATP is trust infrastructure layer of CARE governance. Trust Lineage Chain answers six questions: (1) What happened, (2) Who authorized, (3) What boundaries, (4) Who set boundaries, (5) Who accepts accountability, (6) Why was it authorized."
- **Normative**: Six questions map to EATP elements; Question 6 (WHY) answered by reasoning traces (new in v2.2)
- **Related**: CARE governance, Accountability traceability, Reasoning traces

## 05-standalone-sdk.md

### Concept 55: Standalone SDK Purpose

- **Source**: 05-standalone-sdk.md (Lines 3-5), Section: Purpose
- **Definition**: "The EATP SDK is a standalone Python package (`eatp`) that implements the full Enterprise Agent Trust Protocol specification. It is independent of the Kailash platform, enabling adoption without learning or deploying the broader framework."
- **Normative**: Apache 2.0 licensed, published by Terrene Foundation, open source, implements all 5 elements and 4 operations
- **Related**: Python implementation, Independent adoption, Kailash relationship

### Concept 56: SDK Installation and Extras

- **Source**: 05-standalone-sdk.md (Lines 9-22), Section: Installation
- **Definition**: "Python: `pip install eatp` with optional extras (postgres, dev). Rust: `cargo add eatp`. Requires Python 3.11+."
- **Normative**: Base package via pip; optional PostgreSQL backend; optional dev tools; Rust package via cargo
- **Related**: Storage backends, Language implementations, Development tools

### Concept 57: Quick Start Workflow

- **Source**: 05-standalone-sdk.md (Lines 30-82), Section: Quick Start
- **Definition**: "Five-line path from install to VERIFY: setup store/keys, register authority, create minimal registry, ESTABLISH trust, VERIFY action."
- **Normative**: Working example code showing full workflow with API usage
- **Related**: TrustOperations, Authority registration, Capability request

### Concept 58: CLI Commands (Core Set)

- **Source**: 05-standalone-sdk.md (Lines 89-100), Section: Core Commands
- **Definition**: "Seven core commands: eatp init (authority setup), establish (new agent), delegate, verify, revoke, status, version."
- **Normative**: All MUST be implemented; FilesystemStore used by default with fallback to InMemoryTrustStore
- **Related**: Authority initialization, Agent lifecycle, Trust verification

### Concept 59: CLI Utility Commands

- **Source**: 05-standalone-sdk.md (Lines 101-108), Section: Utility Commands
- **Definition**: "Eight utility commands: audit (query trail), export (JSON/JWT formats), verify-chain (cryptographic verification), plus seven core commands."
- **Normative**: Export supports multiple formats; verify-chain provides cryptographic validation of full chain
- **Related**: Audit query, Format export, Chain verification

### Concept 60: MCP Server (SDK Shipped)

- **Source**: 05-standalone-sdk.md (Lines 139-170), Section: MCP Server
- **Definition**: "The SDK includes MCP server (`eatp.mcp.server`) exposing trust operations as MCP tools and trust data as MCP resources. Uses JSON-RPC over stdio, no external SDK dependency."
- **Normative**: 5 tools (eatp_verify, eatp_status, eatp_audit_query, eatp_delegate, eatp_revoke); 4 resources (authorities, agents, chains, constraints)
- **Related**: MCP integration, Tool schemas, Resource URIs

### Concept 61: Enforcement Utilities (Three Modes)

- **Source**: 05-standalone-sdk.md (Lines 172-271), Section: Enforcement Utilities
- **Definition**: "Three enforcement modes: StrictEnforcer (production, blocks unauthorized), ShadowEnforcer (observation, never blocks), Decorators (@verified, @audited, @shadow for function wrapping)."
- **Normative**: StrictEnforcer classifies to four verdicts; ShadowEnforcer provides metrics; Decorators support deferred TrustOperations binding; both sync/async supported
- **Related**: Verdict classification, Metrics collection, Function decoration

### Concept 62: Storage Backends (Three Implementations)

- **Source**: 05-standalone-sdk.md (Lines 273-323), Section: Storage Backends
- **Definition**: "Three pluggable storage backends implementing `TrustStore` ABC: InMemoryTrustStore (testing), FilesystemStore (persistent JSON), PostgreSTrustStore (production)."
- **Normative**: All implement same interface (store_chain, get_chain, update_chain, delete_chain, list_chains, count_chains, close); PostgreSQL backend available with `eatp[postgres]` extra
- **Related**: Abstract base class, Transactional operations, Query interface

### Concept 63: Transactional Operations (TransactionContext)

- **Source**: 05-standalone-sdk.md (Lines 313-322), Section: Transactional Operations
- **Definition**: "`TransactionContext` provides atomic update guarantees for multi-chain operations with rollback on exception."
- **Normative**: Async context manager for atomicity; both updates applied atomically or none; rollback if `commit()` not called
- **Related**: ACID properties, Atomic updates, Consistency guarantees

### Concept 64: Trust Scoring Module (SDK)

- **Source**: 05-standalone-sdk.md (Lines 325-351), Section: Trust Scoring
- **Definition**: "Deterministic trust scores (0-100) computed from five weighted factors: Chain completeness (30%), Delegation depth (15%), Constraint coverage (25%), Posture level (20%), Chain recency (10%)."
- **Normative**: Scores map to grades (A 90-100, B 80-89, C 70-79, D 60-69, F 0-59); includes risk indicators and recommendations
- **Related**: Five-factor model, Risk assessment, Quantitative evaluation

### Concept 65: Trust Postures (Five-Level State Machine) ★ CANONICAL

- **Source**: 05-standalone-sdk.md (Lines 353-365), Section: Trust Postures
- **Definition**: "Five-level state machine governing agent autonomy: PSEUDO_AGENT (none, human in-loop), SUPERVISED (low, human approves), SHARED_PLANNING (medium, co-plan), CONTINUOUS_INSIGHT (high, agent executes/human monitors), DELEGATED (full, remote monitoring)."
- **Normative**: Postures can upgrade (performance-based) and downgrade (instantly on condition change); factored into trust scoring
- **Related**: Autonomy levels, Human oversight, State transitions

### Concept 66: Constraint Dimensions (Five) ★ CANONICAL

- **Source**: 05-standalone-sdk.md (Lines 369-379), Section: Constraint Dimensions
- **Definition**: "Five dimensions organizing constraint rules: Financial (transaction limits), Operational (action/visibility controls), Temporal (operating hours/blackout), Data Access (classification ceiling), Communication (channels/rate limits)."
- **Normative**: MUST implement all five at EATP Conformant level (per 07-conformance.md line 50)
- **Related**: Constraint types, Constraint envelope, Policy control

### Concept 67: Constraint Types (Six — SDK View)

- **Source**: 05-standalone-sdk.md (Lines 381-392), Section: Constraint Types
- **Definition**: "Six constraint types: RESOURCE_LIMIT (bounds on consumption), TIME_WINDOW (temporal boundaries), DATA_SCOPE (data access restrictions), ACTION_RESTRICTION (operation limitations), AUDIT_REQUIREMENT (recording mandates), REASONING_REQUIRED (reasoning trace mandates)."
- **Normative**: REASONING_REQUIRED enforcement escalates with verification gradient (advisory at STANDARD, mandatory at FULL)
- **Related**: Five constraint dimensions, Constraint envelope, Enforcement model

### Concept 68: Built-In Constraint Templates (Six)

- **Source**: 05-standalone-sdk.md (Lines 394-406), Section: Built-in Templates
- **Definition**: "Six pre-built constraint templates: governance, finance, community, standards, audit, minimal."
- **Normative**: Loadable via `get_template()`; customizable via `customize_template()`
- **Related**: Template patterns, Customization, Policy reuse

### Concept 69: Reasoning Traces (SDK Implementation)

- **Source**: 05-standalone-sdk.md (Lines 408-420), Section: Reasoning Traces
- **Definition**: "SDK implements optional structured reasoning traces with: structured fields (decision, rationale, confidentiality, etc.), dual-binding signing, five confidentiality levels, SD-JWT selective disclosure, input validation."
- **Normative**: confidence MUST be in [0.0, 1.0]; free-text fields have SHOULD-level max lengths; reasoning_trace_hash always in signing payload
- **Related**: Dual-binding signing, Confidentiality classification, SD-JWT integration

### Concept 70: SDK Package Structure

- **Source**: 05-standalone-sdk.md (Lines 422-456), Section: Package Structure
- **Definition**: "Organized as modules: **init** (public API), authority, chain, crypto, operations, scoring, postures, templates, store (backends), enforce (enforcement modes), interop (credential formats), mcp (server), cli (commands)."
- **Normative**: Public API surface exposed via **init**; all modules have clear responsibilities
- **Related**: Module organization, Public API, Implementation structure

### Concept 71: Wire Format Mapping (SDK to Spec)

- **Source**: 05-standalone-sdk.md (Lines 458-544), Section: Wire Format Mapping
- **Definition**: "SDK uses implementation-specific field names mapping to protocol's abstract element definitions. External implementations must map accordingly."
- **Normative**: Mappings provided for Genesis/Delegation/Capability/Audit; CARE extensions documented; SDK extensions optional for interoperability
- **Related**: Protocol compliance, Interoperability, Schema extensions

### Concept 72: SDK Relationship to Kailash

- **Source**: 05-standalone-sdk.md (Lines 545-551), Section: Relationship to Kailash
- **Definition**: "Standalone SDK implements all five elements, four operations, constraint system, enforcement, interop, MCP server, and CLI. Kailash Kaizen builds on top, adding orchestration, registry, multi-agent coordination, platform features. Using Kailash not required."
- **Normative**: Both open source Apache 2.0, published by Terrene Foundation
- **Related**: Independent adoption, Platform layering, Reference implementations

## 06-interoperability.md

### Concept 73: Interoperability (Seven Formats)

- **Source**: 06-interoperability.md (Lines 5-6), Section: Overview
- **Definition**: "EATP trust chains can be exported to and imported from industry-standard credential and token formats, enabling interoperability without requiring counterparties to adopt EATP natively."
- **Normative**: All modules under `eatp.interop`; use Ed25519 cryptographic primitives; use JCS canonical serialization for reasoning trace hashing
- **Related**: Seven interop formats, Credential formats, Token standards

### Concept 74: Format Matrix (Seven Formats)

- **Source**: 06-interoperability.md (Lines 11-21), Section: Format Matrix
- **Definition**: "Seven export/import formats: JWT (RFC 7519, API auth), W3C VC (cross-org trust), DID (agent identity), UCAN (decentralized delegation), SD-JWT (selective disclosure), Biscuit (attenuation tokens), VerificationBundle (offline audit)."
- **Normative**: All formats supported; each mapped to specific use case
- **Related**: Token formats, Credential types, Interoperability standards

### Concept 75: JWT Mapping to EATP

- **Source**: 06-interoperability.md (Lines 23-77), Section: JWT (RFC 7519)
- **Definition**: "EATP uses standard IETF claims (iss, sub, iat, exp, jti) plus custom `eatp_*` claims carrying chain data. Reasoning traces with public/restricted confidentiality included; confidential+ redacted to hash only."
- **Normative**: EdDSA (Ed25519) signing; `eatp_chain` claim serializes lineage; reasoning traces included/excluded by confidentiality level
- **Related**: Bearer tokens, Standard claims, Reasoning trace disclosure

### Concept 76: W3C VC Mapping

- **Source**: 06-interoperability.md (Lines 79-134), Section: W3C Verifiable Credentials
- **Definition**: "EATP chains/capabilities expressed as W3C VCs with issuer DID, credentialSubject containing chain elements, Ed25519Signature2020 proof. Fields use camelCase per W3C convention."
- **Normative**: Supports both TrustChain and CapabilityAttestation types; reasoning traces included per confidentiality level; Ed25519 proof required
- **Related**: Verifiable credentials, W3C standards, Cross-org trust

### Concept 77: DID (Decentralized Identifiers)

- **Source**: 06-interoperability.md (Lines 137-211), Section: DID
- **Definition**: "EATP defines `did:eatp` method for agent/authority identity and supports `did:key` for cross-system compatibility. `did:eatp:<id>` for agents/authorities; `did:key:z6Mk...` for Ed25519-based identity."
- **Normative**: DID documents follow W3C DID Core; use Ed25519VerificationKey2020 for verification methods
- **Related**: Self-sovereign identity, W3C DID Core, Key-based identity

### Concept 78: UCAN v0.10.0 Mapping

- **Source**: 06-interoperability.md (Lines 213-265), Section: UCAN v0.10.0
- **Definition**: "EATP delegation records expressed as UCAN tokens with delegator/delegatee DIDs, capabilities as attenuations, expiration, reasoning trace in facts (if public/restricted)."
- **Normative**: EdDSA signing; full verification: structure, header, signature, expiry, fact extraction; reasoning traces included per confidentiality level
- **Related**: Decentralized authorization, Token format, Delegation expression

### Concept 79: SD-JWT (Selective Disclosure)

- **Source**: 06-interoperability.md (Lines 267-342), Section: SD-JWT
- **Definition**: "Allows holder to present subset of claims while maintaining cryptographic integrity. EATP uses `_sd` array (SHA-256 hashes) plus disclosures. Reasoning trace disclosure varies by confidentiality."
- **Normative**: PUBLIC: always-disclosed; RESTRICTED: selectively disclosed to chain participants; CONFIDENTIAL/SECRET/TOP_SECRET: hash only, content withheld
- **Related**: Selective disclosure, Privacy-preserving proofs, Confidentiality-driven behavior

### Concept 80: Biscuit / Macaroon Tokens

- **Source**: 06-interoperability.md (Lines 344-393), Section: Biscuit / Macaroon Tokens
- **Definition**: "Authorization token format supporting attenuation: anyone holding token can add restrictions without original signing key. EATP format: authority block, attenuation blocks, Ed25519 signature chain."
- **Normative**: Binary layout with authority facts and attenuator blocks; signature chaining ensures tampering invalidates downstream signatures
- **Related**: Attenuation model, Constraint tightening, Signature chains

### Concept 81: EU AI Act Mapping

- **Source**: 06-interoperability.md (Lines 395-409), Section: Regulatory Framework Mapping
- **Definition**: "Maps EATP mechanisms to EU AI Act high-risk requirements: Risk management (trust scoring), Technical documentation (lineage chain), Record-keeping (AUDIT), Transparency (chain verification), Human oversight (trust postures)."
- **Normative**: Maps six requirements to EATP mechanisms; reasoning traces document decision logic
- **Related**: Compliance, Regulatory alignment, AI governance

### Concept 82: NIST AI RMF Mapping

- **Source**: 06-interoperability.md (Lines 411-418), Section: NIST AI Risk Management Framework
- **Definition**: "Maps EATP to NIST AI RMF functions: Govern (genesis records + postures), Map (capabilities + dimensions), Measure (trust scoring + shadow metrics), Manage (StrictEnforcer + AUDIT)."
- **Normative**: Four-function mapping showing EATP coverage
- **Related**: NIST framework, Risk governance, Management functions

### Concept 83: OWASP Agentic Top 10 Coverage

- **Source**: 06-interoperability.md (Lines 420-429), Section: OWASP Agentic Top 10
- **Definition**: "Addresses all six OWASP Agentic risks: Excessive agency (constraints), Privilege escalation (monotonic tightening), Uncontrolled delegation (chain tracking/depth limits), Lack of accountability (audit trail), Insufficient authorization (VERIFY checks), Opaque decision-making (lineage chain + reasoning traces)."
- **Normative**: Complete coverage of OWASP top risks
- **Related**: Security risks, Mitigation strategies, Agent governance

### Concept 84: VerificationBundle (Self-Contained Export)

- **Source**: 06-interoperability.md (Lines 431-556), Section: VerificationBundle
- **Definition**: "Self-contained export format packaging everything needed to independently verify EATP trust chain without access to original trust store. Complements individual credential formats for audit/compliance/offline verification use cases."
- **Normative**: MUST contain 8 fields; MAY include reasoning_traces, delegations, constraint_snapshot, metadata; JSON Schema structure defined
- **Related**: Offline verification, Audit package, Compliance export

### Concept 85: VerificationBundle Fields

- **Source**: 06-interoperability.md (Lines 447-479), Section: Structure
- **Definition**: "Bundle contains MUST fields: bundle_id (UUID), created_at (ISO 8601), eatp_version, chain, anchors, verification (verified_at, level, result, score, warnings, chain_hash), bundle_signature, signer_public_key. MAY: reasoning_traces, delegations, constraint_snapshot, metadata."
- **Normative**: All MUST fields required; verification contains five MUST sub-fields; reasoning traces follow confidentiality rules
- **Related**: Bundle composition, Verification metadata, Confidentiality filtering

### Concept 86: Round-Trip Fidelity (Export/Import Semantics)

- **Source**: 06-interoperability.md (Lines 560-562), Section: Design Principles — Round-Trip Fidelity
- **Definition**: "All export/import pairs preserve EATP semantics. Chain exported to JWT and imported back produces equivalent TrustLineageChain. Reasoning traces included/excluded based on confidentiality within disclosed scope."
- **Normative**: Fidelity applies within disclosed scope; all formats support round-trip conversion
- **Related**: Interoperability guarantee, Format equivalence, Semantic preservation

### Concept 87: No External Dependencies (Interop)

- **Source**: 06-interoperability.md (Lines 564-566), Section: No External Dependencies
- **Definition**: "DID, W3C VC, UCAN, SD-JWT, Biscuit modules implemented directly using standard library and PyNaCl. No external interop libraries required."
- **Normative**: Only PyNaCl (always available) and standard library used
- **Related**: Minimal dependencies, Implementation self-sufficiency, Standard protocols

### Concept 88: Cryptographic Consistency (Cross-Format)

- **Source**: 06-interoperability.md (Lines 568-570), Section: Cryptographic Consistency
- **Definition**: "All interop formats use Ed25519 signing from `eatp.crypto`, ensuring same key material works across all export formats. Single authority keypair can sign JWTs, VCs, UCANs, and Biscuit tokens."
- **Normative**: Uniform Ed25519 usage across all formats
- **Related**: Key reuse, Cross-format verification, Signature consistency

## 07-conformance.md

### Concept 89: Three Conformance Levels

- **Source**: 07-conformance.md (Lines 5-6), Section: Overview
- **Definition**: "Three normative conformance levels enabling implementations to declare coverage and adopters to evaluate interoperability guarantees: EATP Compatible, EATP Conformant, EATP Complete."
- **Normative**: RFC 2119 keywords apply; conformance is distinct from reference implementation status
- **Related**: Implementation certification, Interoperability guarantees, Coverage tiers

### Concept 90: EATP Compatible (Level 1)

- **Source**: 07-conformance.md (Lines 11-40), Section: Conformance Level 1
- **Definition**: "Lightweight integration level: MUST implement Genesis Record, Audit Anchor with hashing, at least one constraint dimension, ESTABLISH and AUDIT operations, VERIFY at QUICK level, SHA-256 hashing, Ed25519 signing, JSON serialization, ISO 8601 timestamps, reasoning_trace_hash in payloads."
- **Normative**: MUST NOT support Delegation, Capability Attestations, postures, verification gradient, or reasoning traces
- **Related**: Lightweight adoption, Basic trust anchoring, Audit trails

### Concept 91: EATP Conformant (Level 2)

- **Source**: 07-conformance.md (Lines 42-78), Section: Conformance Level 2
- **Definition**: "Full protocol compliance: all five elements, all four operations, VERIFY at STANDARD/FULL levels, four-category verification gradient, cascade revocation, reasoning trace acceptance and dual-binding signing, REASONING_REQUIRED constraint type."
- **Normative**: MUST implement all five dimensions; SHOULD pass 80% of SHOULD-level test cases; SHOULD implement persistent storage, push-based revocation, trust scoring
- **Related**: Full protocol support, Production readiness, Core functionality

### Concept 92: EATP Complete (Level 3)

- **Source**: 07-conformance.md (Lines 80-116), Section: Conformance Level 3
- **Definition**: "Comprehensive compliance: all postures, all verification levels with reasoning verification at FULL, all five confidentiality levels (immutable after signing), both enforcement modes (StrictEnforcer + ShadowEnforcer), export to ≥2 of 6 formats, VerificationBundle."
- **Normative**: MUST implement all five postures with upgrade/downgrade logic including environmental conditions; MUST support export to ≥2 formats + VerificationBundle; SHOULD pass all SHOULD-level tests, implement Merkle tree verification, key rotation, multi-sig, circuit breaker
- **Related**: Full feature set, Enterprise-grade, Advanced functionality

### Concept 93: Conformance Declaration Requirements

- **Source**: 07-conformance.md (Lines 118-128), Section: Conformance Declaration
- **Definition**: "Implementations declaring conformance MUST include: conformance level, EATP spec version, constraint dimensions supported, verification levels supported, interoperability formats supported."
- **Normative**: MUST include four metadata fields; implementations MAY self-declare; Terrene Foundation MAY publish conformance test suite
- **Related**: Metadata disclosure, Interoperability guarantees, Test certification

### Concept 94: Reference Implementation Status

- **Source**: 07-conformance.md (Lines 130-137), Section: Reference Implementation
- **Definition**: "Reference implementation is Foundation-maintained implementation serving as normative baseline for protocol behavior. Orthogonal to conformance level; reference status denotes Foundation stewardship and normative authority, not a conformance tier."
- **Normative**: Current reference implementations: Python (`pip install eatp`, Apache 2.0) and Rust (`cargo add eatp`, Apache 2.0)
- **Related**: Foundation stewardship, Protocol authority, Multiple implementations

---

## Summary

**Total concepts enumerated:** 94

### Canonical Five Trust Postures (★ confirmed)

1. **PSEUDO_AGENT** — None (human in-the-loop; agent is interface only)
2. **SUPERVISED** — Low (human in-the-loop; agent proposes, human approves)
3. **SHARED_PLANNING** — Medium (human on-the-loop; human and agent co-plan)
4. **CONTINUOUS_INSIGHT** — High (human on-the-loop; agent executes, human monitors)
5. **DELEGATED** — Full (human over-the-loop; remote monitoring)

### Canonical Five Constraint Dimensions (★ confirmed)

1. **Financial** — Transaction limits, spending caps, cumulative budgets
2. **Operational** — Permitted and blocked actions, visibility level
3. **Temporal** — Operating hours, blackout periods, time-bounded authorizations
4. **Data Access** — Classification ceiling, read-only mode, PII access
5. **Communication** — Permitted channels, rate limits, external access

Terrene-naming rule verified: these five match what `rules/terrene-naming.md` declares canonical. ✓

### Contradictions found

**None.** The EATP specification is internally consistent across the seven source files.

### Gaps flagged (for future spec work, not FORGE v1)

1. **Pre-v2.2 migration tooling** — spec notes re-signing requirement but provides no migration/rollback procedure
2. **Clock skew tolerance** — VERIFY lists clock skew as failure mode but gives no threshold
3. **Key rotation protocol** — listed as SHOULD for EATP Complete but no protocol specified
4. **Multi-signature delegation** — listed as SHOULD for EATP Complete but no coordination model
5. **Circuit breaker thresholds** — listed as SHOULD for EATP Complete but no trigger values

### Notes on authority lineage

- EATP's five trust postures map to CO Principle 4 (Human-on-the-Loop) and CARE's human-oversight taxonomy.
- EATP's constraint envelope (five dimensions) is the **canonical shared schema** across CARE, EATP, and CO — see concept 4.
- Reasoning traces (v2.2) answer CARE's sixth accountability question (WHY was it authorized) — see concept 54.
- Cross-references to CARE and CO are structural, not decorative: changes to EATP's envelope schema propagate to both.
