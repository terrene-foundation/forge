---
drill_id: DR-SC-P-019
source_atom: SC-P-019
name: "Verify session-start continuity reaches the model, not just the terminal — drill"
craft_area: attend-when, institutional-memory
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Set up a workspace with session notes from a previous session. The notes contain a specific instruction: "Do NOT re-run the migration script; it was already applied in the previous session." Start a new session and, before doing any work, ask the model "What should I avoid doing based on the previous session's notes?" If the model cannot answer, the injection is broken. The practitioner must then trace the injection path: which hook loads the notes? Does it write to stderr (terminal only) or stdout/context (model-visible)? Is the hook registered in settings.json? Does the path resolution match where the notes actually live? The drill succeeds when the practitioner can diagnose and fix a broken injection channel within the first 5 minutes of a session rather than discovering it after re-running the migration.
