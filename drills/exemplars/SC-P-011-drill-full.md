---
drill_id: DR-SC-P-011-FULL
source_atom: SC-P-011
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC, COR, COE]
---

# Detect Audit Categorization Structural Blindness

## Setup

The practitioner receives **three audit reports**, each from a different codebase with a different architecture. Each audit used the same detection method: `grep -r "import\|require\|from.*import" --include="*.py" --include="*.js"` to find all references to skill files, then flagged any skill file not found in the grep results as an "orphan."

**Audit Report A — Conventional Python Package:**

```
Codebase: analytics-pipeline (47 Python files)
Architecture: Standard Python package with explicit imports
Detection: grep for import statements
Result: 3 of 28 utility modules are orphans (not imported anywhere)
Recommendation: Delete the 3 orphan files
```

The 3 flagged files are: `utils/date_helpers.py`, `utils/cache_warmup.py`, `utils/legacy_compat.py`.

**Audit Report B — Plugin System:**

```
Codebase: data-connectors (62 Python files)
Architecture: Plugin system — connectors are discovered at runtime via
  entry_points in pyproject.toml and loaded by importlib.metadata
Detection: grep for import statements
Result: 22 of 35 connector modules are orphans (not imported anywhere)
Recommendation: Delete the 22 orphan connectors, keeping only the 13
  that are explicitly imported in tests or CLI
```

**Audit Report C — CC Skill Library:**

```
Codebase: kailash-coc-claude-py (440 skill files across 35 directories)
Architecture: Progressive disclosure — SKILL.md in each directory declares
  trigger patterns; Claude Code loads skills on-demand when user input
  matches the trigger, not via explicit imports
Detection: grep for import/reference statements
Result: 204 of 350 skill files are orphans (59% orphan rate)
Recommendation: Consolidate to ~146 actively referenced skills,
  archive the 204 orphans
```

## Task

1. For each audit report (A, B, C), answer three questions:
   - What is the **architecture's discovery mechanism** (how does the system find and load these files)?
   - What is the **audit's detection mechanism** (how does the audit determine whether a file is used)?
   - Are the two mechanisms **compatible** (does the detection method reliably identify unused files in this architecture)?

2. For each incompatible audit, explain:
   - Why the orphan count is **wrong** (what the audit missed)
   - What detection method **would** be valid for this architecture
   - What is the **actual risk** of acting on the audit's recommendation

3. Write a one-paragraph **meta-lesson**: when is a grep-for-references audit valid, and when does it produce structurally blind results?

## Model Answer

**1. Architecture vs detection compatibility:**

| Report | Architecture Discovery                                                                                                                    | Audit Detection                      | Compatible?                                                                                                                                               |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A**  | Explicit `import` statements — Python's standard import mechanism                                                                         | grep for import statements           | **YES** — if a module is not imported anywhere, it is genuinely unused in a conventional Python package                                                   |
| **B**  | Runtime discovery via `importlib.metadata` entry_points — connectors are registered in `pyproject.toml`, not imported in source code      | grep for import statements           | **NO** — entry_point-based plugins are never imported in source code by design; grep will flag 100% of plugin modules as orphans                          |
| **C**  | Trigger-based progressive disclosure — `SKILL.md` files declare when a skill loads; Claude Code matches user input to triggers at runtime | grep for import/reference statements | **NO** — skills are loaded by the Claude Code runtime based on trigger matching, not by source-level references; grep cannot detect trigger-based loading |

**2. Incompatible audit analysis:**

**Report B — Plugin System:**

- **Why wrong:** The 22 "orphan" connectors are registered as entry_points in `pyproject.toml` and loaded at runtime by `importlib.metadata.entry_points(group="data_connectors")`. They are never imported in source code because that is how plugin architectures work. The audit flagged 22 of 35 connectors — but if the system has 35 registered entry_points and 13 are also imported in tests, the "22 orphans" are simply the connectors that don't have dedicated test files importing them.
- **Valid detection:** Parse `pyproject.toml` `[project.entry-points]` section to enumerate registered connectors, then verify each registered connector's module exists and is loadable (`importlib.import_module()`).
- **Risk of acting:** Deleting 22 connectors would silently break any runtime that discovers connectors via entry_points. Users would see "no connector found for source type X" errors with no indication that the connector was deliberately deleted.

**Report C — CC Skill Library:**

- **Why wrong:** The 204 "orphan" skills are loaded on-demand by Claude Code when user input matches the trigger declared in their directory's `SKILL.md`. Numbered directories (e.g., `01-core-sdk/`, `02-dataflow/`) ARE distilled documentation loaded via `SKILL.md` triggers — they look unreferenced because no source file imports them. The 59% orphan rate is an artifact of the detection method, not evidence of unused files.
- **Valid detection:** Parse each directory's `SKILL.md` for trigger patterns, then verify that the trigger pattern is syntactically valid and that the skill files within the directory are internally consistent (referenced by SKILL.md or by other files in the same directory).
- **Risk of acting:** Archiving 204 skills would strip 59% of the skill library from Claude Code's available knowledge. Users would lose access to skills that are correctly loadable but were invisible to the audit. The loom journal records that this exact recommendation was nearly acted upon before the audit method was questioned (loom-meta 0005, retracted in 0011).

**Report A — Conventional Package:**

- The audit IS valid for this architecture. However, the practitioner should still verify the 3 flagged files:
  - `legacy_compat.py` might be imported dynamically (`importlib.import_module`) for backwards compatibility
  - `cache_warmup.py` might be invoked via a CLI entry_point or cron job, not imported by other Python files
  - `date_helpers.py` is the most likely genuine orphan if no dynamic loading exists
  - Even with a compatible detection method, spot-check the findings before deletion.

**3. Meta-lesson:**

A grep-for-references audit is valid when — and only when — the architecture's discovery mechanism IS source-level references. Conventional imports, explicit includes, and static linking all use source-level references that grep can detect. Plugin systems, trigger-based progressive disclosure, entry_point registration, convention-based discovery (e.g., Django's `app/models.py`), and runtime reflection all use mechanisms that are invisible to grep. The audit method must match the architecture's wiring, or it will produce false orphans whose deletion breaks the system in ways that only surface at runtime. The structural blindness is not in the audit tool (grep works fine) but in the assumption that the tool's detection mechanism matches the architecture's discovery mechanism.

## Scoring

| Criterion                                                                         | Points |
| --------------------------------------------------------------------------------- | ------ |
| Report A correctly identified as compatible                                       | 1      |
| Report B correctly identified as incompatible with entry_point reasoning          | 2      |
| Report C correctly identified as incompatible with trigger-based reasoning        | 2      |
| Valid alternative detection methods proposed for B and C                          | 2      |
| Risks of acting on false orphan data are specific (not just "things might break") | 2      |
| Meta-lesson identifies the detection-discovery mismatch as the structural issue   | 1      |
| **Total**                                                                         | **10** |

## Extensions

- **Variant A (harder):** Report A's `cache_warmup.py` is actually invoked via a `subprocess.run(["python", "utils/cache_warmup.py"])` call in a shell script, not via Python import. The grep-for-imports audit correctly reports it as an orphan from Python's perspective, but it IS used. Design a detection method that catches cross-language invocations.
- **Variant B (COR application):** A literature review audit flags 15 papers as "uncited" because they don't appear in the bibliography. But 8 of them are cited indirectly via a survey paper that aggregates their findings. The citation graph has the same structural blindness. How do you audit citation completeness when indirect citations exist?
