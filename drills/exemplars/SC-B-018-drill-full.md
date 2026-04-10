---
drill_id: DR-SC-B-018-FULL
source_atom: SC-B-018
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Policy Decision Point vs Policy Enforcement Point Separation

## Setup

The practitioner receives a governance module from an internal platform. The module has a single function that handles all authorization:

```python
# governance/authorize.py

from datetime import datetime, timezone
from typing import Any

class GovernanceError(Exception):
    pass

def authorize_and_execute(action: str, role: dict, context: dict) -> Any:
    """Check governance constraints and execute the action if permitted."""

    # Check 1: Role vacancy
    if role.get("status") == "vacant":
        raise GovernanceError(f"Role {role['name']} is vacant — cannot execute")

    # Check 2: Envelope — financial limit
    if action == "approve_expense":
        amount = context.get("amount", 0)
        limit = role.get("envelope", {}).get("financial_limit", 0)
        if amount > limit:
            raise GovernanceError(
                f"Amount ${amount} exceeds envelope limit ${limit}"
            )

    # Check 3: Envelope — temporal window
    now = datetime.now(timezone.utc)
    window_start = role.get("envelope", {}).get("window_start")
    window_end = role.get("envelope", {}).get("window_end")
    if window_start and window_end:
        if not (window_start <= now <= window_end):
            raise GovernanceError(
                f"Action outside temporal window: {window_start} to {window_end}"
            )

    # Check 4: Blocked actions list
    blocked = role.get("envelope", {}).get("blocked_actions", [])
    if action in blocked:
        raise GovernanceError(f"Action '{action}' is blocked for role {role['name']}")

    # Check 5: Compartment access
    compartment = context.get("compartment")
    allowed_compartments = role.get("envelope", {}).get("compartments", [])
    if compartment and compartment not in allowed_compartments:
        raise GovernanceError(
            f"Role {role['name']} has no access to compartment '{compartment}'"
        )

    # All checks passed — execute
    return _execute_action(action, context)


def _execute_action(action: str, context: dict) -> dict:
    """Execute the governed action. Side effects happen here."""
    # ... actual business logic (database writes, API calls, notifications)
    return {"status": "executed", "action": action, "timestamp": datetime.now(timezone.utc)}
```

The module is used in three call sites across the platform:

```python
# Call site 1: API endpoint (main path)
@app.post("/actions")
def handle_action(request):
    role = get_role(request.user_id)
    return authorize_and_execute(request.action, role, request.context)

# Call site 2: Batch processor (background job)
def process_batch(actions: list[dict]):
    for item in actions:
        role = get_role(item["user_id"])
        authorize_and_execute(item["action"], role, item["context"])

# Call site 3: Admin override (emergency path)
@app.post("/admin/force-execute")
def admin_force_execute(request):
    # "We trust admins — skip governance"
    return _execute_action(request.action, request.context)
```

## Task

1. **Identify the PDP/PEP boundary** within `authorize_and_execute`. Draw the line: which lines are the policy decision (evaluating constraints and producing a judgment) and which lines are the policy enforcement (acting on the judgment)?

2. **Refactor into two modules.** Produce:
   - A `PolicyDecider` class with a method `evaluate(action, role, context) -> Verdict` that returns a data structure (not a raised exception).
   - An `Enforcer` class with a method `enforce(verdict, action, context) -> Any` that consumes the verdict and either executes or blocks.
   - A `Verdict` data class with four possible outcomes: `auto_approved`, `flagged`, `held`, `blocked`.

3. **Write a PDP test** that verifies a blocked verdict is returned for an out-of-envelope action — without triggering any execution side effects. The test must prove that the PDP can be tested in complete isolation from the PEP.

4. **Audit the call sites.** Identify which call site bypasses the PEP entirely. Propose a fix that makes bypass structurally impossible (not just "add a check").

## Model Answer

**1. PDP/PEP boundary in the merged function:**

```
authorize_and_execute()
│
├── Lines 1-25: POLICY DECISION POINT (PDP)
│   ├── Check 1: vacancy status → should produce BLOCKED verdict
│   ├── Check 2: financial envelope → should produce BLOCKED verdict
│   ├── Check 3: temporal window → should produce BLOCKED verdict
│   ├── Check 4: blocked actions list → should produce BLOCKED verdict
│   └── Check 5: compartment access → should produce BLOCKED verdict
│
└── Line 27: POLICY ENFORCEMENT POINT (PEP)
    └── _execute_action() → the actual side effect
```

The merge pathology: the PDP communicates its decision via raised exceptions (GovernanceError) rather than a returned data structure. This means the only way to observe the PDP's decision is to catch the exception — there is no inspectable verdict object. The PDP and PEP are coupled by control flow (exception vs fall-through), not by a data contract (verdict object).

**2. Refactored modules:**

```python
# governance/verdict.py

from dataclasses import dataclass
from enum import Enum
from typing import Optional

class VerdictType(Enum):
    AUTO_APPROVED = "auto_approved"
    FLAGGED = "flagged"
    HELD = "held"
    BLOCKED = "blocked"

@dataclass(frozen=True)
class Verdict:
    type: VerdictType
    action: str
    role_name: str
    reason: Optional[str] = None
    checks_passed: tuple[str, ...] = ()
    check_failed: Optional[str] = None
```

```python
# governance/policy_decider.py

from datetime import datetime, timezone
from governance.verdict import Verdict, VerdictType

class PolicyDecider:
    """Pure policy evaluation. No side effects. Returns a Verdict, never raises."""

    def evaluate(self, action: str, role: dict, context: dict) -> Verdict:
        role_name = role.get("name", "unknown")
        passed = []

        # Check 1: Role vacancy
        if role.get("status") == "vacant":
            return Verdict(
                type=VerdictType.BLOCKED,
                action=action,
                role_name=role_name,
                reason=f"Role '{role_name}' is vacant",
                checks_passed=tuple(passed),
                check_failed="vacancy",
            )
        passed.append("vacancy")

        # Check 2: Financial envelope
        if action == "approve_expense":
            amount = context.get("amount", 0)
            limit = role.get("envelope", {}).get("financial_limit", 0)
            if amount > limit:
                return Verdict(
                    type=VerdictType.BLOCKED,
                    action=action,
                    role_name=role_name,
                    reason=f"Amount ${amount} exceeds envelope limit ${limit}",
                    checks_passed=tuple(passed),
                    check_failed="financial_envelope",
                )
        passed.append("financial_envelope")

        # Check 3: Temporal window
        now = datetime.now(timezone.utc)
        window_start = role.get("envelope", {}).get("window_start")
        window_end = role.get("envelope", {}).get("window_end")
        if window_start and window_end:
            if not (window_start <= now <= window_end):
                return Verdict(
                    type=VerdictType.BLOCKED,
                    action=action,
                    role_name=role_name,
                    reason=f"Outside temporal window: {window_start} to {window_end}",
                    checks_passed=tuple(passed),
                    check_failed="temporal_window",
                )
        passed.append("temporal_window")

        # Check 4: Blocked actions
        blocked = role.get("envelope", {}).get("blocked_actions", [])
        if action in blocked:
            return Verdict(
                type=VerdictType.BLOCKED,
                action=action,
                role_name=role_name,
                reason=f"Action '{action}' is on the blocked list",
                checks_passed=tuple(passed),
                check_failed="blocked_actions",
            )
        passed.append("blocked_actions")

        # Check 5: Compartment access
        compartment = context.get("compartment")
        allowed_compartments = role.get("envelope", {}).get("compartments", [])
        if compartment and compartment not in allowed_compartments:
            return Verdict(
                type=VerdictType.BLOCKED,
                action=action,
                role_name=role_name,
                reason=f"No access to compartment '{compartment}'",
                checks_passed=tuple(passed),
                check_failed="compartment_access",
            )
        passed.append("compartment_access")

        # All checks passed
        return Verdict(
            type=VerdictType.AUTO_APPROVED,
            action=action,
            role_name=role_name,
            checks_passed=tuple(passed),
        )
```

```python
# governance/enforcer.py

from typing import Any
from governance.verdict import Verdict, VerdictType

class Enforcer:
    """Consumes a Verdict and either executes, holds, or blocks. The ONLY
    call site for _execute_action in the entire codebase."""

    def __init__(self, executor):
        self._executor = executor

    def enforce(self, verdict: Verdict, action: str, context: dict) -> dict[str, Any]:
        match verdict.type:
            case VerdictType.AUTO_APPROVED:
                result = self._executor(action, context)
                return {"verdict": "auto_approved", "result": result}
            case VerdictType.FLAGGED:
                result = self._executor(action, context)
                self._log_for_review(verdict)
                return {"verdict": "flagged", "result": result, "review_pending": True}
            case VerdictType.HELD:
                return {
                    "verdict": "held",
                    "result": None,
                    "awaiting_approval": True,
                    "reason": verdict.reason,
                }
            case VerdictType.BLOCKED:
                return {
                    "verdict": "blocked",
                    "result": None,
                    "reason": verdict.reason,
                    "check_failed": verdict.check_failed,
                }

    def _log_for_review(self, verdict: Verdict) -> None:
        # Structured audit log for async human review
        pass
```

**3. PDP isolation test:**

```python
# tests/test_policy_decider.py

from governance.policy_decider import PolicyDecider
from governance.verdict import VerdictType

def test_blocked_verdict_for_over_envelope_expense():
    """PDP returns BLOCKED for an amount exceeding the financial envelope.
    No executor is instantiated — proving the PDP has zero side effects."""
    decider = PolicyDecider()

    role = {
        "name": "budget_holder",
        "status": "active",
        "envelope": {
            "financial_limit": 5000,
            "compartments": ["operations"],
        },
    }
    context = {"amount": 7500, "compartment": "operations"}

    verdict = decider.evaluate("approve_expense", role, context)

    assert verdict.type == VerdictType.BLOCKED
    assert verdict.check_failed == "financial_envelope"
    assert "7500" in verdict.reason
    assert "5000" in verdict.reason
    assert verdict.checks_passed == ("vacancy",)
    # No executor was created, called, or imported — the PDP is fully isolated


def test_auto_approved_verdict_within_envelope():
    """PDP returns AUTO_APPROVED when all checks pass."""
    decider = PolicyDecider()

    role = {
        "name": "budget_holder",
        "status": "active",
        "envelope": {
            "financial_limit": 10000,
            "compartments": ["operations"],
        },
    }
    context = {"amount": 3000, "compartment": "operations"}

    verdict = decider.evaluate("approve_expense", role, context)

    assert verdict.type == VerdictType.AUTO_APPROVED
    assert verdict.checks_passed == (
        "vacancy", "financial_envelope", "temporal_window",
        "blocked_actions", "compartment_access",
    )
    assert verdict.check_failed is None
```

The test proves PDP isolation: `PolicyDecider` is instantiated without any reference to an executor, database, or side-effecting system. The verdict is a frozen dataclass that can be inspected purely by value. If the test imports or instantiates an executor, the isolation property is violated.

**4. Call site audit:**

| Call Site                               | Uses PEP?                                 | Status                                              |
| --------------------------------------- | ----------------------------------------- | --------------------------------------------------- |
| API endpoint (`/actions`)               | Yes (through `authorize_and_execute`)     | Governed — but should use the refactored `Enforcer` |
| Batch processor                         | Yes (through `authorize_and_execute`)     | Governed — same refactor needed                     |
| Admin override (`/admin/force-execute`) | **No** — calls `_execute_action` directly | **BYPASS — ungoverned execution path**              |

The admin override is the textbook PEP bypass. It calls `_execute_action` directly, skipping all five governance checks. The comment "We trust admins" encodes a policy decision in prose rather than in the PDP.

**Structural fix — make bypass impossible:**

Make `_execute_action` private to the `Enforcer` — it is injected via the constructor and cannot be imported or called from anywhere else. The admin endpoint must go through the Enforcer like every other call site:

```python
# Refactored admin endpoint
@app.post("/admin/force-execute")
def admin_force_execute(request):
    role = get_admin_role(request.user_id)  # Admin role with wide envelope
    verdict = decider.evaluate(request.action, role, request.context)
    return enforcer.enforce(verdict, request.action, request.context)
```

The admin's wide authority is expressed through the admin role's envelope (high financial limits, all compartments, no blocked actions) — not through bypassing the governance system. If the admin truly needs to override a block, the PDP should have a `VerdictType.HELD` path where the admin provides explicit justification that is recorded in the audit trail, not a silent bypass.

The structural guarantee: `_execute_action` is passed to `Enforcer.__init__` and is not importable from any other module. A `grep` for `_execute_action` should return exactly one hit (the Enforcer constructor). Any additional hit is a bypass. This matches the atom's guidance: "grep for every call site of the governed operation and verify that each one passes through a PEP."

## Scoring

| Criterion                                                                             | Points |
| ------------------------------------------------------------------------------------- | ------ |
| PDP/PEP boundary correctly identified (checks 1-5 = PDP, `_execute_action` = PEP)     | 1      |
| PDP returns a data structure (Verdict), not a raised exception                        | 2      |
| Verdict type has all four outcomes (auto-approved, flagged, held, blocked)            | 1      |
| PEP handles all four verdict types with distinct behaviour                            | 1      |
| PDP test demonstrates zero side effects (no executor instantiated or imported)        | 2      |
| Admin bypass identified as ungoverned call site                                       | 1      |
| Fix is structural (executor only accessible through Enforcer), not just "add a check" | 2      |
| **Total**                                                                             | **10** |

## Extensions

- **Variant A (shadow enforcement):** The team wants to deploy the refactored governance module without breaking existing behaviour. Design a `ShadowEnforcer` that evaluates every action through the PDP but always executes regardless of the verdict, logging the would-be block for analysis. Write the `ShadowEnforcer.enforce()` method. Explain when to switch from shadow to strict and what evidence would justify the switch.
- **Variant B (per-node enforcement):** The platform runs multi-step workflows (step 1: validate, step 2: transform, step 3: write). The current PDP evaluates once at step 1. Design a per-node PEP that re-evaluates the verdict at each step boundary, handling the case where an envelope constraint changes mid-workflow (e.g., temporal window closes between step 2 and step 3).
- **Variant C (audit trail completeness):** The `Verdict` dataclass records which check failed but not the input values that caused the failure. Extend it with a `check_inputs` field that captures the relevant values (amount, limit, window boundaries) without logging sensitive data (role credentials, compartment contents). Define the boundary between auditable context and sensitive context.
