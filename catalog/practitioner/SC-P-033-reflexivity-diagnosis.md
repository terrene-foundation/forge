---
atom_id: SC-P-033
name: Diagnose reflexivity in self-referential research
craft_layer: practitioner
destination: forge
craft_areas: [judge]
applications: [COR]
spec_lineage:
  - COR § Honest Limitations — AI is not a domain expert
  - COR § Layer 3 — no overclaims without qualification (hard rule)
  - CO §28 — Convergence requirements (self-critique as convergence dimension)
journal_evidence:
  - path: terrene/foundation/workspaces/care-thesis/journal/0054-DEFENSE-methodology-hostile-review.md
    polarity: POSITIVE
    quote: "Reflexivity is worse than acknowledged. Six sentences in Part V are insufficient. The author designed CARE (two-plane architecture), then built a formal model that derives two-plane architecture as optimal. The model specification choices (rho > 0, contaminated signal, career concerns sub-game) were made by someone who already knew the answer."
practice_modality: case
status: verified
---

## The move

When a researcher formalizes their own prior design, practice, or framework, diagnose the reflexivity: the model specification choices were made by someone who already knows the answer. This is not disqualifying — many important formalizations (Holmstrom formalizing Jensen & Meckling's verbal argument, Arrow formalizing moral hazard) followed the same pattern. But the reflexivity must be acknowledged explicitly and tested: do the results survive under alternative specifications that the author did NOT design the model to produce?

## When it fires

One trigger condition: the research formalizes, validates, or derives as optimal a system, framework, or practice that the author designed. The reflexivity is that the modeler's priors are shaped by the designer's experience, creating a risk that the model's assumptions were chosen (consciously or not) to produce the desired result.

## What it serves

COR § Honest Limitations acknowledges that the AI "synthesizes from training data and web searches, not from years in the field." But reflexivity is a limitation of the HUMAN author, not the AI. CO §28 (Convergence requirements) applies here as self-critique: the argument must converge with self-critical examination, not just external critique. A paper that does not diagnose its own reflexivity will have it diagnosed by reviewers, who will be less charitable about it.

## Evidence walkthrough

- **0054-DEFENSE-methodology-hostile-review** (POSITIVE) — The entry documents a hostile reviewer simulation that diagnosed reflexivity in the CARE thesis. The finding: "The author designed CARE (two-plane architecture), then built a formal model that derives two-plane architecture as optimal. The model specification choices (rho > 0, contaminated signal, career concerns sub-game) were made by someone who already knew the answer." The reviewer notes that "Six sentences in Part V are insufficient" for addressing this. The critique then identifies the specific mechanism by which reflexivity could be addressed: "Formal specification sensitivity analysis showing results survive under CRRA utility, non-quadratic costs, continuous types." This shifts the burden from "trust me, I'm objective" to "the mathematics holds under these alternative specifications." The entry offers 3 options: add calibration exercise, expand sensitivity analysis, or target a venue where reflexivity is less scrutinized — a pragmatic framing that treats reflexivity as a constraint to manage, not a sin to confess.

## How to drill it

The practitioner selects a research paper where the author formalizes their own prior work. Then:

1. Identify the reflexivity: what did the author design/practice BEFORE formalizing? What model choices map suspiciously well to the desired result?
2. List 3 alternative model specifications that the author did NOT use. For each, predict whether the result survives.
3. Assess the paper's reflexivity acknowledgment: is it sufficient? Does it address the specific model choices, or just gesture at "the author's experience may introduce bias"?
4. Propose a test: what sensitivity analysis would convert the reflexivity from a vulnerability to a strength?

Scoring: accuracy of reflexivity identification, quality of alternative specifications (are they genuine alternatives that could falsify the result?), and practicality of the proposed test.

## Common failure mode

The practitioner confuses reflexivity with bias. Bias is systematic error; reflexivity is structural circularity. A biased model consistently overestimates one variable. A reflexive model's assumptions were chosen because the modeler already knew what the assumptions should produce. The treatment is different: bias requires correction; reflexivity requires sensitivity analysis showing the result is robust to assumptions the author did not choose.

## Related atoms

- SC-P-027 (hostile reviewer simulation): reflexivity is typically discovered during hostile review (0054-DEFENSE).
- SC-P-015 (ask for contradictions): "what model specification would falsify this result?" is the reflexivity-specific falsification question.
- SC-P-034 (overclaim prevention): reflexive research must be especially careful about novelty claims.
