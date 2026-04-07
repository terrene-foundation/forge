---
drill_id: DR-SC-A-014-FULL
source_atom: SC-A-014
difficulty: intermediate
time_box_minutes: 30
---

# Decompose Milestones with Hard-Blocking Approval Gates

## Setup

The practitioner receives a feature brief describing a governance engine implementation with 12 requirements that have non-obvious dependencies.

**Feature brief:**

> Build a PACT governance engine that supports: (1) role compilation from an org definition, (2) bridge creation between departments with bilateral consent, (3) envelope enforcement across 7 dimensions, (4) write-time tightening validation, (5) vacancy handling with interim envelopes, (6) EATP record emission for all governance actions, (7) audit trail generation, (8) knowledge access checks that block vacant roles, (9) action verification that blocks vacant roles, (10) configurable designation deadlines, (11) red team validation of all paths, (12) cross-SDK parity verification against the Rust implementation.
>
> Known dependencies: Vacancy handling (#5) requires role compilation (#1) because interim envelope computation for vacant roles needs compiled org data. Bridge consent (#2) requires role compilation (#1) because bridge endpoints must be valid compiled roles. EATP record emission (#6) requires audit trail (#7) because EATP records reference audit anchors. Cross-SDK parity (#12) requires all features to be implemented first. Red team (#11) requires all features to be implemented first.
>
> Estimated effort: "several sessions."

The brief deliberately presents requirements as a flat list without milestone structure, dependency arrows, or parallelisation analysis. The practitioner must impose structure.

## Task

1. Extract the 12 requirements from the brief and identify all dependency relationships (both stated and implied). Draw or describe the dependency graph.
2. Group the requirements into milestones. Each milestone must have:
   - A single named approval gate
   - Evidence required to pass that gate (not "tests pass" but specific acceptance criteria)
   - Explicit upstream dependencies (which milestones must be complete before this one starts)
3. Identify which milestones can run in parallel and which must be sequential. Mark the maximum parallelisation: how many milestones can execute concurrently at each stage?
4. Identify the convergence milestone -- the one that depends on all others and represents the integration point.
5. For each milestone, estimate effort in sessions and explain the estimate.

## Model Answer

**Dependency graph** (stated + implied):

- Requirements #1 (compilation) has no upstream dependencies -- it is the root
- #2 (bridges) depends on #1 (bridge endpoints must be valid compiled roles)
- #3 (envelope enforcement) has no upstream dependencies
- #4 (tightening validation) depends on #3 (needs envelope data structures)
- #5 (vacancy handling) depends on #1 (interim envelope needs compiled org)
- #6 (EATP emission) depends on #7 (references audit anchors)
- #7 (audit trail) has no upstream dependencies
- #8 (knowledge access) depends on #1 (role-based access requires compiled roles)
- #9 (action verification) depends on #1 and #3 (role lookup + envelope check)
- #10 (configurable deadlines) depends on #5 (deadline is a vacancy parameter)
- #11 (red team) depends on all 1-10
- #12 (cross-SDK parity) depends on all 1-10

**Milestone decomposition** (modelled on journal entry 0016, kailash-py PACT sprint):

| Milestone | Requirements | Depends On | Gate | Evidence |
|---|---|---|---|---|
| M1: Compilation + Bridges | #1, #2, #8 | None | Compilation gate | `compile_org()` handles headless departments correctly. `create_bridge()` validates bilateral consent. `check_access()` blocks vacant roles. All three tested with positive and negative cases. |
| M2: Envelope Enforcement | #3, #4 | None | Tightening gate | `validate_tightening()` checks all 7 dimensions. Write-time validation rejects envelope widening on every dimension, verified by attempting to widen each one. |
| M3: Vacancy + Deadlines | #5, #9, #10 | M1 | Vacancy gate | Interim envelope is computed for vacant roles. `verify_action()` blocks vacant roles. Designation deadline is configurable (not hardcoded to 24h). |
| M4: EATP Integration | #6, #7 | None | EATP gate | Every governance action (compile, bridge, envelope change, vacancy designation) emits a corresponding EATP record. Audit trail anchors are referenced in each record. Zero governance actions exist without EATP records. |
| M5: Validation | #11, #12 | M1, M2, M3, M4 | Convergence gate | Red team passes with zero CRITICAL findings. Cross-SDK parity matrix shows all 12 requirements satisfied in both Python and Rust. All GitHub issues from prior audits are closed. |

**Parallelisation map:**

- Stage 1: M1, M2, M4 execute in parallel (3 concurrent milestones). All three have no upstream dependencies.
- Stage 2: M3 executes (depends on M1 for compiled org data). If M2 and M4 are still running, M3 runs in parallel with their remaining work.
- Stage 3: M5 executes after all of M1-M4 are complete. This is the convergence point.

**Maximum concurrency:** 3 milestones at Stage 1. The critical path is M1 then M3 then M5 (compilation is the root that enables vacancy handling, and vacancy is the last feature-level milestone before validation).

**Effort estimates:**
- M1: 1 session (compilation + bridges + access checks are well-understood)
- M2: 1 session (envelope enforcement across 7 dimensions)
- M3: 0.5 session (vacancy handling is small but depends on M1 output)
- M4: 1 session (EATP wiring requires new import chains and audit trail integration)
- M5: 1 session (red team + cross-SDK + issue closure)
- Total: 4.5 sessions sequential, 3 sessions with parallelisation

This structure mirrors the real decomposition from journal 0016: 4 PACT conformance issues decomposed into 14 todos across 5 milestones, with M1/M2/M4 parallel, M3 depending on M2, and M5 as convergence.

## Scoring Criteria

1. **Dependency graph correctness** (pass/fail): The practitioner must identify that vacancy (#5) depends on compilation (#1) and that EATP (#6) depends on audit trail (#7). Missing either dependency means the milestone decomposition will allow work to start before its prerequisites are met.
2. **No artificial sequencing** (pass/fail): M1, M2, and M4 must be identified as parallelisable. A practitioner who sequences them (M1 then M2 then M4) forces serial execution of work that has no dependency relationship. The kailash-py journal 0016 explicitly notes: "M1, M2, M4 can execute in parallel."
3. **Gate evidence is concrete** (pass: 4/5 milestones): Each gate must name specific acceptance criteria, not "tests pass" or "done." "validate_tightening() checks all 7 dimensions, verified by attempting to widen each" is concrete. "Envelope enforcement works" is not.
4. **Convergence milestone identified** (pass/fail): M5 must be explicitly named as the convergence point that depends on all prior milestones. Without it, there is no integration verification and the feature ships without end-to-end validation.
5. **Effort estimate uses parallelisation** (pass/fail): The practitioner must provide both a sequential estimate and a parallel estimate, showing the time saved by concurrent execution. A flat "4.5 sessions" without the parallel alternative misses the autonomous execution model.

## Common Mistakes

1. **Flat sequence instead of dependency graph.** The practitioner lists M1 then M2 then M3 then M4 then M5 in numerical order, forcing serial execution. M1/M2/M4 have no dependency relationship and should run in parallel. The kz-engage journal entry (0007) explicitly calls this out: "M1, M2, M3, M5 can all progress concurrently after M0 completes. This is the highest-leverage parallelisation in the project."

2. **Gates without evidence.** "M1 complete" is not a gate. "M1 passes: compile_org() handles headless departments, create_bridge() validates bilateral consent, check_access() blocks vacant roles -- all verified with positive and negative test cases" is a gate. The nexus-parity journal (0002) shows the pattern: NP-009 is a verification milestone with explicit acceptance criteria, not a placeholder.

3. **Missing the convergence milestone.** The practitioner decomposes the work into feature milestones but does not create an integration/validation milestone at the end. Without convergence, the feature ships as 4 independently-tested components that have never been verified together. The red-team-round and cross-SDK parity requirements from the brief are the signal that a convergence milestone is needed.

## Extensions

1. **Cross-workspace dependencies.** Add a requirement that the governance engine depends on a DataFlow schema change in a different workspace. The practitioner must extend the milestone graph across workspace boundaries, identifying the external dependency and adding a blocking gate for it. This connects to SC-B-013 (map cross-workspace dependency surface).

2. **Mid-plan re-decomposition.** After the practitioner produces their milestone plan, introduce a new requirement: "The Rust SDK just added a new governance feature (knowledge clearance levels) that Python must now match." The practitioner must insert this requirement into the existing plan without disrupting the dependency graph. Does it slot into an existing milestone, or does it require a new one? Does it change any dependency relationships?

3. **Long-pole splitting.** Tell the practitioner that M1 (compilation + bridges + access) is now estimated at 3 sessions instead of 1, making it the critical-path bottleneck that blocks M3. The practitioner must apply SC-P-018 (split the long-pole): decompose M1 into M1a (compilation only, 1 session) and M1b (bridges + access, 2 sessions). M3 depends on M1a but not M1b, so M3 can start earlier. Score on whether the split is correct (M3 depends on compilation but not bridges).
