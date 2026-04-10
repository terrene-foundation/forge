---
drill_id: DR-SC-A-013-FULL
source_atom: SC-A-013
difficulty: advanced
time_box_minutes: 45
modality: build
application: [COC]
---

# Design Agent Specialisation Routing as Explicit Layer 1 Intent

## Setup

The practitioner receives a project directory for a supply-chain management platform ("Meridian") built on Kailash SDK. The project has four agents already defined as `.claude/agents/` markdown files, but no routing table and no routing rules. The agent descriptions are intentionally underspecified — they name the agent's domain but do not explicitly claim or disclaim specific request types.

**File: `.claude/agents/code-agent.md`**

```markdown
---
name: code-agent
description: Writes and modifies application source code for the Meridian platform.
---

# Code Agent

Specialist for implementing features, fixing bugs, and refactoring code in the
Meridian codebase. Works with Kailash Core SDK workflows, DataFlow models, and
Nexus endpoints. Follows all project rules (security, testing, zero-tolerance).
```

**File: `.claude/agents/test-agent.md`**

```markdown
---
name: test-agent
description: Writes and maintains the test suite for the Meridian platform.
---

# Test Agent

Specialist for creating unit tests, integration tests, and E2E tests. Maintains
test fixtures, manages test database setup/teardown, and ensures coverage targets
are met. Uses the 3-tier testing methodology.
```

**File: `.claude/agents/review-agent.md`**

```markdown
---
name: review-agent
description: Reviews code changes for quality, security, and compliance.
---

# Review Agent

Specialist for code review. Checks for security vulnerabilities, rule compliance,
naming conventions, and architectural consistency. Produces review reports with
findings categorised as CRITICAL, WARNING, or INFO.
```

**File: `.claude/agents/deploy-agent.md`**

```markdown
---
name: deploy-agent
description: Manages deployment, release, and infrastructure for the Meridian platform.
---

# Deploy Agent

Specialist for deploying the Meridian platform. Handles Docker image builds,
database migrations, environment configuration, release tagging, and health
check verification.
```

**The 10 request types the system must handle:**

| #   | Request                                                                           |
| --- | --------------------------------------------------------------------------------- |
| 1   | "Write a function that calculates shipping costs based on weight and destination" |
| 2   | "Run the test suite and report any failures"                                      |
| 3   | "Review this PR for security issues"                                              |
| 4   | "Deploy the latest changes to staging"                                            |
| 5   | "Add the `pydantic` dependency to the project"                                    |
| 6   | "Fix the failing test in `test_shipping.py`"                                      |
| 7   | "Refactor the order processing module to use async workflows"                     |
| 8   | "Update the API documentation for the /shipments endpoint"                        |
| 9   | "Triage this bug report: orders over $10K are silently dropped"                   |
| 10  | "Create a release for version 2.1.0"                                              |

## Task

1. **Build the routing table.** For each of the 10 request types, assign exactly one agent. Write the table as a structured artifact with columns: Request, Assigned Agent, Rationale (one sentence explaining why this agent and not another).

2. **Identify ambiguous cases.** At least two of the 10 requests do not cleanly map to a single agent. For each ambiguous case: (a) name which agents could plausibly handle it, (b) state the routing conflict, (c) resolve it by either splitting the request into sub-tasks routed to different agents, or by assigning ownership to one agent with a documented justification. Write the resolution as a routing rule, not a prose explanation.

3. **Author the routing artifact.** Write a `routing.md` file for `.claude/rules/` that encodes the routing table as a machine-readable directive. The file must:
   - List every request type with its assigned agent
   - Include a "default route" for request types not in the table
   - Include an "ambiguity protocol" — what happens when a request matches two agents
   - Be loadable by the rules system (always-in-context) so the orchestrator consults it every turn

4. **Update the agent descriptions.** For each of the four agents, add a `routes:` field to the frontmatter listing the request types that agent owns. After the update, the routing is encoded in two places (agent descriptions + routing rule) as defense in depth.

5. **Validate uniqueness.** Verify that no request type appears in two agents' `routes:` fields. If it does, the routing is ambiguous and the artifact is not complete.

## Model Answer

**1. Routing table:**

| #   | Request                      | Agent                      | Rationale                                                                                                                                    |
| --- | ---------------------------- | -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Write shipping cost function | code-agent                 | Pure implementation work — writing new application logic                                                                                     |
| 2   | Run test suite               | test-agent                 | Test execution and failure reporting is core test-agent responsibility                                                                       |
| 3   | Review PR for security       | review-agent               | Security review is explicitly in review-agent's description                                                                                  |
| 4   | Deploy to staging            | deploy-agent               | Deployment is deploy-agent's primary function                                                                                                |
| 5   | Add dependency               | code-agent                 | Dependency management modifies the project's source configuration (`pyproject.toml`), which is application source code — code-agent's domain |
| 6   | Fix failing test             | **AMBIGUOUS** — see step 2 | Could be code-agent (the fix is in application code) or test-agent (the symptom is a test failure)                                           |
| 7   | Refactor to async            | code-agent                 | Refactoring application code is code-agent's domain, even though the refactoring may require test updates                                    |
| 8   | Update API documentation     | **AMBIGUOUS** — see step 2 | Could be code-agent (docs live alongside code) or review-agent (documentation quality is a review concern)                                   |
| 9   | Triage bug report            | **AMBIGUOUS** — see step 2 | Could be code-agent (the fix requires code changes) or review-agent (triage is an analysis/assessment task)                                  |
| 10  | Create release               | deploy-agent               | Release creation (tagging, changelog, artifact publishing) is deploy-agent's domain                                                          |

**2. Ambiguous case resolution:**

**Request 6 — "Fix the failing test":**

- Conflict: test-agent owns test execution and knows the test framework; code-agent owns the application code where the bug likely lives.
- Resolution: **Route to code-agent.** The failing test is a symptom; the root cause is a bug in application code. The test itself is probably correct — it is detecting a real regression. If the test is flawed (wrong assertion, outdated fixture), code-agent discovers this during the fix and corrects both the code and the test in a single atomic commit. Routing to test-agent would create a handoff: test-agent diagnoses, then delegates the code fix to code-agent, adding a round-trip.
- Routing rule: `"fix a failing test" → code-agent (owns the code under test; may update the test as part of the fix)`

**Request 8 — "Update API documentation":**

- Conflict: code-agent writes code and could update inline docstrings and OpenAPI specs; review-agent assesses documentation quality.
- Resolution: **Route to code-agent.** API documentation updates typically require modifying docstrings, OpenAPI schema annotations, or README sections that live alongside the source code. The documentation update is a code change. Review-agent reviews the result, but does not author it.
- Routing rule: `"update documentation" → code-agent (authors docs as code changes); review-agent reviews the result at the PR gate`

**Request 9 — "Triage this bug report":**

- Conflict: review-agent performs analysis; code-agent performs fixes.
- Resolution: **Split the request.** Triage has two phases: (a) reproduce and diagnose (analysis), (b) fix (implementation). Route phase (a) to review-agent, which produces a diagnosis with severity, root cause, and suggested fix. Route phase (b) to code-agent, which implements the fix. The split is encoded as a two-step workflow, not a single routing entry.
- Routing rule: `"triage a bug" → review-agent (diagnose) then code-agent (fix). Review-agent produces a diagnosis artifact; code-agent consumes it.`

**3. Routing artifact (`rules/routing.md`):**

```markdown
# Agent Routing Rules

## Routing Table

| Request Type                   | Assigned Agent | Notes                                         |
| ------------------------------ | -------------- | --------------------------------------------- |
| Write/implement a feature      | code-agent     |                                               |
| Fix a bug in application code  | code-agent     |                                               |
| Fix a failing test             | code-agent     | Owns code under test; updates test if needed  |
| Add/remove/update a dependency | code-agent     |                                               |
| Refactor a module              | code-agent     |                                               |
| Update documentation           | code-agent     | Authors docs; review-agent reviews at PR gate |
| Run the test suite             | test-agent     |                                               |
| Create/update test fixtures    | test-agent     |                                               |
| Review a PR or code change     | review-agent   |                                               |
| Security audit                 | review-agent   |                                               |
| Triage a bug report (diagnose) | review-agent   | Produces diagnosis; code-agent implements fix |
| Deploy to an environment       | deploy-agent   |                                               |
| Create a release               | deploy-agent   |                                               |
| Run database migrations        | deploy-agent   |                                               |

## Default Route

Any request type not listed above routes to **code-agent**. Code-agent is the
broadest specialist and can delegate to others if the request falls outside
its domain. An unrouted request is a signal that this table needs a new entry.

## Ambiguity Protocol

When a request appears to match two agents:

1. Check this table first — if the request type is listed, follow the table
2. If not listed, determine whether the request is primarily **authoring**
   (code-agent), **evaluating** (review-agent), **testing** (test-agent), or
   **deploying** (deploy-agent)
3. If still ambiguous, split the request into sub-tasks and route each sub-task
   independently
4. Never dispatch the same request to two agents in parallel — one agent owns
   it, the other reviews the output at the gate
```

**4. Updated agent frontmatter (code-agent example):**

```markdown
---
name: code-agent
description: Writes and modifies application source code for the Meridian platform.
routes:
  - write/implement a feature
  - fix a bug in application code
  - fix a failing test
  - add/remove/update a dependency
  - refactor a module
  - update documentation
---
```

Equivalent `routes:` fields added to test-agent (run test suite, create/update test fixtures), review-agent (review a PR, security audit, triage a bug report — diagnose phase), and deploy-agent (deploy to environment, create a release, run database migrations).

**5. Uniqueness verification:**

Cross-checking all four agents' `routes:` fields: no request type appears in more than one agent's list. "Triage a bug report" is split across review-agent (diagnose) and code-agent (fix) — these are two distinct sub-tasks with distinct routing entries, not one request mapped to two agents. The routing is unique.

## Scoring

| Criterion                                                                      | Points |
| ------------------------------------------------------------------------------ | ------ |
| All 10 request types assigned to exactly one agent (or explicitly split)       | 2      |
| At least 2 ambiguous cases identified with named conflict and resolution       | 2      |
| Routing artifact includes routing table, default route, and ambiguity protocol | 2      |
| Agent frontmatter updated with `routes:` field matching the routing table      | 1      |
| Uniqueness verified — no request type appears in two agents' routes            | 1      |
| Ambiguous case resolutions justified with reasoning (not arbitrary assignment) | 1      |
| Routing artifact is a rule file (always-loaded) not a skill or command         | 1      |
| **Total**                                                                      | **10** |

## Extensions

- **Variant A (new capability insertion):** After the practitioner completes the routing table, introduce a new requirement: "Meridian now needs an MCP integration for external inventory systems." The practitioner must decide whether to (a) add MCP work to an existing agent's routes, (b) create a fifth agent (mcp-specialist) with its own route entries, or (c) split MCP work between code-agent (implementation) and deploy-agent (server configuration). The practitioner must update the routing artifact and all affected agent descriptions. Score on whether the practitioner's decision is justified by the routing principles (one owner per request type, no ambiguous dispatch) and whether the routing table remains consistent after the insertion.
- **Variant B (runtime routing validation):** The practitioner writes a validation script that reads all agent files' `routes:` fields and the `rules/routing.md` table, then checks three invariants: (a) every route in the agent frontmatter appears in the routing table (no orphan routes), (b) every routing table entry maps to an agent that exists in `.claude/agents/` (no phantom agents), (c) no route appears in two agents' frontmatter (uniqueness). The script outputs a pass/fail report. This tests whether the practitioner understands that routing is a structural invariant that can and should be validated deterministically, not just reviewed by a human.
