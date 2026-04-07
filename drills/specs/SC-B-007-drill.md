---
drill_id: DR-SC-B-007
source_atom: SC-B-007
name: "Worktree-per-agent for safe parallel execution — drill"
craft_area: institutional-memory, intervene-how
practice_modality: build
difficulty: advanced
time_box_minutes: 45
---

## How to drill it

Set up a repository with three open issues that touch overlapping files. Attempt to resolve all three sequentially in one session and time it. Then create three worktrees (`git worktree add ../repo-agent-1 -b fix/issue-1`), resolve each issue in its own worktree in parallel, and merge. The drill succeeds when the practitioner (a) creates worktrees before starting parallel work, (b) runs tests in each worktree independently, (c) handles merge conflicts at the end rather than mid-work, and (d) sets up the environment correctly (PYTHONPATH, virtualenv) per worktree.
