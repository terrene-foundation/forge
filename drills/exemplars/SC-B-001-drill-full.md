---
drill_id: DR-SC-B-001-FULL
source_atom: SC-B-001
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Read-then-Merge Sync Semantics

## Setup

The practitioner receives the file listings for two repositories that participate in a COC sync relationship. Repository A (the BUILD repo, `loom/kailash-py/.claude/`) is the source. Repository B (the USE template, `kailash-coc-claude-py/.claude/`) is the target. A `/sync` operation is about to run.

**Repository A (source) — 8 files:**

| #   | Path                            | Last modified | Content summary                          |
| --- | ------------------------------- | ------------- | ---------------------------------------- |
| 1   | `rules/zero-tolerance.md`       | 2026-04-08    | v3 — added Rule 6 (implement fully)      |
| 2   | `rules/security.md`             | 2026-04-05    | v2 — added Kailash-specific section      |
| 3   | `rules/git.md`                  | 2026-03-20    | v1 — conventional commits, branch naming |
| 4   | `skills/05-kailash-mcp/`        | 2026-04-07    | 12 files, MCP server patterns            |
| 5   | `skills/07-development-guides/` | 2026-04-06    | 8 files, async patterns, custom nodes    |
| 6   | `agents/dataflow-specialist.md` | 2026-04-03    | v2 — added bulk-operation delegation     |
| 7   | `agents/security-reviewer.md`   | 2026-04-01    | v1 — security audit agent                |
| 8   | `rules/testing.md`              | 2026-04-09    | v4 — added test-once protocol            |

**Repository B (target) — 10 files:**

| #   | Path                                   | Last modified | Content summary                                                                                                                              |
| --- | -------------------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | `rules/zero-tolerance.md`              | 2026-04-02    | v2 — missing Rule 6 (older than source)                                                                                                      |
| 2   | `rules/security.md`                    | 2026-04-07    | v3 — added DataFlow-specific input validation section NOT present in source                                                                  |
| 3   | `rules/git.md`                         | 2026-03-20    | v1 — identical to source                                                                                                                     |
| 4   | `skills/05-kailash-mcp/`               | 2026-04-07    | 12 files, identical to source                                                                                                                |
| 5   | `skills/07-enterprise-infrastructure/` | 2026-04-04    | 6 files, store factory, task queues — TARGET-ONLY, does not exist in source                                                                  |
| 6   | `agents/dataflow-specialist.md`        | 2026-04-03    | v2 — identical to source                                                                                                                     |
| 7   | `agents/security-reviewer.md`          | 2026-04-01    | v1 — identical to source                                                                                                                     |
| 8   | `rules/testing.md`                     | 2026-04-04    | v2 — older than source, missing test-once protocol                                                                                           |
| 9   | `rules/project-local-conventions.md`   | 2026-04-06    | Target-only rule for project-specific CI patterns                                                                                            |
| 10  | `skills/07-development-guides/`        | 2026-04-06    | 8 files — NUMBERING COLLISION with source item 5, but different content (target has Rust-specific guides, source has Python-specific guides) |

The practitioner also receives the single-page sync protocol reference:

- Sync is additive: target-only files are never deleted.
- Per-file diff is mandatory: no bulk overwrite.
- Numbering collisions halt the run until a human classifies the collision as **renumber** (one side gets a new number), **variant** (both are valid, route to variant directories), or **merge** (content is complementary, combine).
- Content drifts where the target has newer content require a **decision**: accept source (losing target additions), accept target (blocking the source update), or merge (combine both).

## Task

1. Inventory the target repository's current state. For each file in Repository B, record whether it has a matching file in Repository A and what the relationship is: **identical**, **drift** (content differs, state which side is newer or richer), **collision** (same canonical slot, different content), or **target-only** (no source counterpart).

2. Produce a per-file merge plan as a table with four columns: `file`, `source state`, `target state`, `decision`. The decision column must contain one of: **overwrite** (source wins), **keep** (target wins), **merge** (combine content), **skip** (no action needed — files are identical), **halt** (collision or authority question that requires human classification), or **preserve** (target-only file, do not touch).

3. For any file where you chose **halt**, write a one-paragraph escalation note explaining what the human must decide and why the sync agent cannot resolve it autonomously.

4. For any file where you chose **merge**, write a one-paragraph merge strategy explaining which sections come from source, which from target, and how conflicts within the file will be resolved.

5. State explicitly: how many files would a bulk `rsync`-style overwrite have damaged, and what content would have been lost?

## Model Answer

**Step 1 — Inventory:**

| #   | Target file                            | Source match | Relationship                                                                                     |
| --- | -------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------ |
| 1   | `rules/zero-tolerance.md`              | A1           | DRIFT — source is newer (v3 vs v2), source has additional content                                |
| 2   | `rules/security.md`                    | A2           | DRIFT — target is newer (v3 vs v2), target has DataFlow-specific section not in source           |
| 3   | `rules/git.md`                         | A3           | IDENTICAL                                                                                        |
| 4   | `skills/05-kailash-mcp/`               | A4           | IDENTICAL                                                                                        |
| 5   | `skills/07-enterprise-infrastructure/` | —            | TARGET-ONLY                                                                                      |
| 6   | `agents/dataflow-specialist.md`        | A6           | IDENTICAL                                                                                        |
| 7   | `agents/security-reviewer.md`          | A7           | IDENTICAL                                                                                        |
| 8   | `rules/testing.md`                     | A8           | DRIFT — source is newer (v4 vs v2), source has additional content                                |
| 9   | `rules/project-local-conventions.md`   | —            | TARGET-ONLY                                                                                      |
| 10  | `skills/07-development-guides/`        | A5           | COLLISION — slot 07 used for different content in source (Python guides) vs target (Rust guides) |

**Step 2 — Per-file merge plan:**

| File                                   | Source state              | Target state                         | Decision                                                         |
| -------------------------------------- | ------------------------- | ------------------------------------ | ---------------------------------------------------------------- |
| `rules/zero-tolerance.md`              | v3 (newer, has Rule 6)    | v2 (older)                           | **overwrite** — source is strictly newer, no target-only content |
| `rules/security.md`                    | v2                        | v3 (has DataFlow validation section) | **merge** — source base + target's DataFlow section              |
| `rules/git.md`                         | v1                        | v1                                   | **skip** — identical                                             |
| `skills/05-kailash-mcp/`               | 12 files                  | 12 files identical                   | **skip** — identical                                             |
| `skills/07-enterprise-infrastructure/` | —                         | 6 files                              | **preserve** — target-only, do not delete                        |
| `agents/dataflow-specialist.md`        | v2                        | v2                                   | **skip** — identical                                             |
| `agents/security-reviewer.md`          | v1                        | v1                                   | **skip** — identical                                             |
| `rules/testing.md`                     | v4 (newer, has test-once) | v2 (older)                           | **overwrite** — source is strictly newer, no target-only content |
| `rules/project-local-conventions.md`   | —                         | target-only                          | **preserve** — target-only, do not delete                        |
| `skills/07-development-guides/`        | Python-specific, 8 files  | Rust-specific, 8 files at slot 07    | **halt** — numbering collision, human must classify              |

**Step 3 — Escalation note for slot 07 collision:**

The source repository uses slot `07` for `development-guides/` (Python-specific async patterns, custom nodes). The target repository uses slot `07` for `enterprise-infrastructure/` (store factory, task queues). These are different skill directories occupying the same canonical number. The sync agent cannot resolve this because the question is structural, not content-level: does the target's Rust-specific `07-development-guides/` belong in a variant directory (`variants/rs/skills/07-development-guides/`), or should it be renumbered to a free slot (e.g., `08-development-guides/`), or should the content be merged into a single language-agnostic directory? This decision sets a precedent for all future numbering across both repositories and must be made by a human who can see the full numbering scheme. Journal entry 0020-DISCOVERY-rs-skill-numbering-divergence documents exactly this class of failure: 12 numbers had different meanings across Python and Rust repositories.

**Step 4 — Merge strategy for `rules/security.md`:**

The source's v2 contains the base security rules (no hardcoded secrets, parameterized queries, input validation, output encoding) plus a Kailash-specific section covering DataFlow access controls, Nexus authentication, and Kaizen prompt injection protection. The target's v3 contains everything in source v2 plus an additional subsection under "Kailash-Specific Security" covering DataFlow input validation patterns (model-level field validation, type coercion guards, bulk-operation size limits). The merge takes the source's v2 as the base and appends the target's DataFlow input validation subsection into the Kailash-Specific Security section. No conflicts exist because the target's addition is purely additive — it does not modify any existing section.

**Step 5 — Blast radius of bulk overwrite:**

A bulk `rsync`-style overwrite would have damaged **4 items**:

1. `rules/security.md` — target's DataFlow input validation section (v3 content) destroyed, reverted to source's v2.
2. `skills/07-enterprise-infrastructure/` — 6 target-only files deleted (not present in source).
3. `rules/project-local-conventions.md` — target-only file deleted.
4. `skills/07-development-guides/` — target's Rust-specific guides overwritten with source's Python-specific guides, destroying all Rust content with no recovery path.

Total: 4 damaged items, of which 2 are irreversible content losses (the target-only files and the Rust guides have no other source).

## Scoring

| Criterion                                                                                                            | Points |
| -------------------------------------------------------------------------------------------------------------------- | ------ |
| Inventory correctly classifies all 10 files (identical/drift/collision/target-only)                                  | 2      |
| Merge plan has correct decision for all 10 files, including halt on the collision                                    | 3      |
| Escalation note for slot 07 identifies the structural nature of the collision and names the three resolution options | 2      |
| Merge strategy for security.md identifies target-only content and describes an additive merge                        | 1      |
| Blast-radius count is correct (4 items) with specific content losses named                                           | 2      |
| **Total**                                                                                                            | **10** |

## Extensions

- **Variant A (harder):** Add a second collision: `agents/pact-specialist.md` exists in both repos but with different content (source defines the agent for PACT (spec) conformance, target defines it for PACT (primitives) SDK work). The practitioner must halt on this too, but the resolution is different from the skill-directory collision — agent files can coexist as separate specialists, not as variants. Does the practitioner distinguish the two collision types?
- **Variant B (operational):** After completing the merge plan, the practitioner writes the actual sync commands (file-by-file `cp`, `mv`, or editor merge — no `rsync`) and a post-sync verification checklist that confirms each decision was applied correctly. Score on whether the verification step re-reads the target after writing, or assumes the write succeeded.
