---
atom_id: SC-A-001
name: Codify-with-explicit-NOT-codified
craft_layer: artifact
destination: both
craft_areas: [institutional-memory]
applications: [COC, COR, COE]
spec_lineage:
  - CO §39 — Layer 5 — Learning
  - CO §47 — Knowledge Curator
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/kailash-rs/workspaces/kailash-rl/journal/0003-DECISION-codify-phase0.md
    polarity: POSITIVE
    quote: "What was NOT codified (and why) — RL algorithm implementations: These are code, not institutional knowledge. The source is the authority. Environment physics: CartPole/Pendulum/MountainCar physics match Gymnasium — this is established science, not a decision to capture. Prior art search results: Already captured in journal 0002-DISCOVERY."
  - path: loom/kailash-py/workspaces/kailash/journal/0012-DECISION-codify-session7-ci-fixes.md
    polarity: POSITIVE
    quote: "What Was NOT Codified (and why) — SQLite read-back in Express.create(): Implementation detail, not a pattern. The code handles it transparently — users don't need to know about the read-back. id_type.__name__ AttributeError fix: Bug fix in generated node code, not a recurring pattern. SQLite cursor leak fix in async_sql.py: One-off bug, already fixed."
practice_modality: drill
status: draft
---

## The move

Every codify entry has two named sections, not one: **What Was Codified** and **What Was NOT Codified (and why)**. The NOT-codified section is mandatory, not a footnote. Each excluded item is named with one of four reasons: (a) it is code, and the source is the authority; (b) it is established science, not a decision; (c) it is a one-off bug, not a recurring pattern; or (d) it is already captured elsewhere by full repo-relative path. Any excluded item that does not fit one of these four reasons gets re-examined — the practitioner is excluding it on aesthetic grounds and will regret it.

## When it fires

Every `/codify` session, without exception. The trigger is the codify phase itself, not a heuristic about whether the session has "enough" learning to record. A codify session that records only what was kept is not a codify session — it is a release note.

## What it serves

CO §39 (Layer 5 — Learning) is the structural reason — Layer 5 is the persistence layer for institutional knowledge, and what it _omits_ shapes the next session's prior just as much as what it includes. CO §47 (Knowledge Curator) names the role: the curator's job is not to capture everything, it is to decide what crosses the threshold from session ephemera to institutional knowledge. CO §17 (Single Source of Truth) is the safety check — the four NOT-codified reasons each correspond to a different existing source of truth (the code, the literature, the bug tracker, the prior journal entry), and naming the reason is naming where the truth lives instead.

## Evidence walkthrough

- **0003-DECISION-codify-phase0** in kailash-rl (POSITIVE) — A clean worked example of all three NOT-codified reasons in a single session: code (RL algorithm implementations), established science (Gymnasium physics), and prior journal entry (prior art search). The entry reads as a checklist the next codify session can use directly.
- **0012-DECISION-codify-session7-ci-fixes** in kailash-py (POSITIVE) — The "one-off bug" reason in operation. Three items were excluded specifically because they were bugs, not patterns: SQLite cursor leak, `id_type.__name__` AttributeError, SQLite read-back. The entry illustrates the discrimination — three of the codified items were patterns (`__del__` super-call, `import warnings` inside `__del__`, PACT bridge approval), and the three excluded items were bugs. Same session, same code base, the difference is whether the issue recurs and constrains future work.

## How to drill it

Hand the practitioner a recent fix log from a real session — five to ten distinct changes with no annotation. The drill: classify each as CODIFY (and which artifact: rule / skill / agent / command / hook) or NOT-CODIFIED (and which of the four reasons). Score on three checkpoints — (a) every excluded item has a named reason, (b) the named reason is one of the four (not "out of scope" or "low priority", which are aesthetic), and (c) every codified item names which artifact it lands in. The kailash-py 0012 entry is a ready-made answer key for a six-item drill.

## Common failure mode

Practitioner records only the wins. The codify entry has a "What was codified" section and nothing else. The next session inherits the wins and re-examines the same exclusion candidates from scratch, often choosing differently — producing a slow drift where the same one-off bugs get codified as patterns three sessions later because nobody recorded that they were rejected the first time. The cure is the mandatory NOT-codified section: it is a decision the practitioner can be held to in a future review.

## Related atoms

- SC-B-002 (authority-chain escalation) — the upstream side of the same problem: when the codified item is in the wrong repo, the NOT-codified section is what tells you so.
- SC-B-004 (specialist delegation) — generates the corrections list that often has both codify-worthy patterns and one-off bugs in the same set, exactly the discrimination this atom is designed to make easy.
