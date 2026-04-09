---
case_id: CASE-20
source_path: loom/kailash-py/workspaces/kailash/journal/0015-CONNECTION-pact-eatp-bridge-never-wired.md
title: "Adjacent But Disconnected"
atoms_served: [adjacent-module-gap-detection, spec-wiring-verification]
spec_concepts: [PACT §40, PACT §42, EATP §14, CO §17]
craft_layer: brokerage
recommended_use: discussion
destination: both
application: COC
---

# CASE-20: Adjacent But Disconnected

## 1. The Situation

The Kailash Python SDK's `src/kailash/trust/` package contained two governance modules side by side. The PACT module (`src/kailash/trust/pact/`) implemented organizational governance: D/T/R accountability grammar, operating envelopes, bridge approvals, and an audit trail. The EATP module (`src/kailash/trust/chain.py` + `src/kailash/trust/operations/`) implemented trust lineage: genesis records, capability attestations, delegation records, and cryptographic verification. Both modules lived in the same `trust/` package tree. Both were structurally complete and independently functional.

The two modules solved complementary halves of the same problem. PACT answered "who is allowed to do what under which constraints." EATP answered "can we cryptographically verify the chain of authority that granted this permission." Together they would provide governance decisions backed by verifiable trust lineage. Apart, each was incomplete.

## 2. The Trigger

An analysis of the codebase revealed that the two modules had zero integration. PACT's `engine.py` contained 0 imports from `kailash.trust.chain`. PACT's `audit.py` defined its own `AuditAnchor` class -- a different class from EATP's `AuditAnchor` in `chain.py`, despite sharing the same name. EATP's `TrustOperations` exposed `establish()`, `delegate()`, `verify()`, and `audit()` methods -- none of which were called by any PACT code. EATP's `TrustStore` (with filesystem, SQLite, and memory backends) was never instantiated by PACT. The modules were structurally adjacent but functionally isolated.

The attentional skill here is **spec-wiring verification**: checking whether modules that the specification says should interact actually do interact at the code level. The adjacency in the package tree created an appearance of integration that the import graph contradicted.

## 3. The Move

The move is **adjacent-module-gap-detection** combined with **spec-wiring-verification**. The practitioner mapped the designed integration point: Issue #199 (EATP record emission) was identified as the bridge. When PACT assigns an envelope to a role, that is a `DelegationRecord` in EATP terms. When a clearance is granted, that is a `CapabilityAttestation`. When the organization is created, that is a `GenesisRecord`. The design is dual emission: PACT keeps its own audit chain for tamper-evident governance trail AND emits EATP types for cross-system interoperability.

The non-obvious aspect is the minimum-coupling architecture proposed: `GovernanceEngine.__init__()` gains an optional `trust_chain_store: TrustStore | None` parameter. When provided, EATP records are emitted alongside PACT audit anchors. When `None`, PACT runs standalone -- backward compatible. This preserves both modules' independence while creating the bridge. The `AuditAnchor` name collision between the two modules was also flagged as a concrete usability risk for anyone importing both.

## 4. The Outcome

The analysis produced a concrete integration design: an optional dependency from PACT to EATP types (not vice versa), dual emission of audit records, and backward compatibility via `None` default. Three open questions were explicitly documented for discussion: whether the dual audit trail should be permanent or whether PACT should eventually migrate entirely to EATP record types; whether the `AuditAnchor` name collision creates real confusion for users importing both modules; and whether standalone mode should still produce EATP record objects for local inspection even without persisting them.

## 5. The Spec Connection

**PACT §40 (EATP Integration)** specifies that PACT governance decisions should emit EATP-compatible records for cross-system interoperability. The case is a direct violation: the spec says the two systems should be integrated, the code says they are not. The integration is not a nice-to-have; it is a named requirement in the PACT specification that went unimplemented despite both modules being structurally complete.

**PACT §42 (Delegation Record EATP Backing)** specifies that when PACT delegates authority (envelope assignment, bridge approval), the delegation should be backed by an EATP `DelegationRecord` with cryptographic verification. The case shows that PACT performs delegations and emits its own custom audit anchors, but never produces the EATP-backed delegation records that §42 requires. The governance action is auditable within PACT but not verifiable within EATP's trust lineage.

**EATP §14 (Trust Lineage Chain)** defines the chain of records (genesis, capability, delegation) that establishes verifiable trust. PACT's governance decisions -- particularly envelope assignments and clearance grants -- are exactly the kind of authority events that should appear in the trust lineage chain. Their absence means the EATP chain has a gap: it can verify delegations created directly through EATP operations but cannot verify delegations created through PACT governance, even though PACT is the primary governance mechanism.

**CO §17 (Single Source of Truth)** is relevant because of the duplicate `AuditAnchor` class. Both PACT's `audit.py` and EATP's `chain.py` define an `AuditAnchor` with overlapping semantics but different implementations. This violates Single Source of Truth at the type level: two classes with the same name and similar purpose exist in the same package tree with no declared relationship between them.

## 6. Discussion Questions

1. If the practitioner had wired the integration by making PACT depend directly on EATP (hard dependency, not optional), what would the consequences be for users who only need PACT governance without cryptographic trust verification? Conversely, if the integration stays optional forever, what happens to the spec promise of PACT §40 -- is an optional integration sufficient to satisfy a specification requirement?

2. The dual `AuditAnchor` problem (same name, different class, same package tree) was flagged but not resolved. What are the three possible resolutions -- rename one, merge both into a shared type, or namespace them apart -- and what does each resolution signal about the long-term relationship between PACT and EATP? Which resolution best serves a user who starts with PACT-only and later adds EATP?

3. Consider the counterfactual: if the two modules had been in different packages (`kailash-pact` and `kailash-eatp`) instead of the same `trust/` tree, would the gap have been discovered sooner or later? What role does physical adjacency in the codebase play in creating expectations of integration -- and is that expectation a help or a hindrance?
