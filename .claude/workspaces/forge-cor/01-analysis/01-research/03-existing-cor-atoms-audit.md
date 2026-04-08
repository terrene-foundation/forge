---
title: COR Application Audit — 18 Cross-Tagged Atoms
date: 2026-04-08
type: audit
---

# COR Application Audit: 18 Cross-Tagged Atoms

## Executive Summary

All 18 atoms tagged `applications: [COC, COR, COE]` transfer directly to research contexts with minimal or no COR-specific variant needed. The atoms cluster into three categories:

1. **Governance & workflow moves** (7 atoms) — Transfer universally because they operate at the methodological layer independent of application domain
2. **Empirical/ablation moves** (6 atoms) — Explicitly designed for experimental comparison; highly relevant to research methodology
3. **Institutional knowledge & learning** (5 atoms) — Apply equally to any domain requiring knowledge capture and reuse

The audit reveals **zero atoms requiring COR-specific variants**. Each atom's formulation, spec lineage, and evidence already serve research contexts adequately. The COR pass should focus on NEW atoms capturing research-specific craft (literature review moves, reproducibility requirements, citation verification, peer review integration) rather than on re-formulating existing atoms.

---

## Atom-by-Atom Audit

### BROKERAGE LAYER (5 atoms)

#### SC-B-005: Spec-to-code conformance audit

**Spec lineage**: PACT §40, PACT §10, CO §49  
**Practice modality**: brokerage-rep  
**Evidence sources**: kailash-py 0014 (POSITIVE), kailash-py 0015 (NEGATIVE), kailash-rs 0001 (NEGATIVE)

**COR relevance**: Transfers directly. A research context has "specs" (methodology papers, experiment protocols, assumed invariants) and "code" (implementations, analysis scripts, simulation engines). The move — read every normative claim in the spec, grep the code for enforcement, produce a gap list — is mechanically identical. A researcher comparing RLHF protocol specification against implementation would use the same audit structure as a codegen implementation checking PACT governance code.

**COR instantiation example**: "Audit the kailash-align RLHF implementation against the methodology paper's normative claims: does every training loop enforce the reward model update frequency specified in §3.2? Does the preference-ranking process implement the pairwise comparison model from §2.4, or a different one? Does the temperature scheduling match the protocol?"

**Verdict**: No variant needed. The formulation already covers both applications.

---

#### SC-B-006: Multi-round red team to numeric convergence

**Spec lineage**: CO §28, CO §49  
**Practice modality**: brokerage-rep  
**Evidence sources**: loom 0034 (POSITIVE), loom 0039 (POSITIVE), kailash-ml-rs 0003 (POSITIVE), kailash-ml-rs 0005 (POSITIVE)

**COR relevance**: Transfers universally. Research applications depend on convergence measurement: experiment planning, literature review completeness, statistical validity, methodology robustness. The move — define numeric convergence criteria BEFORE red teaming, run rounds, track finding counts, refuse to advance until threshold is met — is exactly how research validates experimental design. The loom 0034-0039 pattern (15 findings → 8 → 3 → converged) directly parallels "Does additional literature search surface new contradictions?" or "Do independent reviewers converge on the interpretation?"

**COR instantiation example**: "Run three independent literature review passes on a topic: pass 1 finds 23 distinct theories, pass 2 finds 18 (5 dedup'd), pass 3 finds 17 (1 new). The finding count convergence indicates the literature map is approaching completeness. Define convergence threshold (0 new papers in a pass) before starting."

**Verdict**: No variant needed. Directly applicable to research survey and validation work.

---

#### SC-B-008: Convergence-as-validation across independent reds

**Spec lineage**: CO §5.4, CO §49  
**Practice modality**: observation  
**Evidence sources**: kz-engage 0009 (POSITIVE), kailash-rs 0008 (POSITIVE), loom 0042 (POSITIVE)

**COR relevance**: Transfers directly. Research validation inherently uses independent perspectives: different methodologies, different models, different statistical tests. When two independent approaches (e.g., ablation + causal analysis) converge on the same finding, the finding's validity is corroborated. When they diverge, the divergence itself is high-signal: it reveals method-dependence or missing variables.

**COR instantiation example**: "Two independent evaluation teams assess a model using different benchmarks and metrics. Team A (via accuracy) concludes the model improved by 3%. Team B (via F1 + calibration) concludes 2%. The convergence on magnitude gives confidence; the divergence on metric choice reveals that calibration shifted while raw accuracy plateaued — a finding neither perspective alone would highlight."

**Verdict**: No variant needed. The observation modality and convergence principle are inherently applicable to research validation.

---

#### SC-B-015: Reinterpret MUST rules from "human performs" to "human approves"

**Spec lineage**: CARE §4, CO §28, CO §4  
**Practice modality**: brokerage-rep  
**Evidence sources**: loom 0021 (POSITIVE)

**COR relevance**: Transfers with minor context shift. A research context has rules ("statisticians must approve the p-value threshold," "peer review must check citations," "methodology changes require ethics approval"). Many are formulated as "human performs X" when human judgment is only needed to approve X. The move — reinterpret and amend the rule text — is identical. A researcher running parallel ablation studies on model variants could reinterpret "human must run each experiment" to "human approves parallel execution plan" if the parallel runs are trustworthy.

**COR instantiation example**: "The rule 'Human approves every experimental run' becomes 'Human approves the experiment design and oversight sampling strategy; agents execute runs autonomously, human reviews aggregate results.' The reinterpretation preserves authority (human still gates validity) while removing throughput bottleneck."

**Verdict**: No variant needed. The interpretation logic applies to any context with human-gateable processes.

---

#### SC-B-017: Assign verification-gradient zone explicitly before delegating

**Spec lineage**: PACT §19, PACT §11  
**Practice modality**: brokerage-rep  
**Evidence sources**: loom 0021 (POSITIVE), loom 0025 (POSITIVE), kaizen-cli-rs 0025 (MIXED)

**COR relevance**: Transfers directly. Research workflows delegate analysis tasks to agents and humans at different trust levels. A preliminary literature scan (flagged zone: proceed, human approves batch), a statistical analysis (held zone: pause before writing claims), or a code review (blocked zone: human blocks before merge) each require explicit zone assignment. The four-zone gradient (auto-approved, flagged, held, blocked) is the governance mechanism research needs.

**COR instantiation example**: "When delegating literature extraction to agents, use flagged zone: agents extract papers, human approves 3-paper samples from each category. When delegating statistical claim authoring, use held zone: agents propose claims, human reviews before inclusion. The zone assignment is visible in the session plan."

**Verdict**: No variant needed. The PACT §19 principle is domain-agnostic governance.

---

### ARTIFACT LAYER (1 atom)

#### SC-A-001: Codify-with-explicit-NOT-codified

**Spec lineage**: CO §39, CO §47, CO §17  
**Practice modality**: drill  
**Evidence sources**: kailash-rl 0003 (POSITIVE), kailash-py 0012 (POSITIVE)

**COR relevance**: Transfers directly and is especially critical for research. Every research session produces code, experiment logs, results, and interpretations. The codify discipline — explicitly naming what is NOT codified and why — prevents the same "is this a pattern or a one-off bug?" question being re-examined across research sessions. Kailash-py 0012 exemplifies the reasoning: algorithm implementations (code, not pattern), statistical tests (established science, not decision), and specific bug fixes (one-off, not recurring) should NOT be codified. Literature notes, experimental design decisions, and discovered assumptions SHOULD be.

**COR instantiation example**: "Codify session 3: **What was codified**: Lab best practice for GPU memory management with multiple model variants (decision captured in protocols/), discovered assumption that model.eval() mode reduces VRAM by 30% (baseline requirement), requirement for seed documentation in ablation runs (methodology). **What was NOT codified**: The specific CUDA version fix (code, source is the authority), Hugging Face model download logistics (established infrastructure, not research decision), the initial three failed random seed choices (one-off trial, not pattern)."

**Verdict**: No variant needed. The four NOT-codified categories apply equally to research as to codegen.

---

### PRACTITIONER LAYER (12 atoms)

#### SC-P-002: Run empirical model-vs-rule ablation in clean-room

**Spec lineage**: CO §1, CO §22  
**Practice modality**: build  
**Evidence sources**: loom 0049 (POSITIVE)

**COR relevance**: Transfers directly; is a core research move. Ablation is foundational to research methodology: testing what changes when you remove each component. The entry 0049 demonstrates the exact research workflow: clean-room isolation (no context contamination), three phase testing (baseline, rule-loaded, cross-model), and documented results feeding model gap analysis. This is ablation study methodology.

**COR instantiation example**: "Research ablation: hypothesis is that pretraining corpus size drives model behavior on safety tasks. Setup: fine-tune identical architectures on (a) clean-room baseline (random initialization), (b) rule-loaded models (with existing safety artifacts), (c) different corpus sizes. Measure which factor drives safety compliance. Do not run in-session ablation by telling the model to 'ignore safety' — that contaminates the baseline. Use clean-room environment without safety rules."

**Verdict**: No variant needed. Ablation study is already research methodology. The formulation directly applies.

---

#### SC-P-007: Define session completion state before starting

**Spec lineage**: CO §28, CO §29  
**Practice modality**: drill  
**Evidence sources**: loom 0012 (POSITIVE), loom 0031 (POSITIVE)

**COR relevance**: Transfers directly. Research sessions have indefinite scope without a completion state definition. The move — write down "done" as a testable checklist BEFORE implementation, not as a narrative summary AFTER — is essential to experimental methodology. Loom 0031 (17 workspaces, 5 phases, dependency sequencing) is exactly a multi-phase research programme: completion state definitions gate each phase.

**COR instantiation example**: "Research session completion state for ablation study: **In scope**: baseline model training (test_baseline_converges passes), three variant models (all ≥90% validation accuracy), ablation metrics document (mean ± std across 5 runs each), comparison table in paper draft. **Out of scope**: hyperparameter optimization (using defaults), cross-dataset validation (single dataset only), statistical significance testing (deferred to paper revision). **Completion gate**: ablation_metrics.csv exists with all 6 columns populated, paper draft compiles with no TODOs in results section."

**Verdict**: No variant needed. The completion state discipline applies to any session.

---

#### SC-P-011: Detect when audit categorization is structurally blind

**Spec lineage**: CO §15, CO §49  
**Practice modality**: case  
**Evidence sources**: loom 0005 (NEGATIVE), loom 0011 (POSITIVE)

**COR relevance**: Transfers directly to research contexts. Research audits — literature review comprehensiveness, code coverage, experiment reproducibility — can be structurally blind to the thing they're measuring. If literature is discovered via citation networks (progressive disclosure), a "referenced by name" grep reports false negatives. If research code uses dynamic imports or plugin systems, an orphan-detection method that checks only explicit imports will misclassify working code as dead. The move — verify the audit method matches the discovery mechanism — applies universally.

**COR instantiation example**: "Audit: Are all citations in the paper's related work section verified? Audit method: grep the paper for `[1]`, `[2]` reference markers and check each against the bibliography. Structural blindness: if citations are discovered dynamically (e.g., via a citation generation tool that reads the BibTeX file and injects them), grepping the paper source may miss programmatically generated citations. Better method: regenerate the paper's citations from the BibTeX file and check coverage."

**Verdict**: No variant needed. The audit-method-matching discipline applies to any measurement.

---

#### SC-P-012: Decide whether to demote or strengthen a rule when ablation shows partial redundancy

**Spec lineage**: CO §1, CO §39, CO §22  
**Practice modality**: case  
**Evidence sources**: loom 0006 (POSITIVE), loom 0049 (MIXED)

**COR relevance**: Transfers directly to research methodology optimization. A research protocol ("all code must be version-controlled," "all results must be reproducible via provided scripts") may be partially model-native: some models do this naturally, others require enforcement. The move — classify each partially-redundant protocol as "demote to on-demand" or "strengthen with enforcement" based on failure cost, not compliance percentage — is decision logic research teams need. A 99%-compliant protocol with catastrophic failure mode (unreproducible results) should be strengthened; a 50%-compliant protocol with trivial failure mode (minor style difference) might not need enforcement.

**COR instantiation example**: "Ablation data shows: (a) baseline models check types naturally (95% compliance) but occasional missing annotations cause downstream failures → strengthen with static type checker. (b) baseline models write clear variable names (90% compliance) but occasional poor naming causes reviewer confusion → demote to style guide, not enforced. Decision hinges on failure cost, not percentage."

**Verdict**: No variant needed. The decision framework is methodologically universal.

---

#### SC-P-013: Treat internal estimate inconsistency as high-confidence signal

**Spec lineage**: CO §28  
**Practice modality**: drill  
**Evidence sources**: loom 0034 (NEGATIVE), loom 0035 (POSITIVE), loom 0036 (POSITIVE)

**COR relevance**: Transfers directly to research planning. An experiment plan may contain two effort estimates (detailed analysis says 7 sessions; overview says 2-3 sessions for the same scope). This is exactly the pattern 0034-0035 demonstrates: a single analytical process producing contradictory outputs signals incomplete analysis, not that "the truth is in the middle." For research: if literature review scope estimates diverge by 2x within the same survey plan, the analytical process has missed scope.

**COR instantiation example**: "The research plan estimates: detailed statistical design = 3 weeks, critical path summary = 1 week for the same experiment. This 3x inconsistency signals that one estimate compressed away complexity. Trace the divergence: detailed design accounts for optional secondary analyses, summary did not. Response: reject the plan and force reconciliation, not average them."

**Verdict**: No variant needed. Estimate consistency is a quality gate independent of domain.

---

#### SC-P-014: Distinguish pre-existing failures from pre-existing gaps

**Spec lineage**: CO §29, CO §21  
**Practice modality**: case  
**Evidence sources**: sync-express 0002 (POSITIVE)

**COR relevance**: Transfers directly. In research, discovering missing baselines (gap: never implemented) is different from discovering broken evaluation code (failure: broke on refactor). The move — classify each discovery and fix failures, track gaps as separate work — prevents the zero-tolerance rule from forcing infinite scope expansion. A researcher discovering that a competitor's code is closed-source (gap) should not be forced to open-source their own code in the same session; a researcher discovering their own evaluation script broke (failure) must fix it.

**COR instantiation example**: "During model evaluation session: discovered that baselines X and Y have no reproduction code (gap: never built). Discovered that my own baseline's evaluation script has a bug (failure: broke). Fix the failure (rerun evaluation, correct reported numbers). Track the gap (file issue: implement X and Y baselines) as separate research direction."

**Verdict**: No variant needed. Failure/gap classification is a decision framework independent of domain.

---

#### SC-P-015: End complex prompts with "what would invalidate this?"

**Spec lineage**: CO §28  
**Practice modality**: drill  
**Evidence sources**: loom 0034 (POSITIVE), loom 0046 (POSITIVE), loom 0036 (POSITIVE)

**COR relevance**: Transfers directly and is critical to research peer review. Research validation depends on falsification questions: "what would prove this hypothesis wrong?", "what evidence would contradict these findings?", "name the assumption that, if violated, breaks this conclusion." The journal rule (rules/journal.md mandates "For Discussion" sections with falsification questions) institutionalizes this move. Every research paper's limitations section is implicitly a falsification list.

**COR instantiation example**: "Review a paper's methodology section. Instead of 'does this look rigorous?', ask: 'What measurement error would invalidate these conclusions? Which ablation result would force a different interpretation? Name the three strongest assumptions about generalization.' Falsification framing produces different critical feedback than confirmation framing."

**Verdict**: No variant needed. Falsification prompting is research rigor encoded as a practitioner move.

---

#### SC-P-016: Detect overcorrection in both directions as contamination

**Spec lineage**: CO §3.3, CO §1  
**Practice modality**: case  
**Evidence sources**: loom 0049 (NEGATIVE)

**COR relevance**: Transfers directly to research baseline measurement. A researcher testing whether a methodological change (e.g., different loss function) is necessary may contaminate the baseline: agents told to "ignore the new loss function" over-correct by using the opposite loss, not the original baseline loss. The move — compare against clean-room baseline in isolation, not in-session negation — prevents contamination. This is a fundamental research measurement principle.

**COR instantiation example**: "Hypothesis: model optimization bias causes overcorrection on unseen data. Test: compare (a) model with optimization bias visible (overcorrects), (b) model told to ignore bias (over-corrects the other way), (c) model in clean isolation without bias knowledge (true baseline). Outcomes a and b both diverge from c, so a and b are equally contaminated. Only c is reliable baseline."

**Verdict**: No variant needed. Baseline contamination detection is research methodology.

---

#### SC-P-018: Split a multi-session blocker into fast-unblock and parallel-completion

**Spec lineage**: CO §27, CO §28  
**Practice modality**: drill  
**Evidence sources**: loom 0034 (POSITIVE)

**COR relevance**: Transfers directly to research project planning. A blocking task (e.g., "build the evaluation framework") may have a minimum subset (core metrics) that unblocks parallel work (model development), while the remainder (extended metrics, dashboard) can run in parallel. The move — split at the seam, gate each half independently — collapses the critical path in research projects just as in engineering.

**COR instantiation example**: "Evaluation framework is blocking three model teams. Split: (a) core metrics suite (1 week, unblocks models immediately), (b) extended metrics + dashboard (2 weeks, parallel with model work). Both must complete before paper, but core metrics' independent gate unblocks downstream."

**Verdict**: No variant needed. Critical-path collapse is a planning principle independent of domain.

---

#### SC-P-020: Treat second occurrence of a drift class as structural signal

**Spec lineage**: CO §3.2, CO §17  
**Practice modality**: case  
**Evidence sources**: loom 0014 (NEGATIVE), loom 0020 (NEGATIVE), loom 0028 (NEGATIVE)

**COR relevance**: Transfers directly to research. A research team may encounter drift classes: experiment logs scattered across three locations (first incident: fix instances; second incident: implement central logging). Methodology documents drifting from actual practice (first: correct the docs; second: implement validation checks). The move — shift from instance-fix to mechanism-fix at second occurrence — prevents the accumulation pattern.

**COR instantiation example**: "First occurrence: found experiment log in researcher A's laptop; moved to shared repo. Second occurrence: found another researcher's logs on cloud storage; now implement automated log aggregation and pre-commit hook. The second occurrence signals that ad-hoc fixing does not scale."

**Verdict**: No variant needed. Drift-class escalation is a structural intervention principle.

---

#### SC-P-023: Insert a gating workspace at integration seams rather than assertions

**Spec lineage**: CO §27, CO §28  
**Practice modality**: drill  
**Evidence sources**: loom 0034 (NEGATIVE)

**COR relevance**: Transfers directly to multi-team research. A research project with N independent workstreams (model development, data collection, evaluation) may have integration seams (does the data format match the model's input? does evaluation work with this model?). The move — create an explicit integration gate, not scattered assertions — makes seams structural and visible. A researcher verifying that results integrate across three independent analyses uses the same gating principle.

**COR instantiation example**: "Three research teams: Team A develops model, Team B collects data, Team C designs evaluation. Each team passes internal tests. Create integration gate: reference app using A+B+C end-to-end, exercises all seams, gates Phase 2. Without gate, Team A discovers misalignment with B's data format when implementing Team C's evaluation."

**Verdict**: No variant needed. Integration gating is a workflow principle independent of domain.

---

#### SC-P-024: Intervene when temporary workaround accumulates second consumer

**Spec lineage**: CO §3.2, CO §5  
**Practice modality**: case  
**Evidence sources**: loom 0034 (NEGATIVE)

**COR relevance**: Transfers directly to research. A temporary workaround (e.g., "use synthetic data until real data collection completes") becomes problematic when it gains a second consumer (two model families depend on synthetic data). The move — choose at second consumer: either complete the proper solution before third consumer, or promote the workaround to permanent — prevents silent architecture drift. Research teams often accumulate workarounds (temporary experimental protocol, quick evaluation script) that become permanent when they solve a real need better than the intended solution.

**COR instantiation example**: "Workaround: random baseline model developed quickly while proper baseline trains. First consumer: hyperparameter search uses random baseline. Second consumer: model comparison uses random baseline. At second consumer, decide: finish proper baseline before third consumer uses workaround, or accept random baseline as permanent with proper documentation."

**Verdict**: No variant needed. Workaround permanence detection is a governance principle.

---

## Cross-Cutting Observations

### Cluster: Rule Optimization (SC-P-002 → SC-P-016 → SC-P-012 → SC-P-022)

Four atoms form a teaching sequence for optimizing research methodology rules:
1. **SC-P-002**: Run ablation experiment
2. **SC-P-016**: Detect contamination in baseline
3. **SC-P-012**: Classify rule by failure cost
4. (Note: SC-P-022 not in the 18 cross-tagged atoms, but feeds this sequence)

This sequence applies directly to research: optimizing which methodology requirements are essential vs. optional depends on ablation data, contamination detection, and failure-cost judgment.

### Cluster: Estimate Quality (SC-P-013 → SC-P-015 → SC-B-006)

Three atoms form a teaching sequence for validating research estimates:
1. **SC-P-013**: Catch internal estimate contradictions
2. **SC-P-015**: Use falsification questions to stress-test estimates
3. **SC-B-006**: Multi-round convergence on estimates

This sequence ensures research planning is evidence-based rather than intuitive.

### Cluster: Structural Interventions (SC-P-018 → SC-P-023 → SC-P-024 → SC-P-020)

Four atoms address how research structures prevent drift:
1. **SC-P-018**: Split blockers to enable parallelism
2. **SC-P-023**: Gate integration seams explicitly
3. **SC-P-024**: Intervene on workaround permanence
4. **SC-P-020**: Escalate drift from instance-fix to mechanism-fix

These are the research project governance atoms.

---

## COR-Specific Atoms NEEDED (Outside Scope of This Audit)

The audit confirms that the 18 cross-tagged atoms handle governance, validation, and optimization. Research-specific craft still requires NEW atoms covering:

1. **Literature review as a move** (not a phase) — How to build a literature ledger, progressive discovery, citation verification
2. **Reproducibility as spec conformance** — Auditing whether claimed results reproduce, handling non-reproducible papers
3. **Ablation design craft** — How to structure ablations so they falsify specific claims, handling confounds
4. **Publication-ready vs. internal discrimination** — When to promote internal research to publication state
5. **Peer review integration** — How to treat reviewer feedback as structured red teaming
6. **Cross-model generalization** — When results on one model family transfer to another (complementary to SC-P-022)

These are research-specific instantiations of CO principles, not variants of existing atoms.

---

## Recommendations for COR Pass

1. **Do not re-author the 18 atoms.** They are already correct for both COC and COR.

2. **Use these 18 as the foundation layer.** Every COR atom should reference the applicable cross-tagged atom(s) it builds on. Example: "SC-COR-003 Literature review convergence" should relate to SC-B-006 and SC-B-008 (convergence measurement).

3. **Author 15-25 NEW atoms** for research-specific craft that these 18 do not cover.

4. **Update the catalog README** to note that COR atoms rest on this 18-atom foundation, with explicit callouts to which cross-tagged atoms each COR atom extends.

5. **Cross-tag COR atoms backwards** to COC and COE when the new moves apply to those applications too (e.g., "Literature review convergence" may also apply to learner evaluation).

---

## Conclusion

All 18 atoms are immediately COR-ready. The cross-tagging was correct. The COR pass should focus on new atoms capturing research methodology moves that COC evidence does not surface (because codegen journals do not exercise those moves). This audit confirms that the foundation is solid; the next phase builds upward, not sideways.

