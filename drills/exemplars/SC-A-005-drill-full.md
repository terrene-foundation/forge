---
drill_id: DR-SC-A-005-FULL
source_atom: SC-A-005
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Lock Down Encapsulation on a Signed Governance Struct

## Setup

The practitioner receives a Python dataclass representing a delegation record in a trust chain. The struct is used in a PACT-style governance system where one entity delegates authority to another, with constraints on what the delegate may do. The delegation record is **signed** — a cryptographic signature covers the serialized payload, and any downstream consumer verifies the signature before accepting the record.

All fields are currently public (no leading underscore, no property accessors, no frozen dataclass). The practitioner also receives a short test file that demonstrates the system working correctly under normal use.

**`delegation.py`:**

```python
import hashlib
import json
from dataclasses import dataclass, field


@dataclass
class DelegationRecord:
    """A signed record of trust delegation from one entity to another."""

    # Identity
    delegator_id: str
    delegate_id: str

    # Governance constraints
    dimension_scope: set[str]    # Which dimensions this delegate may act on
    max_depth: int               # How many levels deep this delegate may re-delegate
    revoked: bool = False        # Whether this delegation has been revoked

    # Cryptographic
    signature: str = ""          # Covers the serialized payload
    signed_payload_hash: str = ""  # Hash of the payload at signing time

    # Metadata
    created_at: str = ""
    notes: str = ""

    def sign(self, signing_key: str) -> None:
        """Sign the current state of the record."""
        payload = self._serialize_payload()
        self.signed_payload_hash = hashlib.sha256(payload.encode()).hexdigest()
        # Simplified: real implementation uses asymmetric crypto
        self.signature = hashlib.sha256(
            f"{signing_key}:{payload}".encode()
        ).hexdigest()

    def verify(self, signing_key: str) -> bool:
        """Verify the signature matches the current payload."""
        payload = self._serialize_payload()
        current_hash = hashlib.sha256(payload.encode()).hexdigest()
        if current_hash != self.signed_payload_hash:
            return False
        expected_sig = hashlib.sha256(
            f"{signing_key}:{payload}".encode()
        ).hexdigest()
        return self.signature == expected_sig

    def _serialize_payload(self) -> str:
        """Serialize the governance-relevant fields for signing."""
        return json.dumps({
            "delegator_id": self.delegator_id,
            "delegate_id": self.delegate_id,
            "dimension_scope": sorted(self.dimension_scope),
            "max_depth": self.max_depth,
            "revoked": self.revoked,
        }, sort_keys=True)


def accept_delegation(record: DelegationRecord, signing_key: str) -> bool:
    """A downstream consumer that accepts a delegation if it verifies."""
    if not record.verify(signing_key):
        return False
    if record.revoked:
        return False
    return True
```

**`test_delegation.py` (existing, all passing):**

```python
from delegation import DelegationRecord, accept_delegation

KEY = "test-signing-key-001"


def test_valid_delegation_accepted():
    record = DelegationRecord(
        delegator_id="org-root",
        delegate_id="team-lead-alpha",
        dimension_scope={"financial", "operational"},
        max_depth=2,
    )
    record.sign(KEY)
    assert accept_delegation(record, KEY) is True


def test_revoked_delegation_rejected():
    record = DelegationRecord(
        delegator_id="org-root",
        delegate_id="team-lead-alpha",
        dimension_scope={"financial", "operational"},
        max_depth=2,
    )
    record.sign(KEY)
    record.revoked = True
    record.sign(KEY)  # Re-sign after revocation
    assert accept_delegation(record, KEY) is False


def test_tampered_signature_rejected():
    record = DelegationRecord(
        delegator_id="org-root",
        delegate_id="team-lead-alpha",
        dimension_scope={"financial", "operational"},
        max_depth=2,
    )
    record.sign(KEY)
    record.signature = "forged-signature"
    assert accept_delegation(record, KEY) is False
```

## Task

1. Identify which of the 9 fields on `DelegationRecord` are **security-sensitive** — meaning that unrestricted mutation after signing could bypass a governance invariant. For each field, state whether it should be **private (read-only accessor)**, **private (approved-mutation method)**, or **public (safe to leave open)**. Justify each classification in one sentence.

2. Design the accessor API for the secured struct. Specify: (a) which fields get read-only properties, (b) what the single approved-construction path looks like (constructor, builder, or factory), and (c) how revocation works if `revoked` is no longer a publicly settable field.

3. Write a **bypass test** that demonstrates the security failure with the current public-field design. The test must: clone or copy a signed record, mutate a security-sensitive field without re-signing, and show that the mutated record is accepted by `accept_delegation`. This test must PASS against the current code (proving the vulnerability exists).

4. Write a **fix test** that demonstrates the secured design blocks the bypass. This test must FAIL against the current code and PASS against the fixed code.

## Model Answer

**1. Field classification:**

| Field                 | Classification                                 | Justification                                                                                                                                                   |
| --------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `delegator_id`        | Private (read-only)                            | Changing the delegator after signing would reassign who granted the authority — a privilege escalation if accepted without re-verification.                     |
| `delegate_id`         | Private (read-only)                            | Changing the delegate after signing would transfer authority to an unintended party.                                                                            |
| `dimension_scope`     | Private (read-only)                            | Widening the scope set after signing grants the delegate authority over dimensions the delegator never authorized — a direct violation of monotonic tightening. |
| `max_depth`           | Private (read-only)                            | Increasing depth after signing allows deeper re-delegation than authorized.                                                                                     |
| `revoked`             | Private (approved-mutation: `revoke()` method) | Must be settable (revocation is a legitimate operation) but only through a method that re-signs the record, ensuring the revocation is cryptographically bound. |
| `signature`           | Private (read-only)                            | Direct mutation of the signature field lets an attacker replace the signature with one computed from a tampered payload.                                        |
| `signed_payload_hash` | Private (read-only)                            | Direct mutation of the hash lets an attacker make a tampered payload appear to match the original signing.                                                      |
| `created_at`          | Public (safe)                                  | Metadata field not included in the signed payload. Mutation does not affect governance invariants.                                                              |
| `notes`               | Public (safe)                                  | Metadata field not included in the signed payload. Mutation does not affect governance invariants.                                                              |

**Security-sensitive fields (7 of 9):** `delegator_id`, `delegate_id`, `dimension_scope`, `max_depth`, `revoked`, `signature`, `signed_payload_hash`. Only `created_at` and `notes` are safe to leave public.

**2. Accessor API design:**

**(a) Read-only properties** for all 7 security-sensitive fields:

```python
@property
def delegator_id(self) -> str:
    return self._delegator_id

@property
def dimension_scope(self) -> frozenset[str]:
    return self._dimension_scope  # frozenset, not set — prevents in-place mutation
```

Using `frozenset` for `dimension_scope` is critical. A read-only property returning a mutable `set` would allow `record.dimension_scope.add("data_access")` to bypass the encapsulation without touching the private field.

**(b) Single approved-construction path** — constructor plus builder:

```python
class DelegationRecord:
    def __init__(
        self,
        delegator_id: str,
        delegate_id: str,
        dimension_scope: set[str],
        max_depth: int,
        *,
        created_at: str = "",
        notes: str = "",
    ):
        self._delegator_id = delegator_id
        self._delegate_id = delegate_id
        self._dimension_scope = frozenset(dimension_scope)
        self._max_depth = max_depth
        self._revoked = False
        self._signature = ""
        self._signed_payload_hash = ""
        self.created_at = created_at
        self.notes = notes
```

There is no setter for any security-sensitive field. Once constructed, the only mutations are `sign()` and `revoke()`.

**(c) Revocation via approved method:**

```python
def revoke(self, signing_key: str) -> None:
    """Revoke this delegation and re-sign to bind the revocation cryptographically."""
    self._revoked = True
    self.sign(signing_key)
```

Revocation MUST re-sign because `revoked` is part of the signed payload. Without re-signing, a consumer verifying the signature would see a mismatch and reject the record — but for the wrong reason (signature failure, not revocation). The `revoke()` method ensures the revocation is intentional, cryptographically bound, and verifiable.

**3. Bypass test (passes against current code, proving the vulnerability):**

```python
import copy
from delegation import DelegationRecord, accept_delegation

KEY = "test-signing-key-001"


def test_bypass_scope_widening_without_resign():
    """VULNERABILITY: clone a signed record, widen scope, accepted without re-signing."""
    # Create a legitimately signed record with narrow scope
    original = DelegationRecord(
        delegator_id="org-root",
        delegate_id="team-lead-alpha",
        dimension_scope={"financial"},
        max_depth=1,
    )
    original.sign(KEY)
    assert accept_delegation(original, KEY) is True  # legitimate record accepted

    # Clone and widen scope — adding dimensions the delegator never authorized
    tampered = copy.deepcopy(original)
    tampered.dimension_scope = {"financial", "operational", "data_access", "temporal"}
    tampered.revoked = False

    # Re-sign with the SAME key (attacker has access to signing key or
    # the struct allows self-signing without authority verification)
    tampered.sign(KEY)

    # The widened record is accepted — monotonic tightening violated
    assert accept_delegation(tampered, KEY) is True
    assert tampered.dimension_scope == {"financial", "operational", "data_access", "temporal"}
    assert original.dimension_scope == {"financial"}
```

This test passes against the current code because: (a) all fields are public, so `dimension_scope` can be set to any value; (b) `sign()` is a public method that re-signs whatever the current field state is; and (c) `accept_delegation` only checks that the signature matches the current state — it does not check whether the scope was widened relative to the original delegation.

Note: this bypass is even worse than it appears. In a chain of delegations (root -> lead -> member), if the lead can widen their own scope and re-sign, they can grant themselves authority the root never delegated. The encapsulation fix prevents the field mutation, but a full chain-of-trust implementation would also need to verify that each record's scope is a subset of its parent's scope.

**4. Fix test (fails against current code, passes against fixed code):**

```python
def test_fix_scope_widening_blocked():
    """FIXED: security-sensitive fields cannot be mutated after construction."""
    record = DelegationRecord(
        delegator_id="org-root",
        delegate_id="team-lead-alpha",
        dimension_scope={"financial"},
        max_depth=1,
    )
    record.sign(KEY)

    # Attempting to set dimension_scope raises AttributeError (no setter)
    try:
        record.dimension_scope = {"financial", "operational", "data_access"}
        assert False, "Should have raised AttributeError — field should be read-only"
    except AttributeError:
        pass  # Expected: property has no setter

    # Attempting to mutate via the frozenset fails
    try:
        record.dimension_scope.add("operational")
        assert False, "Should have raised AttributeError — frozenset is immutable"
    except AttributeError:
        pass  # Expected: frozenset has no add method

    # Attempting to set revoked directly raises AttributeError
    try:
        record.revoked = False
        assert False, "Should have raised AttributeError — field should be read-only"
    except AttributeError:
        pass  # Expected: property has no setter

    # Revocation only via approved method
    record.revoke(KEY)
    assert record.revoked is True
    assert accept_delegation(record, KEY) is False  # revoked records rejected
```

This test fails against the current code because `record.dimension_scope = ...` succeeds (public field assignment on a dataclass). It passes against the fixed code because the property has no setter, raising `AttributeError`.

## Scoring

| Criterion                                                                                                               | Points |
| ----------------------------------------------------------------------------------------------------------------------- | ------ |
| All 5 governance-sensitive fields identified (`delegator_id`, `delegate_id`, `dimension_scope`, `max_depth`, `revoked`) | 2      |
| Both cryptographic fields (`signature`, `signed_payload_hash`) identified as private                                    | 1      |
| `created_at` and `notes` correctly classified as safe with payload-exclusion rationale                                  | 1      |
| Approved-mutation path is constructor or builder, not a setter (no `set_scope()` method)                                | 1      |
| `dimension_scope` uses `frozenset` or equivalent to prevent in-place mutation through the accessor                      | 1      |
| Bypass test is executable (not a hypothetical description) and demonstrates actual scope widening                       | 2      |
| Fix test fails against current code and passes against fixed code (both directions verified)                            | 1      |
| Revocation design re-signs the record (cryptographically binds the revocation)                                          | 1      |
| **Total**                                                                                                               | **10** |

## Extensions

- **Variant A (serialization backward compatibility):** Add a new field `communication_scope: frozenset[str] | None` to the secured struct. The practitioner must handle backward compatibility: existing signed records serialized before this field existed must still verify correctly. The fix uses conditional serialization (`if self._communication_scope is not None`) so that the payload hash for old records remains unchanged. This tests whether the practitioner understands that adding a field to a signed struct is itself a security-sensitive operation — the new field must default to the least-privileged interpretation (`None` meaning "no communication authority") and must not invalidate existing signatures.
- **Variant B (Rust ownership model):** Rewrite the same struct in Rust, where the language's ownership system provides stronger encapsulation guarantees. Fields marked `pub(crate)` are inaccessible to external crates. The practitioner must decide which fields are `pub(crate)` (readable within the crate but not settable by external consumers) versus truly private. The test uses a separate crate to verify that external code cannot mutate security-sensitive fields. This tests whether the practitioner can translate the Python encapsulation pattern to a language where the compiler enforces the boundary rather than relying on runtime `AttributeError`.
