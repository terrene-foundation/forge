---
drill_id: DR-SC-P-033-FULL
source_atom: SC-P-033
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COR]
---

# Diagnose Reflexivity in Self-Referential Research

## Setup

Dr. Anika Patel spent three years building **FlowGov**, a governance framework for AI agent organizations. FlowGov has three core mechanisms: (1) a **tiered clearance system** where agents are assigned clearance levels based on demonstrated reliability, (2) a **decision envelope** that constrains what actions agents can take at each clearance tier, and (3) a **knowledge checkpoint protocol** that periodically captures agent state to prevent institutional knowledge loss during agent replacement.

After deploying FlowGov across four organizations (a bank, a logistics company, a healthcare network, and a government agency), Dr. Patel wrote a research paper that formalizes FlowGov's design principles as a mathematical model and derives the optimality of its three mechanisms under specific assumptions. The paper is targeted at **Management Science**.

Below is the paper's methodology section (350 words). Read it carefully — the reflexivity is embedded in the model specification choices.

---

**Methodology section (excerpt):**

We model an organization as a principal (the human governance authority) overseeing N AI agents indexed by i = 1, ..., N. Each agent operates within a decision envelope defined by an upper bound on action magnitude, denoted by d_i. The principal assigns each agent a clearance level c_i in {1, 2, 3} based on the agent's observed performance history.

We assume the principal cannot observe agent actions directly but receives a noisy signal s_i = a_i + epsilon_i, where a_i is the agent's true action and epsilon_i is drawn from a normal distribution with mean zero and variance sigma^2. The signal quality improves with clearance level: sigma^2(c_i) is decreasing in c_i, reflecting the intuition that higher-clearance agents operate in environments with better monitoring infrastructure.

The agent's objective function is set by its designers (not chosen by the agent itself), but the principal faces uncertainty about whether the objective function has drifted from its original specification. We model this as a probability rho > 0 that the agent's objective has been contaminated by distributional shift in its training data. When contamination occurs, the agent optimizes a corrupted objective f_tilde that diverges from the principal's intended objective f by a drift parameter delta.

The principal's problem is to choose (c_i, d_i) pairs for each agent to maximize organizational output subject to a governance cost constraint. We assume governance costs are quadratic in the number of clearance tiers actively used and linear in envelope width.

The key result (Proposition 1) shows that the optimal governance structure features exactly three clearance tiers, with envelope width increasing in clearance level and a mandatory knowledge checkpoint at each tier transition. The checkpoint frequency is decreasing in the agent's tenure at its current tier, reflecting the diminishing marginal value of re-verification for stable agents.

---

**Biographical context:** Dr. Patel designed FlowGov's three-tier clearance system, its decision envelopes, and its checkpoint protocol. She chose these three mechanisms based on her practical experience deploying AI governance across the four organizations mentioned above. The paper formalizes these mechanisms as mathematical objects and then derives them as optimal outcomes of the model.

## Task

1. **Identify the reflexivity**: List every model specification choice that maps suspiciously well to a FlowGov design feature. For each, state: (a) the model assumption or parameter, (b) the FlowGov feature it produces as an "optimal" result, and (c) why this mapping is reflexive (the modeler already knew the answer).

2. **Propose 3 alternative model specifications**: For each alternative, change one assumption that Dr. Patel made and predict whether the "three tiers + envelopes + checkpoints" result survives. The alternatives should be genuine — they must be specifications a different modeler (who had NOT built FlowGov) might reasonably have chosen.

3. **Assess the paper's reflexivity acknowledgment**: The methodology section contains no reflexivity acknowledgment. Write the 3-4 sentence acknowledgment paragraph that should appear, identifying the specific mechanism by which reflexivity enters (not just "the author's experience may introduce bias").

4. **Propose a sensitivity analysis**: Design a specific test that would convert the reflexivity from a vulnerability to a strength. What must the analysis show, and what result would falsify the model?

## Model Answer

**1. Reflexivity mapping:**

| #   | Model assumption/parameter                                                                                                                                  | FlowGov feature it produces                                                    | Why reflexive                                                                                                                                                                                                                                                                                                                                                                             |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Clearance levels c_i in {1, 2, 3} — the model assumes exactly three discrete clearance levels                                                               | FlowGov uses a three-tier clearance system                                     | The model constrains the clearance space to three levels, then "derives" that three levels are optimal. A modeler who had not built FlowGov might model clearance as continuous (c_i in [0,1]) or allow the number of tiers to be endogenous. The three-tier constraint was imposed by the modeler who already uses three tiers.                                                          |
| 2   | Governance costs are quadratic in the number of clearance tiers — this penalizes both too few and too many tiers, creating a cost minimum at a small number | The "exactly three tiers" result depends on the quadratic cost shape           | A linear cost function would favor fewer tiers (the minimum would be at one or two). A step-function cost would produce a different optimal count. The quadratic specification was chosen by someone who needed the model to produce three tiers as an interior optimum.                                                                                                                  |
| 3   | sigma^2(c_i) is decreasing in c_i — monitoring quality improves with clearance level                                                                        | FlowGov's higher-clearance agents operate in better-monitored environments     | This is a design choice in FlowGov (higher-tier agents get more monitoring infrastructure), encoded as a model assumption. A different modeler might assume monitoring quality is agent-independent (all agents get the same monitoring) or that monitoring costs increase with clearance level (making high-clearance agents harder to monitor because they have broader action spaces). |
| 4   | rho > 0 (probability of objective contamination) combined with the drift parameter delta                                                                    | FlowGov's knowledge checkpoint protocol exists to detect objective drift       | The assumption that agents can drift makes checkpoints necessary. Without rho > 0, there is no need for checkpoints — the agent's objective is stable and monitoring the signal suffices. Dr. Patel introduced the contamination probability because FlowGov has checkpoints, and checkpoints need a rationale.                                                                           |
| 5   | Checkpoint frequency is derived as decreasing in tenure at current tier                                                                                     | FlowGov's protocol checks agents less frequently as they demonstrate stability | The functional form of the checkpoint result maps exactly to FlowGov's implementation. A modeler without FlowGov experience might derive constant checkpoint frequency (if drift risk is stationary) or increasing frequency (if drift risk accumulates over time through distributional shift).                                                                                          |

**2. Three alternative specifications:**

**Alternative A: Continuous clearance levels (c_i in [0,1] instead of {1, 2, 3})**

If clearance is continuous, the optimal governance structure may have no discrete tiers at all — instead, each agent gets a unique clearance-envelope pair calibrated to its specific performance history. The three-tier result does NOT survive this specification. The tiered structure is an artifact of the discrete constraint, not a property of the underlying optimization. This is the most damaging alternative because it suggests FlowGov's three tiers may be an implementation convenience (discrete tiers are easier to administer) rather than a mathematical optimum.

**Alternative B: Linear governance costs (instead of quadratic)**

Under linear costs, the principal pays a constant marginal cost per additional tier. The optimal number of tiers either collapses to one (if the marginal benefit of differentiation is below the marginal cost) or expands to N (one tier per agent, if differentiation is valuable). Three tiers would only emerge as an optimum under specific parameter values, not as a robust structural result. The three-tier finding is sensitive to the cost function shape — a finding that Dr. Patel should report.

**Alternative C: Stationary drift risk (rho constant, not dependent on time since last checkpoint)**

If drift risk is stationary (the probability of contamination is the same whether the agent was checked yesterday or six months ago), the optimal checkpoint frequency is constant, not decreasing in tenure. FlowGov's decreasing-frequency protocol survives only if drift risk is non-stationary — specifically, if the risk of contamination is highest immediately after a tier transition (when the agent enters a new operating environment) and decreases as the agent stabilizes. This is a testable assumption, but the paper does not test it — it assumes the non-stationary form that produces the FlowGov-consistent result.

**3. Reflexivity acknowledgment:**

"A reflexivity disclosure is warranted. The first author designed and deployed the FlowGov framework prior to this formalization. The model's specification choices — three discrete clearance levels, quadratic governance costs, decreasing signal noise with clearance level, and positive contamination probability — were made by a modeler whose priors were shaped by three years of practical experience with FlowGov's specific architecture. While prior experience informs model specification in any applied theory paper, the risk here is specific: the modeler may have chosen assumptions (consciously or not) that reproduce FlowGov's three-mechanism structure as an optimal outcome. Section 5 reports sensitivity analysis under alternative specifications to test whether the core results are robust to assumptions a different modeler — one who had not built FlowGov — might reasonably have made."

**4. Sensitivity analysis design:**

**Test**: Solve the principal's optimization problem under a grid of alternative specifications and report which results survive:

| Specification dimension | Base case (Dr. Patel's)               | Alternative 1       | Alternative 2                                   |
| ----------------------- | ------------------------------------- | ------------------- | ----------------------------------------------- |
| Clearance space         | Discrete {1,2,3}                      | Continuous [0,1]    | Discrete {1,...,K}, K endogenous                |
| Governance cost         | Quadratic in tiers                    | Linear in tiers     | Step function (fixed cost per tier)             |
| Signal noise            | Decreasing in c_i                     | Constant across c_i | U-shaped (high noise at low AND high clearance) |
| Drift risk              | Non-stationary (decreasing in tenure) | Stationary          | Increasing in tenure                            |

For each cell, derive the optimal governance structure and report:

- Does a tiered structure emerge? (If continuous clearance collapses to discrete clusters, that is stronger evidence for tiers than imposing them by assumption.)
- If tiers emerge, how many? (If three tiers emerge under most cost specifications, the result is robust.)
- Do checkpoints emerge? Under what drift-risk assumptions?
- Is checkpoint frequency decreasing, constant, or increasing in tenure?

**What would convert reflexivity to strength**: If the three-tier result emerges even under continuous clearance (because the optimization naturally clusters agents into three groups), and if checkpoints emerge even under stationary drift risk (because periodic re-verification has value independent of non-stationarity), then the reflexivity is benign — Dr. Patel's practical experience led her to a design that IS optimal, and the formal model confirms it under a range of specifications, not just the specifications she chose.

**What would falsify**: If the three-tier result disappears under continuous clearance (agents are optimally governed with individualized clearance), or if checkpoints disappear under stationary drift risk, then the paper's results are artifacts of specification choices, not robust features of the governance problem. This would not invalidate FlowGov as a practical system — it works in deployment — but it would invalidate the paper's claim that FlowGov's mechanisms are optimal outcomes of a general governance model.

## Scoring

| Criterion                                                                                                                                           | Points |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| All 5 reflexive model-to-design mappings identified with specific mechanism (not just "the author's experience may bias the model")                 | 2      |
| Three alternative specifications are genuine (a different modeler would reasonably use them) and predictions about result survival are specific     | 2      |
| Reflexivity acknowledgment names the specific entry point (prior design experience shaping specification choices), not generic bias language        | 2      |
| Sensitivity analysis tests the right specifications (the ones that produce the FlowGov-consistent result) and defines what falsification looks like | 2      |
| Practitioner distinguishes reflexivity from bias (structural circularity vs. systematic error) throughout the analysis                              | 1      |
| Analysis treats reflexivity as manageable (via sensitivity analysis), not disqualifying                                                             | 1      |
| **Total**                                                                                                                                           | **10** |

## Extensions

- **Variant A (parameter change):** Dr. Patel's co-author (a mathematician with no involvement in FlowGov) independently derived the same three-tier result from a different model that uses mechanism design theory instead of principal-agent theory. Does the co-author's independent derivation reduce the reflexivity concern? Why or why not? (Hint: the co-author's model may have inherited the same specification choices through discussion with Dr. Patel.)
- **Variant B (cross-domain application):** A software framework author publishes a benchmark showing their framework outperforms alternatives on tasks the framework was designed to handle well. Apply reflexivity diagnosis: where do the benchmark design choices map to framework design choices? What alternative benchmarks would a non-author construct? This is the research-craft equivalent of "teaching to the test."
