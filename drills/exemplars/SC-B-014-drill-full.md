---
drill_id: DR-SC-B-014-FULL
source_atom: SC-B-014
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Decision Reversal Journaling with Explicit Supersession

## Setup

The practitioner manages the journal for a Kailash SDK project. The journal currently contains 16 entries (0001 through 0016). The practitioner is given the following existing entry:

**Entry 0012-DECISION-sqlite-for-test-fixtures.md:**

```markdown
# 0012-DECISION — Use SQLite for All Test Fixtures

**Date:** 2026-02-14
**Status:** Active
**Context:** The project needs a database backend for integration and E2E tests.
Three options were considered:

1. **SQLite in-memory** — zero infrastructure, instant teardown, portable
2. **PostgreSQL via Docker** — production-parity, but requires Docker daemon and
   takes 3-8 seconds to spin up per test run
3. **PostgreSQL via testcontainers** — production-parity with cleaner lifecycle,
   but adds testcontainers dependency and still requires Docker

**Decision:** Use SQLite in-memory (`sqlite:///:memory:`) for all test fixtures
across all three test tiers (unit, integration, E2E).

**Rationale:** The project is in early development. Test speed matters more than
production parity at this stage. SQLite in-memory gives sub-second setup,
no external dependencies, and works identically on CI and local. When the
project matures and needs production-parity testing, revisit this decision.

**Consequences:**

- All test fixtures use `DataFlow("sqlite:///:memory:")`
- No Docker dependency in CI pipeline
- Test suite runs in <10 seconds total
- SQL must be dialect-portable (no PostgreSQL-specific features in tests)
```

The practitioner now encounters the following situation during a red team review of the DataFlow module:

**New analysis (red team Round 2 findings):**

The DataFlow module has grown significantly since entry 0012. Three issues have surfaced that are directly caused by the SQLite-only testing decision:

1. **JSONB column queries:** DataFlow now supports JSONB columns for flexible metadata storage. SQLite has no native JSONB type — test fixtures use `TEXT` with `json()` functions, which do not replicate PostgreSQL's `@>`, `?`, `?|` operators. Six tests pass on SQLite but would fail on PostgreSQL because they test JSON containment logic that behaves differently across dialects.

2. **Advisory locks:** DataFlow's new distributed task queue uses PostgreSQL advisory locks (`pg_advisory_lock`) for job deduplication. This feature is completely untestable on SQLite — the advisory lock tests are currently skipped with `@pytest.mark.skip(reason="requires PostgreSQL")`. Three skipped tests are hiding potential deadlock bugs.

3. **Transaction isolation:** DataFlow's concurrent access tests use SQLite's serialized access model, which masks race conditions that PostgreSQL's MVCC would expose. The red team found a potential lost-update bug in the bulk upsert path that only manifests under `READ COMMITTED` isolation, which SQLite does not support.

## Task

1. Write journal entry **0017-DECISION** that reverses entry 0012. The entry must satisfy four requirements:
   - (a) Name entry 0012 by number and title
   - (b) Contain an explicit "supersedes" statement
   - (c) Explain why 0012 was wrong or incomplete
   - (d) Be readable as a standalone entry without requiring the reader to look up 0012

2. The entry must record the new decision with enough specificity that a future agent can implement it without ambiguity. What database backend should tests use now? Does the answer differ by test tier?

3. Entry 0012 is still on disk as `0012-DECISION-sqlite-for-test-fixtures.md`. Should you edit it to add a "Superseded by 0017" note? Or leave it untouched? Justify your choice.

4. A colleague reads your 0017 entry and says: "Why not just delete 0012? It's wrong now." Explain why deletion violates the journal's append-only property and what institutional knowledge would be lost.

## Model Answer

**1. Entry 0017-DECISION-postgresql-for-integration-and-e2e.md:**

```markdown
# 0017-DECISION — Use PostgreSQL for Integration and E2E Test Fixtures

**Date:** 2026-04-10
**Status:** Active
**Supersedes:** 0012-DECISION — Use SQLite for All Test Fixtures

## Context

Entry 0012 decided to use SQLite in-memory for all test fixtures across all
three test tiers. That decision was correct for early development: the project
had no PostgreSQL-specific features, test speed was the priority, and dialect
portability was achievable.

The codebase has since outgrown that decision. Three PostgreSQL-specific
features are now in production code that cannot be tested under SQLite:

1. **JSONB column queries** — DataFlow supports JSONB metadata columns.
   SQLite's `json()` functions do not replicate PostgreSQL's containment
   operators (`@>`, `?`, `?|`). Six tests pass on SQLite but exercise
   different semantics than production.

2. **Advisory locks** — DataFlow's distributed task queue uses
   `pg_advisory_lock` for job deduplication. SQLite has no equivalent.
   Three tests are currently skipped entirely, hiding potential deadlock bugs.

3. **Transaction isolation** — Concurrent access tests run under SQLite's
   serialized model, which masks race conditions that PostgreSQL's MVCC
   (READ COMMITTED) would expose. A potential lost-update bug in bulk upsert
   was identified by red team but cannot be reproduced on SQLite.

Entry 0012's rationale ("revisit when the project matures") is now triggered.
The project has matured past the point where dialect-portable SQL is sufficient.

## Decision

Split the test database backend by tier:

- **Tier 1 (unit tests):** Continue using SQLite in-memory. Unit tests mock
  the database layer anyway, and speed is paramount. No change from 0012.
- **Tier 2 (integration tests):** Use PostgreSQL via testcontainers. Every
  integration test gets a fresh PostgreSQL instance with automatic lifecycle
  management. This adds the `testcontainers` dependency and requires Docker
  on CI, but gives production-parity for JSONB, advisory locks, and
  transaction isolation.
- **Tier 3 (E2E tests):** Use PostgreSQL via testcontainers, same as Tier 2.
  E2E tests must reflect production behavior; SQLite is not acceptable.

## Rationale

A full PostgreSQL mandate for all tiers would slow unit tests unnecessarily.
SQLite remains correct for Tier 1 where the database is mocked and the test
target is business logic, not data access. The tier split preserves 0012's
speed benefit where it still applies while eliminating the three failure modes
where it no longer applies.

## Consequences

- CI pipeline adds Docker-in-Docker or Docker socket mount for integration/E2E
- Test setup time for Tier 2/3 increases from <1s to 3-5s per test session
  (testcontainers caches the PostgreSQL image after first pull)
- The 6 SQLite-passing-but-wrong JSONB tests must be rewritten against PostgreSQL
- The 3 skipped advisory lock tests are unskipped and run against PostgreSQL
- The bulk upsert race condition can now be reproduced and fixed
- `@pytest.mark.skip(reason="requires PostgreSQL")` is eliminated as a pattern —
  if a test needs PostgreSQL, it runs on PostgreSQL
```

**2. Specificity of the new decision:**

The entry specifies: Tier 1 stays SQLite, Tiers 2 and 3 move to PostgreSQL via testcontainers. The implementation path is unambiguous: add `testcontainers[postgresql]` to dev dependencies, create a `@pytest.fixture` that yields a PostgreSQL DataFlow instance for Tier 2/3, update CI to have Docker available, rewrite the 6 JSONB tests, unskip the 3 advisory lock tests, and add a test for the bulk upsert race condition under `READ COMMITTED`.

**3. Should 0012 be edited?**

Add a one-line forward reference at the top of 0012: `**Note:** Superseded by 0017-DECISION — Use PostgreSQL for Integration and E2E Test Fixtures.` This is not a content edit — it is a navigational pointer. The body of 0012 remains untouched. The forward reference helps a future reader who lands on 0012 first (e.g., via search) find the current decision without reading the entire journal sequentially. This is the same pattern used in RFC series (e.g., "Obsoleted by RFC 9110") and does not violate append-only semantics because the original decision text is preserved.

**4. Why deletion is wrong:**

Deleting 0012 destroys three pieces of institutional knowledge:

- **The decision context:** Why SQLite was chosen in the first place (speed, no Docker dependency, early-stage pragmatism). Without this, a future reader of 0017 cannot tell whether the original decision was careless or considered.
- **The validity period:** 0012 was correct from 2026-02-14 until the project added PostgreSQL-specific features. Downstream work done during that period (test fixtures, CI configuration, development setup docs) was built on a valid decision. Deleting 0012 makes that downstream work look like it was built on nothing.
- **The reversal trigger:** 0017 explicitly says "0012's rationale ('revisit when the project matures') is now triggered." Without 0012, the reader cannot verify that the reversal was principled (honouring the original decision's own escape clause) rather than arbitrary. The supersession chain is the evidence that the knowledge system is self-correcting.

## Scoring

| Criterion                                                                           | Points |
| ----------------------------------------------------------------------------------- | ------ |
| Entry names 0012 by number and title in a supersedes statement                      | 2      |
| Entry explains why 0012 was wrong or incomplete (all three failure modes cited)     | 2      |
| Entry is standalone — a reader unfamiliar with 0012 can understand the new decision | 1      |
| New decision is tier-specific (not a blanket "use PostgreSQL for everything")       | 2      |
| Forward-reference answer distinguishes navigational edit from content edit          | 1      |
| Deletion answer names specific institutional knowledge that would be lost           | 1      |
| Practitioner did NOT edit 0012's body or create the new decision inside 0012        | 1      |
| **Total**                                                                           | **10** |

## Extensions

- **Variant A (same-session reversal):** The entry being superseded is 0015, written 2 hours ago in the current session. The practitioner feels awkward reversing their own recent decision. Write entry 0018 that supersedes 0015. Does the speed of reversal change the supersession format? (No — same format, same discipline. The journal entry `0004-DECISION-dataflow-is-the-fabric` in the kailash-py corpus superseded `0001-DECISION-dataflow-extension-not-separate-package` within the same session.)
- **Variant B (chain reversal):** Entry 0017 (PostgreSQL for Tier 2/3) itself needs to be partially reversed: a new lightweight PostgreSQL-compatible engine (DuckDB) makes the testcontainers overhead unnecessary for Tier 2. Write entry 0022 that supersedes 0017 for Tier 2 only while keeping Tier 3 on PostgreSQL. The challenge is superseding one tier of a multi-tier decision without ambiguity.
