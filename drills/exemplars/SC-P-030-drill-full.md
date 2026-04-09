---
drill_id: DR-SC-P-030-FULL
source_atom: SC-P-030
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COR]
---

# Calibrate Academic Register with Detection-Bias Awareness

## Setup

Dr. Lena Okonkwo has co-authored a paper with an AI research assistant on the governance costs of autonomous decision-making in financial institutions. The paper formalizes a principal-agent model where the agent is an AI system rather than a human employee. She is targeting **The Review of Financial Studies (RFS)** for submission.

RFS register conventions:

- Formal but direct prose (not conversational, not hyperformalized)
- Domain-specific vocabulary from financial economics (agency costs, moral hazard, incomplete contracts, monitoring intensity)
- Empirical grounding expected even in theoretical papers (calibration exercises, stylized facts)
- First person plural ("we") is standard; singular first person is rare
- Paragraphs earn their position through argumentative sequence, not connective tissue

The following 400-word passage is the opening of Part I. It was drafted in a generic academic register and has not been calibrated to RFS conventions or screened for detection-flag patterns.

---

**Draft passage (opening of Part I):**

Furthermore, the rapid advancement of artificial intelligence has created unprecedented challenges for corporate governance frameworks. It is important to note that traditional principal-agent models assume the agent is human, which introduces specific behavioral assumptions about effort aversion, career concerns, and bounded rationality. Moreover, when the agent is an AI system, these assumptions require fundamental reconsideration.

First, AI agents do not experience effort aversion in the conventional sense. Second, AI agents lack career concerns that traditionally discipline human agents. Third, AI agents operate under computational rather than cognitive constraints on rationality. These three differences suggest that the classical governance apparatus — monitoring, incentive alignment, and termination threats — may be poorly calibrated for AI agents.

The implications of this observation are far-reaching and significant. Organizations deploying AI agents face a novel governance dilemma: the tools designed to align human agents' interests with those of the principal are built on behavioral assumptions that do not hold for non-human agents. Consequently, the entire framework of agency theory must be re-examined in light of these technological developments.

In this paper, we develop a formal model that addresses this gap in the literature. Our model builds on the incomplete contracts tradition (Hart & Moore, 1990) and extends it to incorporate non-human agents with fundamentally different behavioral characteristics. We demonstrate that the optimal governance structure for AI agents differs systematically from that prescribed by standard agency theory. Additionally, we show that monitoring intensity — traditionally the primary governance lever — has diminishing returns when applied to AI agents due to the transparency of algorithmic decision-making.

It is worth mentioning that our approach differs from recent work on AI governance in several important ways. Unlike prior studies that focus on ethical guidelines or regulatory frameworks, we provide a formal economic analysis grounded in contract theory. Our contribution is thus both theoretically novel and practically relevant for organizations navigating the deployment of AI decision-making systems.

---

**Known AI-detection flag patterns** (from Kobak et al. 2024, Reinhart et al. 2025, Liang et al. 2023):

| Pattern                 | Description                                                 | Example from draft                                                             |
| ----------------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Smooth transition words | Connectives inserted as glue rather than earned by argument | "Furthermore", "Moreover", "Consequently", "Additionally"                      |
| Hedging fillers         | Throat-clearing phrases that add no information             | "It is important to note", "It is worth mentioning"                            |
| Parallel triads         | Lists of exactly three items in parallel structure          | "First... Second... Third..." paragraph                                        |
| Uniform sentence length | Sentences cluster around 20-25 words with low variance      | Most sentences in paragraphs 1, 4, 5                                           |
| Generic academic filler | Broad claims without specific content                       | "far-reaching and significant", "theoretically novel and practically relevant" |

## Task

1. **Identify detection-flag patterns**: Find every instance of the five detection-flag patterns in the draft. For each, quote the specific phrase and name the pattern category.

2. **Assess register mismatch**: Identify 3 specific ways the draft's register does not match RFS conventions. For each mismatch, explain what RFS expects instead.

3. **Rewrite the passage**: Produce a revised 350-450 word version that (a) eliminates all detection-flag patterns without replacing them with equivalent flags, (b) matches the RFS register, and (c) preserves the full argument (principal-agent model, three behavioral differences, incomplete contracts extension, monitoring-intensity finding).

4. **Verify your rewrite**: For each detection-flag pattern you identified in step 1, confirm it is absent from your rewrite AND that you did not substitute an equivalent flag (e.g., replacing "Furthermore" with "Building on this" is still a smooth transition).

5. **Write a margin note** explaining one non-obvious register choice you made in the rewrite — what you chose, what alternatives you considered, and why your choice serves both the argument and the venue.

## Model Answer

**1. Detection-flag inventory:**

| #   | Phrase                                                                             | Pattern                                                                             |
| --- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 1   | "Furthermore, the rapid advancement..."                                            | Smooth transition word — opens the paper with a connective that connects to nothing |
| 2   | "It is important to note that..."                                                  | Hedging filler — adds no information                                                |
| 3   | "Moreover, when the agent is an AI system..."                                      | Smooth transition word                                                              |
| 4   | "First, AI agents do not... Second, AI agents lack... Third, AI agents operate..." | Parallel triad — three items in identical syntactic structure                       |
| 5   | "The implications of this observation are far-reaching and significant."           | Generic academic filler — broad claim without content                               |
| 6   | "Consequently, the entire framework..."                                            | Smooth transition word                                                              |
| 7   | "Additionally, we show that..."                                                    | Smooth transition word                                                              |
| 8   | "It is worth mentioning that..."                                                   | Hedging filler                                                                      |
| 9   | "Our contribution is thus both theoretically novel and practically relevant..."    | Generic academic filler — paired adjectives asserting importance                    |

**2. Register mismatches:**

| #   | Mismatch                                                                                                                                                                                                                                                                  | RFS expectation                                                                                                                                         |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Opening with a broad claim about "rapid advancement of AI" — conversational framing that reads like a policy brief, not a financial economics paper                                                                                                                       | RFS papers open with the specific economic problem. The first sentence should name the economic question, not the technology trend.                     |
| 2   | No domain-specific vocabulary in the first three paragraphs — "governance frameworks", "behavioral assumptions", "technological developments" are generic; missing financial economics terms (agency costs, residual claimant, contractible vs. non-contractible actions) | RFS readers expect the vocabulary of financial economics from the first paragraph. Generic terms signal the paper may not be in the right conversation. |
| 3   | Excessive hedging and self-congratulation ("far-reaching and significant", "theoretically novel and practically relevant") — RFS lets the model speak for itself; asserting significance is considered a sign of weak results                                             | RFS papers state the result and let the reader assess significance. The result should be specific enough that significance is obvious.                  |

**3. Rewrite:**

Traditional agency theory assumes the agent is human. The human agent avoids effort, responds to career incentives, and reasons under cognitive constraints. When firms deploy AI systems as autonomous decision-makers, all three assumptions fail — and the governance apparatus built on them (monitoring, incentive alignment, termination threats) loses its behavioral foundation.

This paper asks what optimal governance looks like when the agent is not human. We work within the incomplete contracts tradition of Hart and Moore (1990). In their framework, governance structures exist because contracts cannot specify actions for every contingency. The principal retains residual control rights as a second-best solution to the hold-up problem. We extend this framework by replacing the human agent — who has private information about effort and ability — with an AI agent whose decision process is partially observable but whose objective function is set by its designers, not by the agent itself.

The extension produces three results. The AI agent's lack of effort aversion means that monitoring serves a different function: it verifies alignment with the principal's objective function rather than deterring shirking. Because the AI has no career concerns, reputation mechanisms that discipline human agents (Holmstrom 1999) have no purchase. And because algorithmic decisions are partially transparent, monitoring intensity has sharply diminishing returns — the marginal cost of additional oversight exceeds its governance benefit beyond a low threshold.

We formalize these observations in a two-period model with incomplete contracts, derive the optimal monitoring intensity as a function of algorithmic transparency, and characterize the conditions under which the principal prefers to constrain the AI's action space ex ante rather than monitor ex post. The model predicts that firms governing AI agents will shift resources from monitoring toward ex ante constraint design — a prediction we examine against survey evidence from 140 firms in Section 5.

**4. Flag-absence verification:**

| Original flag                                                   | Status in rewrite                                                                                                                                                                                                  |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| "Furthermore" (smooth transition)                               | Absent. Opening sentence stands alone — no connective needed because it is the first claim.                                                                                                                        |
| "It is important to note" (hedging filler)                      | Absent. No throat-clearing phrases remain.                                                                                                                                                                         |
| "Moreover" (smooth transition)                                  | Absent. The sentence "When firms deploy AI systems..." follows from the prior sentence by argumentative necessity, not connective glue.                                                                            |
| Parallel triad ("First... Second... Third...")                  | Replaced with three results embedded in a paragraph with varied syntax: "The AI agent's lack of...", "Because the AI has no...", "And because algorithmic decisions..." — same three points, different structures. |
| "far-reaching and significant" (generic filler)                 | Absent. The results are stated specifically; no assertion of importance.                                                                                                                                           |
| "Consequently" (smooth transition)                              | Absent.                                                                                                                                                                                                            |
| "Additionally" (smooth transition)                              | Absent.                                                                                                                                                                                                            |
| "It is worth mentioning" (hedging filler)                       | Absent.                                                                                                                                                                                                            |
| "theoretically novel and practically relevant" (generic filler) | Absent. The final paragraph states what the paper does and what it predicts — the reader assesses relevance.                                                                                                       |

No equivalent flags introduced: no "Building on this", "In addition", "Notably", or other smooth-transition substitutes. Paragraph sequence is earned by argumentative progression (assumption failure → question → model → results → prediction).

**5. Margin note:**

> **Choice**: "Traditional agency theory assumes the agent is human." as the opening sentence.
> **Alternative 1**: "What does optimal governance look like when the agent is not human?" — question opening, more engaging but unusual for RFS; risks sounding like a popular-press piece.
> **Alternative 2**: "Since Jensen and Meckling (1976), agency theory has assumed human agents." — historical opening, signals the conversation clearly but buries the insight (the assumption, not the history, is the point).
> **Why this choice**: The flat declarative names the assumption directly. RFS readers in agency theory will recognize it as their assumption being challenged. The sentence is short (8 words), creating a tonal contrast with the formal model exposition that follows — the paper signals authority through directness, not through elaborate framing. This matches RFS convention where strong theoretical papers open with the economic problem in plain terms (e.g., Hart 1995, Holmstrom and Milgrom 1991).

## Scoring

| Criterion                                                                                                   | Points |
| ----------------------------------------------------------------------------------------------------------- | ------ |
| All 9 detection-flag instances identified with correct pattern classification                               | 2      |
| Three register mismatches identified with specific RFS expectations (not generic "make it more formal")     | 2      |
| Rewrite eliminates all detection-flag patterns without substituting equivalent flags                        | 2      |
| Rewrite matches RFS register (domain vocabulary, direct statement of results, no self-congratulation)       | 2      |
| Rewrite preserves the full argument (three behavioral differences, incomplete contracts, monitoring result) | 1      |
| Margin note documents a genuine deliberation with real alternatives and argument-serving rationale          | 1      |
| **Total**                                                                                                   | **10** |

## Extensions

- **Variant A (parameter change):** The target venue is changed to **AI & Society** (interdisciplinary, accessible to social scientists, philosophers, and CS researchers; formal models are rare and must be motivated for a non-economics audience). Recalibrate the same draft for this venue. Which register choices reverse? Which detection-flag patterns are more or less problematic at this venue?
- **Variant B (cross-domain application):** A policy analyst is co-writing a regulatory impact assessment with an AI assistant. The document targets a parliamentary committee — lay audience, no jargon, but formal institutional register. Apply the same two-axis calibration (venue register + detection-bias awareness). Which detection-flag patterns matter more in policy writing, and which matter less?
