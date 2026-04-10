---
drill_id: DR-SC-A-001-FULL
source_atom: SC-A-001
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Codify-with-Explicit-NOT-Codified

## Setup

The practitioner receives a **fix log** from a single development session on a Python SDK project. The session addressed 8 distinct items across bug fixes, pattern discoveries, and documentation updates. No codification annotations exist — the log is raw output from the session.

**Session fix log (8 items):**

| #   | Change                                                                   | Description                                                                                                                                                                                                                                                                           |
| --- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Fixed `async_sql.py` cursor leak                                         | SQLite cursor was not closed in the `finally` block of `execute_query()`, leaking file handles under concurrent access. One-line fix: added `cursor.close()` to `finally`.                                                                                                            |
| 2   | Added `__del__` super-call pattern to all node base classes              | Discovered that 4 node base classes overrode `__del__` without calling `super().__del__()`, preventing cleanup of parent resources. Added super-call to all 4. This is a recurring pattern — any new node base class will need the same treatment.                                    |
| 3   | Fixed typo in error message: "Connetion refused" to "Connection refused" | String literal fix in `connection_pool.py`.                                                                                                                                                                                                                                           |
| 4   | Discovered that `import warnings` inside `__del__` is unsafe             | Python may have already torn down the `warnings` module by the time `__del__` runs during interpreter shutdown. Moved `import warnings` to module level in 3 files. This affects every file that warns inside `__del__` — it is a constraint on all future `__del__` implementations. |
| 5   | Fixed `Express.create()` read-back for SQLite                            | After inserting a row, `Express.create()` re-read it to return the full object. The read-back used a query that failed on SQLite because of a type mismatch on the ID column. Users never see the read-back — it is transparent.                                                      |
| 6   | Updated README installation section from `pip install` to `uv sync`      | The project migrated to `uv` three sessions ago but the README was not updated.                                                                                                                                                                                                       |
| 7   | Added PACT bridge approval pattern to the specialist agent               | Bridge creation now requires bilateral consent from both endpoint roles. Extracted from the PACT spec and validated against the governance engine. This pattern will recur every time a new bridge type is added.                                                                     |
| 8   | Fixed `id_type.__name__` AttributeError in generated node code           | Generated code called `id_type.__name__` but `id_type` was a string like `"uuid"`, not a type object. One-off generation bug — the template has been fixed.                                                                                                                           |

## Task

1. For each of the 8 items, classify as either **CODIFY** or **NOT-CODIFIED**. For each CODIFY item, name the artifact type it lands in (rule, skill, agent, command, or hook). For each NOT-CODIFIED item, name the reason from the four valid reasons: (a) it is code — the source is the authority, (b) it is established science — not a decision, (c) it is a one-off bug — not a recurring pattern, or (d) it is already captured elsewhere (cite the location).

2. Verify that every NOT-CODIFIED item uses one of the four valid reasons. If you find yourself writing "out of scope," "low priority," or "not important enough," re-examine — those are aesthetic judgments, not structural reasons.

3. Write a two-sentence summary of what was codified and what was not, suitable for the session's codify entry header.

4. For one CODIFY item of your choice, write the opening paragraph of the artifact it would land in (3-4 sentences, enough to show where it lives and what it constrains).

## Model Answer

| #   | Classification | Artifact / Reason                         | Rationale                                                                                                                                                                                                                                                                                            |
| --- | -------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | NOT-CODIFIED   | (a) Code — source is the authority        | Cursor leak is a one-line bug fix. The code itself (the `finally` block in `async_sql.py`) is the authority — no institutional knowledge to capture beyond the fix.                                                                                                                                  |
| 2   | CODIFY         | Rule                                      | The `__del__` super-call is a recurring pattern. Every new node base class must call `super().__del__()`. This is a constraint on future development, not a one-off fix. Lands in a rule file scoped to node development.                                                                            |
| 3   | NOT-CODIFIED   | (c) One-off bug — not a recurring pattern | Typo fix. No pattern to capture, no constraint on future work.                                                                                                                                                                                                                                       |
| 4   | CODIFY         | Rule                                      | The `import warnings` inside `__del__` constraint applies to every file that warns during cleanup. This is a Python-level constraint that will recur — any developer who writes `import warnings` inside `__del__` will hit the same teardown issue. Lands in a rule about `__del__` implementation. |
| 5   | NOT-CODIFIED   | (a) Code — source is the authority        | The SQLite read-back is an implementation detail inside `Express.create()`. Users do not see it, and the code handles it transparently. The source is the authority.                                                                                                                                 |
| 6   | NOT-CODIFIED   | (d) Already captured elsewhere            | The migration to `uv` was codified three sessions ago in the project's development rules. The README update is a consequence of that decision, not a new decision.                                                                                                                                   |
| 7   | CODIFY         | Agent (specialist)                        | The PACT bridge approval pattern is a governance constraint that will recur for every new bridge type. It belongs in the specialist agent's checklist so that future bridge implementations are reviewed against it.                                                                                 |
| 8   | NOT-CODIFIED   | (c) One-off bug — not a recurring pattern | The `id_type.__name__` bug was in generated code, not in a pattern. The template has been fixed. No constraint on future work — the generation template is the authority.                                                                                                                            |

**Codify entry header:** "Three items codified: `__del__` super-call requirement (rule), `import warnings` not inside `__del__` (rule), and PACT bridge bilateral consent (agent checklist). Five items not codified — three were one-off bugs or code-level fixes, one was a typo, and one was already captured in the `uv` migration decision."

**Sample artifact opening (item 2 — `__del__` super-call rule):**

> All node base classes that override `__del__` MUST call `super().__del__()` as the last statement in the override. Omitting the super-call prevents parent-class resource cleanup (file handles, database connections, thread pools) from executing. Four existing base classes were found missing the super-call in session 7, causing leaked resources under garbage collection. New node base classes inherit this constraint — the `__del__` override template includes the super-call by default.

## Scoring

| Criterion                                                                                  | Points |
| ------------------------------------------------------------------------------------------ | ------ |
| All 5 NOT-CODIFIED items correctly identified with valid reasons from the four-reason set  | 3      |
| All 3 CODIFY items correctly identified with appropriate artifact types named              | 3      |
| No aesthetic reasons used ("out of scope", "low priority", "not important enough")         | 1      |
| Codify entry header accurately summarizes both what was and was not codified               | 1      |
| Sample artifact opening names the constraint, the evidence, and the scope of applicability | 2      |
| **Total**                                                                                  | **10** |

## Extensions

- **Variant A (ambiguous items):** Replace items 1 and 5 with changes where the one-off-vs-pattern boundary is genuinely unclear — e.g., a database connection timeout that might be a one-off environment issue or might be a recurring pattern under load. The practitioner must argue for their classification with evidence, not default to "probably one-off."
- **Variant B (cross-repo):** Add a 9th item where the codified pattern belongs in a different repository's artifact set (e.g., a template-level constraint discovered in a downstream repo). The practitioner must classify it as CODIFY but name the upstream repo as the landing location, connecting to the authority-chain escalation move (SC-B-002).
