---
case_id: CASE-13
source_path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md
title: "Public Fields as Security Holes"
atoms_served: [encapsulation-as-security-boundary, signed-struct-integrity]
spec_concepts: [EATP §16, PACT §10, CO §5, EATP §33]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-13: Public Fields as Security Holes

## 1. The Situation

The Kailash Rust SDK's EATP module contained a `DelegationRecord` struct in `crates/eatp/src/delegation.rs` that represented a signed delegation of authority from one entity to another. The struct tracked who delegated what capability to whom, under what constraints, and whether the delegation had been revoked. The delegation chain was the backbone of the trust protocol -- every capability assertion in the system traced back through a chain of delegation records.

All fields on `DelegationRecord` were declared `pub` (public), following a Rust convention common in data-oriented structs. This included fields that were security-sensitive: `revoked` (whether the delegation had been revoked), `signature` (the cryptographic signature validating the record), `dimension_scope` (the envelope dimensions the delegation was constrained to), `multi_sig_policy` (the multi-signature requirements), and `multi_sig_bundle` (the collected signatures).

## 2. The Trigger

A security review during a red team phase examined what operations were possible with a cloned `DelegationRecord`. Because all fields were `pub`, code holding a cloned record could directly mutate any field. The reviewer identified two specific attack vectors: flipping `revoked` from `true` to `false` on a cloned record (un-revoking a revoked delegation) and widening `dimension_scope` on a cloned record (expanding the authority beyond what was originally granted). Both mutations would bypass the monotonic tightening invariant -- the PACT principle that delegated authority can only be narrowed, never widened, as it flows down the chain.

The attentional pattern is **access-level audit on trust-bearing structs**: the reviewer did not look for bugs in the delegation logic but instead asked what the type system allowed any holder of the struct to do.

## 3. The Move

The move is **encapsulation-as-security-boundary** combined with **signed-struct-integrity**. Eight security-sensitive fields were changed from `pub` to `pub(crate)`, restricting direct field access to within the EATP crate itself. Read-only getter methods were added for external consumers that needed to inspect these fields. A `DelegationRecord::new()` constructor was introduced as the only way to create records, and a `set_multi_sig()` method was added as the single approved mutation path for the one field that legitimately needed post-construction modification.

All external consumers -- tests, examples, FFI bindings -- were updated to use the getter methods instead of direct field access. The change was mechanical in the sense that every external access site needed the same transformation (field access to method call), but the skill was in identifying which fields were security-sensitive and which could safely remain public.

## 4. The Outcome

The fix eliminated the attack surface for cloned-record mutation. Records returned by query methods became read-only at the field level. The journal notes that cloning a record and modifying it already had no effect on the chain's internal state (the chain validates signatures on every operation), but the public fields created a false affordance: code that mutated a cloned record's `revoked` field would compile and run without error, silently producing an invalid record that would only fail when presented back to the chain. The encapsulation change moved the failure from runtime signature validation to compile-time access denial -- a strictly better error surface.

The residual risk assessment in the journal confirmed that records within the chain's internal HashMap were already protected, and the getter-based API provided the same read access that external consumers needed without the mutation affordance.

## 5. The Spec Connection

**EATP §16 (Delegation Chain Integrity)** requires that delegation records form a tamper-evident chain where each record's validity depends on its relationship to the records above it. Public mutable fields violated this requirement not by breaking the chain's internal validation but by making it trivially easy to construct records that looked valid in isolation but would fail chain validation. The integrity requirement implies that the record type itself should resist mutation, not just the chain that contains it.

**PACT §10 (Monotonic Tightening)** states that delegated authority can only be narrowed as it flows down the organizational hierarchy -- a child delegation's envelope must be a subset of its parent's. The `pub dimension_scope` field allowed any code holding a cloned record to widen the scope, violating monotonic tightening at the data level. The fix enforced tightening at the type level by removing the public setter path for scope.

**CO §5 (Guardrails)** requires that constraints be enforced automatically rather than relying on developers to remember them. The original design relied on the developer knowing not to mutate security-sensitive fields on a delegation record -- a guardrail-by-convention. The fix replaced convention with compilation: the Rust type system now enforces the constraint, making the guardrail automatic.

**EATP §33 (Signature Validation)** specifies that every operation on a trust chain must validate the cryptographic signatures of the records involved. The public `signature` field meant that a cloned record's signature could be overwritten with an arbitrary value. While the chain would catch this on the next validation pass, the public field created a window where invalid signatures could exist in application memory without any compile-time or construction-time warning. The getter-only approach ensures signatures are set once at construction and never modified externally.

## 6. Discussion Questions

1. The journal notes that cloning and mutating a record already had no effect on the chain's state because signatures are validated on every operation. If the runtime already caught the mutation, why is the compile-time fix still valuable? Under what conditions would the runtime check alone be insufficient?

2. The fix changed 8 fields from `pub` to `pub(crate)`. In a language without crate-level visibility (e.g., Python), how would you achieve the same security boundary? What are the trade-offs of each approach?

3. The `set_multi_sig()` method was added as an approved mutation path for multi-signature bundles. Why does multi-signature collection require post-construction mutation when other fields do not? What invariants must `set_multi_sig()` enforce to remain safe?

4. If you were designing a new trust-bearing struct from scratch, would you start with all fields private and add getters as needed, or start with all fields public and restrict as security reviews identify risks? What are the failure modes of each approach in a fast-moving codebase?
