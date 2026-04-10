---
drill_id: DR-SC-P-023-FULL
source_atom: SC-P-023
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Gating Workspace at Integration Seams

## Setup

The practitioner is reviewing a plan for building a CLI tool that aggregates data from three internal services. The plan has four workspaces:

**WS-A: Data Model**

```
Deliverable: A shared data model library defining 4 entity types
  (User, Project, Activity, Report) with serialization to JSON and Parquet.
Tests: Unit tests validate serialization round-trips for all 4 types.
  All 28 tests pass.
Status: COMPLETE
```

**WS-B: Service Connectors**

```
Deliverable: Three connector modules (UserService, ProjectService, ActivityService)
  that fetch data from internal APIs and return populated entity instances.
  Each connector accepts a config dict and returns a list of the corresponding entity type.
Tests: Unit tests validate each connector against mock API responses.
  All 15 tests pass (mocked -- no live service calls).
Status: COMPLETE
```

**WS-C: Report Engine**

```
Deliverable: A report generator that accepts lists of User, Project, and Activity
  entities and produces a Report entity with aggregated metrics.
  The generator exposes a single function: generate_report(users, projects, activities) -> Report.
Tests: Unit tests validate report generation with hardcoded entity instances
  constructed directly in the test file. All 12 tests pass.
Status: COMPLETE
```

**WS-D: CLI Interface** (next up)

```
Deliverable: A CLI that wires together the connectors (WS-B) to fetch live data,
  passes it through the report engine (WS-C), and outputs formatted reports.
Depends on: WS-A (data model), WS-B (connectors), WS-C (report engine).
Tests: TBD -- will test full CLI invocation with live services.
Status: NOT STARTED
```

The practitioner is asked: **"All three prerequisite workspaces pass their tests. Is the plan ready for WS-D?"**

## Task

1. Identify every integration seam between the four workspaces -- the specific points where one workspace's output must be consumed by another workspace's input. For each seam, state what the producing workspace promises and what the consuming workspace expects.

2. For each seam, determine whether the existing tests validate the integration. If they do not, explain specifically what could go wrong at that seam despite all current tests passing.

3. Propose a structural solution. Your solution must be a **gating workspace or phase** that sits between the completed workspaces (A, B, C) and the downstream workspace (D). Define what the gate tests, how it tests it, and what it blocks.

4. Explain why adding integration assertions to WS-C's test suite (the "closest to the consumer" workspace) is NOT an adequate substitute for the gate. Your explanation must address visibility, ownership, and structural enforcement.

## Model Answer

**Step 1 -- Integration seams:**

| Seam | Producer             | Consumer             | Producer promises                                              | Consumer expects                                                                                                                                      |
| ---- | -------------------- | -------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| S1   | WS-A (Data Model)    | WS-B (Connectors)    | Entity types (User, Project, Activity) with JSON serialization | Connector return types match WS-A's entity definitions; JSON deserialization from API responses produces valid entity instances                       |
| S2   | WS-A (Data Model)    | WS-C (Report Engine) | Entity types with all fields needed for aggregation            | `generate_report()` receives entities with the fields it accesses; field names, types, and nullability match                                          |
| S3   | WS-B (Connectors)    | WS-C (Report Engine) | Lists of entity instances populated from live API data         | Entity instances from connectors have the same shape as the hardcoded instances WS-C's tests use; no fields are missing, no extra fields cause errors |
| S4   | WS-C (Report Engine) | WS-D (CLI)           | A Report entity from `generate_report()`                       | The Report entity has the fields the CLI formatter expects; serialization to display format works                                                     |
| S5   | WS-B (Connectors)    | WS-D (CLI)           | Connectors accept a config dict                                | The CLI's config parsing produces dicts in the shape the connectors expect; key names, types, and required fields match                               |

**Step 2 -- Validation gaps at each seam:**

**S1 (Data Model -> Connectors): NOT VALIDATED.** WS-B's tests use mock API responses, not live responses. The mocks were hand-constructed in WS-B's test file. If WS-A changed a field name (e.g., `user_id` to `id`) after the mocks were written, WS-B's tests still pass because the mocks use the old field name, but live deserialization would fail. Neither WS-A nor WS-B tests the real deserialization path: WS-A tests serialization round-trips (entity -> JSON -> entity), and WS-B tests against static JSON that may not match WS-A's actual output.

**S2 (Data Model -> Report Engine): NOT VALIDATED.** WS-C's tests construct entity instances directly in the test file using hardcoded values. These instances may not match WS-A's current entity definitions. If WS-A added a required field to Activity after WS-C's tests were written, WS-C's hardcoded Activity instances would lack the field but still pass WS-C's tests (because WS-C never imports WS-A's constructors). At runtime, WS-C would receive entities from WS-B that have the new field, and `generate_report()` might fail or produce wrong aggregations.

**S3 (Connectors -> Report Engine): NOT VALIDATED.** This is the composition of S1 and S2. WS-B produces entity instances from live API data. WS-C consumes entity instances. But WS-B was tested with mocks and WS-C was tested with hardcoded instances. Nobody has verified that the entities WS-B produces from live data are the same shape as the entities WS-C expects. The mock/hardcoded divergence could mask field mismatches, type differences (string vs int for a metric field), or nullability issues (the API returns `null` for a field WS-C assumes is always present).

**S4 (Report Engine -> CLI): NOT VALIDATED.** WS-D has not started, so no tests exist. But even after WS-D is built, this seam will only be tested if WS-D's tests exercise `generate_report()` with realistic inputs. If WS-D's tests also use hardcoded Report instances, the seam remains untested.

**S5 (Connectors -> CLI config): NOT VALIDATED.** The connector config dict shape is defined implicitly by WS-B's implementation. WS-D will parse CLI arguments into a config dict. Nothing validates that WS-D's config structure matches WS-B's expected structure. A key named `api_url` in WS-D and `base_url` in WS-B would fail silently (the connector ignores unknown keys and uses defaults) or loudly (KeyError at runtime).

**Step 3 -- Gating workspace:**

Insert **WS-ABC (Integration Gate)** between the three completed workspaces and WS-D.

**What it tests:** A reference integration test that wires together actual imports from all three workspaces in a single test suite:

```
Test 1 (S1): Import WS-A entity types. Import WS-B connectors.
  Call each connector with test config. Verify returned objects are
  instances of WS-A entity types, with all required fields populated.

Test 2 (S2): Import WS-A entity types. Import WS-C generate_report.
  Construct entities using WS-A's constructors (not hardcoded dicts).
  Call generate_report(). Verify it returns a valid Report entity.

Test 3 (S3): Import WS-B connectors. Import WS-C generate_report.
  Call connectors with test config. Pass connector output directly
  to generate_report(). Verify it produces a valid Report without
  errors -- this is the end-to-end data path that no existing test covers.

Test 4 (S5): Define the config dict that WS-D will produce.
  Pass it to each connector. Verify no KeyError, no silent default
  substitution.
```

**When it runs:** After WS-A, WS-B, and WS-C are marked COMPLETE. The gate must pass before WS-D starts implementation.

**What it blocks:** WS-D cannot begin until all 4 integration tests pass. If a test fails, the fix goes into the producing workspace (e.g., if Test 1 fails because WS-B's deserialization does not match WS-A's schema, WS-B is reopened), not into the gate itself.

**Step 4 -- Why adding assertions to WS-C is not adequate:**

**Visibility:** Integration assertions buried inside WS-C's test suite are invisible to WS-A and WS-B. When WS-A changes its entity schema, the developer working in WS-A sees WS-A's tests pass and has no signal that WS-C's tests (which they do not own and may not run) will break. A gating workspace is visible in the plan: it appears as a named phase, it has explicit pass/fail status, and its failure blocks downstream work in a way that everyone reviewing the plan can see.

**Ownership:** WS-C owns report generation. Placing integration assertions in WS-C makes WS-C's owner responsible for validating WS-A's schema and WS-B's connector output -- concerns that WS-C was never scoped for. If WS-B changes its return type, WS-C's owner must understand and update tests for a module they did not write. A gating workspace owns the seams explicitly. Its scope is integration, not any single domain.

**Structural enforcement:** An assertion inside WS-C's test file can be deleted, commented out, or skipped without anyone outside WS-C noticing. A gating workspace in the plan is structural: WS-D is blocked until the gate passes. Deleting the gate requires changing the plan, which is visible to everyone. The gate enforces the integration contract; the assertion merely checks it within one domain's local context.

## Scoring

| Criterion                                                                                                                             | Points |
| ------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| At least 4 of the 5 integration seams identified with specific producer/consumer contracts (not just "A connects to B")               | 2      |
| For each identified seam, a specific failure scenario is described (what goes wrong despite tests passing), not just "it is untested" | 2      |
| Gate is proposed as a named workspace or phase, not as assertions added to existing workspaces                                        | 2      |
| Gate tests exercise actual imports across workspace boundaries (not mocks of other workspaces)                                        | 1      |
| Gate blocks WS-D structurally (explicit dependency, not just "should run before")                                                     | 1      |
| Explanation of why WS-C assertions are inadequate addresses all three concerns: visibility, ownership, and structural enforcement     | 2      |
| **Total**                                                                                                                             | **10** |

## Extensions

1. **Evolving seams.** After the gate passes and WS-D is halfway through implementation, WS-A's owner adds a new required field to the Activity entity for a feature request. The practitioner must explain what happens to the gate: does it need to re-run? Who triggers the re-run? What is the blast radius if the gate is not re-run? This tests whether the practitioner understands that the gate is not a one-time checkpoint but a living contract that must be re-validated when any producing workspace changes.

2. **Gate granularity.** The practitioner is given a larger plan with 8 workspaces and 12 integration seams. Not all seams justify a dedicated gate. The practitioner must decide which seams to gate explicitly and which to cover with other mechanisms (type checking, schema validation at import time, contract tests in CI). Score on whether the practitioner applies the gate selectively based on risk and coupling rather than either gating everything (over-engineering) or gating nothing (under-engineering).
