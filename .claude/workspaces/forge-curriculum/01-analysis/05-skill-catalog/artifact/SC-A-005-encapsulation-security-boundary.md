---
atom_id: SC-A-005
name: Encapsulation as security boundary in signed structs
craft_layer: artifact
destination: co-codegen
craft_areas: [attend-how, intervene-how]
applications: [COC]
spec_lineage:
  - EATP §16 — Element 2 — Delegation Record
  - PACT §10 — Monotonic Tightening Invariant
  - CO §5 — Deterministic Enforcement Over Probabilistic Compliance
journal_evidence:
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0009-RISK-delegation-record-pub-fields.md
    polarity: NEGATIVE
    quote: "Security review found that all fields on DelegationRecord (in crates/eatp/src/delegation.rs) were pub, including security-sensitive fields: revoked, signature, dimension_scope, multi_sig_policy, multi_sig_bundle. Code holding a cloned record could flip revoked = false or widen dimension_scope, bypassing the monotonic tightening invariant."
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0004-RISK-signature-invalidation.md
    polarity: MIXED
    quote: "DelegationRecord is a signed struct — the signature covers the serialized payload. Adding dimension_scope: Option<HashSet<String>> changes the serialized representation, potentially invalidating ALL existing delegation chain signatures."
practice_modality: case
status: draft
---

## The move

When a struct carries a cryptographic signature or participates in a governance chain (delegation records, capability attestations, operating envelopes), default every field to private with read-only accessors. Provide a single approved-mutation constructor path. Field visibility on signed/governed structs is a security boundary, not an ergonomics decision. The question is not "will callers want to read this field?" (they will) but "can callers mutate this field without invalidating the security invariant?" (they must not).

## When it fires

Every time a struct is created or modified that (a) is signed, (b) participates in a trust chain, or (c) enforces a monotonic constraint (values can tighten but never widen). The trigger is the presence of a signature field or a constraint-bearing field on the struct — if either exists, all fields must be audited for visibility.

## What it serves

EATP §16 (Element 2 — Delegation Record) defines the delegation record as "a signed record of trust delegation from one entity to another." The signature covers the serialized payload — any field mutation after signing invalidates the signature. PACT §10 (Monotonic Tightening Invariant) is the safety guarantee that "no operating envelope at any level can be wider than the envelope at any ancestor level." Public mutable fields on a delegation record allow callers to widen `dimension_scope`, violating monotonic tightening without triggering a signature check. CO §5 (Deterministic Enforcement) requires that critical rules be enforced deterministically, not probabilistically — relying on callers to not mutate public fields is probabilistic enforcement.

## Evidence walkthrough

- **kailash-rs 0009** (NEGATIVE) — A security review found all fields on `DelegationRecord` were `pub`, including `revoked`, `signature`, `dimension_scope`, `multi_sig_policy`, and `multi_sig_bundle`. Code holding a cloned record could flip `revoked = false` or widen `dimension_scope`, directly bypassing the monotonic tightening invariant. The fix changed 8 security-sensitive fields to `pub(crate)` with read-only getters and added a `DelegationRecord::new()` constructor as the single approved mutation path. This is the canonical negative case: the struct was correct in its logic but open in its visibility, and the visibility gap was a trust-model bypass.
- **kailash-rs 0004** (MIXED) — When `dimension_scope` was added to `DelegationRecord`, the entry identified that any change to the serialized representation risks invalidating all existing delegation chain signatures. The mitigation used conditional payload inclusion (`skip_serializing_if = "Option::is_none"`) so that existing records without the field deserialize correctly. This is the secondary constraint: even adding a new field to a signed struct requires backward-compatible serialization, and the new field must default to the least-privileged interpretation (`None` means "all dimensions" — backward compatible but maximally permissive, which is then tightened by explicit scoping).

## How to drill it

Present the practitioner with a Rust or Python struct that has five fields, three of which are security-sensitive (a signature, a revocation flag, and a scope set). All fields are public. The drill: (a) identify which fields must be private, (b) design the accessor API (read-only getters vs. approved-mutation methods), (c) write one test that demonstrates the bypass if the fields remain public (clone the record, flip the revocation flag, confirm the chain accepts the modified record), and (d) write one test that demonstrates the fix blocks the bypass. Score on: all three security-sensitive fields identified, the approved-mutation path is a constructor or builder (not a setter), and the bypass test is a real executable scenario, not a hypothetical.

## Common failure mode

Practitioner defaults to `pub` for convenience during initial implementation, intending to tighten visibility later. "Later" never arrives because the struct works — callers read the fields they need, tests pass, the API is ergonomic. The security boundary is invisible until a red team reviewer or a real attacker discovers that mutation is unrestricted. The cure is the default: private + accessor + constructor. The ergonomic cost is paid once at struct creation; the security cost of `pub` is paid on every future audit.

## Related atoms

- SC-A-006 (negative tests for deny-by-default) — both atoms address the pattern of "default-open unless explicitly closed." Encapsulation is the struct-level version; deny-by-default is the middleware-level version.
- SC-A-003 (defense-in-depth codification) — the encapsulation boundary is one enforcement surface; the signature verification is a second; the chain's internal validation is a third. All three must hold independently.
