---
drill_id: DR-SC-P-005
source_atom: SC-P-005
name: "Diagnose friction-gradient inversions where the best API has the worst discoverability — drill"
craft_area: attend-how, intervene-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a module that has three API entry points at different abstraction levels (e.g., a low-level primitive, a broken engine, and a working engine). Include a patterns document that mentions the broken engine and the primitive but not the working one. The practitioner must:

1. Map the friction gradient: for each entry point, record discovery cost (is it in the quick-start? in CLAUDE.md? buried in a subdirectory?) and usage quality (does it work? how many lines? does it handle errors?).
2. Identify the inversion: which entry point has the worst discoverability-to-quality ratio?
3. Trace the root cause through COC layers: which layer (Context, Guardrails, Learning) is responsible for the inversion persisting?
4. Write a one-paragraph intervention: what specific COC artifact change would correct the gradient?

Score on whether the practitioner identifies the inversion AND traces it to a COC layer failure, not just to "bad docs." The drill fails if the practitioner recommends "write better documentation" without specifying which COC artifact (CLAUDE.md, a skill file, a rule, a hook) should change and how.
