---
atom_id: SC-P-019
variant_of: SC-P-019
name: Verify session-start continuity reaches the model, not just the terminal
craft_layer: practitioner
destination: co-codegen
craft_areas: [attend-when, institutional-memory]
applications: [COC]
spec_lineage:
  - CO §39 — Layer 5 (Learning)
  - CO §9 — Layer 1 (Intent)
journal_evidence:
  - path: loom/journal/0017-DISCOVERY-session-notes-broken.md
    polarity: NEGATIVE
    quote: "Even when found, only age was printed to stderr — the hook printed `[WORKSPACE] Session notes: 3h ago` but never injected the actual content or file path into Claude's context. SessionStart hook stderr goes to the user's terminal, not to Claude."
practice_modality: drill
status: verified
---

## The move

At every session cold start, verify two things before beginning work: (1) that session notes exist and are written as instructions-to-next-self (imperative directives: "resume X, check Y, do not repeat Z") rather than as a log (past-tense narrative: "we did X, found Y, decided Z"); and (2) that the injection channel actually delivers those notes into the model's context, not just to the user's terminal. The practitioner tests the injection by checking whether the model can quote or reference the session notes without being given their content — if it cannot, the notes are not reaching the model regardless of what the terminal displays.

## When it fires

Every session start. This is not a conditional trigger — it fires unconditionally because session-start is the single moment where continuity either works or silently breaks. The practitioner does not assume that because session notes were written last time, they are being read this time. The injection channel (hook, prompt injection, anti-amnesia mechanism) is the weakest link and fails silently.

## What it serves

CO §39 (Layer 5 — Learning) requires "observation mechanisms that capture patterns" and that "new practitioners inherit accumulated knowledge." Session notes are the minimum viable knowledge-transfer mechanism between sessions — the simplest instantiation of Layer 5's capture-and-inherit requirement. When the injection channel is broken, every session restarts from zero, violating the compounding property of CO Principle 7 (Knowledge Compounds). CO §9 (Layer 1 — Intent) requires that "each agent MUST carry domain-specific institutional knowledge." Session notes are the ephemeral layer of that knowledge — the session-specific context that CLAUDE.md and rules do not carry. A broken injection channel means Layer 1 loads generic institutional knowledge but not session-specific intent.

## Evidence walkthrough

- **0017-DISCOVERY-session-notes-broken** (NEGATIVE) — The session-notes mechanism had three independent failure modes, all silent: (1) `getSessionNotes()` searched only `workspaces/<dir>/.session-notes` but the notes were at repo root, so they were never found; (2) even when found, the hook only printed the file's age to stderr (the user's terminal), never injecting content or path into Claude's context; (3) the `user-prompt-rules-reminder.js` hook that does inject into Claude's context had no session-notes awareness. The result: every session started from zero despite notes existing on disk. The practitioner who wrote the notes believed continuity was working because the terminal showed "[WORKSPACE] Session notes: 3h ago" — but the model never saw them. The fix added a `[SESSION-NOTES] Read <path> before starting work` injection into the prompt-level hook that reaches the model.

## How to apply

At the start of every session, verify that the session-continuity mechanism actually delivers prior session notes into the model's context, not just to the user's terminal. Test by asking the model to reference a specific instruction from the notes without providing it directly. If the model cannot, trace the injection path: which hook loads the notes, does it write to stderr (terminal only) or to the model-visible context, and does the path resolution match where the notes actually live. Write session notes as imperative directives ("resume X, do not repeat Y") rather than past-tense logs ("we did X, found Y").

## Common failure mode

The practitioner writes session notes as a log ("Session 12: implemented auth module, ran into timeout issue, fixed by increasing pool size") instead of as instructions ("Resume at: integration tests for auth module. Do NOT re-run pool migration. Timeout fix applied — verify it holds under load test"). Log-format notes provide context but no directives, so even when the injection works, the model must infer what to do next rather than being told. The second failure mode is assuming that terminal output = model visibility. The journal entry shows that the hook printed to stderr, which the user saw but the model did not — the practitioner confused their own awareness with the model's.

## Related atoms

- SC-P-007 (session completion state) — completion-state definition is the precondition for useful session notes; if the practitioner does not define what "done" means at session end, the notes have nothing actionable to convey at session start
- SC-A-003 (defense-in-depth codification) — the session-notes injection should be defense-in-depth (hook + prompt reminder + CLAUDE.md reference), not a single point of failure
- SC-A-004 (Layer 3 hook security audit) — session-start hooks that inject continuity notes must be audited for the same security vulnerabilities as other hooks (e.g., command injection via crafted session notes).
- SC-A-012 (CLAUDE.md as Layer 2 context) — the master directive is one of the injection points for session-continuity information; if CLAUDE.md does not reference the workspace state, cold start loses context
