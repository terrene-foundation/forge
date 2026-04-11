---
title: Observation modality
date: 2026-04-10
destination: forge
modality: observation
---

# Observation

Observation is the practice of watching a move unfold live and classifying what is seen against the atom's definition. The practitioner does not execute the move — they watch it, name it, and reason about whether it was done well, poorly, or missed. Observation is the rarest modality in the catalog (one atom, SC-B-008) because most atoms are better served by drills or cases, but for moves that are best learned by attending to someone else's execution, observation is the correct choice.

## What an observation is

An observation has four elements:

1. **A live or near-live setting** — a session, a red team round, a code review, a governance meeting. Not a recording viewed hours later; not a transcript read after the fact. "Near-live" means within hours, not days.
2. **A target move** — the practitioner knows in advance which atom they are watching for. Observation without a target is attention without focus; the practitioner learns nothing.
3. **A classification task** — after watching, the practitioner must say: was the move made, was it made well, was it missed, was it inverted. Classification forces specificity that passive watching does not.
4. **A reasoning artifact** — the practitioner writes down what they observed and what their classification was. The artifact is short (a paragraph) but required.

## When to pick observation

Choose the observation modality for an atom when:

- The move is a meta-move about _whether_ some other move happened correctly. The observer-atom is one level up from the atom being observed.
- The practitioner benefits more from pattern-matching than from executing — usually because executing is cost-prohibitive or premature.
- The move is best taught by watching multiple examples in sequence, comparing them against each other.
- The skill is attentional rather than operational — the practitioner needs to learn what to _notice_, not what to _do_.

Do not pick observation when:

- The practitioner can execute the move themselves (use `drill`, `build`, or `brokerage-rep`).
- The move is best taught through a past narrative (use `case`).

Observation is narrow by design. Most "watching" pedagogy is better served by cases (past narratives) or by drills in pairs (one practitioner executes, the other classifies).

## How to run an observation

**Setup phase:**

1. Identify the session to observe. This can be a real red team round, a live governance discussion, a code review, or a working session on a real project.
2. Brief the observer on the atom. They must know what they are watching for, what it looks like when done well, and what the common failure modes are.
3. The observer does not participate. They are present but silent.

**Observation phase:** 4. The session runs normally. The people being observed may or may not know the observer is watching for a specific atom; the facilitator decides based on context. 5. The observer takes notes. Time-stamped if possible. Short annotations: "T+12: agent A proposed X, agent B challenged with Y — this looks like convergence but only on surface." 6. The observer does not interrupt, correct, or participate. Interruption contaminates the observation.

**Classification phase:** 7. Immediately after the session, the observer classifies: was the target move made? Made well? Missed? Inverted? 8. The observer writes a paragraph of reasoning. Specifically: what did they see, how does it map to the atom's definition, what would they have done differently.

**Debrief phase:** 9. If safe to do so, share the observation with the session participants. They may confirm or challenge the classification. The disagreements are often more valuable than the agreements. 10. The observer records the debrief conversation in their reflection.

**Rep count:** Observation, like brokerage-rep, benefits from repetition across varied examples. A single observation produces a single pattern; 3-5 observations across different sessions begin to build the attentional skill that observation is designed to teach.

## What counts as complete

An observation is complete when:

- The observer has watched a live or near-live session targeting the atom.
- The classification is made explicitly (not hedged).
- A paragraph of reasoning exists in writing.
- A debrief has happened, even if short.

An observation is not complete when the observer says "I think I saw something like that." Vague observations are evidence of vague attention.

## Common failure modes

1. **Watching without a target.** The observer is told "watch this session and see what you notice." This is sightseeing, not observation. Fix: specify the atom before the session begins.

2. **Participating instead of observing.** The observer cannot help jumping in. This contaminates the session (the people being observed modify their behavior) and contaminates the observation (the observer confuses their own move with the one they were watching for). Fix: structural enforcement — the observer joins the session with the explicit role of observer and is not allowed to speak during it.

3. **Recording instead of watching live.** The observer reviews a transcript or recording hours later. The live context is gone — the pauses, the glances, the moment of hesitation before a decision. Observation loses most of its value when the temporal immediacy is removed. Fix: if live is impossible, substitute a case modality and admit the trade-off.

4. **Writing the reasoning before the classification.** The observer drafts a paragraph of reasoning and then concludes "so probably the move was made well." This inverts the task — classification comes first, reasoning justifies it. Fix: commit to a classification (three words) before writing the paragraph.

5. **Skipping the debrief.** The observation stays in the observer's head. The people being observed never learn what was noticed; the observer never gets their classification challenged. Fix: make the debrief a required output, even if it is a five-minute conversation.

## Assessment compatibility

Observation works for assessment when the assessor can confirm what the observer saw — by watching the same session, or by having the session participants corroborate. Observation does not work well for one-off assessment without ground truth, because the observer's classification cannot be independently verified.

The 3/5 convergence threshold from SC-B-008 (kz-engage 0009 calibration) is a useful assessment benchmark: an observer who can identify 3 out of 5 convergent findings in a red team round has mastered the atom; fewer than 3 suggests insufficient attention to the move.

## Atoms using this modality

Currently one atom uses the observation modality:

- SC-B-008 Convergence-as-validation across independent reds

This is expected to remain rare. Observation is a sharp tool for a narrow use case, and most atoms benefit from active practice rather than attentive watching.

See `catalog/README.md` Index 4 (Practice Modality) for confirmation.
