---
atom_id: SC-B-007
name: Worktree-per-agent for safe parallel execution
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory, intervene-how]
applications: [COC]
spec_lineage:
  - CO §71 — COC Observation-Instinct-Evolution Pipeline
journal_evidence:
  - path: loom/kailash-py/workspaces/gh-issues-consolidation/journal/0008-DISCOVERY-session1-parallel-execution.md
    polarity: POSITIVE
    quote: "Isolated git worktrees prevent file conflicts between parallel agents. Each agent:\n1. Gets its own branch and file copy\n2. Can run tests independently\n3. Commits without interfering with other agents"
  - path: loom/journal/0025-DECISION-parallel-backlog-resolution.md
    polarity: POSITIVE
    quote: "Used a team of 5+ parallel agents to resolve ALL outstanding backlog items from sessions 0019-0024 in a single session, rather than addressing items sequentially across multiple sessions."
  - path: loom/kailash-py/workspaces/kailash/journal/0018-DECISION-five-workspace-parallel-implementation.md
    polarity: POSITIVE
    quote: "Implemented 5 new workspaces in a single autonomous session: dataflow-enhancements (8 features), nexus-transport-refactor (Transport ABC), mcp-platform-server (unified FastMCP), kailash-ml (9-engine ML framework), kailash-align (LLM alignment framework)."
practice_modality: build
status: verified
---

## The move

Before launching parallel agents that will edit code in the same repository, create one git worktree per agent. Each worktree gets its own branch, its own file copy, and its own test-run environment. Agents commit to their branch without interfering with each other. Merge happens after all agents complete, with conflict resolution as a separate human-gated step. Never allow two agents to edit the same working directory concurrently.

## When it fires

Whenever the autonomous execution model calls for parallel work on the same codebase — parallel issue resolution, parallel feature implementation, parallel workspace execution. The trigger is "more than one agent needs to write files in the same repo at the same time." If agents are read-only (analysis, inspection), worktree isolation is unnecessary.

## What it serves

The autonomous execution model (`rules/autonomous-execution.md`) mandates maximum parallelization to achieve the 10x throughput multiplier. But parallel writes to a single working directory cause file conflicts, partial commits, and test interference. Worktree-per-agent is the mechanism that makes the parallelization mandate safe. CO §71 (COC Observation-Instinct-Evolution Pipeline) names the observation-capture layer that records workflow patterns; worktree-per-agent is itself a pattern that emerged from observation of early parallel-execution failures and was codified into practice.

## Evidence walkthrough

- **kailash-py gh-issues 0008** (POSITIVE) — Documents the first successful parallel execution session. Three of five PRs completed simultaneously in isolated worktrees (PACT (primitives) security, governance vacancy, fabric bugs), producing 6,228 passing tests and 36 new tests with zero regressions. The entry names the key practical lesson: agents need PYTHONPATH overrides to use worktree files instead of the editable-installed main repo.
- **loom-meta 0025** (POSITIVE) — A team of 5+ parallel agents cleared the entire sync backlog (Gate 1 rs classification, co-template drift, 20 downstream repos, GH issues) in a single session. The entry explicitly rejects sequential execution as inconsistent with the autonomous execution model and notes that human classification was delegated to agents.
- **kailash-py 0018** (POSITIVE) — The most ambitious parallel session: five workspaces (dataflow-enhancements, nexus-transport-refactor, mcp-platform-server, kailash-ml, kailash-align) analyzed, planned, implemented, and red-teamed in a single session. Produced approximately 80 new source files and 730 new tests. Cross-workspace dependencies were mapped in Phase 0 to avoid ordering conflicts.

## How to drill it

Set up a repository with three open issues that touch overlapping files. Attempt to resolve all three sequentially in one session and time it. Then create three worktrees (`git worktree add ../repo-agent-1 -b fix/issue-1`), resolve each issue in its own worktree in parallel, and merge. The drill succeeds when the practitioner (a) creates worktrees before starting parallel work, (b) runs tests in each worktree independently, (c) handles merge conflicts at the end rather than mid-work, and (d) sets up the environment correctly (PYTHONPATH, virtualenv) per worktree.

## Common failure mode

Practitioner launches parallel agents in the same working directory, reasoning that "the files they touch don't overlap." This works until two agents both modify a shared file (e.g., `__init__.py`, `conftest.py`, a shared fixture). The resulting partial writes corrupt the file and both agents' test runs fail with unrelated errors. The fix is unconditional isolation: worktrees for all parallel agents, even when the files seem disjoint.

## Related atoms

- SC-B-004 (specialist delegation) — specialists often run in parallel, making worktree isolation a prerequisite for specialist delegation at scale.
- SC-B-006 (multi-round red team) — red team rounds can be parallelized across angles if each angle runs in its own worktree.
- SC-B-013 (cross-workspace dependency mapping) — the dependency map determines which worktrees can run concurrently; worktree isolation is the mechanism, dependency mapping is the governance.
- SC-B-017 (verification-gradient zone assignment) — worktree isolation is the mechanical prerequisite for parallel agents; zone assignment is the governance prerequisite that determines each agent's authority.
