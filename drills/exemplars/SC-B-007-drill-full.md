---
drill_id: DR-SC-B-007-FULL
source_atom: SC-B-007
difficulty: advanced
time_box_minutes: 45
modality: build
application: [COC]
---

# Worktree-per-Agent Parallel Execution

## Setup

The practitioner receives a git repository (`acme-platform`) with the following structure:

```
acme-platform/
├── src/
│   ├── __init__.py
│   ├── auth/
│   │   ├── __init__.py
│   │   ├── middleware.py      # 180 lines — session validation, CORS, rate limiting
│   │   └── providers.py       # 90 lines — OAuth, API key providers
│   ├── billing/
│   │   ├── __init__.py
│   │   ├── invoices.py        # 120 lines — invoice generation
│   │   └── subscriptions.py   # 150 lines — plan management
│   └── shared/
│       ├── __init__.py        # re-exports from config.py and logging.py
│       ├── config.py          # 40 lines — env-based configuration
│       └── logging.py         # 30 lines — structured logger setup
├── tests/
│   ├── conftest.py            # 60 lines — shared fixtures (db, client, auth headers)
│   ├── test_auth.py
│   └── test_billing.py
├── pyproject.toml
└── .git/
```

Three open issues need resolution:

**Issue #41** — `auth/middleware.py`: Rate limiter uses a global dict that leaks memory. Fix: replace with a TTL cache. Touches `middleware.py` and `test_auth.py`.

**Issue #52** — `billing/subscriptions.py`: Plan upgrade doesn't prorate. Fix: add proration logic. Touches `subscriptions.py`, `test_billing.py`, and `shared/__init__.py` (new `calculate_proration` re-export).

**Issue #67** — `auth/providers.py` + `billing/invoices.py`: API key provider doesn't propagate tenant ID to invoice headers. Fix: thread tenant context through both modules. Touches `providers.py`, `invoices.py`, `conftest.py` (new tenant fixture), `test_auth.py`, and `test_billing.py`.

Note: Issues #41 and #67 both touch `test_auth.py`. Issues #52 and #67 both touch `test_billing.py` and `shared/__init__.py` or `conftest.py`. No pair of issues is file-disjoint.

## Task

**Part 1 — Sequential baseline (10 minutes):**
Sketch the sequential resolution plan: which issue do you start with, in what order, and why? Estimate wall-clock time if each issue takes ~15 minutes of agent execution. Note every point where one issue's changes would block or conflict with another's in-progress work.

**Part 2 — Worktree setup (15 minutes):**
Write the exact shell commands to:

1. Create three worktrees, one per issue, each on its own branch.
2. Set up the Python environment in each worktree so that `import acme_platform` resolves to that worktree's source, not the main repo's editable install.
3. Verify isolation by running `python -c "import acme_platform; print(acme_platform.__file__)"` in each worktree and confirming distinct paths.

**Part 3 — Conflict resolution plan (10 minutes):**
After all three agents complete, you will merge all three branches into `main`. Predict which files will produce merge conflicts. For each predicted conflict, write a one-sentence resolution strategy.

**Part 4 — Merge execution (10 minutes):**
Write the merge sequence (which branch merges first, second, third) and the commands. Explain why the merge order matters and what ordering minimises conflict resolution effort.

## Model Answer

**Part 1 — Sequential baseline:**

Sequential order: #41 (rate limiter, self-contained) then #52 (proration, no auth dependency) then #67 (tenant threading, touches both domains). Estimated wall-clock: 3 x 15 = **45 minutes sequential**. Conflict points in sequential mode: #67 must resolve after #41 because both modify `test_auth.py` — if #67 runs before #41's changes land, its `test_auth.py` edits are against the pre-fix state and will need rework when #41's TTL cache changes the test fixture setup. Similarly, #67 and #52 both touch `conftest.py`/`shared/__init__.py`.

**Part 2 — Worktree setup:**

```bash
# Create worktrees — each gets its own branch and working copy
git worktree add ../acme-agent-41 -b fix/issue-41
git worktree add ../acme-agent-52 -b fix/issue-52
git worktree add ../acme-agent-67 -b fix/issue-67

# Set up isolated Python environments per worktree
cd ../acme-agent-41
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
# CRITICAL: override PYTHONPATH so imports resolve to THIS worktree
export PYTHONPATH="$(pwd)/src:$PYTHONPATH"
python -c "import acme_platform; print(acme_platform.__file__)"
# Expected: /path/to/acme-agent-41/src/acme_platform/__init__.py

# Repeat for agent-52 and agent-67
cd ../acme-agent-52
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
export PYTHONPATH="$(pwd)/src:$PYTHONPATH"

cd ../acme-agent-67
python -m venv .venv
source .venv/bin/activate
pip install -e ".[dev]"
export PYTHONPATH="$(pwd)/src:$PYTHONPATH"
```

The PYTHONPATH override is essential. Without it, `pip install -e .` links the editable install to whichever worktree ran it last, and all three agents resolve imports to the same copy — defeating isolation. The journal evidence (kailash-py gh-issues 0008) documents this exact lesson: agents need PYTHONPATH overrides to use worktree files instead of the editable-installed main repo.

**Part 3 — Conflict resolution plan:**

| File                 | Conflicting branches                 | Resolution strategy                                                                                                                                   |
| -------------------- | ------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `test_auth.py`       | fix/issue-41 + fix/issue-67          | Both add test cases — concatenate both sets of new tests, keeping the TTL cache fixture from #41 and the tenant fixture from #67.                     |
| `test_billing.py`    | fix/issue-52 + fix/issue-67          | Both add test cases — concatenate, ensure both the proration assertions and the tenant-header assertions are present.                                 |
| `conftest.py`        | fix/issue-52 + fix/issue-67          | Both add new fixtures — merge both fixtures into the file; no logical overlap since one adds `proration_fixture` and the other adds `tenant_fixture`. |
| `shared/__init__.py` | fix/issue-52 (possibly fix/issue-67) | Accept both re-exports — simple line addition.                                                                                                        |

Files that will NOT conflict: `middleware.py` (#41 only), `subscriptions.py` (#52 only), `providers.py` (#67 only), `invoices.py` (#67 only).

**Part 4 — Merge execution:**

```bash
# Return to main worktree
cd /path/to/acme-platform

# Merge the most isolated branch first (fewest file overlaps)
git merge fix/issue-41      # Rate limiter — touches middleware.py, test_auth.py
# No conflicts expected (first merge)

# Merge the next branch
git merge fix/issue-52       # Proration — touches subscriptions.py, test_billing.py, conftest.py, shared/__init__.py
# No conflicts expected (no file overlap with #41)

# Merge the cross-cutting branch last
git merge fix/issue-67       # Tenant threading — touches providers.py, invoices.py, conftest.py, test_auth.py, test_billing.py
# CONFLICTS EXPECTED in: test_auth.py, test_billing.py, conftest.py
# Resolve each by accepting both sides' additions

# After resolution
git add tests/conftest.py tests/test_auth.py tests/test_billing.py
git commit -m "fix: merge issue-67 tenant threading with resolved test conflicts"

# Run full test suite to verify
pytest tests/ -v

# Clean up worktrees
git worktree remove ../acme-agent-41
git worktree remove ../acme-agent-52
git worktree remove ../acme-agent-67
```

Merge order rationale: the cross-cutting branch (#67) goes last because it shares files with both other branches. Merging it first would force two subsequent merges to resolve against #67's changes. Merging it last concentrates all conflict resolution into a single step, where the practitioner has both #41's and #52's final state visible. The journal evidence (loom-meta 0025) records the same principle: the most cross-cutting work merges last so that conflict resolution is a single, focused pass rather than distributed across multiple merges.

Wall-clock for parallel execution: ~15 minutes (all three agents run simultaneously) + ~10 minutes merge = **25 minutes total** vs **45 minutes sequential** — a 1.8x speedup on a trivial example, scaling to the 3-5x multiplier documented in the autonomous execution model as the number of parallel agents increases.

## Scoring

| Criterion                                                                                  | Points |
| ------------------------------------------------------------------------------------------ | ------ |
| Sequential baseline correctly identifies file overlaps between all three issue pairs       | 1      |
| Worktree creation commands are correct (`git worktree add` with `-b` flag, distinct paths) | 2      |
| PYTHONPATH override present and justified (not just `pip install -e .` alone)              | 2      |
| Isolation verification step included (import path check per worktree)                      | 1      |
| Conflict prediction identifies the correct files (test_auth, test_billing, conftest)       | 1      |
| Merge ordering places the cross-cutting branch last with stated rationale                  | 2      |
| Worktree cleanup commands present (`git worktree remove`)                                  | 1      |
| **Total**                                                                                  | **10** |

## Extensions

- **Variant A (scale):** Increase to 5 issues and 5 worktrees, where two pairs share the same `conftest.py` fixture setup. Design a worktree dependency graph: which worktrees can run fully in parallel, and which must be sequenced because their shared fixture changes are semantically dependent (not just textually conflicting)?
- **Variant B (failure recovery):** Agent-52 encounters a test failure mid-execution and needs to inspect Agent-41's branch for a related fixture change. Write the commands to read files from another worktree's branch without switching the current worktree's state (`git show fix/issue-41:tests/conftest.py`). Explain why checking out the other branch in the current worktree would break isolation.
- **Variant C (CI integration):** The CI pipeline runs on push to any `fix/*` branch. All three agents push within seconds of each other. Design a CI configuration that runs each branch's tests in isolation (using the worktree's own environment) rather than against a shared checkout, preventing the "last push wins" race condition.
