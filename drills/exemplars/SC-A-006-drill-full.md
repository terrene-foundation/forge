---
drill_id: DR-SC-A-006-FULL
source_atom: SC-A-006
difficulty: intermediate
time_box_minutes: 30
modality: drill
application: [COC]
---

# Negative Tests for Deny-by-Default

## Setup

The practitioner receives a minimal RBAC (Role-Based Access Control) middleware implementation in Python. The middleware protects API routes by checking whether the authenticated user's role has permission to access the requested route.

**Implementation (22 lines):**

```python
class RBACMiddleware:
    def __init__(self, app, route_permissions: dict, deny_by_default: bool = True):
        self.app = app
        self.route_permissions = route_permissions
        self.deny_by_default = deny_by_default

    async def __call__(self, request):
        route = request.path
        user_role = request.auth.role

        if route in self.route_permissions:
            allowed_roles = self.route_permissions[route]
            if user_role in allowed_roles:
                return await self.app(request)
            else:
                log.warning(f"Denied: role={user_role} route={route}")
                return Response(status=403, body="Forbidden")
        else:
            # Route not in permission map
            if self.deny_by_default:
                return await self.app(request)  # BUG: should deny
            else:
                return await self.app(request)
```

**Route permission map provided:**

```python
route_permissions = {
    "/api/users": ["admin", "manager"],
    "/api/reports": ["admin", "analyst"],
    "/api/public": ["admin", "manager", "analyst", "viewer"],
}
```

The middleware has the **kailash-rs 0002 bug**: both branches of the `deny_by_default` flag execute identical code — they both pass the request through to the inner app. The `deny_by_default = True` branch should deny the request, but it does not. Any route not listed in `route_permissions` (e.g., `/api/admin/settings`) is accessible to any authenticated user regardless of role.

The practitioner also has access to a test file with 4 existing positive tests:

```python
def test_admin_can_access_users():
    # Passes -- admin is in allowed_roles for /api/users
    ...

def test_analyst_can_access_reports():
    # Passes -- analyst is in allowed_roles for /api/reports
    ...

def test_viewer_cannot_access_users():
    # Passes -- viewer is NOT in allowed_roles for /api/users, returns 403
    ...

def test_viewer_can_access_public():
    # Passes -- viewer is in allowed_roles for /api/public
    ...
```

All 4 existing tests pass. The bug is invisible to positive testing because every test exercises a mapped route.

## Task

1. **Before reading the implementation**, write a negative test that exercises an unmapped route (`/api/admin/settings`) with `deny_by_default = True` and an authenticated user with role `viewer`. The test should assert: (a) the response status is 403, and (b) the denial is logged with the route and role.

2. Run the test mentally against the provided implementation. Predict whether it passes or fails, and explain why.

3. Fix the implementation. The fix must change only the `deny_by_default = True` branch — do not modify the `deny_by_default = False` branch or restructure the entire method.

4. Write a second negative test for a different deny scenario: an **unknown role** (e.g., `"intern"`) accessing a mapped route (`/api/users`) where `"intern"` is not in the allowed roles list. Predict whether this test passes or fails against the original implementation.

5. Explain why the 4 existing positive tests could not have caught this bug, and state the general principle about negative testing for deny-by-default enforcement.

## Model Answer

**1. Negative test (written before reading the implementation):**

```python
def test_unmapped_route_denied_when_deny_by_default():
    """An unmapped route must return 403 when deny_by_default is True."""
    middleware = RBACMiddleware(
        app=mock_app,
        route_permissions={
            "/api/users": ["admin", "manager"],
            "/api/reports": ["admin", "analyst"],
            "/api/public": ["admin", "manager", "analyst", "viewer"],
        },
        deny_by_default=True,
    )
    request = make_request(path="/api/admin/settings", role="viewer")
    response = await middleware(request)

    assert response.status == 403, (
        f"Expected 403 for unmapped route with deny_by_default=True, got {response.status}"
    )
    assert_log_contains(
        level="warning",
        message_contains="Denied",
        route="/api/admin/settings",
        role="viewer",
    )
```

**2. Prediction:** The test **fails**. The implementation's `deny_by_default = True` branch (line 20) executes `return await self.app(request)` — it passes the request through instead of denying it. The response status will be 200 (or whatever the inner app returns), not 403. No denial log entry is written because the denial code path is never reached.

**3. Fix:**

```python
        else:
            # Route not in permission map
            if self.deny_by_default:
                log.warning(f"Denied: role={user_role} route={route} (unmapped, deny_by_default)")
                return Response(status=403, body="Forbidden")
            else:
                return await self.app(request)
```

The fix changes only the `deny_by_default = True` branch to return a 403 response with a log entry, mirroring the denial pattern used for mapped routes with unauthorized roles. The `deny_by_default = False` branch remains unchanged — it correctly passes unmapped routes through.

**4. Second negative test:**

```python
def test_unknown_role_denied_on_mapped_route():
    """A role not in the allowed list for a mapped route must return 403."""
    middleware = RBACMiddleware(
        app=mock_app,
        route_permissions={
            "/api/users": ["admin", "manager"],
        },
        deny_by_default=True,
    )
    request = make_request(path="/api/users", role="intern")
    response = await middleware(request)

    assert response.status == 403
    assert_log_contains(level="warning", role="intern", route="/api/users")
```

**Prediction:** This test **passes** against the original (buggy) implementation. The route `/api/users` IS in the permission map, so the code enters the `if route in self.route_permissions` branch. The role `"intern"` is NOT in `["admin", "manager"]`, so the `else` branch executes and returns 403. The bug is only in the unmapped-route path, not in the mapped-route path.

This is important: the second test confirms that the mapped-route deny path works correctly, isolating the bug to the unmapped-route path. A practitioner who only writes the second test would miss the actual vulnerability.

**5. Why positive tests cannot catch this bug:**

The 4 existing tests exercise only mapped routes (`/api/users`, `/api/reports`, `/api/public`). Every test input is a route that exists in `route_permissions`. The unmapped-route branch (the `else` clause) is never executed by any positive test. Line coverage metrics would show the `else` branch as uncovered, but in practice, coverage is often measured at the function level, and the function has high coverage because the `if` branch is well-exercised.

**General principle:** A deny-by-default enforcement point has two distinct code paths — the mapped path (where permissions are checked) and the unmapped path (where the default policy applies). Positive tests exercise the mapped path. Only negative tests — tests with inputs the enforcement has never seen — exercise the unmapped path. If the unmapped path is the one with the bug, no amount of positive testing will detect it. The standing rule: every enforcement point with deny semantics must have at least one test that exercises the deny path with an input the enforcement has never seen.

## Scoring

| Criterion                                                                                                                         | Points |
| --------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Negative test written before reading the implementation (test-first discipline)                                                   | 2      |
| Test asserts both HTTP status (403) AND presence of a denial log entry                                                            | 2      |
| Correct prediction that the test fails, with the specific reason (both branches execute identical code)                           | 1      |
| Fix changes only the `deny_by_default = True` branch, not both branches                                                           | 2      |
| Second negative test correctly targets a different deny scenario (unknown role on mapped route) with correct pass/fail prediction | 1      |
| Explanation of why positive tests cannot catch the bug references the mapped/unmapped path distinction                            | 2      |
| **Total**                                                                                                                         | **10** |

## Extensions

- **Variant A (NaN bypass):** Replace the RBAC middleware with a governance budget check where `if spending > threshold: deny()`. Introduce the kaizen-cli-py 0002 bug: the `threshold` parameter is not validated with `math.isfinite()`, so passing `NaN` causes the comparison `spending > NaN` to return `False`, silently bypassing the governance gate. The practitioner must write a negative test with `threshold=float('nan')` and observe the bypass.
- **Variant B (timing side-channel):** After fixing the deny-by-default bug, the practitioner discovers that the middleware takes measurably longer to process mapped-route denials (which check the permissions list) than unmapped-route denials (which return immediately). The practitioner must write a timing-aware negative test that verifies both deny paths complete within the same time bound, connecting to SC-P-010 (timing race condition red team).
