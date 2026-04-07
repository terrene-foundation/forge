---
atom_id: SC-B-014
name: Journal decision reversals with explicit supersession references
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §28 — Approval Gate
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/journal/0035-DECISION-redteam-round1-resolutions.md
    polarity: POSITIVE
    quote: "RL/RLHF ships as kailash-rl (NOT kailash-ml-rl), a first-class Kailash framework package."
  - path: loom/journal/0038-DECISION-redteam-round2-resolutions.md
    polarity: POSITIVE
    quote: "**Decision**: Replace `kailash-rl` with two scoped solutions:\n- **`kailash-align`** — New first-class Kailash framework for LLM fine-tuning/alignment (DPO, RLHF, LoRA/QLoRA, evaluation, Ollama/vLLM serving). [...]\n**Consequence**: Journal 0035 Decision 1 is superseded."
  - path: loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md
    polarity: POSITIVE
    quote: "Ship the data fabric engine as an optional extension of kailash-dataflow, installed via pip install kailash-dataflow[fabric]."
  - path: loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md
    polarity: POSITIVE
    quote: "This supersedes 0001-DECISION-dataflow-extension-not-separate-package.md. That decision recommended extension; this goes further — DataFlow's identity evolves."
practice_modality: brokerage-rep
status: verified
---

## The move

When a decision is reversed, create a new journal entry that (a) names the prior entry by number and title, (b) states explicitly "this supersedes entry NNNN," (c) explains why the prior decision was wrong or incomplete, and (d) records the new decision. Never silently update the old entry. Never create a new entry that contradicts the old one without referencing it. The journal is append-only; the supersession chain is what makes it navigable.

## When it fires

Any moment the current session's analysis or red team reveals that a prior decision should change — whether the reversal happens within the same session (hours apart) or across sessions (days apart). The trigger is "I am about to make a decision that contradicts an existing journal entry."

## What it serves

CO §28 (Approval Gate) requires human judgment at structural transitions. A decision reversal is a structural transition in the institutional knowledge base — the old decision shaped downstream work, and the reversal reshapes it. Without an explicit supersession, the reversal is invisible at the approval gate: the human sees the new decision but does not know that it contradicts a prior one, and cannot judge whether the downstream consequences have been addressed. CO §17 (Single Source of Truth) requires no contradictions; two journal entries that say opposite things without a supersession link are a contradiction in the institutional record.

## Evidence walkthrough

- **0035-DECISION-redteam-round1-resolutions** (POSITIVE) — Decides that RL/RLHF ships as `kailash-rl`, a first-class Kailash framework package, separate from kailash-ml because RL (environments, policies, replay buffers) is a different paradigm from supervised ML. This was the correct decision given the information at the time.
- **0038-DECISION-redteam-round2-resolutions** (POSITIVE) — Round 2 red team reveals that DPO is not reinforcement learning and the on-prem SLM use case is alignment, not RL. The entry explicitly states "Journal 0035 Decision 1 is superseded" and replaces kailash-rl with two scoped solutions: kailash-align (LLM alignment) and kailash-ml[rl] (classical RL as optional extra). The supersession is textually explicit, naming the prior entry by number.
- **data-fabric-engine 0001-DECISION** (POSITIVE) — Decides to ship the fabric as a DataFlow extension (`kailash-dataflow[fabric]`), not a separate package. This was a considered decision with alternatives analysed.
- **data-fabric-engine 0004-DECISION** (POSITIVE) — Three turns later in the same session, further analysis reveals that DataFlow's architecture is already source-agnostic. The entry opens with "This supersedes 0001-DECISION-dataflow-extension-not-separate-package.md. That decision recommended extension; this goes further — DataFlow's identity evolves." The speed of reversal (same day) demonstrates that supersession discipline applies regardless of elapsed time.

## How to drill it

Present the practitioner with a journal containing entry 0012 (Decision: use SQLite for all test fixtures) and a new analysis showing PostgreSQL-specific features are needed. Ask them to write entry 0017 that reverses 0012. Score on four criteria: (a) does the new entry name 0012 by number and title? (b) does it contain an explicit "supersedes" statement? (c) does it explain why 0012 was wrong or incomplete? (d) does it record the new decision as a standalone entry readable without 0012? Deduct if the practitioner edits 0012 instead of creating a new entry.

## Common failure mode

Practitioner creates the new decision entry but omits the supersession reference, treating the reversal as a fresh decision rather than a correction. Six months later, a different agent reads the journal and finds two contradictory entries with no link between them. The agent cannot tell which is current, and the institutional knowledge is corrupted. The supersession reference is the navigational link that prevents this.

## Related atoms

- SC-B-001 (read-then-merge sync) — when a sync resolves a drift by superseding an earlier decision, the supersession must be journaled at sync time, not after.
- SC-B-005 (spec-to-code conformance) — conformance audits may trigger reversals that need supersession journaling.
- SC-B-006 (multi-round red team convergence) — red team rounds are the primary source of decision reversals.
- SC-B-010 (drift detection audit) — drift audits surface discrepancies that trace back to decisions that should have been superseded but were not.
- SC-B-015 (MUST rule reinterpretation) — a rule reinterpretation is a form of decision reversal that requires the same supersession discipline applied to the rule text.
