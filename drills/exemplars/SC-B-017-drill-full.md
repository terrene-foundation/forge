---
drill_id: DR-SC-B-017-FULL
source_atom: SC-B-017
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC, COR, COE]
---

# Verification Gradient Zone Assignment

## Setup

The practitioner manages a CO workspace with **five parallel agents** executing against the same codebase. Each agent has a different task profile:

| Agent       | Task                                                                               | Reversibility                                           | Blast Radius                                     |
| ----------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------ |
| **Agent A** | Update inline code comments and docstrings across 12 files                         | Fully reversible (git revert)                           | Low (no runtime impact)                          |
| **Agent B** | Refactor authentication middleware — extract shared logic into a reusable module   | Partially reversible (refactor may break callers)       | High (auth affects all protected routes)         |
| **Agent C** | Add 15 new integration tests for the PACT governance module                        | Fully reversible                                        | Low (tests don't affect production code)         |
| **Agent D** | Modify database migration to add a NOT NULL column with backfill to a 2M-row table | Irreversible in production (data migration)             | High (schema change affects all queries)         |
| **Agent E** | Update CI pipeline configuration to add a new deployment stage                     | Partially reversible (CI changes propagate immediately) | Medium (affects deployment but not runtime code) |

The PACT verification gradient defines **four zones**:

| Zone              | Meaning                                                                | Human involvement                       |
| ----------------- | ---------------------------------------------------------------------- | --------------------------------------- |
| **Auto-approved** | Agent proceeds without notification                                    | None until periodic review              |
| **Flagged**       | Agent proceeds; human reviews in batch after completion                | Async review                            |
| **Held**          | Agent prepares change but does NOT commit; human reviews before commit | Sync review before action               |
| **Blocked**       | Agent must not attempt the task; escalate to human                     | Human performs or explicitly authorises |

## Task

1. Assign each agent (A-E) to exactly one verification zone. Justify each assignment in one sentence referencing the reversibility and blast-radius dimensions.

2. For Agent B and Agent D (the two highest-risk agents), specify what the human review checkpoint should examine — not just "review the code" but the specific verification the human should perform.

3. The workspace supervisor has standing trust level "Shared Planning" (EATP posture where human and agent co-plan but agent executes). Does this trust level change any of your zone assignments? If so, which and why?

4. Agent C finishes its 15 tests and 3 of them fail. Under your zone assignment, what happens next? Does the zone change?

## Model Answer

**1. Zone assignments:**

| Agent | Zone              | Justification                                                                                                                                                                                             |
| ----- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A     | **Auto-approved** | Documentation changes are fully reversible with zero runtime impact — the lowest possible risk profile.                                                                                                   |
| B     | **Held**          | Auth middleware refactoring is partially reversible and high blast radius — every protected route depends on it; human must verify before commit.                                                         |
| C     | **Flagged**       | New tests are fully reversible and low blast radius, but test failures could indicate misunderstanding of the module being tested; batch review is sufficient.                                            |
| D     | **Blocked**       | Production database migration is irreversible with high blast radius — schema changes affecting 2M rows require human to review migration SQL, backfill strategy, and rollback plan before any execution. |
| E     | **Held**          | CI pipeline changes propagate immediately and are partially reversible; human should verify the deployment stage configuration before it affects the release process.                                     |

**2. Human review checkpoints:**

- **Agent B (held):** Human verifies (a) all callers of the old auth middleware still compile and pass tests after refactor, (b) no authentication bypass is introduced by the extraction (negative test: unauthenticated request to a protected route still returns 401/403), (c) the reusable module's interface does not expose internal session state.

- **Agent D (blocked):** Human reviews (a) the migration SQL including the backfill default value, (b) estimated lock duration on the 2M-row table, (c) existence of a tested rollback migration, (d) whether the NOT NULL constraint should be deferred (add column nullable, backfill, then add constraint) to avoid long table locks.

**3. Trust level impact:**

"Shared Planning" means the agent can co-plan and execute within the agreed plan. This does NOT change the zone assignments for B and D because zone assignment is about the action's inherent risk, not the agent's trustworthiness. An agent at "Delegated" trust might move Agent E from "held" to "flagged" (CI changes within its standing authority), but B and D stay at their zones regardless of trust level — the reversibility and blast radius dimensions are properties of the task, not the agent.

**4. Test failures under flagged zone:**

Under the "flagged" zone, Agent C has already committed the failing tests. The 3 failures appear in the human's batch review. The human evaluates: are the failures due to (a) bugs in the tests (agent error — fix tests), (b) bugs in the PACT module the tests discovered (real findings — escalate), or (c) environmental issues (flaky — investigate). The zone does NOT automatically change — the human's batch review is the designed checkpoint. However, if the failures reveal that Agent C misunderstood the PACT module's contract, the human might reassign Agent C to "held" for future test-writing tasks against that module.

## Scoring

| Criterion                                                                       | Points |
| ------------------------------------------------------------------------------- | ------ |
| Agents A and C correctly in lowest zones (auto-approved / flagged)              | 2      |
| Agent D correctly blocked (irreversible + high blast radius)                    | 2      |
| Agents B and E correctly held (partially reversible + medium-high blast radius) | 2      |
| Review checkpoints are specific (not just "review the code")                    | 2      |
| Trust level answer distinguishes task risk from agent trust                     | 1      |
| Test failure scenario correctly handled within zone semantics                   | 1      |
| **Total**                                                                       | **10** |

## Extensions

- **Variant A (harder):** Agent A discovers a TODO comment containing a hardcoded API key while updating docstrings. Under auto-approved zone, the agent would commit the file with the key still present. Redesign the zone assignment to prevent this — does Agent A need a different zone, or does the zone system need a content-based override?
- **Variant B (COE application):** Reframe for an education context: five teaching assistants grading student work with different risk profiles (formative quiz, midterm exam, final grade submission, academic integrity case, course evaluation). Assign zones.
