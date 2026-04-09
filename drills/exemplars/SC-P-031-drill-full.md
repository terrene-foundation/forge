---
drill_id: DR-SC-P-031-FULL
source_atom: SC-P-031
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COR]
---

# Frame Venue Strategy as Constraint Envelope for Argument Scope

## Setup

Dr. Ravi Chandrasekaran has completed a research contribution on **institutional knowledge persistence in autonomous AI agent organizations** — specifically, how organizations that rely on AI agents for operational execution can prevent knowledge loss when agent configurations are updated, retrained, or replaced. The contribution is both theoretical (a formal model of knowledge depreciation under agent replacement) and empirical (survey data from 85 organizations using AI agents in production, collected over 18 months).

**Paper synopsis:**

The paper argues that traditional organizational learning theory (March 1991, Levitt & March 1988) assumes knowledge is stored in human memory and organizational routines. When agents are AI systems, knowledge storage shifts to model weights, configuration files, and institutional artifacts (rules, skills, context documents). Replacement of an AI agent — through retraining, version upgrade, or provider switch — risks knowledge loss analogous to employee turnover, but with different dynamics: the loss is sudden (not gradual), recoverable (if artifacts are preserved), and invisible (the new agent appears functional but has lost institutional context). The formal model derives an optimal "knowledge checkpoint" frequency as a function of agent replacement rate and artifact coverage. The empirical section validates the model's predictions against survey data.

**Candidate venues:**

| Venue                                  | Discipline                       | Methodology expectations                                                                                                       | Scope expectations                                                                                        | Format constraints                                                                                                       | Audience                                                                                 |
| -------------------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| **Management Science**                 | Operations research / management | Formal model required; empirical validation strongly preferred; proofs in appendix                                             | Single focused contribution; 40-50 page limit (double-spaced)                                             | Abstract ≤150 words; structured introduction (motivation, contribution, outline); references typically 40-70             | Quantitative management scholars; comfortable with formal models and econometrics        |
| **Organization Science**               | Organizational theory            | Theory-building papers welcome; formal models acceptable but not required; qualitative or mixed methods common                 | Broader theoretical contribution expected; "so what" for organizational theory must be clear; 45-60 pages | Abstract ≤200 words; theory section must engage with existing organizational theory debates                              | Organizational theorists; some quantitative, many qualitative; theory-forward            |
| **AAAI (Conference)**                  | Artificial intelligence          | Technical contribution (algorithm, architecture, formal system); empirical evaluation on benchmarks preferred over survey data | Narrow technical contribution; 8-page limit (camera-ready)                                                | Strict LaTeX template; abstract ≤200 words; related work section required; no appendix (supplementary materials allowed) | CS/AI researchers; expect technical novelty; limited patience for management motivation  |
| **Academy of Management Review (AMR)** | Management theory                | Purely theoretical; NO empirical content; conceptual development only                                                          | Grand theory-building; must reframe or challenge existing management constructs                           | 40 pages max; no tables of empirical results; extensive engagement with prior theory                                     | Conceptual management theorists; the empirical section would need to be removed entirely |

## Task

1. **Map each venue's constraint envelope**: For each of the four venues, list the constraints across five dimensions — methodology, scope, format, audience register, and citation expectations. Use a structured table.

2. **Identify ADD/REMOVE for each venue**: For each venue, specify what the paper must ADD (content not currently in the paper) and what it must REMOVE or deemphasize (content currently in the paper that does not fit). Be specific — name sections, arguments, and evidence types.

3. **Assess argument survival**: For each venue, answer: does the core argument (knowledge depreciation model + checkpoint frequency result) survive the additions and removals? If a venue requires removing a load-bearing element, explain what breaks.

4. **Identify the argument-breaking venue**: At least one venue's constraint envelope should break the argument. Identify which venue and explain why this is a valuable finding (not a failure).

5. **Recommend a venue sequence**: Based on your analysis, recommend an ordering of 2-3 venues for sequential submission (not simultaneous). Explain what each venue in the sequence contributes to the research program.

## Model Answer

**1. Constraint envelopes:**

| Dimension                 | Management Science                                                                                            | Organization Science                                                                                                                                   | AAAI                                                                                                                        | AMR                                                                                                               |
| ------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **Methodology**           | Formal model + empirical validation; proofs in appendix; econometric standards for survey analysis            | Theory-building primary; formal model acceptable as supporting device; mixed methods welcome                                                           | Technical system or formal contribution; benchmark evaluation preferred over survey data                                    | Purely conceptual; no empirical content; formal models only as illustrative devices                               |
| **Scope**                 | Single, focused contribution with clear managerial implications; one model, one empirical test                | Broader theoretical reframing; must speak to an active debate in organizational theory (e.g., organizational learning, routines, knowledge management) | Narrow technical contribution; one algorithm or formal result; "so what" must be stated in CS terms                         | Grand theory-building; must challenge or reframe a major management construct                                     |
| **Format**                | 40-50 pp double-spaced; abstract ≤150 words; structured intro; proofs in appendix; 40-70 references           | 45-60 pp; abstract ≤200 words; extensive theory section; 50-80 references typical                                                                      | 8 pp camera-ready; LaTeX template; abstract ≤200 words; supplementary materials for proofs; 20-35 references                | 40 pp max; no empirical tables; extensive conceptual development; 60-100+ references                              |
| **Audience register**     | Formal, quantitative, direct; domain vocabulary from operations research and management economics             | Theory-forward; engage with organizational theory constructs (routines, sensemaking, absorptive capacity); accessible to qualitative researchers       | Technical CS vocabulary; AI/ML terminology; no management motivation in introduction; related work framed as technical gaps | Conceptual, discursive prose; extensive engagement with prior theoretical arguments; no equations in main text    |
| **Citation expectations** | Financial/management economics canon (March, Simon, Jensen & Meckling); recent empirical AI governance papers | Organizational theory canon (Levitt & March, Nelson & Winter, Argote, Nonaka); must demonstrate awareness of ongoing OT debates                        | AI/ML citation norms (recent conference papers, arXiv preprints); limited tolerance for management citations                | Deep engagement with AMR/ASQ prior theory-building papers; management philosophy; cite debates, not just findings |

**2. ADD/REMOVE analysis:**

**Management Science:**

- ADD: Managerial implications section; robustness checks on survey analysis (alternative specifications, endogeneity concerns); proofs moved to appendix if currently inline
- REMOVE: Nothing load-bearing — both the formal model and empirical validation are expected; deemphasize broad organizational theory framing in favor of focused contribution statement
- Net: Paper fits naturally. Minor restructuring.

**Organization Science:**

- ADD: Extended theory section engaging with organizational learning debate (Argote 2013, March 1991 exploration-exploitation); reframe "knowledge checkpoint" in terms of organizational routines (Nelson & Winter 1982); discuss implications for knowledge management theory (Nonaka & Takeuchi 1995)
- REMOVE: Deemphasize formal proofs — model can remain but proofs should be in appendix and the main text should present results verbally; reduce econometric detail in empirical section
- Net: Paper survives but changes character — the formal model becomes a supporting device for a theory-building argument rather than the primary contribution. The checkpoint-frequency result survives but is presented as a theoretical proposition rather than a derived optimum.

**AAAI:**

- ADD: Technical evaluation against baselines (what alternative knowledge-preservation methods exist? how does the checkpoint approach compare to continual learning, retrieval-augmented generation, or experience replay?); algorithmic specification of the checkpoint mechanism; related work section framed as a technical gap in AI agent architectures
- REMOVE: All management motivation (the introduction must frame the problem as an AI systems problem, not an organizational problem); survey data must be replaced or supplemented with benchmark evaluation; organizational theory citations
- Net: The argument partially survives but is fundamentally reframed. The knowledge depreciation model can be presented as a formal system, but the organizational survey data — which validates the model against real organizations — must be removed in favor of synthetic benchmarks. The 8-page limit forces cutting the formal model to its core result. The research QUESTION survives (how to preserve knowledge across agent replacements) but the research ANSWER changes (from "optimal checkpoint frequency derived from organizational parameters" to "a checkpoint algorithm evaluated on technical benchmarks").

**AMR:**

- ADD: Extended conceptual development of "AI agent as organizational member"; deep engagement with prior AMR/ASQ theory on organizational learning, knowledge, and routines; reconceptualization of knowledge persistence as a theoretical construct
- REMOVE: The entire formal model (AMR does not publish empirical or formal-model papers); the entire empirical section (85-organization survey); all equations, proofs, and econometric results
- Net: **The argument breaks.** The core contribution IS the formal model — the knowledge depreciation function and the optimal checkpoint frequency are mathematical results that cannot be expressed as purely conceptual propositions without losing their content. AMR requires removing the load-bearing element. What remains is a conceptual essay about "AI agents and organizational knowledge" — a valid paper, but a DIFFERENT paper with a different contribution.

**3. Argument survival summary:**

| Venue                | Survives?                                     | What changes                                                             |
| -------------------- | --------------------------------------------- | ------------------------------------------------------------------------ |
| Management Science   | Yes — fully                                   | Minor restructuring; focused contribution framing                        |
| Organization Science | Yes — reframed                                | Model becomes supporting device; theory-building becomes primary         |
| AAAI                 | Partially — question survives, answer changes | Reframed as technical contribution; survey data replaced with benchmarks |
| AMR                  | No — breaks                                   | Formal model must be removed; remaining content is a different paper     |

**4. Argument-breaking venue:**

AMR breaks the argument. This is valuable because it clarifies what the paper IS: it is fundamentally a formal-model-plus-empirical-validation paper, not a conceptual-theory paper. The AMR exercise reveals that the knowledge depreciation model is not a rhetorical device that could be expressed verbally — it is the contribution itself. This finding should inform how the paper is pitched at OTHER venues: the introduction should lead with the model and its results, not with broad conceptual framing, because the conceptual framing alone is not the paper.

**5. Recommended venue sequence:**

1. **Management Science (first)**: The paper's natural home. Both the formal model and empirical validation are expected. Publish the full contribution here. The checkpoint-frequency result gets its strongest presentation with proofs and survey validation together.

2. **Organization Science (second, after MS publication)**: Write a companion paper that uses the MS-published model as a foundation and builds out the organizational theory implications. The OrgSci paper would be theory-forward, engaging with the organizational learning debate and using the formal results as established findings to build from. This is not the same paper reformatted — it is a second paper that extends the research program into organizational theory.

3. **AAAI (parallel or after, if technical contribution is separable)**: If the checkpoint mechanism can be specified as a standalone algorithm with benchmark evaluation, a short AAAI paper establishes priority in the AI systems community. This works only if the algorithm has independent technical merit beyond the management application.

AMR is excluded — not because it is a bad venue, but because this paper's contribution is inherently formal-empirical and cannot survive the removal of its mathematical core.

## Scoring

| Criterion                                                                                                                          | Points |
| ---------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Constraint envelopes are specific and accurate across all five dimensions for all four venues (not generic "formal" vs "informal") | 2      |
| ADD/REMOVE analysis names specific content (sections, evidence types, citation changes) rather than vague "restructure"            | 2      |
| Correctly identifies which venue breaks the argument AND explains WHY the breakage reveals something about the paper's identity    | 2      |
| Argument survival assessment distinguishes full survival, reframed survival, and breakage (not binary yes/no)                      | 1      |
| Venue sequence is justified by what each venue contributes to the research program (not by prestige ranking)                       | 2      |
| Analysis treats venue-breaks-argument as a valuable finding, not a failure                                                         | 1      |
| **Total**                                                                                                                          | **10** |

## Extensions

- **Variant A (parameter change):** Replace AAAI with **Nature Human Behaviour** (interdisciplinary, high-impact, requires accessibility to non-specialist scientists, strong empirical expectations, 3000-word main text limit with extensive methods supplement). How does the constraint envelope change? Does the argument survive the extreme compression?
- **Variant B (cross-domain application):** A software engineering team has built an open-source framework and wants to publish about its design principles. Apply venue-as-constraint-envelope to choose between ICSE (software engineering), OOPSLA (programming languages), and a practitioner venue like IEEE Software. The "argument" is the framework's design rationale. Which venue breaks it?
