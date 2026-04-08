---
atom_id: SC-P-027
name: Run hostile reviewer simulation as preemptive defense
craft_layer: practitioner
destination: both
craft_areas: [judge]
applications: [COR]
spec_lineage:
  - CO §28 — Convergence requirements
  - COR § Layer 1 — argument-critic agent (adversarial reviewer simulation)
  - COR § Layer 4 — /challenge command (hostile reviewer simulation)
  - COR § Layer 4 — FATAL findings gate (must address before continuing section)
journal_evidence:
  - path: terrene/foundation/workspaces/care-thesis/journal/0018-DEFENSE-incomplete-contracts-critique.md
    polarity: POSITIVE
    quote: "I am going to review this model with the same rigor I would apply to a referee report for a top-5 journal. Let me be direct: the ambition is admirable, the mapping is strained, and in its current form this would not survive at JFE or AER."
  - path: terrene/foundation/workspaces/care-thesis/journal/0054-DEFENSE-methodology-hostile-review.md
    polarity: POSITIVE
    quote: "Reflexivity is worse than acknowledged. Six sentences in Part V are insufficient. The author designed CARE (two-plane architecture), then built a formal model that derives two-plane architecture as optimal."
practice_modality: drill
status: verified
---

## The move

Before submitting a research argument, commission a hostile reviewer simulation: instruct an agent to adopt the persona of a veteran reviewer in the target venue and critique the argument from every angle. The reviewer should identify fatal flaws (claims that are asserted rather than derived, mechanisms that do not operate, normative claims disguised as positive results), major issues (missing comparisons, insufficient formalization), and desk-rejection triggers. The simulation must be adversarial — the agent's goal is to find real weaknesses, not to affirm the argument. Each critique should include: the attack, why it matters (what a referee would write), and a suggested defense or revision path.

## When it fires

Two trigger conditions:

1. A theoretical argument has been constructed and the author believes it is ready for submission. The hostile review tests that belief against the venue's actual standards.
2. A specific section contains a strong claim (novelty, optimality, impossibility) that a referee will challenge. Preemptive defense prepares the author to respond without the 6-month round-trip of actual peer review.

## What it serves

CO §28 (Convergence requirements) states that quality assertions must be backed by evidence of convergence. In research, convergence means the argument survives adversarial examination — if independent hostile reviewers converge on the same weaknesses, those weaknesses are real. COR § Layer 1 defines an argument-critic agent specializing in "adversarial argument analysis and reviewer simulation" with "venue-specific reviewer expectations" as required knowledge. COR § Layer 4 establishes a FATAL findings gate: findings from /challenge "must address before continuing section."

## Evidence walkthrough

- **0018-DEFENSE-incomplete-contracts-critique** (POSITIVE) — The entry documents a full hostile reviewer simulation for the CARE paper's application of Hart & Moore (1990) to AI governance. The reviewer persona is specified: "veteran researcher in the Grossman-Hart-Moore incomplete contracts tradition" who has "published in JPE, QJE, and Econometrica." The critique identifies 2 FATAL issues (AI agents do not make investments in the Hart-Moore sense; the hold-up mechanism does not operate), 2 MAJOR issues (complementarity result is asserted not derived; normative claim disguised as positive result), and specific desk-rejection triggers ("Claiming AI agents make 'investments' without redefining what investment means for non-strategic entities"). Crucially, the critique also offers 3 constructive paths forward: human-human hold-up, Aghion-Tirole authority delegation, and mechanism design for constraint specification. The simulation changed the paper's theoretical foundation — it was not a validation exercise but a discovery tool.

- **0054-DEFENSE-methodology-hostile-review** (POSITIVE) — A second hostile review targeting methodology, not theory. The critique identifies the reflexivity problem: "The author designed CARE (two-plane architecture), then built a formal model that derives two-plane architecture as optimal. The model specification choices were made by someone who already knew the answer." It offers 3 revision options: add calibration exercise, expand reflexivity analysis, or target RAND/JET instead of AER. The entry also catches a factual error: the user flagged "single case study" but the paper is actually pure theory — the hostile reviewer corrected the user's own framing.

## How to drill it

The practitioner selects a 2-page research argument (their own or from a published paper). Then:

1. Specify the target venue and the reviewer persona (discipline, seniority, known biases of that venue's reviewers).
2. Run the hostile review: identify at least 1 FATAL issue, 2 MAJOR issues, and 2 desk-rejection triggers.
3. For each finding, write: the attack (what is wrong), why it matters (what the referee report would say), and a suggested defense or revision path.
4. Classify each finding as fixable-in-revision vs. requires-fundamental-rethink.

Scoring: number of genuine weaknesses found (not surface issues like formatting), quality of the defense paths (do they actually address the critique?), and whether the drill changed the practitioner's assessment of the argument's submission readiness.

## Common failure mode

The practitioner runs a "review" that is actually a summary with hedged praise: "The argument is interesting but could be strengthened by..." This is not hostile review; it is politeness theater. The 0018-DEFENSE entry demonstrates the correct tone: "in its current form this would not survive at JFE or AER." A hostile reviewer who concludes "the paper is strong" after examining it should be treated as a failed simulation — either the argument is genuinely airtight (rare) or the reviewer is not being adversarial enough.

## Related atoms

- SC-B-006 (multi-round convergence): run multiple hostile reviewers with different personas. When they converge on the same weakness, it is real.
- SC-B-008 (convergence across independent reds): two independent hostile reviewers finding the same flaw is stronger evidence than one finding two flaws.
- SC-P-015 (ask for contradictions): the hostile review prompt should end with "what would make you reject this paper?"
- SC-P-033 (reflexivity diagnosis): hostile review is how reflexivity problems are discovered (0054-DEFENSE).
