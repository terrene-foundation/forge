---
drill_id: DR-SC-P-027-FULL
source_atom: SC-P-027
difficulty: intermediate
time_box_minutes: 30
modality: drill
application: [COR]
---

# Hostile Reviewer Simulation as Preemptive Defense

## Setup

The practitioner receives a 2-page argument excerpt from a working paper titled _"Bounded Delegation: A Formal Model of Human-AI Authority Allocation Under Uncertainty."_ The paper targets the _American Economic Review: Papers and Proceedings_ (AER P&P) — a venue known for concise, technically rigorous contributions evaluated by economists who expect formal derivations, not verbal arguments. The paper has one shot: AER P&P submissions are 5 pages maximum with no revision opportunity.

**Argument excerpt:**

> **Section 2: Model Setup**
>
> Consider an organization with a principal (human manager) and an agent (AI system). The principal delegates a set of decisions $D = \{d_1, d_2, \ldots, d_n\}$ to the agent. Each decision $d_i$ has an optimal action $a_i^*$ that depends on a state of the world $\theta_i$ drawn from a known distribution $F_i$.
>
> The AI agent observes a signal $s_i$ about $\theta_i$ with precision $\pi_A > \pi_H$, where $\pi_H$ is the principal's signal precision. That is, the AI is strictly more accurate than the human on every individual decision.
>
> **Proposition 1:** _Despite $\pi_A > \pi_H$ for all $i$, full delegation ($D_{delegated} = D$) can produce lower organizational payoff than partial delegation ($D_{delegated} \subset D$)._
>
> **Proof sketch:** The principal's organizational payoff depends not only on individual decision quality but also on coherence across decisions. Define the coherence function $C(a_1, \ldots, a_n) = 1 - \frac{1}{n}\sum_{i<j} \mathbb{1}[a_i \text{ contradicts } a_j]$. Under full delegation, the AI optimizes each $d_i$ independently, maximizing individual accuracy but ignoring cross-decision coherence. Under partial delegation, the human retains a subset of correlated decisions $D_H \subseteq D$ and ensures coherence on $D_H$ despite lower individual precision.
>
> We show that for $n$ sufficiently large and inter-decision correlation $\rho > \bar{\rho}$, the coherence loss from full delegation exceeds the precision gain, yielding:
>
> $$E[U(D_{partial})] > E[U(D_{full})]$$
>
> **Proposition 2:** _The optimal delegation envelope $D^__{delegated}$ is characterized by a threshold correlation: delegate $d_i$ if and only if $\max_{j \neq i} \rho\_{ij} < \hat{\rho}(\pi_A, \pi_H, n)$.\*
>
> This threshold result implies that delegation decisions should be based on the correlation structure of the decision space, not on the relative accuracy of human vs AI on individual decisions. Decisions that are highly correlated with other decisions should be retained by the human regardless of AI accuracy, because the coherence cost of independent optimization exceeds the precision benefit.
>
> **Section 3: Implications**
>
> Our model provides a micro-foundation for "delegation envelopes" — formal boundaries on AI authority derived from the correlation structure of organizational decisions. This contributes to the literature on human-AI collaboration by showing that the delegation question is not "which decisions should AI make?" but "which decisions must remain jointly governed to preserve coherence?"
>
> The result is novel in three ways: (1) it formalizes the delegation decision as a graph-theoretic problem on the correlation structure, (2) it shows that accuracy superiority is neither necessary nor sufficient for optimal delegation, and (3) it derives a closed-form threshold that practitioners can compute from observable correlation data.

**Venue context:** AER P&P reviewers are professional economists with specializations in contract theory, mechanism design, organizational economics, or information economics. They expect:

- Formal models with clearly stated assumptions and derived (not asserted) results
- Engagement with the canonical literature (Holmstrom, Hart-Moore, Aghion-Tirole)
- Awareness of the distinction between positive results (what IS) and normative claims (what SHOULD BE)
- Skepticism toward "novel" claims that are actually special cases of known results

## Task

1. **Define the reviewer persona.** Specify a hostile reviewer for AER P&P: their disciplinary specialty, what they have published, what biases they bring to this submission, and what they consider a desk-rejection trigger. The persona must be specific enough to generate venue-calibrated critique (not a generic "tough reviewer").

2. **Run the hostile review.** In the voice of your reviewer persona, critique the argument excerpt. Identify:
   - At least 1 FATAL issue (a flaw that, if unaddressed, means the paper cannot be published at this venue)
   - At least 2 MAJOR issues (flaws that require substantial revision)
   - At least 2 desk-rejection triggers (signals that would cause a referee to recommend rejection in the first paragraph of the report)

3. **For each finding, write three elements:**
   - **The attack:** What specifically is wrong (quote the problematic passage)
   - **Why it matters:** What the referee report would say (in the reviewer's voice)
   - **Defense or revision path:** How the author should respond — and whether the response is fixable-in-revision or requires-fundamental-rethink

4. **Assess submission readiness.** Based on the hostile review, answer: should this paper be submitted to AER P&P as written? If not, what is the minimum viable revision that makes it submittable?

## Model Answer

**1. Reviewer persona:**

**Professor R** — a contract theory specialist who has published on delegation and authority allocation in the Aghion-Tirole tradition. Has 3 papers in Econometrica and 1 in JPE on optimal delegation under moral hazard. Biases: deeply skeptical of papers that claim "novelty" for results that are special cases of known delegation models; considers verbal proof sketches in formal papers to be a failure of rigor; views any paper that ignores Aghion & Tirole (1997) as evidence the author is unfamiliar with the field. Desk-rejection triggers: (a) a "proposition" that is actually an assumption dressed as a result, (b) a novelty claim that is falsified by a known result in the literature, (c) absence of the canonical delegation literature.

**2. & 3. Hostile review findings:**

---

**FATAL Issue: Proposition 1 is not derived — it is assumed.**

- **The attack:** The proof sketch defines a coherence function $C$ and asserts that "for $n$ sufficiently large and $\rho > \bar{\rho}$, the coherence loss exceeds the precision gain." But this is not a derivation — it is a restatement of the proposition's claim using mathematical notation. No mechanism is specified for WHY independent optimization destroys coherence. The AI agent could in principle optimize for coherence as well as accuracy if instructed to do so. The proposition assumes that the AI optimizes each decision independently, but this is a design choice, not a structural feature of AI systems.

- **Why it matters (referee voice):** "Proposition 1 is the paper's central result, yet it is not derived from primitives. The authors assume that the AI agent optimizes each decision independently (the $\mathbb{1}[a_i \text{ contradicts } a_j]$ coherence function is imposed, not derived from the agent's optimization problem). This is equivalent to assuming the conclusion. A well-designed AI agent given the objective function $U$ that includes coherence would optimize for coherence. The proposition should read: 'If the AI agent's objective function excludes cross-decision coherence, then full delegation produces lower organizational payoff.' But this is trivially true and not a contribution — it says that an agent optimizing the wrong objective produces suboptimal results."

- **Defense or revision path:** **Requires fundamental rethink.** The authors need either (a) a micro-foundation for why AI agents optimize independently — perhaps an information-processing constraint, a communication cost, or a mechanism design result showing that coherent multi-decision optimization is not incentive-compatible — or (b) reformulation of Proposition 1 as a characterization of when independent optimization is costly, conditional on the assumption being stated explicitly as an assumption rather than hidden in the proof.

---

**MAJOR Issue 1: No engagement with Aghion & Tirole (1997).**

- **The attack:** The implications section claims novelty for "formalizing the delegation decision" and "deriving a threshold," but Aghion & Tirole (1997) already formalize delegation as a trade-off between the principal's loss of control and the agent's improved initiative. The paper does not cite, engage with, or differentiate from the canonical delegation model.

- **Why it matters (referee voice):** "The authors claim three forms of novelty. At minimum, claim (1) — formalizing delegation as a structural problem — must be situated against Aghion & Tirole, who did exactly this in 1997 with a model that has been cited over 3,000 times. The absence of this citation suggests unfamiliarity with the field, which is a serious concern for a submission to AER."

- **Defense or revision path:** **Fixable in revision.** Add a paragraph in Section 2 that distinguishes the model from Aghion-Tirole: the present model's contribution is the correlation-structure dimension (Aghion-Tirole consider bilateral delegation of a single decision; this model considers simultaneous delegation of correlated decisions). The novelty claim becomes: "We extend the Aghion-Tirole delegation framework from single decisions to correlated decision portfolios."

---

**MAJOR Issue 2: The coherence function is ad hoc.**

- **The attack:** The coherence function $C(a_1, \ldots, a_n) = 1 - \frac{1}{n}\sum_{i<j} \mathbb{1}[a_i \text{ contradicts } a_j]$ is presented without derivation or justification. What does "contradicts" mean formally? The indicator function $\mathbb{1}[a_i \text{ contradicts } a_j]$ is not defined in terms of the model's primitives ($\theta, s, \pi, F$).

- **Why it matters (referee voice):** "The entire result hinges on the coherence function, yet it is imposed rather than derived. The paper needs to specify the payoff structure that makes coherence valuable. Is coherence a complementarity in the production function? A constraint on downstream processes? A preference of a downstream consumer? Without a micro-foundation, the coherence function is a modeling degree of freedom that the authors exploit to get the result they want."

- **Defense or revision path:** **Fixable in revision.** Define coherence through the organizational payoff function: $U = \sum_i v(a_i, \theta_i) + \lambda \sum_{i<j} g(a_i, a_j)$ where $g$ captures complementarities. The coherence cost then emerges from the cross-terms rather than from an imposed indicator function. This is a modeling improvement, not a rethink.

---

**Desk-rejection trigger 1: "Novel in three ways" framing.**

- **The attack:** The implications section claims the result is "novel in three ways." In a 5-page AER P&P submission, claiming triple novelty for a model with one proposition and one proof sketch signals overclaiming.

- **Why it matters (referee voice):** "Claiming three novelties is a red flag at this venue. AER P&P contributions are expected to be precise and modest. One clean result, clearly situated in the literature, is sufficient. The three-novelty framing invites scrutiny of each claim individually, and as noted above, claim (1) overlaps with Aghion-Tirole, which undermines the entire novelty argument."

- **Defense or revision path:** **Fixable.** Replace the three-novelty list with a single-sentence contribution statement: "We extend Aghion-Tirole delegation to multi-decision settings where the correlation structure of the decision space determines the optimal delegation boundary."

---

**Desk-rejection trigger 2: "Proof sketch" in a formal paper.**

- **The attack:** Proposition 1 is supported by a "proof sketch" rather than a proof. At AER P&P, formal propositions require formal proofs (even if concise). A proof sketch is appropriate for a workshop presentation, not a proceedings paper.

- **Why it matters (referee voice):** "The authors present a formal proposition with mathematical notation, then support it with a verbal argument labeled 'proof sketch.' This mismatch between the formality of the claim and the informality of the support is a signal of incomplete work. If the proof is available, include it. If it is not, the proposition is a conjecture, not a result."

- **Defense or revision path:** **Fixable.** Either (a) compress the full proof into the 5-page limit by cutting the implications section, or (b) relegate the proof to an online appendix and state "Proof in Online Appendix A" with a clear derivation path.

---

**4. Submission readiness assessment:**

**Not submittable as written.** The FATAL issue (Proposition 1 assumes its conclusion) requires rethinking the model's micro-foundations before any other fix matters. The two MAJOR issues are fixable but depend on the FATAL issue being resolved first — engagement with Aghion-Tirole and a derived coherence function both require that the core model be sound.

**Minimum viable revision:** (1) Add an explicit assumption about why the AI agent cannot optimize for coherence (information-processing cost, communication cost, or incentive constraint), making Proposition 1 a genuine result conditional on a stated assumption. (2) Cite and differentiate from Aghion-Tirole. (3) Replace the proof sketch with a proof. (4) Replace the three-novelty framing with a single contribution statement. Estimated effort: fundamental rethink of the model setup (the FATAL fix), plus approximately 2 pages of rewriting.

## Scoring

| Criterion                                                                                                                        | Points |
| -------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Reviewer persona is venue-specific with stated biases and desk-rejection triggers (not a generic "tough reviewer")               | 1      |
| FATAL issue correctly identifies that a core result is assumed rather than derived, with explanation of why                      | 3      |
| MAJOR issues target substantive flaws (missing literature, ad hoc modeling) rather than surface issues (formatting, length)      | 2      |
| Each finding includes all three elements: attack, referee-voice explanation, and defense/revision path                           | 2      |
| Desk-rejection triggers identify signals (overclaiming, proof sketch in a formal venue) that cause immediate negative assessment | 1      |
| Submission readiness assessment distinguishes fundamental-rethink items from fixable-in-revision items                           | 1      |
| **Total**                                                                                                                        | **10** |

## Extensions

- **Variant A (parameter change — different venue):** Rerun the hostile review targeting _Organization Science_ instead of AER P&P. The reviewer persona changes: an organizational theorist replaces the contract theorist. The FATAL issue may shift — OrgScience reviewers may accept a verbal proof sketch but will demand richer institutional context and empirical motivation. The Aghion-Tirole gap becomes less severe (the org theory audience may not expect contract theory citations), but a new gap emerges: no engagement with the organizational design literature (Puranam, Burton & Obel). The exercise tests whether the practitioner can recalibrate the review for a different venue's standards rather than applying a single "tough review" template.
- **Variant B (cross-domain — hostile review of a policy proposal):** Apply the hostile reviewer simulation to a policy document rather than an academic paper. The practitioner receives a 2-page policy brief proposing AI governance regulation. The reviewer persona is a senior policy advisor who has seen dozens of similar proposals fail. The failure modes shift: instead of "proposition assumes its conclusion," the policy analogue is "the regulation assumes compliance without specifying an enforcement mechanism." The drill tests whether the practitioner can transfer the hostile-review skill from academic argumentation to policy argumentation, where the standards of "rigor" are different but the structure of preemptive defense is the same.
