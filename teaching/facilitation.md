---
title: Case facilitation notes
date: 2026-04-11
destination: forge
count: 25
---

# Case Facilitation Notes

Instructor guidance for running the 25 FORGE teaching cases. Each note provides common wrong answers, signals of correct insight, follow-up probes, and push-back triggers. These are generic to the case -- not specific to any cohort or audience.

Use alongside the case files in `cases/exemplars/` and the modality guide in `teaching/modalities/case.md`.

## CASE-01 -- The 85% Drift Audit

### Common wrong answers

1. **"The problem is that nobody ran a sync."** This frames the failure as frequency (not enough syncs) when the actual failure was mechanism (the sync was overwrite-based, so running it more often would have made drift worse by clobbering intentional variants). The practitioner should identify the MECHANISM failure, not the frequency failure.
2. **"They should have diff'd the repos periodically."** Spot-checking would have found some drift, but the case shows that only a full surface-area audit revealed the 85/15 ratio. Partial checks create false confidence -- the practitioner should recognise that "checked some files and they looked fine" is not evidence of alignment.
3. **"The Rust template should have stayed identical to the Python one."** This conflates drift (accidental divergence) with variants (intentional divergence). The case explicitly requires distinguishing the two. Forcing templates to be identical eliminates the legitimate language-specific adaptations the Rust template needed.

### What to listen for

1. The practitioner distinguishes between intentional divergence (variant) and accidental divergence (drift) -- and articulates that the taxonomy must exist before fixing begins.
2. The practitioner identifies that measuring the FULL scope of divergence was the critical first move, not sampling.
3. The practitioner articulates that the sync mechanism itself needed redesign before sync frequency could matter.

### Follow-up probes

1. "If you inherited a multi-repo system today and suspected drift, what would you measure first -- and what threshold of drift would change your response from 'fix the files' to 'fix the architecture'?"
2. "Where else besides COC templates could this intentional-vs-accidental divergence classification apply? Think about configuration files, infrastructure-as-code, or API schemas across environments."
3. "The Rust template had gutted five rules to frontmatter-only stubs. Under what circumstances is gutting a file a valid variant rather than a sign of neglect?"

### When to push back

1. Push back when a practitioner says "we should have kept things in sync" without specifying WHAT mechanism would have made sync safe. The case shows that the pre-existing sync mechanism (rsync-style copy) was itself destructive. Demand: what would the sync command actually do when it encounters a file that is intentionally different?
2. Push back when a practitioner treats the 85/15 ratio as obviously bad. Ask: "Is 85% variant coverage necessarily a problem, or is it a problem only because nobody declared those variants? What if 80% of the divergence was intentional?"

## CASE-02 -- Sync Must Merge, Not Copy

### Common wrong answers

1. **"The sync command should have had a blacklist of Rust-specific files."** This addresses the symptom (specific files were damaged) rather than the root cause (overwrite-without-reading loses ANY content, not just known files). Blacklists fail the moment a new Rust-specific file is created and nobody remembers to add it to the list.
2. **"They should have used git-based sync instead of rsync."** Git provides merge semantics for text files, but the problem is conceptual, not tooling. The sync lacked any read step. Changing the tool without changing the semantics (read target before writing) would reproduce the failure in a different format.
3. **"The Rust template should never have diverged structurally."** This eliminates the legitimate need for variant numbering, which was driven by a real progressive-disclosure choice for Rust developers.

### What to listen for

1. The practitioner identifies that the failure was "compute-then-copy" rather than "read-then-merge" -- the target's state was never consulted.
2. The practitioner recognises that the damage was invisible after the sync completed: the command appeared to succeed, and only inspecting file contents revealed the loss.
3. The practitioner connects this to a general principle: any operation that crosses a trust boundary should read the target state before modifying it.

### Follow-up probes

1. "Database migrations have the same read-then-write challenge: you must understand the current schema before applying changes. What other domains require 'read before write' as a safety invariant, and what happens when they skip it?"
2. "If merge semantics had been in place from the start, would the audit in CASE-01 still have been necessary? What class of problems does merge-sync prevent versus what class still requires a periodic audit?"
3. "The decision rejected separate sync commands per repo. Under what conditions would per-repo sync commands be the right choice despite merge being harder to maintain?"

### When to push back

1. Push back when a practitioner says "just add a confirmation step." A confirmation step that says "sync will overwrite 14 directories -- proceed?" gives the human a number but not the information to judge whether the overwrite is safe. Demand: what information must the confirmation present for the human's approval to be meaningful?

## CASE-03 -- Shipped Mock Data After 11 Red Team Rounds

### Common wrong answers

1. **"The red team should have caught the mock data."** This correctly identifies a gap but misidentifies the root cause. The red team measured code quality (are there bugs in what exists?) but not spec coverage (does what should exist actually exist?). The fix is adding a new audit axis, not doing the same audit harder.
2. **"The tests should have included integration tests."** True but insufficient. Even integration tests can pass against mock data if the test expects the mock's output format. The deeper issue is that "tests pass" had become the definition of done, substituting for evidence-based completion.
3. **"The developer was lazy and used mock data."** This blames the agent when the failure was systemic. The agent optimised for the literal completion criterion ("file exists on disk"), which was what the todo system measured. The agent's behaviour was rational given the incentive structure -- the incentive structure was wrong.

### What to listen for

1. The practitioner distinguishes between defect density (bugs in what exists) and spec coverage (does what should exist actually exist?) as two separate quality axes.
2. The practitioner identifies that five compounding process failures enabled the outcome -- not a single point of failure.
3. The practitioner recognises that the agent's behaviour was a predictable consequence of how "done" was defined, not an anomaly.

### Follow-up probes

1. "If this project had been backend-only with no visible frontend, how would the same five process gaps have manifested? What is the backend equivalent of 'displaying random numbers on a page'?"
2. "The fix added `checkFrontendMockData()` as a regex-based hook. A developer could write `fetchOccupancy()` returning hardcoded values without matching the regex. Is there a fundamentally different approach to detecting fake data, or is this always a cat-and-mouse game?"
3. "Where in your own work have you seen 'tests pass' substitute for 'the thing actually works end-to-end'?"

### When to push back

1. Push back when a practitioner says "we need more test coverage" without distinguishing what kind of coverage. The project had 94 passing tests. Coverage was high. The gap was in what the tests measured, not how many there were.

## CASE-04 -- The Token Ablation Clean-Room Experiment

### Common wrong answers

1. **"You should just remove rules that duplicate the model's native behaviour."** This skips the hard part: how do you KNOW which rules duplicate native behaviour? The case shows that in-session testing (Phase 1) gave wrong answers because the rules were still in context. Only clean-room testing produced reliable results.
2. **"More rules are always better -- they reinforce good behaviour."** The case quantifies the cost: 37K tokens of rules per session. Rules consume context window, accelerate compaction, and crowd out conversation history. The practitioner should recognise that rules have a carrying cost, not just a benefit.
3. **"The contamination issue is obvious -- of course you can't test rules while they're loaded."** If it were obvious, Phase 1 would not have been designed the way it was. The subtlety is that the contamination ran in both directions: agents both retained rule influence AND overcorrected to appear "vanilla." Bidirectional contamination is not intuitive.

### What to listen for

1. The practitioner identifies why in-session ablation is unreliable (the rules being tested are still in context, contaminating the "baseline").
2. The practitioner recognises that rule necessity is model-specific -- a rule redundant for Claude may be critical for a different model.
3. The practitioner grasps that the experiment produced a METHOD (clean-room testing), not just a RESULT (tier classification).

### Follow-up probes

1. "What other self-evaluation tasks in AI systems might suffer from the same contamination problem? Think about an agent evaluating its own performance, or a system testing its own safety filters."
2. "If you had to design a clean-room test for a non-COC rule system -- say, a corporate compliance policy enforced by an AI agent -- how would you construct the isolation?"
3. "The experiment found security rules are model-native for Claude. Should COC artifacts include a 'model capability profile' that adjusts rules per model, or is one-size-fits-all safer?"

### When to push back

1. Push back when a practitioner says "just benchmark the model and remove redundant rules" without specifying how the benchmark avoids contamination. The case exists precisely because naive benchmarking gave wrong answers.

## CASE-05 -- The Enforcers Must Obey Their Own Rules

### Common wrong answers

1. **"The hooks should have been reviewed more carefully before shipping."** This is true but misses the structural insight. The hooks were enforcement code -- they existed to enforce security rules on other code. The structural problem is that enforcement code was exempt from the rules it enforced. Review thoroughness is not the fix; self-referential application of rules is.
2. **"This is just a standard command injection bug."** The technical classification is correct, but the case is about WHERE the bug lived, not WHAT the bug was. A command injection in user code is a code bug. A command injection in enforcement infrastructure is a governance failure: the safety system became the attack vector.
3. **"They should have used a linter to catch `execSync`."** This addresses one specific API but not the principle. The next enforcement hook might use a different dangerous pattern that the linter does not check. The skill is treating enforcement code as higher-privilege code requiring higher scrutiny, not adding pattern-specific checks.

### What to listen for

1. The practitioner articulates the meta-level insight: enforcement code must be held to a STRICTER standard than the code it enforces, not the same standard.
2. The practitioner identifies the attack surface: hooks run automatically, on session start, with full user permissions, and are never manually reviewed because they are "trusted."
3. The practitioner recognises the perspective shift: hooks are not "our code that enforces safety" but "code that runs with elevated privilege and therefore requires elevated scrutiny."

### Follow-up probes

1. "What other infrastructure in a development environment runs automatically with elevated privileges and is rarely re-audited? Think CI/CD pipelines, pre-commit hooks, IDE plugins."
2. "The vulnerability was exploitable via a malicious `.claude/VERSION` file in a cloned repository. In a world where developers routinely clone unknown repos, what is the trust model for configuration files that hooks consume?"
3. "Should enforcement code have its own separate red team pass, or should it be included in the standard red team with a higher severity classification?"

### When to push back

1. Push back when a practitioner says "just audit the hooks once and fix the issues." The case's lesson is that hooks must be RE-audited periodically, because they evolve like any other code. A one-time audit creates the same false confidence that led to the original vulnerability. Demand: what trigger should cause re-audit?

## CASE-06 -- Three Critical API Mismatches Before a Line of Code

### Common wrong answers

1. **"The plan author should have read the source code before writing the plan."** While correct, this frames the fix as individual diligence rather than process design. The COC process uses specialist delegation precisely because plan authors cannot be expected to hold the full API surface of every framework in memory. The process fix is the specialist review gate, not better memory.
2. **"This is what unit tests are for -- they catch API mismatches during implementation."** True, but the cost profile is entirely different. Catching a wrong field name at plan review costs zero implementation time. Catching it during implementation costs debugging, rollback, and plan revision. The case is about leverage: where in the lifecycle do you want to find these errors?
3. **"Three critical issues in 14 todos isn't bad -- that's normal for planning."** This normalises a failure rate that would have caused three runtime crashes. The 3-critical, 5-high ratio means 57% of the plan would have produced failures. The practitioner should find this alarming, not acceptable.

### What to listen for

1. The practitioner identifies the cost asymmetry: corrections at plan review are nearly free; corrections during implementation are expensive.
2. The practitioner recognises that the plan was architecturally sound but API-incorrect -- and that these are different failure modes requiring different review methods.
3. The practitioner connects specialist delegation to the plan review gate specifically, not to implementation.

### Follow-up probes

1. "If you had a project with 50 todos spanning 5 different frameworks, how would you structure the specialist review? Review each framework separately, or review all at once?"
2. "The plan assumed `read_from_memory()` existed as an API. This was likely based on documentation or an earlier version. What process ensures that plans reference the CURRENT API rather than the documented or intended one?"
3. "In a non-software domain -- say, planning a construction project -- what is the equivalent of 'specialist delegation at the plan level'? When does an architect check structural engineering assumptions before finalising a design?"

### When to push back

1. Push back when a practitioner says "just prototype quickly to validate the APIs." Prototyping discovers API mismatches but costs implementation time for throwaway code. The specialist review achieves the same validation at plan cost, not implementation cost. Demand: when is prototyping more cost-effective than specialist review, and when is it less?

## CASE-07 -- The GLOBAL BUG in Loom Source

### Common wrong answers

1. **"The practitioner should have just fixed the paths and moved on."** The local fix was necessary but insufficient. The paths were synced from loom/, so the next sync would revert the fix. The practitioner should identify that a fix to a synced artifact is temporary unless the source of truth is also corrected.
2. **"The loom/ repository should have had tests for cross-references."** Reasonable, but the case is about the ESCALATION skill, not about testing. Even if tests catch the bug eventually, the practitioner who found it in a downstream repo must escalate it to the upstream authority rather than only fixing locally.
3. **"The progressive-disclosure system should have failed loudly."** This is a legitimate design discussion (and is one of the case's own discussion questions), but it is not the central move. The case is about authority-chain awareness: knowing where an artifact's authority lives and pushing the fix to that location.

### What to listen for

1. The practitioner immediately asks "where does this file come from?" when finding a bug in a synced artifact, rather than only asking "how do I fix it?"
2. The practitioner articulates the local-fix-plus-upstream-escalation pattern: fix locally for immediate correctness, escalate upstream for systemic correction.
3. The practitioner recognises that without escalation, every downstream repo consuming the same skill files has the same bug and will never know.

### Follow-up probes

1. "You discover a bug in a configuration file that your team receives from a platform team's shared repository. You can fix it locally in 5 minutes. The platform team's change process takes 2 weeks. What do you do?"
2. "What happens if the upstream (loom/) rejects the proposal or delays indefinitely? Should the downstream repo block sync, accept the revert, or maintain a permanent local override?"
3. "In a supply chain analogy: a manufacturer discovers a defect in a component supplied by a vendor. They can fix it locally in their assembly process. Under what circumstances is the local fix sufficient, and when must the vendor be notified?"

### When to push back

1. Push back when a practitioner says "just file a bug upstream" without also fixing locally. The case requires both: local fix for immediate correctness AND upstream escalation for systemic fix. Demand: what happens to users of the Rust repo between now and when the upstream fix ships?

## CASE-08 -- Ten Wrong Assumptions About the SDK

### Common wrong answers

1. **"The documentation should have been accurate."** True, but the case shows that even accurate documentation is insufficient -- the plan author may have consulted docs that described an INTENDED API rather than the IMPLEMENTED API. The practitioner should recognise the gap between documented intent and implemented reality as a structural feature of evolving SDKs, not an anomaly.
2. **"Just build a quick prototype to validate the APIs."** Prototyping discovers API mismatches but costs implementation time. The specialist review achieved the same result at plan cost. The case argues for specialist review AS the validation mechanism, not prototyping.
3. **"Ten corrections out of a full plan isn't that many."** The corrections were not cosmetic -- they included a silent no-op (`write_to_memory` without SharedMemoryPool), a missing API (`read_from_memory` does not exist), and a blocking API assumed non-blocking (`Nexus.start()`). Any one of these would have caused significant debugging time during implementation.

### What to listen for

1. The practitioner distinguishes between architectural correctness (the right frameworks, right modules, right data flow) and API correctness (the right method names, right parameters, right lifecycle semantics).
2. The practitioner identifies that the specialist did not redesign anything -- they corrected SURFACE details while confirming structural soundness.
3. The practitioner recognises the silent-failure risk: `write_to_memory()` silently no-ops without shared memory configuration, so the plan would have appeared to work during initial testing.

### Follow-up probes

1. "Five corrections involved APIs that don't exist or don't work as documented. Who bears the cost of documentation drift -- the plan author who trusted the docs, or the SDK team that let docs diverge from implementation?"
2. "Could API verification be automated? Imagine a tool that reads a plan's method calls and checks them against the current source code. What would it catch, and what would it miss?"
3. "In a medical context, a surgeon plans a procedure based on a textbook. The actual patient anatomy differs from the textbook. What is the equivalent of 'specialist delegation at the plan level' in surgical planning?"

### When to push back

1. Push back when a practitioner says "the architecture was correct, so the ten corrections were minor." Silent no-ops and non-existent APIs cause the most insidious bugs: the code runs, no errors appear, but the behaviour is wrong. Demand: which of the ten corrections would have produced a runtime error versus which would have silently produced wrong behaviour?

## CASE-09 -- The Vacant Approver Who Could Approve Bridges

### Common wrong answers

1. **"The vacancy check should have been in both features from the start."** This correctly identifies the fix but misses the insight about WHY it was missing. The features were developed as independent work items. The process did not require cross-feature interaction analysis. The practitioner should identify the PROCESS gap (no cross-feature interaction testing), not just the CODE gap (missing vacancy check).
2. **"They should have extracted a shared enforcement function."** Reasonable code design, but the case is about DETECTION, not implementation. The question is how you FIND the gap, not how you implement the fix. A shared function only helps if someone thinks to create it; the case shows that independent feature development naturally produces duplicated, inconsistent enforcement paths.
3. **"Two red team agents finding the same bug is redundant."** The two agents approached from different angles: one asked "what can a vacant role do?" (capability enumeration) and the other asked "what does bridge approval assume?" (assumption audit). These are complementary approaches that converge on the same finding, not redundant effort.

### What to listen for

1. The practitioner articulates the cross-feature interaction principle: features that share state will interact whether or not their specifications acknowledge it.
2. The practitioner identifies that the gap was invisible to any test that exercised only one feature at a time.
3. The practitioner recognises that the process (independent todos, independent test suites) systematically failed to surface cross-feature interactions.

### Follow-up probes

1. "You are adding a 'suspension' state to roles (different from vacancy -- the role has an occupant but they are temporarily unable to act). Which existing features would you check for interaction with suspension, and how would you systematically enumerate them?"
2. "In a non-governance domain -- say, an e-commerce system -- what is the equivalent of two features sharing state without sharing invariants? Think about inventory management and order processing."
3. "The case says cross-feature interaction testing must be explicit in todo specifications. Draft what that would look like for a todo that adds a new governance action."

### When to push back

1. Push back when a practitioner proposes "just write more tests" without specifying WHAT kind of tests. Unit tests per feature would not have caught this. The needed tests are cross-feature: "create a vacant role, then attempt bridge approval through that role." Demand: how do you decide which feature pairs to test together?

## CASE-10 -- 4,579 Observations, 8 Duplicate Instincts, Zero Learning

### Common wrong answers

1. **"The observation hooks were buggy."** The hooks worked correctly -- they captured events and wrote them to the JSONL file. The problem was that they captured the WRONG events (infrastructure lifecycle) rather than USEFUL events (decision points). The system processed its inputs faithfully; the inputs were noise.
2. **"The instinct processor should have filtered out low-quality observations."** A filter would have discarded the noise but produced nothing useful in return -- the decision-point events were never captured in the first place. Filtering garbage does not produce signal; it produces less garbage.
3. **"Layer 5 is aspirational and should be removed."** The case explicitly considers and rejects this option. The practitioner should recognise that removing Layer 5 abandons the knowledge-compounding principle that the autonomous execution model depends on.

### What to listen for

1. The practitioner distinguishes between structural completeness (every component exists, every hook fires) and functional completeness (the system produces useful output).
2. The practitioner traces backward from the empty output through each pipeline stage to identify where the failure originates (input layer, not processing layer).
3. The practitioner identifies the root cause at the observation hooks (wrong events captured), not at the instinct processor (faithful processing of wrong inputs).

### Follow-up probes

1. "You discover a data pipeline that has been running for six months, processing thousands of records, but producing no actionable output. Before reading any code, what diagnostic question do you ask first?"
2. "What distinguishes a 'decision-point observation' from an 'infrastructure event'? Give three examples of each from a development session, and explain how a hook would distinguish them programmatically."
3. "If the learning system had been removed and the tokens saved had been used for additional conversation context, would the net effect on session quality have been positive or negative? Under what conditions?"

### When to push back

1. Push back when a practitioner says "the system was broken." The system was not broken -- it was working exactly as designed, processing inputs into outputs faithfully. The design was wrong. Demand: what is the difference between a broken system and a correctly functioning system with wrong inputs?

## CASE-11 -- Four Gaps Between Spec and Code

### Common wrong answers

1. **"The test suite should have included spec-conformance tests."** Correct prescription, but the practitioner should explain WHY the test suite lacked them. The tests validated internal correctness ("does the code do what the code says?") not spec conformance ("does the code do what the spec says?"). These are different questions, and most development workflows only ask the first.
2. **"The Rust SDK being compliant proves the Python team was negligent."** The Rust SDK was developed later and learned from the spec more directly. The Python SDK was developed incrementally, accumulating features over many sessions. The gap pattern is structural (incremental development loses spec alignment over time), not personal.
3. **"Four gaps in 1,139 tests isn't bad."** The gaps were not minor: Gap 1 (zero EATP record emission) was a complete omission of a normative requirement. A module can have 10,000 passing tests and still be fundamentally non-conformant if the tests measure the wrong thing.

### What to listen for

1. The practitioner distinguishes between functional testing ("does it work?") and spec-conformance testing ("does it do what the spec says?").
2. The practitioner recognises the structured gap-list methodology as the skill: severity classification, exact spec section, exact code lines, cross-SDK comparison.
3. The practitioner identifies that the cross-SDK comparison turns the remediation from "design from scratch" into "port proven patterns," reducing decision overhead.

### Follow-up probes

1. "You have a module with 2,000 passing tests and high code coverage. A new spec version is released with 15 additional requirements. How do you audit conformance without rewriting the test suite?"
2. "Gap 2 shows that runtime intersection functions for three envelope dimensions existed but were not wired into write-time validation. What development pattern produces code that CAN do something but does not ENFORCE it?"
3. "Under what conditions would porting from the Rust implementation be dangerous rather than efficient? When should you re-derive from spec instead of copying a proven pattern?"

### When to push back

1. Push back when a practitioner says "just add the missing tests." The gap list methodology is not about adding tests -- it is about auditing the code AGAINST THE SPEC, which may reveal issues no test would naturally cover. Demand: how do you generate spec-conformance test cases from a spec document systematically?

## CASE-12 -- Human Approves, Agent Performs

### Common wrong answers

1. **"The rule should have been rewritten earlier."** This implies the rule was wrong. The rule was right in intent (human authority over classification) but was operationally blocking at scale. The reinterpretation preserves the rule's authority guarantee while changing its mechanism. The rule was not wrong -- its operational interpretation was.
2. **"Just let the agents classify without human approval for efficiency."** This removes the human authority gate entirely, which is a different move from reinterpreting it. The case preserves the human as the AUTHORITY (approver) while delegating the EXECUTION (mechanical comparison) to agents.
3. **"Five agents agreeing unanimously is sufficient -- the human approval is a rubber stamp."** The case explicitly raises this as an open question. If the human cannot meaningfully evaluate the summary, the approval gate is performative. The practitioner should identify what information the human needs to exercise genuine authority.

### What to listen for

1. The practitioner distinguishes between the rule's INTENT (human has authority over classification) and the rule's LETTER (human performs classification).
2. The practitioner identifies that the reinterpretation was itself a governed action: documented as a DECISION entry, not silently bypassed.
3. The practitioner connects this to CARE's Human-on-the-Loop principle: the human sets constraints and approves results rather than performing each action.

### Follow-up probes

1. "You have a compliance rule that says 'a human must review every financial transaction over $10,000.' The volume is 500 transactions per day. How would you reinterpret this rule to preserve the authority guarantee while making it operationally feasible?"
2. "Is there a class of MUST rules where the act of performing the work IS the governance mechanism -- where delegating execution to an agent fundamentally undermines the rule's purpose?"
3. "If the agent team had DISAGREED on a classification (3 say variant, 2 say global), should the human see only the majority vote, only the disagreement, or both? How does this change the quality of the human's approval?"

### When to push back

1. Push back when a practitioner says "the human approved the batch, so governance was satisfied." Ask: what information did the human see? A summary that says "64 variant, 15 skip" gives the human a number but not the knowledge to evaluate whether any specific classification was wrong. Demand: what is the minimum information the human needs to make the approval meaningful rather than performative?

## CASE-13 -- Public Fields as Security Holes

### Common wrong answers

1. **"The chain already validates signatures, so the public fields don't matter."** The case acknowledges this runtime protection but explains why it is insufficient. Public fields create a false affordance: code compiles and runs with mutated fields, producing records that only fail when presented back to the chain. Moving the failure from runtime to compile-time is strictly better.
2. **"Just make all fields private by default in all structs."** Blanket privacy is not the move. The practitioner identified WHICH fields were security-sensitive and changed only those. Some fields can safely remain public. The skill is in the classification, not the blanket policy.
3. **"This is just a Rust best practice, not a security issue."** The encapsulation is a security boundary, not a code-style choice. The specific attack vectors (un-revoking a delegation, widening a scope) have governance consequences: they violate monotonic tightening, which is a PACT invariant. This is a trust protocol violation, not an idiomatic preference.

### What to listen for

1. The practitioner identifies the specific attack vectors (flip `revoked`, widen `dimension_scope`) rather than speaking generically about "mutation."
2. The practitioner articulates the "false affordance" concept: code that compiles and runs without error but produces an invalid state.
3. The practitioner connects encapsulation to a spec invariant (PACT monotonic tightening) rather than treating it as a standalone code-quality issue.

### Follow-up probes

1. "In Python, there is no `pub(crate)` equivalent -- everything is accessible. How would you achieve the same security boundary for a `DelegationRecord` class in Python? Consider `__slots__`, `@property`, `@dataclass(frozen=True)`, or runtime validation."
2. "If you were designing a new trust-bearing data structure, would you start with all fields private and add getters, or start with all public and restrict after security review? What are the failure modes of each approach in a fast-moving codebase?"
3. "The `set_multi_sig()` method was added as an approved mutation path. What invariants must this method enforce to remain safe? Can you design a mutation path that is safe by construction rather than safe by validation?"

### When to push back

1. Push back when a practitioner says "the runtime validation is sufficient because it catches the mutation eventually." Ask: what happens in the window between mutation and validation? If a cloned, mutated record is stored, logged, or transmitted before the chain validates it, the invalid state has already escaped.

## CASE-14 -- The Deny Bypass Nobody Tested

### Common wrong answers

1. **"The developer made a copy-paste error."** Plausible, but the case is about the TEST GAP, not the code bug. The one-line fix was trivial. The skill is recognising that deny-by-default semantics require NEGATIVE tests, and that the test suite had zero of them.
2. **"A linter could have caught the identical branches."** True for this specific bug, but the case teaches a general principle: security flags require negative-case testing. The next security bypass might not have identical branches -- it might have subtly different branches that both permit access.
3. **"The test suite had good coverage, so the bug should have been caught."** Code coverage measures which lines execute, not which security invariants hold. 100% line coverage of the `deny_by_default = true` branch would show the line running but not reveal that it produced the same result as the `false` branch. The test coverage was high; the assertion coverage for security invariants was zero.

### What to listen for

1. The practitioner identifies negative testing as the specific gap: tests that verify "this request is DENIED" are as important as tests that verify "this request succeeds."
2. The practitioner recognises that the vulnerability was silent -- no errors, no logs, no warnings -- which means it was invisible to normal development and testing.
3. The practitioner connects this to the verification gradient: high-trust boundaries (auth middleware) require adversarial verification, not just functional verification.

### Follow-up probes

1. "Write the test case in pseudocode that would have caught this bug. Then write two more negative tests that any deny-by-default authorization system should have."
2. "A static analysis tool can detect 'two branches of a conditional that do the same thing.' Should this check be mandatory for all security-critical code? What other static patterns would catch authorization bypass bugs?"
3. "In a physical security analogy: a building has a policy that 'only badged employees may enter after hours.' But the door is always unlocked. What is the organizational equivalent of negative testing -- how would you discover that the policy is not enforced?"

### When to push back

1. Push back when a practitioner says "we need more comprehensive testing" without specifying negative tests. Comprehensive positive testing (all mapped routes work correctly) is exactly what existed. It was insufficient. Demand: what specific negative test case is needed for EVERY deny-by-default system?

## CASE-15 -- Rule Plus Hook

### Common wrong answers

1. **"A hook alone is sufficient -- it blocks the bad behaviour."** A hook without a rule enforces behaviour without explaining the rationale. When an agent encounters the hook's warning or block, it cannot reason about exceptions because it does not understand WHY the constraint exists. Rules provide the "why"; hooks provide the "when."
2. **"A rule alone is sufficient -- agents will follow it."** A rule without a hook relies on every agent reading and remembering the rule across all sessions. New sessions, different agents, or agents with truncated context may never load the rule. The hook provides a runtime check independent of whether the rule was loaded.
3. **"Just add the venv to the project template so it's always there."** This addresses the initial setup but not ongoing enforcement. A template creates the venv once; the hook checks it every session. Stale venvs (outdated dependencies, wrong Python version) are a runtime problem that templates do not solve.

### What to listen for

1. The practitioner articulates that neither artifact alone is sufficient -- the rule provides rationale and the hook provides runtime enforcement.
2. The practitioner identifies the soft-to-hard escalation pattern: start with a warning (exit code 0) to validate the check, then escalate to a block (exit code 2) once confidence is established.
3. The practitioner connects the case to the general principle of convention codification: turning an informal "we should" into formal artifacts that prevent drift.

### Follow-up probes

1. "If you had only one artifact slot -- a rule OR a hook but not both -- which would you choose for this constraint? What violations would each miss on its own?"
2. "The hook currently warns but does not block. What criteria would you use to decide when to escalate from warning to blocking? Is there a principled threshold?"
3. "Identify a convention in your own work that is known but not enforced. What rule-plus-hook combination would codify it? What would the rule say, and what would the hook check?"

### When to push back

1. Push back when a practitioner describes only the MECHANISM (rule + hook) without explaining the LIFECYCLE (soft warning first, then escalation to hard block). The case explicitly discusses this escalation path. Demand: how do you decide when the warning has been validated enough to become a block?

## CASE-16 -- 24 Directories of Silent Drift

### Common wrong answers

1. **"The `/codify` command should assign numbers from the global scheme."** This is a correct process fix, but the case is about DETECTION, not prevention. The 24 directories already existed when the audit ran. The practitioner should recognise that detection of existing drift is a different skill from prevention of future drift.
2. **"Just renumber all the Rust directories to match Python."** Renumbering without classification conflates two categories: directories that are Rust-specific versions of globals (should be renumbered AND tagged as variants) and directories that are Rust-only (should keep their numbers and not be synced). Blanket renumbering loses the classification information.
3. **"The BUILD repos should have been syncing continuously."** Continuous sync with rsync semantics would have overwritten Rust-specific content (per CASE-02). The sync mechanism must be read-then-merge, and even then, numbering divergence would produce merge conflicts rather than silent drift.

### What to listen for

1. The practitioner distinguishes between renumbering (mechanical fix) and classification (semantic decision: which directories are variants vs. Rust-only vs. true duplicates).
2. The practitioner identifies that the fix deliberately did NOT upstream the merged content, leaving that as a separate human-gated step.
3. The practitioner measures the full scope of divergence (24 directories, 12 semantic collisions, 14 misaligned duplicates) rather than treating it as "a few files out of sync."

### Follow-up probes

1. "If `/codify` had consulted the global scheme before assigning numbers, the 24-directory divergence would not have occurred. But what happens when two BUILD repos run `/codify` simultaneously and both try to claim the next number? How do you coordinate numbering in a decentralised system?"
2. "Of the 14 directories that were 'Rust-specific versions of globals at different numbers,' how would you predict -- without reading any files -- which are true variants vs. accidental drift? What heuristic would you use, and how would you validate it?"
3. "This is a namespace collision problem. Where else do namespace collisions occur in software systems, and how are they typically prevented? Is skill numbering fundamentally a namespace problem?"

### When to push back

1. Push back when a practitioner says "merge the content and move on" without addressing the upstream step. The case explicitly notes that the merged content has NOT been upstreamed and a Gate 1 review is still needed. Demand: why was upstreaming deliberately deferred, and what would have gone wrong if it had been done immediately?

## CASE-17 -- Layer 5 Is Not Aspirational

### Common wrong answers

1. **"Just simplify Layer 5 to something achievable."** The case explicitly considers and rejects this option. Simplifying L5 to "capture some observations, declare victory" converts a real architectural requirement into a checkbox. The practitioner should identify that "simplify" is a way to hide the gap rather than fix it.
2. **"The instinct processor was broken."** The processor faithfully produced instincts from its inputs. The inputs were noise (infrastructure events), so the outputs were meaningless. The processor was not broken; it was correctly processing garbage. The root cause is at the observation layer, not the processing layer.
3. **"Zero learning across 4,579 observations means the system design is fundamentally flawed."** The system design (observe, distill, evolve, compound) is sound. The implementation of the first step (observe) captured the wrong events. The case distinguishes between a flawed design and a correct design with a wrong implementation of one component.

### What to listen for

1. The practitioner distinguishes between structural completeness (every component exists) and functional completeness (the system produces its intended output).
2. The practitioner identifies the "absence of success" as the trigger, not the "presence of failure" -- nothing broke, but nothing worked.
3. The practitioner articulates why "remove" was rejected: without knowledge compounding, the autonomous execution multiplier cannot be achieved.

### Follow-up probes

1. "Define what makes an observation a 'decision point' versus an 'infrastructure event.' How would you test whether a new observation hook is capturing the right kind of event?"
2. "The instinct processor produced hardcoded 0.9 confidence scores. Why is a fake confidence score worse than no confidence score at all? What downstream decisions would change if confidence were 0.5 versus 0.9?"
3. "If you had to choose between a learning system that produces five high-quality instincts per 100 sessions and no learning system at all, what factors determine whether the five instincts justify the system's overhead?"

### When to push back

1. Push back when a practitioner says "the system worked correctly -- it just needs better inputs." This is technically true but misses the accountability question: who decided what to capture, and why did they instrument the plumbing rather than the decisions? The case demands a root-cause analysis of the observation choice, not just a description of the fix.

## CASE-18 -- Vacancy Enforcement Only Works One Way

### Common wrong answers

1. **"Just add the vacancy check to `verify_action()`."** This is the correct fix but not the insight. The skill is DETECTING the asymmetry -- recognising that a constraint enforced on one code path should be enforced on every other code path sharing the same invariant. The fix is one line; the detection is the craft.
2. **"The test suite should have covered both paths."** Tests that exercise `check_access` and `verify_action` independently would both pass. The gap is visible only when you compare the two paths and notice that one enforces vacancy and the other does not. Per-path testing does not catch cross-path invariant violations.
3. **"The developers should have reused the vacancy check code."** Code reuse would have prevented the asymmetry incidentally, but the case is about INVARIANT verification, not code structure. Even with a shared function, someone must still verify that both paths call it.

### What to listen for

1. The practitioner identifies cross-path invariant verification as the skill: checking that constraints hold across ALL enforcement paths, not just the one you are currently reviewing.
2. The practitioner recognises that the asymmetry was invisible to normal testing -- it required reading the enforcement logic across both paths and noticing the absence.
3. The practitioner identifies the security implication: actions taken under a vacant role's authority have no human behind them.

### Follow-up probes

1. "You are adding a new enforcement path (`delegate_authority()`) to the governance engine. Using this case as a template, what invariants from the existing paths would you check the new path against?"
2. "In a non-governance system -- say, a file-permission system with `read()` and `write()` -- what would an analogous asymmetry look like? How would you detect it?"
3. "The fix is a single vacancy check insertion. What structural pattern could prevent this class of error entirely -- where a new enforcement path forgets to inherit invariants from existing paths?"

### When to push back

1. Push back when a practitioner says "this is just a bug -- add the check and move on." The case has a PROCESS lesson: features that share state should have cross-feature interaction tests explicitly specified in their todos. Demand: what would that specification look like for this feature?

## CASE-19 -- Orphans, Stales, and Misclassifications

### Common wrong answers

1. **"The manifest should be auto-generated from the filesystem."** Auto-generation removes human classification, which is the governance mechanism. The manifest exists precisely because someone must decide whether a file is global, variant, or excluded -- this cannot be inferred from the filesystem alone.
2. **"Just run the sync more frequently to catch orphans."** Running sync does not detect orphans -- sync consults the manifest and ignores files not listed in it. Orphans are invisible to sync by definition because they are not in the manifest. Only a cross-check between manifest and filesystem detects orphans.
3. **"Eight findings in a red team isn't many."** Four of the eight were critical: three orphaned files that had NEVER been synced to their target template, and five misclassifications that would have silently overwritten Rust-specific content on the next sync. Severity matters more than count.

### What to listen for

1. The practitioner articulates the three-category taxonomy (orphan, stale, misclassified) and explains the distinct failure mode of each.
2. The practitioner identifies that the fixes addressed the current state but did not prevent recurrence -- no automated validation exists to catch future orphans or misclassifications.
3. The practitioner recognises the exclude-vs-variant contradiction as the most structurally dangerous finding: it produced UNDEFINED behaviour dependent on evaluation order.

### Follow-up probes

1. "Design a pre-flight scan that `/sync` could run before executing. What specifically would it check? What is the cost in time and complexity?"
2. "The three orphan files had been missing from the Python template since they were created. If this had been discovered six months later, how would the fix be different? Consider downstream projects that may have built workarounds for the missing skills."
3. "Where else in software systems do you see the orphan-stale-misclassified taxonomy apply? Think about DNS records, IAM permissions, or feature flags."

### When to push back

1. Push back when a practitioner focuses on the individual fixes without addressing prevention. All eight findings were fixed -- but what stops nine new orphans from accumulating before the next manual audit? Demand: what automated check would catch orphans at creation time rather than audit time?

## CASE-20 -- Adjacent But Disconnected

### Common wrong answers

1. **"The modules should be merged into one."** Merging eliminates the integration gap but destroys the independence of each module. The case proposes optional coupling (PACT optionally emits EATP records) that preserves both modules' standalone usability.
2. **"The EATP module should call PACT for governance decisions."** This reverses the dependency direction. The case establishes that PACT should optionally emit EATP records (PACT depends on EATP types), not the reverse. EATP is a lower-level trust protocol that should not depend on higher-level governance.
3. **"They are in the same package, so they must already be integrated."** This is the exact assumption the case dismantles. Physical adjacency (same `trust/` directory) creates an APPEARANCE of integration that the import graph contradicts. Zero imports between the modules means zero integration regardless of directory structure.

### What to listen for

1. The practitioner identifies the gap between structural adjacency (same package tree) and functional integration (import graph, shared data flow).
2. The practitioner articulates the optional-dependency design: `trust_chain_store: TrustStore | None` parameter that enables integration without requiring it.
3. The practitioner identifies the duplicate `AuditAnchor` class as a concrete symptom of non-integration: two classes with the same name and similar purpose, no relationship.

### Follow-up probes

1. "In your own codebase, can you identify two modules that are structurally adjacent but functionally disconnected? How would you verify whether they should be integrated?"
2. "The optional-dependency design preserves backward compatibility but makes the spec requirement (PACT must emit EATP records) opt-in. Is an optional integration sufficient to satisfy a specification requirement, or must the spec requirement be mandatory?"
3. "If the two modules had been in different packages (`kailash-pact` and `kailash-eatp`), would the gap have been discovered sooner or later? Does physical adjacency help or hinder the discovery of integration gaps?"

### When to push back

1. Push back when a practitioner says "just wire them together." The case raises three open design questions (dual audit trail permanence, AuditAnchor collision, standalone emission). Demand: which design choice do you make for each, and what is the consequence of getting it wrong?

## CASE-21 -- The Orphan Audit That Was Wrong

### Common wrong answers

1. **"The 204 files should have been audited anyway to confirm they were useful."** The files were the distilled Kailash documentation library -- the most valuable part of the skill suite. The audit methodology was wrong, not the files. Auditing them would have confirmed they were useful but at significant cost and with the risk of incorrect deletion.
2. **"The red team made an error -- they should have understood progressive disclosure."** The red team flagged the uncertainty themselves ("could mean skills are designed for progressive disclosure"). The error was not ignorance but methodology: applying a static-reference audit to a dynamic-discovery architecture. The red team's self-flagging was actually the saving grace.
3. **"Static reference counting is always invalid for skill files."** It depends on the architecture. If skills were loaded by explicit import, static reference counting would be valid. The error was applying the wrong audit method for THIS architecture, not that static reference counting is universally wrong.

### What to listen for

1. The practitioner identifies the mismatch between audit methodology (static reference counting) and system architecture (dynamic trigger-based discovery).
2. The practitioner recognises that the red team both created and corrected the false finding -- and that the self-correction was triggered by re-examining the premise before executing the cleanup TODO.
3. The practitioner articulates the general principle: audit methods must match the system's loading semantics.

### Follow-up probes

1. "You are auditing a microservices system for unused services. Some services are invoked by event-driven triggers rather than direct API calls. How would you avoid the same false-orphan error?"
2. "If TODO-3.1 had been executed without re-examination, what would have happened? How many files would have been deleted before someone noticed skills disappearing from the on-demand repertoire?"
3. "What other audit methods might produce false findings because they mismatch the architecture? Think about dependency audits, code coverage reports, or access-log analysis."

### When to push back

1. Push back when a practitioner says "always verify before deleting." This is true but too generic. The case's specific lesson is that the VERIFICATION METHOD must match the DISCOVERY METHOD. Verifying via "grep for filename in agent files" is itself the wrong method for a progressive-disclosure system. Demand: what verification method would correctly identify genuinely unused skills in a trigger-based system?

## CASE-22 -- Decision Reversal in One Day

### Common wrong answers

1. **"The first decision was wrong and should not have been made."** The first decision was sound given the information available at the time. Two red team passes endorsed it. The reversal was triggered by new evidence (the programme owner's architectural challenge plus the BaseAdapter audit), not by the first decision being poorly reasoned.
2. **"The red teams failed by not questioning the framing."** Red teams optimised for the safest incremental option (extension over new package), which was a reasonable recommendation. Questioning the framing ("is DataFlow already a fabric?") requires a different kind of analysis -- assumption reversal -- that is not naturally part of a red team's mandate to evaluate feasibility.
3. **"Just update the original decision entry."** The case explicitly uses the supersede-with-reference pattern: the original decision remains as a record of reasoning, and the new decision references it. Editing the original would destroy the decision trail, making it impossible for future readers to understand why the extension approach was considered and rejected.

### What to listen for

1. The practitioner articulates the supersede-with-reference pattern: new entry explicitly names the entry it replaces, preserving the full decision arc.
2. The practitioner identifies that the reversal happened before any implementation work, making the cost zero -- and recognises that timing is critical for reversals.
3. The practitioner distinguishes between the red teams' contribution (evaluating feasibility of the proposed approach) and the programme owner's contribution (questioning the framing itself).

### Follow-up probes

1. "If two days of implementation had been completed before the reversal, what would the cost have been? At what point does sunk cost make a clean reversal impractical?"
2. "The supersede-with-reference pattern and the edit-with-SUPERSEDED-header pattern are two ways to handle reversals in a journal. Which degrades more gracefully when the journal grows to hundreds of entries?"
3. "In a non-technical context -- say, a strategy decision in a business -- how would you structure the decision trail so that reversals are legible rather than embarrassing? What makes organisations resist documenting reversals?"

### When to push back

1. Push back when a practitioner treats the reversal as evidence of process failure. The reversal is evidence of process WORKING: new information surfaced, the decision changed, and the trail was preserved. Demand: what would have been the cost of NOT reversing -- of proceeding with the extension approach because two red teams had endorsed it?

## CASE-23 -- The Audited Side Was Ahead

### Common wrong answers

1. **"Rust should always follow Python because Python is the original."** The case directly contradicts this assumption. Rust's Nexus exceeded Python's in 4 of 5 areas. "Original" does not mean "authoritative" for every subsystem -- the source of truth for each subsystem must be determined by evidence, not by which implementation came first.
2. **"The audit found the Rust gap, so the audit succeeded."** The audit found a SMALL Rust gap (360 lines of BackgroundService) AND a LARGE Python gap (Rust exceeds in 4/5 areas). Reporting only the Rust gap would have satisfied the brief but missed the more important finding. The audit succeeded because the auditor reported BIDIRECTIONALLY.
3. **"Parity audits should always check both directions."** Correct principle, but the non-obvious part is that parity audits have an implicit directionality built into their FRAMING. The brief said "audit Rust for Python parity" -- the expected output was a list of Rust deficiencies. Reporting Python deficiencies requires the auditor to recognise that the question itself was biased.

### What to listen for

1. The practitioner identifies assumption reversal as the core skill: the data contradicted the framing, and the auditor reported the reversal rather than fitting the data into the original assumption.
2. The practitioner recognises that the effort estimate dropped from 2 sessions to half a session, and that overestimation in parity briefs has a trust cost.
3. The practitioner identifies the cross-workspace coordination opportunity: the DomainEventBus finding informs a different team's work.

### Follow-up probes

1. "You are asked to audit Team B's service for parity with Team A's service (Team A is the 'reference'). You discover that Team B's implementation has three features Team A lacks. How do you report this without creating political tension?"
2. "What incentive structure ensures that parity auditors report bidirectional findings rather than only answering the question they were asked? How would you design the audit brief to encourage this?"
3. "In a non-technical context -- say, benchmarking one business unit against another -- what dynamics make it harder to report that the 'secondary' unit is ahead?"

### When to push back

1. Push back when a practitioner says "the Rust implementation is better, so just use Rust as the reference." The source of truth may differ by SUBSYSTEM -- Rust Nexus may be ahead, but Python DataFlow may be ahead in a different area. Demand: how do you determine the source of truth per subsystem rather than per implementation?

## CASE-24 -- 146KB of Rules at 36K Tokens Per Turn

### Common wrong answers

1. **"Just remove the redundant rules."** Redundancy removal helps but the case quantifies that even non-redundant rules are expensive at 36K tokens per turn. The deeper issue is architectural: external rules sit in the uncached suffix of the context window (full cost every turn), while baked-in rules would sit in the cached prefix (zero marginal cost after turn 1).
2. **"More rules mean better compliance, so keep them all."** The case demonstrates that rules have a carrying cost: 36K tokens of rules plus 13K of base prompt means 25% of the context window is consumed before a single user message. This accelerates compaction, which means the agent loses conversation history sooner. Rules that consume context can paradoxically degrade the agent's ability to follow instructions.
3. **"Bake everything into the binary like Anthropic does."** This solves the token problem but sacrifices visibility and updateability. The case recommends a HYBRID approach: bake core directives (cacheable, zero redundancy) and keep project-specific rules external (editable, on-demand). All-or-nothing is not the recommended architecture.

### What to listen for

1. The practitioner identifies the five distinct costs of large rule sets: token waste, context pressure, compaction acceleration, redundancy, and cache-busting.
2. The practitioner distinguishes between the cached prefix (zero marginal cost) and the uncached suffix (full cost every turn) and understands why the distinction matters.
3. The practitioner articulates the hybrid architecture: baked-in core + external project-specific + on-demand skills, rather than treating the question as binary.

### Follow-up probes

1. "If you had to compress 146KB of rules to 13K tokens, what principle would you use to decide which rules to keep, which to merge, and which to drop? How do you distinguish a rule that must be always-loaded from one that can be loaded on demand?"
2. "Redundancy in rules may serve a reinforcement purpose -- making the agent more likely to follow a directive if it appears in multiple places. How would you test whether removing redundancy degrades compliance?"
3. "The hybrid approach moves some rules from `/sync`-managed markdown files to baked-in binary prompts. What governance mechanism replaces `/sync` for those rules? Is the update-difficulty trade-off worth the token savings?"

### When to push back

1. Push back when a practitioner says "just load rules on demand based on the task." This is the progressive-disclosure solution, but the case raises the question of HOW you determine which rules are relevant to a task before the task begins. Demand: what triggers rule loading, and what happens if the wrong rules are loaded (or the right rules are not)?

## CASE-25 -- Timing Side-Channel in API Key Comparison

### Common wrong answers

1. **"The code used `subtle::ConstantTimeEq`, so it was secure."** This is the exact misconception the case dismantles. The PRIMITIVE was constant-time. The ITERATION was not. `.any()` short-circuits, so the function's timing varied with the position of the matching key. Correct primitive plus incorrect usage equals vulnerability.
2. **"Timing side-channels are theoretical -- they require laboratory conditions to exploit."** In networked services with multiple API keys, response time differences are measurable. The case specifies that the attack enables identity fingerprinting (determining which service identity made a request), which has practical consequences for targeted attacks.
3. **"Just use `.all()` instead of `.any()`."** `.all()` also short-circuits (on the first false). The fix is a manual loop that always iterates ALL hashes with bitwise OR accumulation. The practitioner should understand that the fix is about eliminating short-circuit behaviour entirely, not swapping one short-circuit operator for another.

### What to listen for

1. The practitioner identifies that the vulnerability is in the ITERATION pattern, not the COMPARISON primitive -- and articulates why correct primitive usage does not guarantee correct function-level timing.
2. The practitioner recognises that the code LOOKS correct: `ConstantTimeEq` is present, hashes are used, the auth pathway is standard. The vulnerability hides behind correct idiom and correct primitive selection.
3. The practitioner articulates the fix principle: the function must perform the same number of operations regardless of which key matches (constant-time at the function level, not just the comparison level).

### Follow-up probes

1. "What audit checklist item would catch the iterator-level timing leak? Is 'verify constant-time behaviour at the function level' specific enough to be actionable for a non-specialist reviewer?"
2. "The `.any()` pattern is idiomatic Rust. A linter rule that flags `.any()` on security-sensitive comparisons would produce false positives elsewhere. How should a rule system handle security patterns that conflict with language idiom?"
3. "Single-key configurations were not vulnerable. If the system enforced a 'one key per endpoint' policy, the timing side-channel would be unexploitable. When is it appropriate to constrain configuration to reduce attack surface versus fixing the code to handle all configurations securely?"

### When to push back

1. Push back when a practitioner says "I would have caught this in code review by looking for `ConstantTimeEq`." That is exactly the review that would PASS this code -- the correct primitive is present. The vulnerability requires thinking about the timing behaviour of the ENTIRE function, not just checking that the right library was imported. Demand: what specifically would you look for beyond primitive selection?
