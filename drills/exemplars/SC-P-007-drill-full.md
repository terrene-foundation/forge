---
drill_id: DR-SC-P-007-FULL
source_atom: SC-P-007
difficulty: intermediate
time_box_minutes: 30
modality: drill
application: [COC]
---

# Define Session Completion State Before Starting, Not After Convergence Is Claimed

## Setup

The practitioner receives a session brief describing a multi-milestone release preparation for a Python SDK package. The brief has 7 deliverables across 3 milestones, with dependencies between them and one external dependency that may not be available.

**Session brief:**

> **Goal:** Prepare the `kailash-pact` package for its first PyPI release (v0.1.0).
>
> **Milestone 1 — Package structure** (3 deliverables):
>
> 1. Create `pyproject.toml` with metadata, dependencies, and build configuration.
> 2. Set up the `src/kailash_pact/` package layout with `__init__.py` exporting the public API (Envelope, Clearance, Bridge, GovernanceEngine).
> 3. Add a `py.typed` marker file and ensure all public types have type annotations.
>
> **Milestone 2 — Testing and CI** (2 deliverables):
>
> 4. Write a test suite covering the four public API classes. The governance engine has 6 methods; each needs at least one positive and one negative test case.
> 5. Configure GitHub Actions CI to run tests on Python 3.10, 3.11, and 3.12.
>
> **Milestone 3 — Publishing** (2 deliverables):
>
> 6. Configure PyPI OIDC trusted publisher (requires the PyPI project to be pre-registered — this is an external dependency that may not be available yet).
> 7. Add a `CHANGELOG.md` with the v0.1.0 entry and tag the release commit.
>
> **Dependencies between deliverables:**
>
> - Deliverable 4 depends on deliverable 2 (need the package structure to import from)
> - Deliverable 5 depends on deliverable 4 (CI runs the tests)
> - Deliverable 6 depends on deliverable 5 (OIDC publisher needs the CI workflow file)
> - Deliverable 7 depends on deliverable 6 (release tag triggers the publish workflow)
> - Deliverables 1 and 3 have no dependencies — can be done in parallel with deliverable 2

## Task

1. Write a completion-state checklist BEFORE looking at any code. Each item must be verifiable — not "implement tests" but a specific assertion that can be checked. Convert each of the 7 deliverables into one or more verifiable checklist items. Aim for 10-14 items total (some deliverables expand into multiple checkable items).
2. Draw the dependency graph: which items can be done in parallel and which are sequential? Identify the critical path (the longest chain of sequential dependencies). Name the single item that, if it slips, blocks the most downstream work.
3. Name what is explicitly NOT in scope for this session, with a one-sentence reason for each exclusion. Identify at least 3 plausible out-of-scope items that a practitioner might accidentally start.
4. Read the following session-end narrative and evaluate it against your checklist. Score each checklist item as DONE, PARTIAL, or GAP. Calculate the completion percentage. Identify every claim in the narrative that is not backed by a checklist item.

**Session-end narrative** (presented after the practitioner writes the checklist):

> "Good session. Package structure is complete — `pyproject.toml` is set up with all metadata, the src layout is working, and `__init__.py` exports everything. Types are annotated. Tests are written and passing — 14 tests covering all 4 classes. CI is configured for 3.10 and 3.11. The PyPI OIDC publisher wasn't available yet so we skipped that. Changelog is written. All major work done, just minor polish remaining."

## Model Answer

**Step 1 — Completion-state checklist:**

| #   | Checklist Item                                                                                             | Source Deliverable |
| --- | ---------------------------------------------------------------------------------------------------------- | ------------------ |
| 1   | `pyproject.toml` exists with `name = "kailash-pact"`, `version = "0.1.0"`, and `requires-python >= "3.10"` | D1                 |
| 2   | `pyproject.toml` declares all runtime dependencies (list each by name)                                     | D1                 |
| 3   | `pyproject.toml` build system is configured (`[build-system]` section with setuptools or hatch)            | D1                 |
| 4   | `src/kailash_pact/__init__.py` exports exactly: Envelope, Clearance, Bridge, GovernanceEngine              | D2                 |
| 5   | `from kailash_pact import Envelope, Clearance, Bridge, GovernanceEngine` succeeds in a clean venv          | D2                 |
| 6   | `src/kailash_pact/py.typed` marker file exists                                                             | D3                 |
| 7   | `mypy --strict src/kailash_pact/` passes with zero errors (all public types annotated)                     | D3                 |
| 8   | Test suite has >= 12 test functions (6 methods x 2 cases each, minimum)                                    | D4                 |
| 9   | `pytest tests/` passes with 0 failures                                                                     | D4                 |
| 10  | `.github/workflows/ci.yml` exists and targets Python 3.10, 3.11, AND 3.12                                  | D5                 |
| 11  | CI workflow triggers on push to main and pull request                                                      | D5                 |
| 12  | PyPI OIDC trusted publisher configured, OR gap documented with "blocked on: PyPI project pre-registration" | D6                 |
| 13  | `CHANGELOG.md` exists with a v0.1.0 section listing all changes                                            | D7                 |
| 14  | Release commit is tagged `v0.1.0`                                                                          | D7                 |

**Step 2 — Dependency graph and critical path:**

```
Parallel start:
  D1 (pyproject.toml)  ─┐
  D3 (py.typed + types) ─┼─→ D2 (src layout) ─→ D4 (tests) ─→ D5 (CI) ─→ D6 (OIDC) ─→ D7 (changelog + tag)
                         │
                         └─ (D1 and D3 can start in parallel with D2)
```

**Critical path:** D2 → D4 → D5 → D6 → D7 (4 sequential hops after D2 completes).

**Single point of failure:** D2 (package structure). If the src layout is wrong, tests cannot import, CI cannot run, and everything downstream is blocked. D6 is the external-dependency risk — if PyPI pre-registration is not available, D6 blocks D7.

**Items that can be done in parallel:**

- D1, D2, and D3 can all start simultaneously (no dependencies between them)
- D4 through D7 are strictly sequential

**Step 3 — Explicit exclusions (NOT in scope):**

| Exclusion                                            | Reason                                                                                                                   |
| ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Documentation site (Sphinx/MkDocs)                   | v0.1.0 is a package release, not a documentation release; docs can follow in a separate session                          |
| Pre-commit hooks and linting configuration           | Useful but not a release blocker; adding ruff/black is a separate concern from shipping the package                      |
| README.md with usage examples                        | The PyPI landing page is important but distinct from the release mechanics; can be added before the actual publish       |
| Integration tests against a real governance scenario | The brief specifies unit-level tests (positive + negative per method); end-to-end governance scenarios are Tier 2/3 work |
| Publishing to Test PyPI                              | Good practice but not in the brief; the OIDC publisher covers the real publish path                                      |

**Step 4 — Narrative evaluation:**

| #   | Checklist Item                                     | Narrative Evidence                           | Score                                                                                 |
| --- | -------------------------------------------------- | -------------------------------------------- | ------------------------------------------------------------------------------------- |
| 1   | pyproject.toml with name, version, requires-python | "pyproject.toml is set up with all metadata" | DONE                                                                                  |
| 2   | pyproject.toml declares runtime dependencies       | No mention of dependencies specifically      | PARTIAL — metadata is claimed but dependencies not confirmed                          |
| 3   | pyproject.toml build system configured             | No mention                                   | GAP                                                                                   |
| 4   | **init**.py exports 4 classes                      | "exports everything"                         | DONE (accepting "everything" as covering the 4 named classes)                         |
| 5   | Clean venv import succeeds                         | No mention of venv verification              | GAP                                                                                   |
| 6   | py.typed marker exists                             | No mention                                   | GAP — narrative says "types are annotated" but py.typed is a separate file            |
| 7   | mypy --strict passes                               | No mention of mypy                           | GAP — "types are annotated" does not mean mypy passes                                 |
| 8   | >= 12 test functions                               | "14 tests covering all 4 classes"            | DONE                                                                                  |
| 9   | pytest passes with 0 failures                      | "tests are written and passing"              | DONE                                                                                  |
| 10  | CI targets 3.10, 3.11, AND 3.12                    | "CI is configured for 3.10 and 3.11"         | PARTIAL — **3.12 is missing**. The brief requires 3 versions; the narrative claims 2. |
| 11  | CI triggers on push and PR                         | No mention of trigger configuration          | GAP                                                                                   |
| 12  | OIDC publisher configured or gap documented        | "wasn't available yet so we skipped that"    | PARTIAL — gap acknowledged but no documentation of the blocker for the next session   |
| 13  | CHANGELOG.md with v0.1.0 section                   | "changelog is written"                       | DONE                                                                                  |
| 14  | Release commit tagged v0.1.0                       | No mention of tagging                        | GAP                                                                                   |

**Completion score:** 5 DONE, 3 PARTIAL, 6 GAP = **5 of 14 fully done (36%)**.

The narrative's claim of "all major work done, just minor polish remaining" is contradicted by the checklist. Six items have no evidence of completion. The most impactful gaps are:

- **Missing Python 3.12 in CI** — the brief explicitly requires it; delivering 2 of 3 is a partial, not a done.
- **No release tag** — without the tag, the release workflow cannot trigger, making D7 incomplete even though the changelog exists.
- **No clean venv verification** — the package may not install correctly despite "exports everything."
- **py.typed and mypy gaps** — "types are annotated" is not the same as "py.typed marker exists and mypy passes."

**Unbacked narrative claims:** The phrase "just minor polish remaining" is not backed by any checklist item and functions as an assertion of convergence without evidence. A pre-defined checklist turns this into a testable claim: 36% completion is not "minor polish."

## Scoring

| Criterion                                                                                      | Points |
| ---------------------------------------------------------------------------------------------- | ------ |
| Checklist has 10+ verifiable items (not vague "implement X" items)                             | 1      |
| Each checklist item is testable (could write a script or command to verify it)                 | 1      |
| Dependency graph correctly identifies D2 as the root and the sequential chain D4→D5→D6→D7      | 1      |
| Critical path and single point of failure identified                                           | 1      |
| At least 3 explicit exclusions with reasons (not just "other stuff")                           | 1      |
| Narrative evaluation catches the Python 3.12 omission (PARTIAL, not DONE)                      | 1      |
| Narrative evaluation catches the missing release tag (GAP, not implied by "changelog written") | 1      |
| Narrative evaluation catches py.typed / mypy gap (annotation != marker file + passing mypy)    | 1      |
| Completion percentage calculated and contradicts the narrative's "all major work done"         | 1      |
| "Minor polish remaining" identified as an unbacked convergence claim                           | 1      |
| **Total**                                                                                      | **10** |

## Extensions

1. **Multi-session programme.** Expand the brief to 35 items across 5 domains (DataFlow, Nexus, kailash-ml, MCP, kailash-rs), modelled on journal entry 0031. The practitioner must define completion state at the programme level: identify the critical path (8 items deep), name the two keystones that must start on day 1, and calculate what percentage of items can start with zero dependencies. This tests whether the completion-state discipline scales from a single session to a multi-session plan.

2. **Adversarial narrative.** Present a second session-end narrative that is deliberately vague: "Everything went well. Tests are green. Package is ready. Will tag and publish next session." The practitioner must score this narrative against the same checklist. The expected result is that almost every item scores GAP because the narrative contains no specific evidence. This trains the practitioner to distinguish between convergence assertions and evidence-backed completion claims.

3. **Checklist revision under scope change.** Mid-session, the programme owner adds a new requirement: "The package must also export a `pact_version()` function that returns the package version string, and the CI must include a test that asserts `pact_version() == '0.1.0'`." The practitioner must add 2 new checklist items, reassess the dependency graph (the new test depends on the new function, which depends on the package structure), and determine whether the critical path changes. This tests whether the checklist is a living document or a one-time artifact.
