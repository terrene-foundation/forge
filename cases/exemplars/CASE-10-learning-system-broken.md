---
case_id: CASE-10
source_path: loom/journal/0004-RISK-learning-system-broken.md
title: "4,579 Observations, 8 Duplicate Instincts, Zero Learning"
atoms_served: [learning-pipeline-diagnosis, observation-signal-filtering]
spec_concepts: [CO §39, CO §40, CO §7, CO §51]
craft_layer: artifact
recommended_use: discussion
destination: both
application: COC
---

# CASE-10: 4,579 Observations, 8 Duplicate Instincts, Zero Learning

## 1. The Situation

The CO architecture defines Layer 5 as the Learning layer. Its pipeline is: observe (capture events during sessions), distill (extract patterns from observations), evolve (update artifacts based on patterns), and compound (accumulated changes improve future sessions). The Kailash project had implemented this pipeline across four repositories. The infrastructure was structurally complete:

- Four commands (`/learn`, `/evolve`, `/checkpoint`, plus `/codify`'s Layer 5 integration)
- Four library files (`learning-utils`, `instinct-renderer`, `instinct-processor`, `instinct-evolver`)
- Auto-processing in `session-end.js` that runs after every session
- Observation capture in multiple hooks
- Instinct files that the system reads on startup

Every component existed. Every file was wired. The infrastructure had been built, tested, and deployed to all four repositories.

## 2. The Trigger

An audit of the actual learning outputs revealed that despite structural completeness, the system produced no useful output. The evidence was stark:

- **4,579 observations** had been recorded in `kailash-py`, but 99% were infrastructure noise: session start, session stop, test run completed. The observations captured the plumbing of sessions, not the decisions made within them.
- **8 instincts** had been generated. All 8 were "workflow_builder pattern" with hardcoded 0.9 confidence scores. They were duplicates of each other.
- **Zero evolved artifacts**. The `.claude/learning/evolved/` directory was empty in all four repositories. Not a single agent, skill, rule, or command had been modified by the learning system.
- `/codify` never read from the learning system. The feedback loop -- observe, distill, evolve, feed back into /codify -- was broken at the "feed back" step.

The attentional pattern is **structural-versus-functional audit**: checking whether a system that appears complete actually produces its intended output, rather than assuming that existence of infrastructure implies functioning of purpose.

## 3. The Move

The move is **learning-pipeline-diagnosis** combined with **observation-signal-filtering**. The practitioner traced the pipeline from end to beginning:

1. **Evolved artifacts empty** -- the evolve step was never triggered or never produced output.
2. **Instincts are duplicates** -- the distill step was not extracting varied patterns because the input observations were monotonic.
3. **Observations are noise** -- the capture hooks were recording infrastructure events (session lifecycle) rather than decision events (framework selection, error-fix pairs, node combinations).

The root cause was at the input layer: the observation hooks captured the wrong things. The infrastructure for observation, distillation, evolution, and compounding all worked -- but "garbage in, garbage out" applied. Infrastructure noise went in; meaningless instincts came out; no evolution was triggered because the instincts were not actionable.

The diagnosis identified three mitigation options: fix the observation inputs (retarget hooks to log decision points), simplify the system (keep manual `/learn`, remove auto-processing), or remove the system entirely (accept learning as aspirational). The decision to fix rather than remove was documented in journal entry 0006.

## 4. The Outcome

The audit became the motivating evidence for a decision (journal/0006) that Layer 5 must be functional, not decorative. The observation hooks were identified as the primary fix target: retarget `session-end.js`, `pre-compact.js`, and `validate-workflow.js` to capture decision events rather than infrastructure events.

The deeper outcome is a diagnostic principle: when a system has complete infrastructure but no useful output, trace backward from the output through each pipeline stage. The failure point is usually at the input -- the system is processing correctly but processing the wrong inputs. This is different from the common assumption that the processing itself is broken.

The audit also revealed a secondary issue: `pre-compact.js` violated `cc-artifacts` Rule 4 by performing semantic analysis via regex, which is structurally incapable of extracting the kind of decision-level information the learning system needs.

## 5. The Spec Connection

**CO §39 (Layer 5 -- Learning)** defines the learning layer as the system that observes agent behavior, extracts patterns, and evolves artifacts. §39 describes a pipeline, not a set of files. The Kailash implementation built the files but did not achieve the pipeline: observations existed, but they did not carry signal; instincts existed, but they were not derived from real patterns; evolution infrastructure existed, but it never ran. §39 says the learning layer must compound knowledge; this system compounded noise.

**CO §40 (Observe-Capture-Evolve)** names the three-step cycle. The audit found that step 1 (observe) was capturing the wrong events, step 2 (capture/distill) was faithfully producing instincts from those wrong events, and step 3 (evolve) was correctly refusing to evolve artifacts based on meaningless instincts. The pipeline was honest -- it did not hallucinate useful patterns from noise. But it also did not produce value.

**CO §7 (Knowledge Compounds)** is the principle that institutional knowledge should accumulate over time. After 4,579 observations across hundreds of sessions, the institutional knowledge was: "use workflow_builder pattern" (which the model already does natively). §7 was not merely unachieved -- it was inverted. The learning system consumed tokens and storage on every session while producing no compounding knowledge.

**CO §51 (Learning Velocity)** measures how fast the learning system acquires useful patterns. The velocity was zero. Not slow -- zero. 4,579 observations yielded 8 duplicate instincts yielded 0 evolved artifacts. §51 says learning velocity is a system metric; this audit established the baseline as zero, which is the minimum necessary to know how much improvement is needed.

## 6. Discussion Questions

1. The observation hooks captured session lifecycle events (start, stop) instead of decision events (framework choice, error resolution). The hooks' authors presumably intended to capture useful events. What would cause a developer to instrument the plumbing rather than the decisions? Is this a specification failure (the hook spec said "capture events" without defining which events matter) or an implementation failure (the developer chose the easiest events to capture)?

2. The system produced 8 identical instincts with 0.9 confidence from 4,579 observations. If the confidence threshold had been set higher (say, 0.99), no instincts would have been generated at all, and the system would have appeared to be non-functional rather than functional-but-useless. Which state is better for diagnosing problems -- a system that produces obviously wrong outputs, or a system that produces no outputs? How does this affect the choice of confidence thresholds in learning systems?

3. The audit proposed three options: fix, simplify, or remove. The decision was to fix. Under what circumstances would "remove" have been the right choice? If the learning system had been removed and the tokens saved had been used for additional context instead, would the net effect on session quality have been positive or negative?
