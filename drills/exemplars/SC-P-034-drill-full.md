---
drill_id: DR-SC-P-034-FULL
source_atom: SC-P-034
difficulty: intermediate
time_box_minutes: 30
modality: drill
application: [COR]
---

# Qualify Every Superlative — Prevent Overclaims in Research Writing

## Setup

Dr. Tomasz Wierzbicki has drafted an abstract and introduction for a paper on **organizational governance structures for AI agent teams**. The paper proposes a formal accountability grammar — a set of typed relationships (Delegation, Trust, Responsibility) that define how governance authority flows between human principals and AI agents. The paper targets **Organization Science**.

The draft is well-argued, but the abstract and opening paragraph contain six superlative or novelty claims that are unqualified. Each one is a reviewer magnet — an invitation for the reviewer to find the counterexample that destroys the claim's credibility and, by extension, the paper's.

---

**Draft abstract (180 words):**

This paper presents the first formal accountability grammar for AI agent governance. While prior work has addressed AI ethics guidelines and regulatory frameworks, no existing research provides a rigorous mathematical treatment of how governance authority flows in organizations that deploy autonomous AI agents. We introduce a novel typing system — Delegation, Trust, and Responsibility (D/T/R) — that uniquely captures the three fundamental modes of authority transfer between human principals and AI agents. Our framework is the only one that simultaneously models delegation scope, trust verification, and responsibility attribution within a unified formal system. We demonstrate that our D/T/R grammar produces optimal governance structures under incomplete information, a result that has never been shown in the organizational governance literature. Through application to four real-world deployments spanning financial services, logistics, healthcare, and government, we show that organizations using our grammar achieve unprecedented improvements in governance efficiency while maintaining accountability. Our contribution fundamentally transforms how organizations can design, implement, and audit AI governance structures.

---

**Draft introduction, opening paragraph (150 words):**

The deployment of autonomous AI agents in organizational settings has created an entirely new class of governance problems. Traditional governance theory, developed exclusively for human agents, cannot address the unique challenges posed by AI systems that operate continuously, lack career incentives, and make decisions at computational rather than cognitive speed. Despite growing recognition of this gap, the field has produced no formal framework capable of modeling the full complexity of human-AI governance relationships. This paper fills that void with a comprehensive accountability grammar that, for the first time, provides organizations with a mathematically rigorous tool for designing AI governance structures. Our approach is fundamentally different from prior work in that it treats governance not as a set of ethical guidelines or compliance checklists but as a formal system with provable properties. The implications of our framework are transformative for any organization deploying AI agents at scale.

---

**The six unqualified superlatives** (identified for the practitioner):

| #   | Claim                                                              | Location             | Superlative     |
| --- | ------------------------------------------------------------------ | -------------------- | --------------- |
| 1   | "the first formal accountability grammar for AI agent governance"  | Abstract, sentence 1 | "first"         |
| 2   | "no existing research provides a rigorous mathematical treatment"  | Abstract, sentence 2 | "no existing"   |
| 3   | "uniquely captures the three fundamental modes"                    | Abstract, sentence 3 | "uniquely"      |
| 4   | "the only one that simultaneously models"                          | Abstract, sentence 4 | "only"          |
| 5   | "has never been shown in the organizational governance literature" | Abstract, sentence 5 | "never"         |
| 6   | "unprecedented improvements in governance efficiency"              | Abstract, sentence 6 | "unprecedented" |

**Additional unqualified superlatives in the introduction** (the practitioner must find these):

The introduction contains at least 4 more superlatives that need qualification: "entirely new", "exclusively", "no formal framework", "fundamentally different", "for the first time", "transformative." The practitioner should identify and qualify all of them.

## Task

1. **Qualify the six abstract superlatives**: For each of the six identified claims, apply the three-step qualification: (a) define the scope within which the claim holds, (b) describe what evidence would support the scope being empty (the gap check), and (c) add a temporal qualifier. Then rewrite the claim with all three qualifications incorporated.

2. **Find and qualify the introduction superlatives**: Identify every remaining unqualified superlative or novelty claim in the introduction paragraph. For each, apply the same three-step qualification and rewrite.

3. **Rewrite the abstract**: Produce a revised 160-200 word abstract where every superlative is either qualified (with scope + evidence + temporal qualifier) or replaced with a specific, falsifiable statement of contribution. The revised abstract should be STRONGER than the original, not weaker — qualification done well increases credibility.

4. **Write the gap-check protocol**: For claim #1 ("the first formal accountability grammar for AI agent governance"), write the specific literature search protocol that would provide evidence for the qualification. Include: databases to search, search terms, inclusion/exclusion criteria, and the date the search was conducted.

5. **Identify the claim that should be dropped entirely**: At least one of the six claims cannot be qualified because it is not falsifiable or because the scope required to make it true is so narrow that the claim becomes trivial. Identify which one and explain why dropping it strengthens the paper.

## Model Answer

**1. Qualifying the six abstract superlatives:**

**Claim 1: "the first formal accountability grammar for AI agent governance"**

- Scope: "first formal accountability grammar" within what boundary? Formal = mathematical typing system with provable properties. Accountability grammar = a system that types governance relationships, not just classifies them. AI agent governance = governance of autonomous AI decision-makers in organizational settings (excluding robotics control, autonomous vehicles, military AI).
- Gap check evidence: Systematic search of MIS Quarterly, Management Science, Organization Science, AAAI, NeurIPS, FAccT, and AIES proceedings (2015-2026) plus Google Scholar for ("accountability" OR "governance") AND ("formal" OR "mathematical" OR "typing system") AND ("AI agent" OR "autonomous agent"). No paper found that proposes a typed accountability grammar with provable properties for organizational AI governance.
- Temporal qualifier: "as of March 2026."
- Rewrite: "To our knowledge, no prior work has proposed a formal typing system for accountability relationships in organizational AI agent governance. We introduce such a grammar — Delegation, Trust, and Responsibility (D/T/R) — grounded in a systematic review of governance and AI agent literature through March 2026 (see Section 2)."

**Claim 2: "no existing research provides a rigorous mathematical treatment"**

- Scope: "rigorous mathematical treatment" of what, specifically? Authority flow in organizations with AI agents — not AI ethics, not algorithmic fairness, not robotic control.
- Gap check evidence: Same literature search as above, narrowed to papers with formal models of authority delegation in human-AI organizational contexts. Several papers model trust in human-AI interaction (Lee & See 2004, Glikson & Woolley 2020) but none model authority FLOW (delegation, trust transfer, responsibility attribution) as a formal system.
- Temporal qualifier: "in the organizational governance literature through March 2026."
- Rewrite: "Prior work has modeled trust in human-AI interaction and proposed ethical frameworks for AI governance, but we find no formal treatment of how governance authority flows — through delegation, trust, and responsibility — in organizations deploying AI agents (literature review current through March 2026)."

**Claim 3: "uniquely captures the three fundamental modes"**

- Scope: "uniquely" is unfalsifiable as written — it asserts that no other system captures D/T/R, but another system might capture the same modes under different names. "Fundamental" is an assertion of completeness — are there really only three modes?
- Gap check evidence: This claim requires an exhaustive analysis of alternative accountability frameworks (RACI, COBIT, ISO 38500) to show that none type governance relationships at the same granularity.
- Temporal qualifier: Not applicable — this is a structural claim, not a temporal one.
- Rewrite: "The D/T/R typing system distinguishes three modes of authority transfer — delegation (scope assignment), trust (verification commitment), and responsibility (accountability attribution) — that we argue are jointly necessary and individually insufficient for modeling organizational AI governance (Section 3 provides the formal argument for this decomposition)."

**Claim 4: "the only one that simultaneously models"**

- Scope: "only" within what comparison set? Governance frameworks generally, or formal mathematical frameworks specifically?
- Gap check evidence: Comparison of D/T/R against RACI, COBIT, ISO 38500, and academic formal governance models. RACI models responsibility assignment but not trust verification. ISO 38500 addresses governance principles but is not a formal system. No framework found that simultaneously types all three.
- Temporal qualifier: "among formal governance frameworks reviewed through March 2026."
- Rewrite: "Among governance frameworks reviewed (RACI, COBIT, ISO 38500, and formal models in the organizational governance literature through March 2026), we find none that simultaneously model delegation scope, trust verification, and responsibility attribution within a unified typing system."

**Claim 5: "has never been shown in the organizational governance literature"**

- Scope: What specific result "has never been shown"? Optimal governance structures under incomplete information is a broad space — Hart and Moore (1990), Holmstrom (1979), and others have proven optimality results in governance contexts. The claim needs to be scoped to the SPECIFIC result: optimality of D/T/R-typed structures under incomplete information about agent objectives.
- Gap check evidence: Search for optimality proofs in governance frameworks involving AI agents. Papers on optimal mechanism design for AI (e.g., Conitzer & Sandholm) address different problems (auction design, not organizational governance).
- Temporal qualifier: "in the organizational governance literature through March 2026."
- Rewrite: "We prove that D/T/R-typed governance structures are optimal under incomplete information about agent objectives — a result that, to our knowledge, has not been established for AI agent governance in the organizational theory literature (see Section 4 for the formal statement and proof)."

**Claim 6: "unprecedented improvements in governance efficiency"**

- Scope: "unprecedented" compared to what baseline? What metric defines "governance efficiency"?
- Gap check evidence: This claim requires a benchmark: what governance efficiency levels have been reported in prior empirical studies? Without a benchmark, "unprecedented" is unmeasurable.
- Temporal qualifier: Not meaningful without a baseline.
- Rewrite: "In four organizational deployments, D/T/R-governed agent teams reduced governance overhead (defined as the ratio of governance cost to operational output) by 23-41% relative to the organizations' pre-deployment governance structures, while maintaining full accountability traceability."

**2. Introduction superlatives — identification and qualification:**

| #   | Claim                                                              | Superlative              | Qualification                                                                                                                                                                                                                                                            |
| --- | ------------------------------------------------------------------ | ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 7   | "entirely new class of governance problems"                        | "entirely new"           | Rewrite: "governance problems that differ from those addressed by traditional principal-agent theory in three specific ways: continuous operation, absence of career incentives, and computational decision speed" — name the differences instead of asserting novelty   |
| 8   | "developed exclusively for human agents"                           | "exclusively"            | Rewrite: "developed under the assumption that agents are human" — the assumption is specific and verifiable; "exclusively" overstates (some organizational theory addresses inter-firm agents, technology as actor in Actor-Network Theory)                              |
| 9   | "no formal framework capable of modeling the full complexity"      | "no" + "full complexity" | Rewrite: "no formal framework that simultaneously models delegation, trust, and responsibility in human-AI governance relationships (see Section 2 for systematic review, current through March 2026)" — scope the absence; drop "full complexity" which is unmeasurable |
| 10  | "for the first time"                                               | "first"                  | Redundant with Claim 1 in the abstract; drop entirely — the abstract already establishes priority                                                                                                                                                                        |
| 11  | "fundamentally different from prior work"                          | "fundamentally"          | Rewrite: "differs from prior work in a specific way: it treats governance as a formal typing system with provable properties rather than as a set of normative guidelines" — name the difference instead of asserting degree                                             |
| 12  | "transformative for any organization deploying AI agents at scale" | "transformative" + "any" | Drop entirely — "transformative" is unfalsifiable self-promotion; let the empirical results in the abstract speak for themselves                                                                                                                                         |

**3. Revised abstract (192 words):**

This paper introduces a formal accountability grammar for organizational AI agent governance: Delegation, Trust, and Responsibility (D/T/R). Each type captures a distinct mode of authority transfer between human principals and AI agents — delegation defines scope, trust defines verification commitments, and responsibility defines accountability attribution. We argue these three types are jointly necessary and individually insufficient for modeling governance in human-AI organizations (Section 3 provides the formal argument).

Prior work has modeled trust in human-AI interaction and proposed ethical guidelines for AI deployment, but we find no formal typing system for governance authority flow in organizational settings (systematic review of Management Science, Organization Science, AAAI, and FAccT proceedings through March 2026; see Section 2). Among frameworks reviewed — including RACI, COBIT, and ISO 38500 — none simultaneously model delegation scope, trust verification, and responsibility attribution within a unified formal system.

We prove that D/T/R-typed governance structures are optimal under incomplete information about agent objectives (Proposition 1). In four organizational deployments across financial services, logistics, healthcare, and government, D/T/R-governed agent teams reduced governance overhead by 23-41% relative to pre-deployment structures while maintaining full accountability traceability.

**4. Gap-check protocol for Claim 1:**

**Databases**: Web of Science, Scopus, Google Scholar, ACM Digital Library, SSRN, arXiv (cs.AI, cs.MA, econ.GN)

**Search terms** (three queries, combined with OR):

- Q1: ("accountability grammar" OR "accountability typing" OR "formal accountability") AND ("AI agent" OR "autonomous agent" OR "AI governance")
- Q2: ("delegation" AND "trust" AND "responsibility") AND ("formal model" OR "typing system" OR "formal framework") AND ("AI" OR "autonomous" OR "agent")
- Q3: ("governance framework" OR "governance model") AND ("formal" OR "mathematical" OR "provable") AND ("AI agent" OR "multi-agent" OR "autonomous agent") AND ("organization" OR "organizational" OR "enterprise")

**Inclusion criteria**: Paper proposes a formal (mathematical, typed, or logic-based) system for modeling accountability or governance relationships in contexts where at least one agent is an AI system operating within a human-governed organization.

**Exclusion criteria**: Papers on AI ethics guidelines without formal models; papers on autonomous vehicle governance (different domain); papers on multi-agent systems without organizational governance framing; papers on algorithmic fairness (different concern); papers on robotic control systems (different domain).

**Date range**: 2015-01-01 through 2026-03-31.

**Date conducted**: March 2026.

**Reporting**: If zero papers pass inclusion criteria, report: "Systematic search of [databases] using [queries] for the period 2015-2026 identified [N] candidate papers, of which zero met inclusion criteria for a formal accountability typing system in organizational AI governance." If papers are found, report how they differ from D/T/R (different scope, different formalism, different governance relationship modeled).

**5. Claim to drop entirely:**

**Claim 6 ("unprecedented improvements")** should be dropped — not qualified, dropped.

Reason: "unprecedented" requires comparison against every prior reported governance improvement in the literature. Even with qualification ("23-41% reduction"), calling it "unprecedented" means asserting that no prior governance intervention has achieved a 23% reduction, which is (a) almost certainly false (process improvement literature routinely reports larger gains), (b) irrelevant (the paper's contribution is the formal grammar, not the magnitude of efficiency gains), and (c) a distraction (a reviewer who finds a single prior study showing 50% governance improvement would use it to discredit the paper's empirical section, even though the empirical section is supporting evidence, not the main contribution).

Replacing it with the specific numbers ("23-41% reduction relative to pre-deployment baselines") is stronger because it is falsifiable and meaningful without requiring a claim of historical uniqueness. The numbers stand on their own; calling them "unprecedented" adds risk without adding information.

## Scoring

| Criterion                                                                                                             | Points |
| --------------------------------------------------------------------------------------------------------------------- | ------ |
| All six abstract superlatives qualified with specific scope (not generic hedging like "to the best of our knowledge") | 2      |
| At least 4 introduction superlatives identified and qualified                                                         | 1      |
| Revised abstract is more credible than the original — qualifications add falsifiability, not just hedging             | 2      |
| Gap-check protocol is specific enough to execute (databases, queries, inclusion/exclusion criteria, date)             | 2      |
| Correctly identifies which claim should be dropped entirely and explains why dropping strengthens the paper           | 2      |
| Qualifications include temporal markers ("as of [date]", "through [date]") wherever temporal scope applies            | 1      |
| **Total**                                                                                                             | **10** |

## Extensions

- **Variant A (parameter change):** The paper is rewritten for a computer science venue (AAAI) instead of Organization Science. CS venues have different norms around novelty claims — "we propose the first" is more common and less scrutinized, but "no prior work" still invites counterexamples. Which qualifications become more important at AAAI, and which become less important? Does the gap-check protocol change?
- **Variant B (cross-domain application):** A startup pitch deck claims "the only AI governance platform that combines real-time monitoring with formal accountability." Apply the same three-step qualification framework to investor-facing materials. How does qualification work when the audience rewards confidence and punishes hedging? What is the investor-context equivalent of "a reviewer finds the counterexample"?
