---
title: Brokerage-rep modality
date: 2026-04-10
destination: forge
modality: brokerage-rep
---

# Brokerage Rep

A brokerage rep is the execution of a real governance or sync move as part of the practitioner's actual workflow. The word "rep" is used in the weightlifting sense — repetition under real load. Unlike a drill, the stakes are real: the sync actually commits, the authority-chain escalation actually lands in someone's inbox, the audit actually produces findings that will be acted on.

## What a brokerage-rep is

A brokerage-rep has three elements:

1. **A real trigger** — the practitioner is in a workflow where the move is required. Not contrived; not scheduled by the course.
2. **A real execution** — the practitioner performs the move, on real artifacts, with real consequences.
3. **A real debrief** — after the move lands, the practitioner reviews what happened, names what they noticed, and records lessons.

The rep happens when the workflow calls for it. A course cannot schedule a brokerage-rep the way it schedules a drill — the course can only ensure the practitioner is in a workflow where the trigger will fire, and be ready to coach when it does.

## When to pick a brokerage-rep

Choose the brokerage-rep modality for an atom when:

- The move only exists in real workflow context — it is meaningless without actual stakes.
- The practitioner must develop judgment under pressure, not just procedure under instruction.
- The failure mode of simulation is severe — a scripted sync or a contrived escalation misses the whole point.
- The atom is a governance, sync, or authority-chain move rather than an analytical skill.

Do not pick a brokerage-rep when:

- The atom has a right answer and can be drilled (use `drill`).
- The practitioner can learn it from a journal narrative (use `case`).
- The output is an artifact (use `build`).

## How to run a brokerage-rep

**Pre-workflow phase:**

1. Ensure the practitioner is attached to a real project where the rep will fire. No real project means no rep.
2. Brief the practitioner on the atom before the rep fires. They must know what move they are about to practice, what triggers it, and what the common failure modes are. (Read the atom file.)
3. Set the expectation that the rep will be observed and debriefed.

**Workflow phase:** 4. Wait for the trigger. Brokerage-reps are not scheduled — they fire when the work fires. A sync-related atom fires during an actual sync. An authority-chain atom fires when a real conflict surfaces. An audit atom fires when a real audit is run. 5. When the trigger fires, the practitioner executes the move. The facilitator observes. If the practitioner is about to commit a dangerous error, the facilitator intervenes — otherwise, the rep runs to completion. 6. The practitioner makes real decisions, writes real messages, commits real artifacts. The facilitator does not drive.

**Debrief phase:** 7. Immediately after the rep lands, debrief. Not at the end of the week — immediately, while the context is fresh. 8. Walk through the trigger: what did the practitioner notice that fired the move? 9. Walk through the execution: what did they do, what did they consider doing, what did they reject and why? 10. Walk through the outcome: what actually happened? Did the move succeed, partially succeed, or fail? 11. Name the failure modes they avoided (from the atom's Common Failure Mode section) and any new failure modes they discovered.

**Recording phase:** 12. The practitioner writes a short reflection — three to five sentences — about the rep and what they would do differently next time. This is their journal. It becomes their own institutional memory for the atom.

**Rep count:** Unlike drills, which can be run dozens of times, brokerage-reps are fewer and heavier. Mastery typically requires 3-5 reps across varied triggers. A practitioner who has synced across three different repos with different drift patterns has more skill than one who has drilled the read-then-merge procedure ten times in a sandbox.

## What counts as complete

A rep is complete when:

- The move has landed in the real workflow (not rolled back as practice).
- The practitioner has debriefed immediately after.
- The reflection has been written.

A rep is not complete when the practitioner has walked through the move verbally without executing it. Verbal walk-throughs are pre-briefing, not reps.

## Common failure modes

1. **Simulating the rep.** "Let's pretend we're doing a sync on these test repos." This is at best a drill, at worst a waste of time. The distinguishing feature of a brokerage-rep is the real-world consequence. Fix: if a real workflow is not available, defer the atom to a rep modality; use a different atom in the meantime.

2. **Driving the practitioner through the move.** The facilitator walks alongside and says "now classify this file as variant" and "now halt the sync here." The practitioner becomes a typist. Fix: the facilitator observes silently except in the case of dangerous errors.

3. **Deferring the debrief.** The rep fires on Monday, the debrief is scheduled for Friday. By Friday, the practitioner has rationalized their choices and forgotten the moments of uncertainty that were the most valuable part of the rep. Fix: debrief within 30 minutes of the rep landing.

4. **Skipping the reflection.** The practitioner debriefs but never writes anything down. Next time the atom fires, the lessons are gone. Fix: treat the reflection as structurally required — no reflection, rep not counted.

5. **Confusing brokerage-rep with mentoring.** The facilitator starts coaching every decision during the rep. This turns the rep into a tutorial and erases the decision-making practice. Fix: coaching happens in the debrief, not during the rep.

## Assessment compatibility

Brokerage-reps assess well because the evidence is real: the committed sync, the landed authority-chain message, the audit findings in the tracker. Assessment works by:

- Observing the rep live or by reviewing the artifacts it produced.
- Interviewing the practitioner on specific decisions during the debrief.
- Asking counterfactuals — "what would you have done if the numbering conflict had been in a different file?"

Because reps fire when the workflow fires, assessment cannot be scheduled. Courses using reps for assessment must accept that the assessment window is defined by the work, not the course calendar.

## Atoms using this modality

Currently ~14 atoms use the brokerage-rep modality, all in the brokerage layer:

- SC-B-001 Read-then-merge sync
- SC-B-002 Authority-chain escalation
- SC-B-004 Specialist delegation
- SC-B-005 Spec-to-code conformance audit
- SC-B-006 Multi-round red team to numeric convergence
- SC-B-009 Cross-SDK upstream brokerage
- SC-B-010 Drift detection audit
- SC-B-011 Version tracking cascade
- SC-B-013 Cross-workspace dependency mapping
- SC-B-014 Explicit supersession journaling
- SC-B-015 MUST rule semantic reinterpretation
- SC-B-016 Bidirectional cross-language parity
- SC-B-017 Verification gradient zone assignment
- SC-B-018 PDP/PEP separation

See `catalog/README.md` Index 4 (Practice Modality) for confirmation.
