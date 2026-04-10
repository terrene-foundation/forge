---
drill_id: DR-SC-B-015-FULL
source_atom: SC-B-015
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC, COR, COE]
---

# Reinterpret MUST Rules from "Human Performs" to "Human Approves"

## Setup

The practitioner receives a project rule file (`process-rules.md`) containing five MUST rules for a CO workspace. Each rule was written early in the project when all execution was human-driven. The project has since adopted autonomous agent execution with Human-on-the-Loop governance.

**Rule 1 — Test Review:**

> "Human MUST review every test result before merge. All test output must be read by a human who confirms each test passed for the right reason, not just that the exit code was zero."

**Rule 2 — Changelog:**

> "Human MUST write the changelog entry for every release. The changelog is user-facing prose and must reflect the user's perspective, not the developer's."

**Rule 3 — Deployment Window:**

> "Human MUST decide the deployment window for production releases. The window must account for customer timezone distribution, support team availability, partner SLA deadlines, and upcoming board presentations."

**Rule 4 — Dependency Audit:**

> "Human MUST audit every new dependency added to the project. Each new package must be checked for license compatibility, maintenance status, known vulnerabilities, and transitive dependency count."

**Rule 5 — Naming Conventions:**

> "Human MUST name every new module, class, and public function. Names must follow the project glossary and reflect domain language, not implementation details."

The practitioner also receives a one-page reference sheet:

- **CARE Section 4 — Human-on-the-Loop**: Humans define the operating envelope, observe execution patterns, and refine boundaries. In-the-loop blocks throughput; out-of-the-loop loses oversight. On-the-loop is the target position.
- **CO Section 28 — Approval Gate**: Human judgment at structural transitions. The gate is the decision, not the execution.
- The test from the SC-B-015 atom: **"Is the human's unique contribution the judgment on the result, or the execution itself?"** If judgment, reinterpret. If execution, preserve.

## Task

1. For each of the five rules, classify it as **reinterpretable** (human value is in approval, not execution) or **non-reinterpretable** (human judgment IS the execution and cannot be delegated). Write one sentence per rule explaining your classification.

2. For each reinterpretable rule, write the **amended rule text**. The amended text must:
   - Preserve the human approval gate explicitly.
   - Remove the human as the execution bottleneck.
   - Be self-contained (a future session reading only the amended text must understand the full rule without knowing the original).

3. For each non-reinterpretable rule, explain in two sentences what makes the human's contribution non-delegatable. Reference the specific type of judgment (aesthetic, political, contextual, relational) that agents cannot replicate.

4. One of the five rules is a **trap** — it appears reinterpretable on the surface but is not. Identify it and explain why.

## Model Answer

**1. Classifications:**

| Rule                        | Classification                 | Reasoning                                                                                                                                                                                                                                                                                                                                                          |
| --------------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Rule 1 (Test Review)        | **Reinterpretable**            | Agent teams can run tests, parse output, flag anomalies, and summarise results. The human's value is in judging edge-case pass/fail decisions (e.g., "this test passed but the assertion is too weak"), not in reading raw test output line by line.                                                                                                               |
| Rule 2 (Changelog)          | **Reinterpretable**            | Agent teams can draft changelogs from commit history and PR descriptions. The human's value is in tone and audience judgment — ensuring the prose reads well to users, not in the mechanical act of writing.                                                                                                                                                       |
| Rule 3 (Deployment Window)  | **Non-reinterpretable**        | Choosing a deployment window requires business context that agents cannot access: customer timezone distribution patterns, verbal commitments to partners, support team vacation schedules, and political timing around board presentations. The execution IS the judgment.                                                                                        |
| Rule 4 (Dependency Audit)   | **Reinterpretable**            | Agent teams can check license compatibility against a policy file, query vulnerability databases, count transitive dependencies, and assess maintenance status (last commit date, issue response time). The human's value is in approving the risk assessment, not in performing the mechanical checks.                                                            |
| Rule 5 (Naming Conventions) | **Non-reinterpretable** (trap) | Naming appears mechanical ("just follow the glossary") but domain naming requires understanding the conceptual model deeply enough to choose names that will remain accurate as the domain evolves. An agent can propose names, but the glossary is a living document that reflects human domain understanding — the act of naming IS the act of domain modelling. |

**2. Amended rule text for reinterpretable rules:**

**Rule 1 (amended):**

> "Agent teams MUST run the full test suite, parse results, and produce a structured summary that includes: pass/fail counts, any tests that passed with weak assertions (coverage of a single branch), any tests whose names do not match their actual assertions, and any flaky tests (different results on re-run). Human MUST review the summary and approve or reject the merge. Human MAY drill into raw test output for any flagged item. The human's judgment is on the adequacy of test coverage and assertion quality, not on reading every line of output."

**Rule 2 (amended):**

> "Agent teams MUST draft the changelog entry from the set of merged PRs, using user-facing language (not developer-facing). The draft MUST be structured as: new features, improvements, fixes, breaking changes. Human MUST review the draft for tone, audience-appropriateness, and completeness before publication. Human MAY rewrite any entry. The human's judgment is on whether the prose serves the user audience, not on the mechanical extraction of changes from commit history."

**Rule 4 (amended):**

> "Agent teams MUST audit every new dependency against the project's dependency policy: license must be in the approved-licenses list, last commit within 12 months, no known CVEs of severity HIGH or above, transitive dependency count below the project threshold (currently 50). Agent teams MUST produce a structured audit report with pass/fail per criterion and a risk summary. Human MUST review the audit report and approve or reject the dependency. Human MAY override a policy-fail with documented justification. The human's judgment is on risk tolerance, not on performing the mechanical checks."

**3. Non-reinterpretable justifications:**

**Rule 3 (Deployment Window):** The deployment window decision integrates information that exists only in the human's relational context: informal commitments made in meetings ("I told the VP we wouldn't deploy during earnings week"), partner SLA deadlines that are tracked in external systems the agent has no access to, and support team preferences communicated verbally. This is not a judgment-on-result — it is a judgment-as-execution where the human's contextual awareness IS the decision.

**Rule 5 (Naming Conventions):** Domain naming is an act of conceptual modelling. The glossary records past naming decisions but does not contain the reasoning needed to extend the model to new concepts. When a human names a module `clearance` rather than `permission` or `access`, they are encoding a domain distinction (clearance implies graduated levels + temporal validity; permission implies binary allow/deny) that requires deep domain understanding. An agent can propose names and explain trade-offs, but the selection is irreducibly a domain-modelling judgment.

**4. Trap identification:**

**Rule 5 is the trap.** It looks reinterpretable because naming conventions exist and an agent could follow them mechanically. But the glossary is descriptive (records past decisions), not prescriptive (determines future ones). Extending the naming model to new modules requires the same kind of judgment that created the glossary in the first place. The journal evidence for SC-B-015 (0021-DECISION-autonomous-gate1-classification) shows the reinterpretation pattern working for classification — a task with clear criteria and verifiable outcomes. Naming has neither: there is no objective criterion for whether `clearance` is better than `permission`, and the outcome (how well the name ages) is not verifiable at decision time.

## Scoring

| Criterion                                                                                                 | Points |
| --------------------------------------------------------------------------------------------------------- | ------ |
| Rules 1, 2, and 4 correctly classified as reinterpretable                                                 | 2      |
| Rule 3 correctly classified as non-reinterpretable with valid justification                               | 1      |
| Rule 5 correctly identified as the trap (non-reinterpretable despite surface appearance)                  | 2      |
| Amended text for reinterpretable rules preserves the approval gate explicitly                             | 2      |
| Amended text removes execution bottleneck (specifies what the agent does, not just that it "helps")       | 1      |
| Non-reinterpretable justifications reference the specific type of judgment (relational, domain-modelling) | 1      |
| Amended rules are self-contained (future reader needs no knowledge of the original text)                  | 1      |
| **Total**                                                                                                 | **10** |

## Extensions

- **Variant A (cascading reinterpretation):** Take the amended Rule 1 (test review). Six months later, the agent team's test summaries have been correct for 200 consecutive merges. The human now rubber-stamps the approval. Should the rule be reinterpreted again — from "human approves" to "human spot-checks"? Write the second-generation amended text and define the trigger condition that would revert to full approval (e.g., a false negative in the summary).
- **Variant B (COR application):** A research lab has the rule "Human MUST review every literature search result for relevance before it is included in the evidence base." Classify this as reinterpretable or not. Consider: the agent can assess keyword relevance, but can it assess whether a paper's methodology is sound enough to count as evidence? Write the amended text if reinterpretable, or the justification if not.
- **Variant C (contested reinterpretation):** Two practitioners disagree on Rule 5. Practitioner A says naming is reinterpretable because the agent can propose three candidates with reasoning and the human picks one. Practitioner B says this is still "human performs" because the selection is the execution. Referee the dispute using the SC-B-015 test: is the human's contribution judgment-on-result or judgment-as-execution?
