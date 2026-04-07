---
drill_id: DR-SC-P-014
source_atom: SC-P-014
name: "Distinguish pre-existing failures from pre-existing gaps when applying zero-tolerance Rule 1 — drill"
craft_area: intervene-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a list of 6 issues discovered during a sync-focused session. Three are failures, three are gaps. The practitioner must classify each and state the action:

1. A Ruby binding test for `upsert` that exists but fails with a `NotImplementedError` at runtime. (FAILURE — the test asserts behavior that was promised; fix now.)
2. Node.js has no CRUD bindings at all — no tests, no implementation, no prior art. (GAP — never built; file as separate issue.)
3. A C ABI function `dataflow_query` that segfaults on NULL input. (FAILURE — the function exists and is broken; fix now.)
4. WASM has no DataFlow bindings and no tracking issue. (GAP — never started; file as separate issue.)
5. The Ruby binding's `aggregate` function returns incorrect results when given an empty collection. (FAILURE — the function exists and returns wrong results; fix now.)
6. Go bindings follow the C ABI but lack a high-level wrapper that other Go consumers expect. (GAP — the wrapper was never built; file as separate issue.)

Score on classification accuracy and on whether the practitioner's action statement matches the classification: failures get "fix in this session" with a concrete next step; gaps get "file as issue #NNN with scope description" without attempting implementation.
