---
drill_id: DR-SC-A-004
source_atom: SC-A-004
name: "Layer 3 hook security audit — drill"
craft_area: attend-how, intervene-when
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Hand the practitioner a real hook file (e.g., `session-start.js` or `validate-workflow.js` from any kailash repo) and a checklist of three vulnerability classes: (1) shell injection via `execSync` with untrusted input, (2) path traversal via unsanitized file paths, (3) denial of service via unbounded reads or missing timeouts. The drill: identify all instances where the hook reads external input (files, environment variables, git state) and trace each input to its consumption point. Score on: (a) every external input identified, (b) each consumption point classified as safe or unsafe with a one-sentence rationale, (c) at least one false-positive correctly dismissed (not all external inputs are dangerous). The loom-meta 0050 entry provides an answer key for the `version-utils.js` case.
