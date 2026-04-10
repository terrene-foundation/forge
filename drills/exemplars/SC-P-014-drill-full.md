---
drill_id: DR-SC-P-014-FULL
source_atom: SC-P-014
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Distinguish Pre-Existing Failures from Pre-Existing Gaps

## Setup

A practitioner is midway through a session scoped to **"sync the Express API transport layer to the Rust SDK"** (issue #115). During the sync implementation, the practitioner runs the binding test suites across all language bindings as a pre-flight check and discovers 6 issues that were not part of the original scope. The session's zero-tolerance rules include Rule 1: "If you found it, you own it. Fix it in THIS run."

The practitioner must classify each issue and decide the correct action before proceeding with the sync work.

**Issue 1 — Ruby `upsert` binding:**

```
File: bindings/ruby/spec/dataflow_spec.rb (line 87)
Test: it "upserts a record with conflict resolution"
Status: EXISTS, FAILS at runtime
Error: NotImplementedError: DataFlow#upsert is not yet implemented
        at bindings/ruby/lib/dataflow.rb:142
Note: The test was added in commit abc1234 (3 months ago) alongside
      the Python upsert implementation. The Ruby binding stub was
      committed with the test, promising future implementation.
```

**Issue 2 — Node.js CRUD bindings:**

```
Directory: bindings/nodejs/
Contents: package.json, index.js (re-exports transport layer only)
CRUD files: NONE — no create, read, update, delete, upsert, aggregate
Tests: NONE — no test files for CRUD operations
Tracking: No GitHub issue, no TODO, no roadmap mention
Note: The Node.js binding has only ever provided transport-layer
      functionality. CRUD was never started.
```

**Issue 3 — C ABI `dataflow_query` function:**

```
File: bindings/c/src/dataflow.c (line 203)
Function: dataflow_result_t dataflow_query(const char* sql, ...)
Status: EXISTS, compiled, linked, documented in the header
Behaviour: Segfaults when sql parameter is NULL
Reproducer: dataflow_query(NULL) → SIGSEGV
Note: The function handles non-NULL inputs correctly. The NULL case
      is the only defect.
```

**Issue 4 — WASM DataFlow bindings:**

```
Directory: bindings/wasm/
Contents: wasm-pack configuration for transport layer
DataFlow files: NONE — no DataFlow bindings of any kind
Tests: Transport tests only
Tracking: No GitHub issue, no TODO, no roadmap mention
Note: WASM bindings were added for the transport layer 6 months ago.
      DataFlow support in WASM was never scoped or started.
```

**Issue 5 — Ruby `aggregate` function:**

```
File: bindings/ruby/lib/dataflow.rb (line 198)
Function: def aggregate(collection, pipeline)
Status: EXISTS, runs without error
Behaviour: Returns empty array when collection is empty
Expected: Should return the identity element for the aggregation
          (e.g., 0 for sum, [] for collect, nil for first)
Test: bindings/ruby/spec/dataflow_spec.rb line 134 — test exists,
      asserts identity-element return, FAILS
Note: The function was implemented 4 months ago. The test was added
      2 months ago when the identity-element requirement was clarified.
```

**Issue 6 — Go high-level wrapper:**

```
Directory: bindings/go/
Contents: Low-level CGo wrapper around the C ABI (dataflow.go, 180 lines)
High-level API: NONE — no Go-idiomatic wrapper with error types,
                context support, or iterator patterns
Tests: Low-level CGo tests pass
Tracking: GitHub issue #89 exists: "Go: add high-level DataFlow API"
          (open, unassigned, no milestone)
Note: The C ABI works correctly through CGo. Go consumers currently
      use the low-level C-style API directly. No one has started
      the high-level wrapper.
```

## Task

1. For each of the 6 issues, classify it as either **FAILURE** or **GAP**. State the classification criterion you used: what evidence distinguishes this issue as one category versus the other?

2. For each issue, state the correct action:
   - If FAILURE: what is the concrete next step to fix it in this session? (Not "fix it" — the specific first action.)
   - If GAP: what is the concrete next step to track it without implementing it? (Not "file an issue" — what goes in the issue title, scope description, and dependency notes.)

3. Two of the 6 issues have a subtle classification trap — they could be argued either way. Identify which two, explain the argument for each side, and defend your final classification.

4. Write a one-paragraph **decision rule** that a future practitioner could apply mechanically to classify any discovered issue as FAILURE or GAP without needing to deliberate each time.

## Model Answer

**1. Classifications:**

| #   | Issue                       | Classification | Criterion                                                                                                                                                         |
| --- | --------------------------- | -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Ruby `upsert` binding       | **FAILURE**    | A test exists that asserts specific behaviour. The implementation was committed (stub form). The test fails at runtime. Something was promised and does not work. |
| 2   | Node.js CRUD bindings       | **GAP**        | No implementation, no tests, no tracking issue, no prior art. CRUD was never started for Node.js. Nothing is broken because nothing was built.                    |
| 3   | C ABI `dataflow_query` NULL | **FAILURE**    | The function exists, compiles, is documented in the public header, and handles non-NULL inputs. The NULL-input crash is a defect in existing, shipped code.       |
| 4   | WASM DataFlow bindings      | **GAP**        | No DataFlow files exist. No tests. No tracking. WASM was scoped for transport only. DataFlow in WASM was never started.                                           |
| 5   | Ruby `aggregate` identity   | **FAILURE**    | The function exists and runs. A test exists that asserts the correct behaviour. The test fails. The function returns wrong results for a documented edge case.    |
| 6   | Go high-level wrapper       | **GAP**        | The low-level CGo wrapper works correctly. The high-level wrapper was never started. A tracking issue exists but is unassigned and unmilestoned.                  |

**2. Actions:**

| #   | Classification | Action                                                                                                                                                                                                                                                                                      |
| --- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | FAILURE        | Read `bindings/ruby/lib/dataflow.rb:142` to understand the stub. Implement the `upsert` method using the same pattern as the Python binding's upsert, delegating to the C ABI's `dataflow_upsert`. Run the existing test to verify.                                                         |
| 2   | GAP            | File GitHub issue: "Node.js: add CRUD bindings (create, read, update, delete, upsert, aggregate)". Scope: implement all 6 CRUD operations with tests, matching the Ruby binding's API surface. Dependency: requires C ABI CRUD functions (already exist). Do not implement in this session. |
| 3   | FAILURE        | Add a NULL-check guard at `dataflow.c:203`: `if (!sql) return DATAFLOW_ERR_NULL_INPUT;`. Add a test case for `dataflow_query(NULL)` that asserts the error return.                                                                                                                          |
| 4   | GAP            | File GitHub issue: "WASM: add DataFlow bindings". Scope: expose DataFlow CRUD + query operations via wasm-bindgen. Dependency: requires decision on whether WASM DataFlow uses wasm-sqlite or calls back to host. Do not implement in this session.                                         |
| 5   | FAILURE        | Fix `aggregate` at `dataflow.rb:198` to return the aggregation's identity element when the collection is empty, not an empty array. The existing test at `dataflow_spec.rb:134` defines the expected behaviour. Run the test to verify.                                                     |
| 6   | GAP            | GitHub issue #89 already exists. Add a scope comment to the issue: "Scope: Go-idiomatic wrapper with error types (not sentinel errors), context.Context support, and iterator patterns for query results. Depends on stable C ABI (currently stable)." Do not implement in this session.    |

**3. Subtle classification traps:**

**Issue 1 (Ruby `upsert`) — the trap:** One could argue this is a GAP because the implementation is a stub (`NotImplementedError`), meaning the feature was never actually built. The counterargument: a test exists that asserts specific behaviour, the stub was committed alongside the Python implementation with an implicit promise of parity, and the test is in the CI suite where it fails on every run. The presence of a failing test is the decisive evidence — it means the system claims to offer this capability (the test asserts it) and fails to deliver. **Classification: FAILURE.** The test is the promise; the `NotImplementedError` is the broken promise.

**Issue 6 (Go high-level wrapper) — the trap:** One could argue this is a FAILURE because a GitHub issue exists requesting it, which could be read as the system "promising" a high-level API. The counterargument: the issue is a feature request (open, unassigned, no milestone), not a regression report. The low-level CGo wrapper works correctly. No test asserts high-level wrapper behaviour. No code was started. The existence of a feature request does not convert a gap into a failure — it converts an untracked gap into a tracked gap. **Classification: GAP.** A wish is not a promise; a promise requires code or tests that assert the wished-for behaviour.

**4. Decision rule:**

Does evidence exist that the capability was attempted and is now broken? Look for three signals: (a) code that implements the capability exists but produces wrong results or crashes, (b) a test asserts the expected behaviour and fails, or (c) a prior working state existed and has regressed. If any of these three signals is present, the issue is a **FAILURE** and must be fixed in the current session. If none of the three signals is present — no implementation exists, no test asserts the behaviour, no prior working state has regressed — the issue is a **GAP** and must be tracked as a separate workstream with its own scope, analysis, and implementation session.

## Scoring

| Criterion                                                                              | Points |
| -------------------------------------------------------------------------------------- | ------ |
| Issues 1, 3, 5 correctly classified as FAILURE with evidence cited                     | 2      |
| Issues 2, 4, 6 correctly classified as GAP with evidence cited                         | 2      |
| FAILURE actions include a concrete first step (not just "fix it")                      | 1      |
| GAP actions include issue title, scope, and dependency notes                           | 1      |
| At least one subtle trap identified with both-sides argument                           | 2      |
| Decision rule is mechanically applicable (not "use judgment")                          | 1      |
| Decision rule covers all three failure signals (broken code, failing test, regression) | 1      |
| **Total**                                                                              | **10** |

## Extensions

- **Variant A (harder):** Add a 7th issue: the Go low-level CGo wrapper's `dataflow_delete` function works correctly but uses `unsafe` pointer arithmetic that Miri flags as undefined behaviour. The function has never produced a wrong result in practice. Is this a FAILURE (undefined behaviour is a defect in shipped code) or a GAP (the function works, it just needs a safer implementation)? Defend the classification using the three-signal test from the decision rule.
- **Variant B (scope pressure):** The session has 45 minutes remaining. The three FAILURES are estimated at 20 minutes, 10 minutes, and 15 minutes respectively (45 minutes total). The practitioner cannot fix all three and complete the original sync task. How should they triage? Does zero-tolerance Rule 1 override the session's original scope, or does the session scope constrain how many failures are fixable? Design a triage protocol that respects both the rule and the constraint.
