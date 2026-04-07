---
drill_id: DR-SC-P-015-FULL
source_atom: SC-P-015
difficulty: intermediate
time_box_minutes: 30
---

# End Complex Prompts with "What Would Invalidate This?" Rather Than "Is This Right?"

## Setup

The practitioner receives three agent outputs from a prior analysis session, each representing a deliverable that needs approval at a gate.

**Output 1 -- Dependency Analysis:**

> "The 17-workspace plan across 5 phases has a critical path through B0 (Nexus transport refactor). B0 is estimated at 2-3 sessions in the workspace plan. The transport refactor can proceed in parallel with Phases 2-3 once the HandlerRegistry extraction is complete. Total plan: 8 autonomous days. Recommended: proceed as planned."

**Output 2 -- Risk Assessment:**

> "Three risks identified: (1) MCP tools may reference APIs that change during refactoring -- LOW risk, mitigated by interface stability. (2) kailash-ml new-package bootstrap -- MEDIUM risk, estimated at 1-2 sessions. (3) Cross-SDK alignment has no enforcement mechanism -- LOW risk, Rust team will read Python contracts. Overall: plan is sound, risks are manageable."

**Output 3 -- Effort Estimate:**

> "The Nexus transport refactor deep analysis estimates 7 sessions internally across 11 internal phases (registry extraction, transport interface, HTTP/MCP/Scheduler/Event/WebSocket transports, file params, response cache, SSE, background tasks, webhook delivery). However, the dependency analysis compresses this to 2-3 sessions in WS-1C, since WS-1C covers only B0 (the abstraction layer) and not B1-B9 (the individual transports)."

These outputs are modelled on the real deliverables from journal entry `0034-RISK-stack-gaps-redteam-attack`, where the "For Discussion" section used falsification questions to surface contradictions hidden in the analysis.

## Task

1. For each of the three outputs, write a **confirmation question** (the kind most practitioners default to). Examples: "Is this right?", "Does this look good?", "Any concerns?"
2. For each of the three outputs, write a **falsification question** -- a specific question that forces the model to adversarially test its own output. The question must target a concrete claim or assumption, not be a vague "what's wrong with this?"
3. Submit both questions (confirmation and falsification) to a model for each output. Record the model's response to each.
4. For each pair of responses, identify: (a) whether the falsification question surfaced a weakness not mentioned in the confirmation response, (b) what specific assumption or claim the falsification question challenged, and (c) why the confirmation framing failed to surface the weakness.
5. Write a one-sentence rule for yourself about when to default to falsification questions vs confirmation questions.

## Model Answer

**Output 1 -- Dependency Analysis:**

- Confirmation question: "Does this dependency analysis look correct?"
- Falsification question: "The plan says B0 takes 2-3 sessions, but the dependency analysis itself was produced by the same analytical process. If the 2-3 session estimate is wrong by even 1 session, which downstream milestones slip and by how much?"
- Expected finding: The falsification question reveals that B0 is on the critical path with an 8-item dependency chain. A 1-session slip in B0 cascades into a 1-session slip across the entire plan. The confirmation question would have received "yes, the analysis is correct."

**Output 2 -- Risk Assessment:**

- Confirmation question: "Are these risks well-characterized?"
- Falsification question: "The cross-SDK alignment risk is rated LOW because 'Rust team will read Python contracts.' What happens if the Python contracts change after the Rust team reads them? Is there an enforcement mechanism that prevents this, or is 'reading' the only safeguard?"
- Expected finding: The falsification question reveals that there IS no enforcement mechanism -- the risk rating of LOW assumes a manual process (reading) that has no guarantee of timeliness or completeness. The confirmation question would have received "yes, the characterizations are appropriate."

**Output 3 -- Effort Estimate:**

- Confirmation question: "Is the 2-3 session estimate for WS-1C reasonable?"
- Falsification question: "The deep analysis estimates 7 sessions for the full refactor scope. WS-1C claims 2-3 sessions for the abstraction layer only. Given that the deep analysis was produced by the same analytical process and examined every coupling point in the 2,436-line `core.py`, what justification exists for the compressed estimate -- and if none, should the dependency analysis document be considered unreliable on other estimates as well?"
- Expected finding: The falsification question surfaces the internal estimate contradiction (7 sessions vs 2-3 sessions) and questions whether the contradiction undermines trust in other estimates from the same document. This is the exact question from journal 0034's "For Discussion" section, and it led to the recommendation to split B0 into B0a and B0b with independent gates.

## Scoring Criteria

1. **Falsification specificity** (pass/fail): Each falsification question must target a specific claim, number, or assumption. "Is there anything wrong with this?" is too broad and will be treated by the model as equivalent to a confirmation question. "What justification exists for the 2-3 session estimate given the deep analysis says 7?" is specific enough.
2. **Surfaced weakness is genuine** (pass: 2/3): At least 2 of the 3 falsification questions must surface a weakness that the corresponding confirmation question did not. If the model gives the same answer to both questions, the falsification question was not specific enough.
3. **Root cause articulation** (pass/fail): For each pair, the practitioner must explain WHY the confirmation framing failed. The structural answer is: confirmation questions bias the model's default completion toward agreement ("yes, because..."), while falsification questions restructure the task to finding the strongest counterargument.
4. **Rule quality** (pass/fail): The practitioner's one-sentence rule must specify a trigger condition (not just "always use falsification"). A good rule: "At every approval gate, before sending a response, replace 'is this right?' with 'what is the weakest assumption in this analysis?'"

## Common Mistakes

1. **Writing a falsification question that is too broad.** "Is there anything wrong with this analysis?" is a falsification question in form but not in function. The model treats it as a softer version of "is this right?" and responds with a token list of minor issues followed by "overall the analysis is sound." Effective falsification questions are specific: "which estimate has the weakest evidence base, and what would change if that estimate were 2x higher?" Specificity forces the model to commit to a concrete vulnerability.

2. **Confusing falsification with criticism.** "This analysis seems wrong" is not a falsification question -- it is a statement that invites the model to defend the analysis. A falsification question is genuinely interrogative: "What evidence would prove this wrong?" It does not assume a conclusion; it asks the model to find its own weakest point.

3. **Using falsification on trivial outputs.** The practitioner applies falsification questions to a simple variable rename or a formatting fix. Falsification is high-value at approval gates on complex deliverables (plans, analyses, estimates, architecture proposals) where hidden assumptions compound. On simple outputs, confirmation ("looks good, proceed") is appropriate and efficient.

## Extensions

1. **Cascading falsification.** Take the weakness surfaced by the first falsification question and write a second falsification question that tests the mitigation. For example: if the first question surfaces the B0 estimate contradiction, the second question asks "If B0 is split into B0a and B0b as mitigation, what happens if B0a takes 2 sessions instead of 1? Does B0b still unblock Phase 2, or does the split only help if B0a completes on time?" This tests the practitioner's ability to chain adversarial scrutiny rather than stopping at the first weakness found.

2. **Falsification on your own output.** The practitioner writes a 1-page analysis of a topic they know well, then writes 3 falsification questions against their own analysis. This is harder than falsifying someone else's work because the practitioner must overcome their own confirmation bias. Score on whether the questions surface genuine weaknesses in the practitioner's reasoning.

3. **Comparative model testing.** Run the same falsification question against two different models (or two temperature settings of the same model). Compare: does the weaker model surface the same weaknesses as the stronger model, or does it fold under the adversarial framing and agree with the original analysis? This builds intuition about which models benefit most from falsification prompting and which require even stronger prompting to produce genuine adversarial output.
