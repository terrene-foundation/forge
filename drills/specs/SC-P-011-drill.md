---
drill_id: DR-SC-P-011
source_atom: SC-P-011
name: "Detect when an audit categorization method is structurally blind to the architecture it audits — drill"
craft_area: attend-how
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with three audit findings, each reporting a high orphan/dead-code rate for a different architectural pattern:

1. A conventional codebase with explicit imports — the audit's grep-for-imports method is appropriate.
2. A plugin system with convention-based discovery (e.g., `plugins/*.py` loaded by glob) — the audit greps for `import plugin_name` and finds nothing.
3. A progressive-disclosure system where an index file (SKILL.md) describes triggers and the runtime loads files based on trigger match.

For each finding, the practitioner must: (a) name the architecture's discovery mechanism, (b) name the audit's detection mechanism, (c) state whether the two are compatible, and (d) if incompatible, propose a detection method that matches the architecture. Score on whether the practitioner catches the mismatch in cases 2 and 3 and proposes a valid alternative detection method (e.g., "scan SKILL.md for trigger descriptions that map to directory names" for case 3).
