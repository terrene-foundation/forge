---
drill_id: DR-SC-B-010-FULL
source_atom: SC-B-010
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Cross-Repo Divergence Audit with Numbered Drift-Gap List

## Setup

The practitioner manages a two-repo distribution chain: **upstream U** (the COC template source) and **downstream D** (a project repo that received its `.claude/` artifacts via `/sync` three weeks ago). Both repos have a `sync-manifest.yaml` that declares 20 files in scope.

Since the last sync, both repos have evolved independently. The practitioner's task is to audit D against U before running the next `/sync` pass.

**Upstream U manifest and filesystem** (authoritative — 20 files declared, 20 files present):

```
.claude/
  rules/
    zero-tolerance.md          (v2 — updated last week)
    security.md                (v1 — unchanged)
    git.md                     (v1 — unchanged)
    communication.md           (v1 — unchanged)
    testing.md                 (v2 — updated last week)
    agents.md                  (v1 — unchanged)
    autonomous-execution.md    (v1 — unchanged)
    cc-artifacts.md            (v1 — unchanged)
  skills/
    decide-framework/          (v1 — unchanged)
    workflow-builder/          (v1 — unchanged)
    code-review/               (v1 — unchanged)
  agents/
    analyst.md                 (v1 — unchanged)
    reviewer.md                (v1 — unchanged)
    security-reviewer.md       (v1 — unchanged)
  variants/py/
    rules/
      python-environment.md    (v1 — unchanged)
      connection-pool.md       (v1 — unchanged)
    skills/
      dataflow-patterns/       (v1 — unchanged)
      nexus-deploy/            (v2 — updated last week)
```

**Downstream D filesystem** (what actually exists on disk — 21 files):

```
.claude/
  rules/
    zero-tolerance.md          (v1 — NOT updated, still has old content)
    security.md                (v1 — matches U)
    git.md                     (v1 — matches U)
    communication.md           (v1 — matches U)
    testing.md                 (v1 — NOT updated, still has old content)
    agents.md                  (v1 — matches U)
    autonomous-execution.md    (v1 — matches U)
    cc-artifacts.md            (v1 — matches U)
  skills/
    decide-framework/          (v1 — matches U)
    workflow-builder/          (v1 — matches U)
    code-review/               (v1 — matches U)
  agents/
    analyst.md                 (v1 — matches U)
    reviewer.md                (v1 — matches U)
    security-reviewer.md       (v1 — matches U)
  variants/py/
    rules/
      python-environment.md    (v1 — matches U)
      connection-pool.md       (v1 — matches U)
    skills/
      dataflow-patterns/       (v1 — matches U)
      nexus-deploy/            (v1 — NOT updated, still has old content)
      pact-envelope-helper/    ← EXISTS ON DISK, NOT IN MANIFEST
```

**Downstream D manifest** (what D's `sync-manifest.yaml` declares — 20 entries):

The manifest matches U's manifest exactly (20 entries) — but the filesystem has 21 items. The orphan `pact-envelope-helper/` was created locally by a developer during a PACT integration session and never registered in the manifest.

**Additional planted issue:** U's manifest declares `skills/code-review/` as in scope, and both U and D have the directory on disk. However, D's `code-review/` directory was manually modified by a developer who added a project-specific checklist file inside it. The contents differ from U, but the difference is an addition (new file inside the skill directory), not a modification of existing files.

**Hidden context (not given to practitioner):** The v1→v2 updates on U for `zero-tolerance.md`, `testing.md`, and `nexus-deploy/` happened during a `/codify` session that tightened the rigor bar. D has never re-synced.

## Task

1. Produce a numbered drift-gap list for all 20 manifest entries plus any orphans. For each item, provide columns: **number**, **path**, **classification** (one of: aligned / drift / variant / orphan / stale), and a **one-line description** of the divergence or alignment.

2. Identify which items are safe to sync immediately, which require human review before sync, and which should be left alone. Justify each group in one sentence.

3. For the two unintentional drifts (`zero-tolerance.md` and `testing.md`), explain what would happen if the practitioner runs `/sync` without auditing first. Would the sync command catch these drifts? Would it silently overwrite D's versions?

4. The orphan `pact-envelope-helper/` was created by a developer who says "it works, just add it to the manifest." What is the correct brokerage response?

5. The `code-review/` skill directory has an added file in D that does not exist in U. Classify this divergence and explain whether it survives a sync.

## Model Answer

**1. Numbered drift-gap list:**

| #   | Path                                     | Classification | Description                                                            |
| --- | ---------------------------------------- | -------------- | ---------------------------------------------------------------------- |
| 1   | rules/zero-tolerance.md                  | drift          | D has v1, U has v2 — unintentional; D missed the rigor-bar tightening  |
| 2   | rules/security.md                        | aligned        | Content matches across U and D                                         |
| 3   | rules/git.md                             | aligned        | Content matches                                                        |
| 4   | rules/communication.md                   | aligned        | Content matches                                                        |
| 5   | rules/testing.md                         | drift          | D has v1, U has v2 — unintentional; D missed the testing rule update   |
| 6   | rules/agents.md                          | aligned        | Content matches                                                        |
| 7   | rules/autonomous-execution.md            | aligned        | Content matches                                                        |
| 8   | rules/cc-artifacts.md                    | aligned        | Content matches                                                        |
| 9   | skills/decide-framework/                 | aligned        | Content matches                                                        |
| 10  | skills/workflow-builder/                 | aligned        | Content matches                                                        |
| 11  | skills/code-review/                      | drift          | D has an added project-specific checklist file not present in U        |
| 12  | agents/analyst.md                        | aligned        | Content matches                                                        |
| 13  | agents/reviewer.md                       | aligned        | Content matches                                                        |
| 14  | agents/security-reviewer.md              | aligned        | Content matches                                                        |
| 15  | variants/py/rules/python-environment.md  | aligned        | Content matches                                                        |
| 16  | variants/py/rules/connection-pool.md     | aligned        | Content matches                                                        |
| 17  | variants/py/skills/dataflow-patterns/    | aligned        | Content matches                                                        |
| 18  | variants/py/skills/nexus-deploy/         | drift          | D has v1, U has v2 — unintentional; D missed the nexus-deploy update   |
| 19  | variants/py/skills/pact-envelope-helper/ | orphan         | Exists on D's disk but not in any manifest — invisible to sync         |
| 20  | (all 20 manifest entries accounted for)  | —              | No stale entries found — all manifest entries have corresponding files |

**Summary:** 13 aligned, 4 drift (items 1, 5, 11, 18), 1 orphan (item 19), 0 stale, 0 intentional variant.

**2. Sync safety groups:**

- **Safe to sync immediately (items 1, 5, 18):** These are straightforward upstream-newer-than-downstream drifts with no local modifications — the sync brings D up to date with no conflict risk.
- **Requires human review before sync (item 11):** The `code-review/` directory has a local addition in D; the human must decide whether the project-specific checklist belongs in the project only, should be upstreamed to U, or should be removed.
- **Leave alone (item 19):** The orphan `pact-envelope-helper/` is outside the manifest and must be classified (global, variant, or project-only) before any manifest or sync action.

**3. Sync without audit — consequences for items 1 and 5:**

`/sync` performs a read-then-merge: it reads U's version, reads D's version, and produces a merged result. For `zero-tolerance.md` and `testing.md`, U's v2 content is strictly newer (additions and modifications, no deletions of D-specific content). The sync would correctly identify D's content as a subset of U's and overwrite with U's version. However, the sync would NOT flag that D was behind — it would silently succeed, and the practitioner would never know these files had drifted. The audit's value is not preventing a bad merge; it is making the drift visible so the practitioner can verify that the v2 changes are understood before applying them.

**4. Correct brokerage response to the orphan:**

The correct response is: "This skill directory needs to go through `/codify` at the BUILD repo level, not be added directly to the downstream manifest. It was authored against this project's specific PACT integration context. The brokerage question is: is this a global skill (all downstream repos need it), a Python variant (only Python repos need it), or a project-only artifact (only this repo needs it)? That classification determines where it lives. Adding it to D's manifest makes it invisible to every other downstream repo; adding it to U's manifest without `/codify` bypasses the variant classification gate."

The practitioner must NOT simply add it to the manifest as requested. The developer's request skips the classification step that determines whether the artifact is global, variant, or project-only.

**5. The code-review/ addition:**

The added checklist file inside `code-review/` is classified as **drift** — specifically, a local addition within a synced directory. Whether it survives depends on the sync mechanism's merge strategy. A read-then-merge sync (SC-B-001) would preserve the addition if it only adds a new file and does not modify existing ones. However, a full-directory replacement sync would delete it. The practitioner should flag this to the human: the project-specific checklist should either be moved outside the synced directory (into a project-only location) or upstreamed to U via `/codify`. Leaving it inside a synced directory makes its survival dependent on the sync implementation, which is fragile.

## Scoring

| Criterion                                                                         | Points |
| --------------------------------------------------------------------------------- | ------ |
| All 20 manifest entries appear in the drift-gap list with correct classifications | 2      |
| Orphan correctly identified and classified as orphan (not drift or variant)       | 1      |
| All four drifts correctly identified (items 1, 5, 11, 18)                         | 2      |
| Sync safety grouping separates immediate/review/leave-alone with justification    | 2      |
| Sync-without-audit answer explains silent success (not failure) as the risk       | 1      |
| Orphan response invokes classification gate rather than "just add it"             | 1      |
| Code-review addition correctly assessed for sync survival                         | 1      |
| **Total**                                                                         | **10** |

## Extensions

- **Variant A (harder):** Add a sixth divergence type: D has a file that is in U's manifest but whose content was intentionally modified by the project owner (a local override with a comment `# PROJECT OVERRIDE — do not sync`). How should the drift-gap list classify this? Should the audit tool have a mechanism for declared overrides?
- **Variant B (scale):** Expand to a three-repo chain (U → template T → downstream D). U updates `zero-tolerance.md` to v2, but T still has v1. D syncs from T and gets v1. Produce drift-gap lists for both the U-T pair and the T-D pair. How does the version tracking cascade (SC-B-011) interact with the drift audit?
