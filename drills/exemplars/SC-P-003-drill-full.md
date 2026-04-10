---
drill_id: DR-SC-P-003-FULL
source_atom: SC-P-003
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Write Cross-Feature Interaction Tests Before Individual Feature Tests

## Setup

The practitioner receives a simplified governance engine module (`governance.py`, 180 lines) that implements two features sharing the same role graph.

**Feature A — Vacancy handling** (lines 1–90):

```python
class GovernanceEngine:
    def __init__(self):
        self.roles = {}          # role_id -> Role(name, is_vacant, clearance_level)
        self.bridges = {}        # bridge_id -> Bridge(role_a, role_b, status)

    def create_role(self, role_id, name, clearance_level=1):
        self.roles[role_id] = Role(name=name, is_vacant=False, clearance_level=clearance_level)

    def vacate_role(self, role_id):
        self.roles[role_id].is_vacant = True

    def fill_role(self, role_id):
        self.roles[role_id].is_vacant = False

    def execute_action(self, role_id, action):
        role = self.roles[role_id]
        if role.is_vacant:
            raise VacantRoleError(f"Role {role_id} is vacant and cannot execute")
        return Action(role_id=role_id, action=action, executed=True)

    def check_access(self, role_id, resource):
        role = self.roles[role_id]
        if role.is_vacant:
            raise VacantRoleError(f"Role {role_id} is vacant")
        return role.clearance_level >= resource.required_clearance
```

**Feature B — Bridge approval** (lines 91–180):

```python
    def propose_bridge(self, bridge_id, role_a_id, role_b_id):
        self.bridges[bridge_id] = Bridge(
            role_a=role_a_id, role_b=role_b_id, status="proposed"
        )

    def approve_bridge(self, bridge_id, approver_role_id):
        bridge = self.bridges[bridge_id]
        if approver_role_id not in (bridge.role_a, bridge.role_b):
            raise UnauthorizedError("Approver must be a bridge participant")
        bridge.approvals.add(approver_role_id)
        if len(bridge.approvals) == 2:
            bridge.status = "active"
        return bridge

    def reject_bridge(self, bridge_id, rejector_role_id):
        bridge = self.bridges[bridge_id]
        if rejector_role_id not in (bridge.role_a, bridge.role_b):
            raise UnauthorizedError("Rejector must be a bridge participant")
        bridge.status = "rejected"
        return bridge

    def dissolve_bridge(self, bridge_id, dissolver_role_id):
        bridge = self.bridges[bridge_id]
        if dissolver_role_id not in (bridge.role_a, bridge.role_b):
            raise UnauthorizedError("Dissolver must be a bridge participant")
        bridge.status = "dissolved"
        return bridge
```

**Spec reference** (PACT (spec) §51 — Vacant Roles):

> "A vacant role satisfies the grammatical constraint but cannot execute. Any action taken by a vacant role — including approving, rejecting, or dissolving a bridge — MUST be blocked."

**Existing test suite** (24 tests, all passing):

- `test_vacancy_*.py` — 12 tests covering vacate, fill, execute_action, check_access. All test the vacancy path with dedicated fixtures that never create bridges.
- `test_bridge_*.py` — 12 tests covering propose, approve, reject, dissolve. All test the bridge path with dedicated fixtures where all roles are filled (never vacant).

## Task

1. Read the code for both features. List every piece of shared mutable state between Feature A and Feature B (name the specific data structure, not just "they share state").
2. State the safety invariant for each feature in one sentence. The invariant must be falsifiable — it must be possible to write a test that fails if the invariant is violated.
3. Write one cross-feature interaction test: construct a scenario where Feature A's output (a vacated role) becomes Feature B's input (an approver for a bridge). The test must assert the combined safety property — that the vacancy invariant holds across the bridge approval path.
4. Predict whether the test will pass or fail against the code above. If it fails, identify the exact method(s) missing the vacancy guard and write the fix (the added lines of code).

## Model Answer

**Step 1 — Shared mutable state:**

The single shared data structure is `self.roles` (the role graph dictionary). Both features read from and depend on the same `Role` objects:

- Feature A writes `is_vacant` on `Role` objects via `vacate_role()` and `fill_role()`
- Feature B reads `Role` identity via `approver_role_id` lookups but never checks `is_vacant`

Secondary shared structure: `self.bridges` is written by Feature B and could be read by Feature A in a more complete implementation, but in the current code Feature A does not reference bridges.

**Step 2 — Safety invariants:**

- **Feature A invariant:** "A role with `is_vacant=True` must raise `VacantRoleError` on any execution attempt." (Falsifiable: call `execute_action()` with a vacant role; if no error is raised, the invariant is violated.)
- **Feature B invariant:** "Bridge approval requires bilateral consent from authorized, non-vacant roles." (Falsifiable: approve a bridge with a vacant role; if no error is raised, the invariant is violated.)

**Step 3 — Cross-feature interaction test:**

```python
def test_vacant_role_cannot_approve_bridge():
    engine = GovernanceEngine()
    engine.create_role("cfo", "Chief Financial Officer")
    engine.create_role("cto", "Chief Technology Officer")

    # Propose a bridge between CFO and CTO
    engine.propose_bridge("finance-tech", "cfo", "cto")

    # CTO approves (role is filled — should succeed)
    engine.approve_bridge("finance-tech", "cto")

    # Vacate the CFO role
    engine.vacate_role("cfo")

    # Vacant CFO attempts to approve — MUST be blocked per PACT §51
    with pytest.raises(VacantRoleError):
        engine.approve_bridge("finance-tech", "cfo")
```

Two additional interaction tests at the same seam:

```python
def test_vacant_role_cannot_reject_bridge():
    engine = GovernanceEngine()
    engine.create_role("cfo", "CFO")
    engine.create_role("cto", "CTO")
    engine.propose_bridge("b1", "cfo", "cto")
    engine.vacate_role("cfo")

    with pytest.raises(VacantRoleError):
        engine.reject_bridge("b1", "cfo")

def test_vacant_role_cannot_dissolve_bridge():
    engine = GovernanceEngine()
    engine.create_role("cfo", "CFO")
    engine.create_role("cto", "CTO")
    engine.propose_bridge("b1", "cfo", "cto")
    engine.approve_bridge("b1", "cfo")
    engine.approve_bridge("b1", "cto")
    engine.vacate_role("cfo")

    with pytest.raises(VacantRoleError):
        engine.dissolve_bridge("b1", "cfo")
```

**Step 4 — Prediction and fix:**

The test will **fail**. `approve_bridge()`, `reject_bridge()`, and `dissolve_bridge()` check whether the caller is a bridge participant but never check whether the caller's role is vacant. A vacant role passes the authorization check and successfully approves, rejects, or dissolves the bridge.

The missing guard is a vacancy check at the top of each bridge-action method. The fix adds 3 lines (one per method):

```python
def approve_bridge(self, bridge_id, approver_role_id):
    if self.roles[approver_role_id].is_vacant:                    # ADDED
        raise VacantRoleError(f"Role {approver_role_id} is vacant")  # ADDED
    bridge = self.bridges[bridge_id]
    # ... rest unchanged

def reject_bridge(self, bridge_id, rejector_role_id):
    if self.roles[rejector_role_id].is_vacant:                    # ADDED
        raise VacantRoleError(f"Role {rejector_role_id} is vacant")  # ADDED
    bridge = self.bridges[bridge_id]
    # ... rest unchanged

def dissolve_bridge(self, bridge_id, dissolver_role_id):
    if self.roles[dissolver_role_id].is_vacant:                    # ADDED
        raise VacantRoleError(f"Role {dissolver_role_id} is vacant")  # ADDED
    bridge = self.bridges[bridge_id]
    # ... rest unchanged
```

## Scoring

| Criterion                                                                                | Points |
| ---------------------------------------------------------------------------------------- | ------ |
| Shared state identified as `self.roles` (the role graph), not just "they share state"    | 1      |
| Feature A invariant is falsifiable (names VacantRoleError and execution attempt)         | 1      |
| Feature B invariant references vacancy, not just authorization                           | 1      |
| Interaction test constructs the crossing scenario (vacate then approve)                  | 2      |
| Test targets the seam (approve_bridge with a vacant role), not each feature in isolation | 1      |
| Correct prediction that the test fails                                                   | 1      |
| Missing guard identified in all 3 methods (approve, reject, dissolve), not just approve  | 1      |
| Fix is the correct code (vacancy check before authorization check)                       | 1      |
| Interaction test written BEFORE being prompted (task step 3, not after step 4's hint)    | 1      |
| **Total**                                                                                | **10** |

## Extensions

1. **Three-feature interaction.** Add a third feature: `delegate_authority(from_role, to_role, scope)` that transfers execution rights. The new crossing scenario: a vacant role delegates authority, the delegatee approves a bridge using delegated authority from a vacant role. The practitioner must determine whether the delegation itself should be blocked (yes — vacant roles cannot execute) AND whether existing delegations from a now-vacant role should be revoked or honoured.

2. **Temporal ordering.** Modify the scenario so the role is vacated AFTER the bridge is proposed but BEFORE the second approval. The practitioner must determine whether a partially-approved bridge should be rolled back when one participant becomes vacant, or whether the existing approval stands. The spec does not address this directly — the practitioner must name the gap.

3. **Fixture contamination audit.** Give the practitioner the full 24-test suite with fixtures. The practitioner must audit every fixture and identify which ones create scenarios that COULD test cross-feature interactions but deliberately avoid them (e.g., bridge test fixtures that call `create_role()` but never call `vacate_role()`). Count the fixture-level isolation points that mask the gap.
