---
case_id: CASE-12
source_path: loom/journal/0021-DECISION-autonomous-gate1-classification.md
title: "Human Approves, Agent Performs"
atoms_served: [must-rule-reinterpretation, human-on-the-loop-operationalization]
spec_concepts: [CARE §4, CO §4, CO §28, PACT §19]
craft_layer: practitioner
recommended_use: discussion
destination: both
application: COC
---

# CASE-12: Human Approves, Agent Performs

## 1. The Situation

The Kailash COC system had an artifact-flow rule (rule 4 in `artifact-flow.md`) that stated: "Human Classifies Every Inbound Change." During a Gate 1 review, 79 artifact files needed to be classified as global (shared across all templates), variant (language-specific), or skip (not upstreamed). The classification determines how each file flows through the sync system -- a misclassification can silently overwrite language-specific behavior across all target repositories.

The rule had been written with the assumption that a human would inspect each file individually. At 79 files requiring cross-comparison between Rust and Python codebases across 11 directories, the human lacked the pattern-matching speed to perform meaningful classification. The rule was correct in intent (human authority over classification decisions) but was operationally blocking at this scale.

## 2. The Trigger

The practitioner recognized that the bottleneck was not the classification judgment itself but the mechanical comparison work. Comparing a Rust file against its Python equivalent to determine if differences are language-inherent or accidental drift requires reading both files and understanding the language-specific patterns -- a task where agents have a speed advantage over humans. Five sync-reviewer agents were launched in parallel to classify all 79 files.

The attentional pattern is **rule-intent versus rule-letter detection**: the practitioner noticed that the rule's letter ("Human Classifies") was preventing the rule's intent ("Human Has Authority Over Classification") from being fulfilled at scale.

## 3. The Move

The move is **must-rule-reinterpretation** combined with **human-on-the-loop-operationalization**. Rather than either breaking the rule (classifying without human involvement) or blocking on the rule (asking the human to classify 79 files one at a time), the practitioner reinterpreted the MUST rule's operational meaning.

Five sync-reviewer agents processed the 79 files in approximately two minutes. They reached unanimous agreement: 0 files classified as global, 64 as variant:rs (Rust-specific), 15 as skip (not upstreamed). The human then approved the consolidated result as a single batch decision rather than 79 individual decisions. The rule was amended from "Human Classifies Every Inbound Change" to "Human Approves Classification" -- explicitly documenting that the human's role is authority over the result, not performance of the mechanical work.

The skill is in recognizing that reinterpreting a MUST rule is itself a governed action. The practitioner did not silently bypass the rule. The reinterpretation was documented as a DECISION journal entry with the alternatives considered, the rationale, and the consequences.

## 4. The Outcome

Gate 1 reviews that would have taken hours of human file-by-file inspection completed in minutes. The rule was formally amended, creating a precedent for how MUST rules should be read going forward: the human is the authority, not the executor. The feedback memory from this session was saved so that future sessions would inherit the reinterpreted rule without re-deriving it.

The journal entry also raised two unresolved questions for future sessions: what resolution protocol should apply if the agent team disagrees on a classification (the unanimous result sidestepped this), and whether agent-only classification would remain reliable for conceptual or architectural files rather than code files. These open questions show the practitioner's awareness that the reinterpretation has boundaries -- it was validated for mechanical code comparison but not yet for judgment-heavy classification tasks.

## 5. The Spec Connection

**CARE §4 (Human-on-the-Loop)** distinguishes between human-in-the-loop (human performs each action) and human-on-the-loop (human sets constraints, monitors, and intervenes when needed). This case is a textbook operationalization of that distinction. The original rule implemented human-in-the-loop by requiring the human to classify each file. The reinterpreted rule implements human-on-the-loop by having agents classify and the human approve. The philosophical principle became an operational design choice.

**CO §4 (Autonomous Execution Within Envelopes)** states that agents should execute autonomously within defined boundaries, with human authority governing the boundaries rather than each action. The five sync-reviewer agents operating in parallel is a direct instantiation: they had a defined task (classify as global/variant/skip), a defined scope (79 files), and their output was subject to human approval. The human did not direct each classification -- they approved the aggregate.

**CO §28 (Rule Interpretation)** addresses how MUST rules should be read when their literal wording conflicts with their operational intent. This case demonstrates the correct move: the practitioner did not silently violate the rule, did not ask for the rule to be waived, and did not argue that the rule was wrong. Instead, the practitioner proposed a reinterpretation that preserved the rule's authority guarantee while changing its operational mechanism, and documented the reinterpretation as a formal DECISION entry.

**PACT §19 (Delegation Governance)** governs how authority is delegated from human to agent. The classification task was delegated to a team of sync-reviewer agents, but the delegation was bounded: the agents could classify, but only the human could approve the classification and commit it to the sync manifest. This is delegation with a retained approval gate, which is the PACT-compliant pattern for authority-sensitive operations.

## 6. Discussion Questions

1. The agent team reached unanimous agreement (0/64/15) on all 79 files. If one agent had classified a file as "global" while four classified it as "variant:rs," should the human see only the majority result, only the disagreement, or both? What are the failure modes of each approach?

2. The reinterpretation changed "Human Classifies" to "Human Approves Classification." Is there a class of MUST rules where this reinterpretation would be dangerous -- where the act of performing the work is itself the governance mechanism, not just the approval of the result?

3. The journal notes that the reinterpretation was validated for code files but raises whether it would hold for "conceptual/architectural files." What makes architectural classification harder for agents than code classification? Is the difference in kind or in degree?

4. If this pattern scales -- agents perform, humans approve batches -- what is the minimum information the human needs in the approval summary to exercise genuine authority rather than rubber-stamping? How would you design the summary to make the human's approval meaningful?
