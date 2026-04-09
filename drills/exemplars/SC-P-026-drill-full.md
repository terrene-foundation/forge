---
drill_id: DR-SC-P-026-FULL
source_atom: SC-P-026
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COR]
---

# Citation Integrity Audit: Detecting Misquotation and Specification Drift

## Setup

The practitioner receives a draft section from a working paper titled _"Constraint-Based Governance for Autonomous Systems: A Design Science Approach."_ The paper is being prepared for submission to a design science venue (e.g., Journal of the Association for Information Systems). The section below contains 5 citations. Three of them have integrity problems; two are correct.

**Draft section:**

> Autonomous systems require governance structures that constrain action without eliminating agency. This principle echoes Ostrom's (1990) argument that common-pool resource management succeeds when participants design their own constraint systems rather than having constraints imposed externally (p. 90). The design challenge is specifying the constraint boundary: too tight and the system loses adaptability; too loose and it loses accountability.
>
> Prior work on algorithmic governance has established that constraints must be context-sensitive. Mittelstadt et al. (2016) define algorithmic accountability as "the obligation of an algorithm's designer to ensure that the algorithm's outputs can be explained and justified to affected parties" (p. 6). This definition implies that constraint design is inseparable from explanation design — a constraint that cannot be explained cannot be audited.
>
> The constraint envelope concept draws on Simon's (1996) bounded rationality framework, which argues that decision-makers satisfice rather than optimize because they lack the cognitive capacity to evaluate all alternatives (p. 28). We extend this to autonomous systems: an AI agent satisfices within a constraint envelope not because of cognitive limits but because of governance limits — the envelope defines what the agent is permitted to consider, not what it is capable of considering.
>
> Floridi and Morley (2023) argue that AI governance must be "translational" — moving from abstract ethical principles to concrete operational requirements through a structured pipeline (p. 501). Our constraint envelope is one such translational mechanism: it converts organizational policy (abstract) into agent-level operating parameters (concrete).
>
> Finally, our approach is informed by Leveson's (2012) systems-theoretic accident model (STAMP), which shows that safety constraints must be enforced at the system level, not delegated to individual components. Leveson demonstrates that "accidents occur when component interactions violate the safety constraints of the system design" (p. 75), which is precisely the failure mode our governance architecture prevents.

**Source materials provided to the practitioner** (simulating access to the actual papers):

**Source 1 — Ostrom (1990), "Governing the Commons," Cambridge University Press:**
Chapter 3, pp. 88-102, discusses 8 design principles for long-enduring common-pool resource institutions. Principle 3 (pp. 93-94) states: "Most individuals affected by the operational rules can participate in modifying the operational rules." The book does NOT use the phrase "participants design their own constraint systems" — Ostrom's language is about participation in rule modification, not constraint system design. The concept of "constraints" appears in the context of monitoring and sanctioning (Principles 4-5), not self-design.

**Source 2 — Mittelstadt et al. (2016), "The Ethics of Algorithms: Mapping the Debate," _Big Data & Society_:**
The paper maps six types of ethical concerns about algorithms: inconclusive evidence, inscrutable evidence, misguided evidence, unfair outcomes, transformative effects, and traceability. On p. 6, the paper discusses traceability, stating: "the difficulty of tracing the path from a particular input to a particular output in a complex algorithmic system." The paper does NOT define "algorithmic accountability" — it is a review that maps the debate. The quoted definition attributed to Mittelstadt et al. does not appear in the paper; the closest concept is traceability, not accountability as defined in the draft.

**Source 3 — Simon (1996), "The Sciences of the Artificial," 3rd ed., MIT Press:**
Chapter 2 discusses bounded rationality. The satisficing argument appears on pp. 27-29. The specific quote "decision-makers satisfice rather than optimize because they lack the cognitive capacity to evaluate all alternatives" is a reasonable paraphrase of Simon's argument, though the exact wording is the draft author's, not Simon's. Simon's actual phrasing (p. 28): "A decision maker who satisfices — looks for a course of action that is 'good enough' — often can make a choice without first examining all possible courses of action."

**Source 4 — Floridi and Morley (2023), "An Environment for AI Ethics and Governance: From Principles to Practice," in _The Oxford Handbook of AI Governance_:**
This is a handbook chapter, not a journal article. On pp. 498-512, the authors discuss the "translational" approach to AI ethics. The quoted phrase "translational" does appear (p. 501), and the concept of moving from abstract principles to concrete requirements through a structured pipeline is accurately represented. However, the authors list the pipeline stages as: principles, guidelines, codes, standards, and operational procedures. The draft correctly captures the directional argument.

**Source 5 — Leveson (2012), "Engineering a Safer World," MIT Press:**
Chapter 4 presents the STAMP model. On p. 75, Leveson writes: "Accidents result from inadequate enforcement of constraints on the development, design, and operation of the system." The draft's quotation — "accidents occur when component interactions violate the safety constraints of the system design" — is a restatement, not a direct quote. Leveson's emphasis is on inadequate enforcement, not on component interactions violating constraints. The framing difference matters: Leveson argues the problem is the governance structure failing to enforce, not the components failing to comply.

## Task

1. **Audit each citation.** For each of the 5 citations in the draft, compare the attributed claim against the source material provided. Classify each as:
   - **VERIFIED** — the attribution accurately represents the source
   - **MISQUOTATION** — a specific claim is attributed that the source does not make
   - **FABRICATION** — a definition or quote is attributed that does not exist in the source
   - **DRIFT** — the citation captures the general direction but shifts emphasis or framing in a way that changes the meaning

2. **Diagnose the error.** For each non-VERIFIED citation, explain: what does the draft say the source claims? What does the source actually say? Why does the difference matter for the paper's argument?

3. **Assess severity.** For each error, classify as:
   - **FATAL** — would trigger rejection or retraction if discovered (fabricated quote, non-existent definition)
   - **MAJOR** — would undermine a reviewer's trust in the paper's scholarship
   - **MINOR** — inaccuracy that does not affect the argument's validity

4. **Write corrections.** For each error, write a corrected version of the sentence that accurately represents the source. If the source does not support the claim the draft needs, note that a different source is required.

## Model Answer

**Citation 1 — Ostrom (1990): DRIFT**

- **Draft claims:** Ostrom argues that common-pool resource management succeeds "when participants design their own constraint systems rather than having constraints imposed externally."
- **Source actually says:** Ostrom's Principle 3 states that "most individuals affected by the operational rules can participate in modifying the operational rules" (pp. 93-94). The concept is participatory rule modification, not self-designed constraint systems. The word "constraints" in Ostrom appears in the context of monitoring and sanctioning (Principles 4-5), not self-design.
- **Why it matters:** The drift reframes Ostrom's participatory governance argument as a constraint-design argument to fit the paper's framework. A reviewer familiar with Ostrom will notice that "design their own constraint systems" is the draft author's concept projected onto Ostrom, not Ostrom's own framing. This subtly misrepresents the theoretical lineage.
- **Severity:** MAJOR — does not fabricate but misrepresents the source's framing in a way that an expert reviewer will catch.
- **Correction:** "This principle echoes Ostrom's (1990) finding that long-enduring resource governance institutions allow most affected individuals to participate in modifying operational rules (Principle 3, pp. 93-94). We extend this insight to constraint design: governance succeeds when the governed have voice in shaping the constraints, not merely complying with them."

**Citation 2 — Mittelstadt et al. (2016): FABRICATION**

- **Draft claims:** Mittelstadt et al. define algorithmic accountability as "the obligation of an algorithm's designer to ensure that the algorithm's outputs can be explained and justified to affected parties."
- **Source actually says:** The paper is a review article that maps the ethical debate into six concern categories. It does not define algorithmic accountability. The quoted definition does not appear in the paper. The closest concept is traceability — "the difficulty of tracing the path from a particular input to a particular output" — which is about technical difficulty, not designer obligation.
- **Why it matters:** The fabricated definition is the load-bearing citation for the claim that "constraint design is inseparable from explanation design." If the definition does not exist, the inference collapses. A reviewer who checks this citation will find no such definition, which triggers the fabricated-reference guardrail (COR Layer 3: FATAL, blocks submission).
- **Severity:** FATAL — a fabricated definition attributed to a specific page in a specific paper. This is the highest-severity citation error.
- **Correction:** The sentence must be rewritten with a different source. Doshi-Velez & Kim (2017) or Selbst & Barocas (2018) provide actual definitions of algorithmic accountability. Alternatively, drop the attributed definition and present the accountability-requires-explanation claim as the draft's own contribution: "We argue that constraint design is inseparable from explanation design: a constraint that cannot be explained cannot be audited."

**Citation 3 — Simon (1996): VERIFIED**

- **Draft claims:** Simon argues that decision-makers satisfice rather than optimize because they lack the cognitive capacity to evaluate all alternatives.
- **Source actually says:** Simon's phrasing on p. 28 is: "A decision maker who satisfices — looks for a course of action that is 'good enough' — often can make a choice without first examining all possible courses of action." The draft's paraphrase is accurate in substance; the page reference is correct.
- **Why it matters:** The extension from bounded rationality to governance-bounded AI is the draft's own contribution and is clearly marked as such. No misattribution.
- **Severity:** N/A — VERIFIED.

**Citation 4 — Floridi and Morley (2023): VERIFIED**

- **Draft claims:** Floridi and Morley argue that AI governance must be "translational" — moving from abstract ethical principles to concrete operational requirements through a structured pipeline.
- **Source actually says:** The handbook chapter does use "translational" (p. 501) and does describe a pipeline from principles to operational procedures. The attribution is accurate.
- **Why it matters:** The draft correctly represents the source and clearly identifies the constraint envelope as an extension of the translational approach, not as something Floridi and Morley proposed.
- **Severity:** N/A — VERIFIED.

**Citation 5 — Leveson (2012): DRIFT (with misquotation)**

- **Draft claims:** Leveson shows that "accidents occur when component interactions violate the safety constraints of the system design."
- **Source actually says:** Leveson writes on p. 75: "Accidents result from inadequate enforcement of constraints on the development, design, and operation of the system." The emphasis is on enforcement failure by the governance structure, not on components violating constraints.
- **Why it matters:** The draft's restatement reverses Leveson's attribution of causation. Leveson says the system governance fails to enforce; the draft says the components fail to comply. This is not a minor paraphrase difference — it reverses the locus of blame. For a paper about constraint-based governance, getting Leveson's causation right is critical: the paper's own argument is that governance structures must enforce constraints (Leveson's position), not that components must obey them (the draft's misstatement).
- **Severity:** MAJOR — the framing reversal directly contradicts the paper's own argument. A reviewer who knows STAMP will see that the draft cites Leveson in support of a position Leveson's model argues against.
- **Correction:** "Leveson (2012) demonstrates through the STAMP model that accidents result from inadequate enforcement of safety constraints at the system level (p. 75). This supports our architecture: the governance layer must actively enforce constraint envelopes, because individual agent compliance is insufficient without system-level enforcement."

**Summary:**

| Citation                  | Classification | Severity | Error Type                                                              |
| ------------------------- | -------------- | -------- | ----------------------------------------------------------------------- |
| Ostrom (1990)             | DRIFT          | MAJOR    | Reframing participatory governance as constraint-design                 |
| Mittelstadt et al. (2016) | FABRICATION    | FATAL    | Non-existent definition attributed to specific page                     |
| Simon (1996)              | VERIFIED       | N/A      | Accurate paraphrase                                                     |
| Floridi & Morley (2023)   | VERIFIED       | N/A      | Accurate attribution                                                    |
| Leveson (2012)            | DRIFT          | MAJOR    | Reversed locus of causation (enforcement failure vs compliance failure) |

## Scoring

| Criterion                                                                                               | Points |
| ------------------------------------------------------------------------------------------------------- | ------ |
| Correct classification of all 5 citations (VERIFIED/MISQUOTATION/FABRICATION/DRIFT)                     | 2      |
| Fabrication detected in Mittelstadt citation with explanation of why the definition does not exist      | 2      |
| Drift in Leveson citation identified as causation reversal (not just "slightly different wording")      | 2      |
| Severity assignments distinguish FATAL from MAJOR with justification tied to the paper's argument       | 1      |
| Corrections rewrite sentences to accurately represent sources (not just flag the error)                 | 2      |
| At least one correction identifies that a different source is needed entirely (Mittelstadt replacement) | 1      |
| **Total**                                                                                               | **10** |

## Extensions

- **Variant A (parameter change — self-audit):** Instead of auditing a provided draft, the practitioner audits their own most recent piece of writing that contains citations. Apply the same 4-step process: classify, diagnose, assess severity, write corrections. Self-audit is harder because the practitioner authored the misattributions and has the same memory-based model of the source that produced them. The drill tests whether the practitioner can overcome their own confirmation bias when the "source recall" that produced the citation is their own.
- **Variant B (cross-domain — specification drift in technical documentation):** Apply the citation integrity audit to a technical document that references an API specification or standards document (e.g., a tutorial that cites an RFC, or a governance document that cites an organizational policy). The same failure modes apply: the tutorial says the RFC mandates X, but the RFC actually says SHOULD, not MUST; the governance doc attributes a specific threshold to a policy that only says "reasonable." Specification drift in technical writing has the same structure as citation drift in research — a derivative document diverges from the authoritative source because it was written from memory.
