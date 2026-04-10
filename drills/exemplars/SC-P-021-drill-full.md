---
drill_id: DR-SC-P-021-FULL
source_atom: SC-P-021
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Dangling Reference Grep After Extraction

## Setup

The practitioner receives a small COC repository with the following structure:

```
.claude/
  agents/
    orchestrator.md
    implementer.md
    reviewer.md
  skills/
    05-dataflow/
      SKILL.md
    08-deployment/
      SKILL.md
    12-testing/
      SKILL.md
    15-security/
      SKILL.md
    20-mcp-integration/
      SKILL.md
  rules/
    quality.md
    naming.md
  CLAUDE.md
```

**File contents (relevant sections):**

`CLAUDE.md`:

```markdown
## Skills Index

| Skill               | Path                       | Agent       |
| ------------------- | -------------------------- | ----------- |
| DataFlow operations | skills/05-dataflow/        | implementer |
| Deployment patterns | skills/08-deployment/      | implementer |
| Testing strategies  | skills/12-testing/         | reviewer    |
| Security audit      | skills/15-security/        | reviewer    |
| MCP integration     | skills/20-mcp-integration/ | implementer |

## Quick Reference

For deployment, see [deployment patterns](skills/08-deployment/SKILL.md).
For testing, see [testing strategies](skills/12-testing/SKILL.md).
```

`agents/orchestrator.md`:

```markdown
## Delegations

- Implementation tasks: delegate to **implementer** (skills: 05-dataflow, 08-deployment, 20-mcp-integration)
- Review tasks: delegate to **reviewer** (skills: 12-testing, 15-security)
```

`agents/implementer.md`:

```markdown
## Skills

Load these skills when activated:

- skills/05-dataflow/
- skills/08-deployment/
- skills/20-mcp-integration/

## References

See rules/quality.md for code standards.
```

`agents/reviewer.md`:

```markdown
## Skills

Load these skills when activated:

- skills/12-testing/
- skills/15-security/

## Workflow

After review, hand off deployment tasks to implementer per skills/08-deployment/.
```

`rules/quality.md`:

```markdown
## Scope

Applies to all skills in skills/05-dataflow/ and skills/20-mcp-integration/.
When testing, defer to skills/12-testing/ standards.
```

The practitioner is given two tasks to perform:

1. **Delete** the `skills/12-testing/` directory (the testing skill has been superseded by a new testing framework that lives outside this repo).
2. **Rename** `skills/08-deployment/` to `skills/08-release/` (the team decided "release" is more accurate than "deployment").

## Task

1. Perform the delete and rename operations on the filesystem.

2. Immediately after the structural changes, run a reference sweep across the entire repository. Construct a search that catches references in all formats present in this repo: prose mentions, markdown table cells, markdown links (`[text](path)`), YAML-style lists, and frontmatter references. Record every hit.

3. Update every dangling reference found by the sweep. Each reference must be either updated (for the rename) or removed (for the delete) depending on which operation produced it. Every update must be in the same commit as the delete and rename -- no separate "cleanup" commit.

4. Run the sweep again after all updates. The second sweep must return zero hits for the old names. If any remain, fix them and explain why the first sweep missed them.

## Model Answer

**Step 1 -- Perform the operations:**

Delete `skills/12-testing/` and rename `skills/08-deployment/` to `skills/08-release/`.

**Step 2 -- Reference sweep:**

The sweep needs two search terms: the deleted name (`12-testing`) and the old name from the rename (`08-deployment`). A minimal effective sweep:

```bash
grep -rn "12-testing\|08-deployment" .claude/
```

This returns the following hits:

**Hits for `12-testing` (deleted -- must remove references):**

| File                     | Line       | Content                                                      | Reference Format    |
| ------------------------ | ---------- | ------------------------------------------------------------ | ------------------- |
| `CLAUDE.md`              | table row  | `\| Testing strategies \| skills/12-testing/ \| reviewer \|` | Markdown table cell |
| `CLAUDE.md`              | link       | `[testing strategies](skills/12-testing/SKILL.md)`           | Markdown link       |
| `agents/orchestrator.md` | delegation | `skills: 12-testing, 15-security`                            | Inline list         |
| `agents/reviewer.md`     | skill load | `- skills/12-testing/`                                       | YAML-style list     |
| `rules/quality.md`       | scope      | `defer to skills/12-testing/ standards`                      | Prose mention       |

**Hits for `08-deployment` (renamed -- must update to `08-release`):**

| File                     | Line       | Content                                                              | Reference Format    |
| ------------------------ | ---------- | -------------------------------------------------------------------- | ------------------- |
| `CLAUDE.md`              | table row  | `\| Deployment patterns \| skills/08-deployment/ \| implementer \|`  | Markdown table cell |
| `CLAUDE.md`              | link       | `[deployment patterns](skills/08-deployment/SKILL.md)`               | Markdown link       |
| `agents/orchestrator.md` | delegation | `skills: 05-dataflow, 08-deployment, 20-mcp-integration`             | Inline list         |
| `agents/implementer.md`  | skill load | `- skills/08-deployment/`                                            | YAML-style list     |
| `agents/reviewer.md`     | workflow   | `hand off deployment tasks to implementer per skills/08-deployment/` | Prose mention       |

Total: 10 dangling references (5 for the delete, 5 for the rename).

**Step 3 -- Updates:**

For `12-testing` (deleted): Remove the table row from `CLAUDE.md`, remove the markdown link from the Quick Reference section, remove `12-testing` from the orchestrator's delegation list, remove the skill load line from `reviewer.md`, and remove the deferral sentence from `rules/quality.md`.

For `08-deployment` (renamed to `08-release`): Update the table row path in `CLAUDE.md`, update the markdown link target and text in `CLAUDE.md`, update the orchestrator's delegation list, update the skill load line in `implementer.md`, and update the prose reference in `reviewer.md`.

All 10 updates are staged together with the original delete and rename in a single commit. The commit message references both the delete and rename so that the reference cleanup is traceable:

```
refactor(skills): delete 12-testing, rename 08-deployment → 08-release

Remove superseded testing skill. Rename deployment to release for accuracy.
All dangling references updated in same commit.
```

**Step 4 -- Verification sweep:**

Run the same grep again:

```bash
grep -rn "12-testing\|08-deployment" .claude/
```

Zero hits confirms all references are updated.

A more thorough verification also searches for partial matches that the exact-string grep might miss:

```bash
grep -rn "12-test\|08-deploy" .claude/
```

This catches cases where a reference uses a substring (e.g., "the 08-deploy skill" or "12-test standards" without the full directory name). If any partial-match hits appear, they must be evaluated: some may be coincidental (a prose mention of "deployment" that is not a path reference), but each must be checked.

## Scoring

| Criterion                                                                                                                                                | Points |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Both search terms included in a single sweep (old deleted name AND old renamed name, not just one)                                                       | 1      |
| Sweep covers all 5 reference formats present in the repo: markdown table, markdown link, inline list, YAML-style list, prose mention                     | 2      |
| All 10 dangling references found (5 delete + 5 rename)                                                                                                   | 2      |
| Delete references are removed (not updated to point elsewhere) and rename references are updated (not removed) -- correct disposition per operation type | 1      |
| All changes committed atomically with the delete and rename in a single commit, not as a follow-up cleanup                                               | 2      |
| Verification sweep runs after updates and returns zero hits; practitioner explains what a non-zero result would mean                                     | 1      |
| Practitioner considers partial/substring matches in addition to exact path matches                                                                       | 1      |
| **Total**                                                                                                                                                | **10** |

## Extensions

1. **Cross-repo sweep.** The practitioner is told that `sdk-py/` and `sdk-rs/` downstream repos also reference the template's skill directories in their own agent files. The delete and rename happened in the template. The practitioner must: extend the sweep to the downstream repos, identify which downstream references are now dangling, and explain why the downstream fix cannot be committed atomically with the template change (different repos, different git histories). The practitioner must propose a structural mechanism (a sync validation hook, a manifest check) to catch cross-repo dangling references automatically.

2. **Ambiguous substring match.** Add a rule file `rules/naming.md` that contains the sentence: "Deployment-related skills should follow the naming convention in 08-deployment." After the rename, `grep` finds this hit. But the file also contains: "Testing coverage should target 12 testing scenarios per module." The practitioner must distinguish a true dangling reference ("08-deployment" is a path reference) from a false positive ("12 testing" is a prose phrase, not a reference to `12-testing/`). Score on whether the practitioner evaluates each hit rather than blindly updating all matches.
