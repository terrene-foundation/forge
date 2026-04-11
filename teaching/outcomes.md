---
title: Learning outcomes per atom
date: 2026-04-10
destination: forge
count: 67
---

# FORGE Learning Outcomes

One learning outcome statement per atom. Each outcome is what the practitioner can demonstrably do after drilling the atom. Use these as the learning objectives for any downstream course pulling atoms from FORGE.

See `README.md` in this directory for how outcomes fit into the teaching scaffolding layer.

## Brokerage layer (18 atoms)

### SC-B-001 — Read-then-merge sync semantics

After drilling this, the practitioner can refuse any bulk cross-repo overwrite and instead produce a per-file diff table — columns `source`, `target`, `decision (global/variant/skip/halt)` — that halts the sync at the first numbering collision or content drift rather than resolving it inline, for a mock source/target pair seeded with drift, a collision, and a duplicate.

### SC-B-002 — Authority-chain escalation when local fix gets reverted

After drilling this, the practitioner can trace a defective synced artifact back through USE template → BUILD repo → upstream methodology source, file the fix at the authoring layer as the primary deliverable, and mark any local patch as a time-bounded tourniquet keyed to the upstream landing date rather than shipping the local fix alone.

### SC-B-003 — Variant classification — single-sentence test rule

After drilling this, the practitioner can classify ten unfamiliar CC artifact paths as global / variant-py / variant-rs / skip in under thirty seconds each by applying the single-sentence test "Does this rule apply if the user is NOT using Kailash?" before opening any file, and can explain their reasoning-order (primary test first, secondary tests only after resolution) rather than classifying by file location.

### SC-B-004 — Specialist delegation as required first move (framework work)

After drilling this, the practitioner can, given a feature plan touching a Kailash framework API, open the actual SDK source for each touched surface within the last hour, produce a source-cited corrections list that feeds the todo before any code is written, and catch API-shape mistakes (constructor params, async/sync semantics, method names) at planning time rather than at red team.

### SC-B-005 — Spec-to-code conformance audit

After drilling this, the practitioner can extract every MUST and SHOULD from a normative spec into a numbered checklist, grep or trace each one's enforcement in the implementing codebase, and produce a numbered gap list where each gap names the spec section, expected enforcement, and actual behaviour — checking every code path through the engine, not just the obvious one.

### SC-B-006 — Multi-round red team to numeric convergence

After drilling this, the practitioner can define a numeric convergence threshold before running Round 1, execute successive red team rounds against the same artifact with per-round findings-by-severity counts journaled, and refuse to declare convergence or advance the approval gate while any CRITICAL or HIGH finding remains unresolved or while the finding count has not monotonically decreased below threshold.

### SC-B-007 — Worktree-per-agent for safe parallel execution

After drilling this, the practitioner can set up one git worktree per parallel agent (`git worktree add ../repo-agent-N -b branch`) with independent branch, file copy, PYTHONPATH, and test environment before launching concurrent work, and handle merge conflicts as a separate human-gated step after all agents complete rather than allowing two agents to edit the same working directory.

### SC-B-008 — Convergence-as-validation across independent reds

After drilling this, the practitioner can run two or more independent red team agents with different specializations (security vs architecture vs feasibility) against the same artifact without sharing findings between them, then compute the convergence rate and unique-coverage rate of the finding sets, using the convergence metric to validate the audit method itself rather than just merging the lists.

### SC-B-009 — Cross-SDK upstream brokerage

After drilling this, the practitioner can, upon finding a bug traceable to an SDK-level component shared across SDKs, file a tracked issue against every affected repository (Python AND Rust where applicable), apply only a temporary local mitigation tagged with the upstream issue number, and refuse to ship a silent workaround that would diverge the consumer from canonical SDK behaviour.

### SC-B-010 — Cross-repo divergence audit with numbered drift-gap list

After drilling this, the practitioner can audit a downstream repo against upstream to produce a numbered drift-gap list with columns `number / filename / classification (orphan / stale / variant / drift / aligned) / one-line description`, halt the distribution when unintentional drift exceeds threshold, and correctly distinguish orphans (on disk, not in manifest) from stale entries (in manifest, not on disk) from intentional variants.

### SC-B-011 — Version tracking cascade across the repo chain

After drilling this, the practitioner can wire every repo in the chain so it tracks its own COC artifact version, its upstream's version, and the SDK version the artifacts were authored against, surface mismatches at session start / `/codify` stamp / `/sync` Gate 1 and Gate 2, and identify the specific mismatch at each of the three trigger points in a three-repo chain exercise.

### SC-B-012 — Automate manifest-to-filesystem parity check and halt on staleness

After drilling this, the practitioner can write an automated parity check script that uses set operations to compare `sync-manifest.yaml` against the filesystem, exit non-zero on orphans or stale entries, report parity gaps without halting on them, and wire the check into the `/sync` pre-flight so it is not optional.

### SC-B-013 — Map cross-workspace dependency surface before parallelizing

After drilling this, the practitioner can produce an explicit dependency graph for a set of workspaces showing shared-file bottlenecks, abstraction producer-consumer chains, and sequencing constraints, declaring for each pair which can run in parallel and which must be sequenced — covering both file-level and abstraction-level dependencies rather than only the latter.

### SC-B-014 — Journal decision reversals with explicit supersession references

After drilling this, the practitioner can write a new journal entry that reverses a prior decision by naming the prior entry by number and title, stating an explicit "supersedes entry NNNN" line, explaining why the prior decision was wrong, and recording the new decision as a standalone readable entry — without silently editing the old entry or creating an unlinked contradiction.

### SC-B-015 — Reinterpret MUST rules from "human performs" to "human approves" and amend the rule text

After drilling this, the practitioner can classify each MUST rule in a given rule file as reinterpretable or not based on whether the human's unique contribution is execution or judgment, rewrite reinterpretable rules from "human performs X" to "human approves X" in the rule text itself, and correctly refuse to reinterpret rules requiring human judgment that agents cannot replicate.

### SC-B-016 — Audit cross-language parity bidirectionally — the other SDK may be the reference

After drilling this, the practitioner can run a parity audit between two SDK implementations that records per-abstraction parity direction rather than a blanket "B follows A" assessment, identify abstractions where the assumed follower exceeds the assumed reference, and adjust the effort estimate downward for the "follower" direction where the follower is already ahead.

### SC-B-017 — Assign verification-gradient zone explicitly before delegating to an agent

After drilling this, the practitioner can assign each parallel agent in a delegation scenario to one of PACT's four verification-gradient zones (auto-approved, flagged, held, blocked) with written justification, name an escalation trigger per agent for when scope changes require re-evaluation, and refuse to default multiple agents to the same implicit zone.

### SC-B-018 — Separate the policy decision point from the policy enforcement point in access control

After drilling this, the practitioner can refactor a merged `authorize_and_execute(action, role)` function into a `PolicyDecider.evaluate(action, role) -> Verdict` (PDP returning a data structure for all four zones) and an `Enforcer.enforce(verdict, action)` (PEP as sole call site), audit every call site of the governed operation to confirm each passes through the PEP, and write a PDP test that verifies a blocked verdict without triggering execution side effects.

## Artifact layer (15 atoms)

### SC-A-001 — Codify-with-explicit-NOT-codified

After drilling this, the practitioner can classify each item in a recent fix log as CODIFY (naming which artifact — rule/skill/agent/command/hook — it lands in) or NOT-CODIFIED with one of four named reasons (code is authority, established science, one-off bug, already captured elsewhere by path), rejecting aesthetic reasons like "out of scope" or "low priority".

### SC-A-002 — Frontend mock data detection

After drilling this, the practitioner can extend the no-stubs hook (`scripts/hooks/validate-workflow.js`) to BLOCK edits matching JS/TS mock patterns (`MOCK_*`, `FAKE_*`, `DUMMY_*`, `SAMPLE_*` constants; `generate*()` / `mock*()` functions; `Math.random()` for display values), write regexes with zero false negatives on frontend pages, and identify that hook-layer detection — not rule-file text — is the anti-amnesia mechanism.

### SC-A-003 — Defense-in-depth codification across multiple artifact locations

After drilling this, the practitioner can, for a single new rule, produce a codification plan that lands it in at least four distinct artifact types (rule, hook, agent, skill — or equivalent), explain what each artifact type contributes that the others cannot, and name the specific failure mode if any one landing is missing.

### SC-A-004 — Layer 3 hook security audit

After drilling this, the practitioner can audit a real hook file for three vulnerability classes (shell injection via `execSync` with untrusted input, path traversal via unsanitized file paths, DoS via unbounded reads or missing timeouts), identify every external-input source and classify each consumption point as safe or unsafe with a one-sentence rationale, and correctly dismiss at least one false-positive external input.

### SC-A-005 — Encapsulation as security boundary in signed structs

After drilling this, the practitioner can identify security-sensitive fields (signature, revocation flag, scope set) on a signed struct, refactor all fields to private with read-only getters and a single approved-mutation constructor, write an executable bypass test that demonstrates the vulnerability when fields are public, and write a fix test that demonstrates the encapsulation blocks the bypass.

### SC-A-006 — Negative tests for deny-by-default

After drilling this, the practitioner can write a test-first negative test for a `deny_by_default` flag that exercises an unmapped route expecting denial, observe the test fail against a merged-branch bug, fix only the `deny_by_default = true` branch (not both branches), and verify the fix by asserting both HTTP 403 and the presence of a denial log entry.

### SC-A-007 — Zero-config security audit

After drilling this, the practitioner can audit a framework's zero-config `start()` / `serve()` function by enumerating every network-accessible resource it creates, classifying each as read-only vs read-write and localhost-only vs network-reachable, identifying which security parameters were defaulted permissive, and proposing a one-line change that makes the default deny-by-default without breaking the one-line usability promise.

### SC-A-008 — Rule classification — critical vs advisory

After drilling this, the practitioner can classify each of ten rules from a real rule set as critical (enforced deterministically via hook) or advisory (probabilistic instruction-following) based on whether a single violation causes irreversible damage or is correctable in review, name the specific enforcement mechanism for each critical rule, and estimate the token savings from moving advisory rules to on-demand loading.

### SC-A-009 — Progressive disclosure architecture for CC artifacts

After drilling this, the practitioner can sort a flat CC artifact set of 15-20 rules and 5-10 agents into the four-level hierarchy (master directive / index / topic / deep reference), add `paths:` frontmatter to every topic-level rule, extract any agent over 400 lines into a stub plus a skill file, remove restated rules from CLAUDE.md, and achieve a measurable always-loaded token reduction without removing institutional knowledge.

### SC-A-010 — Token budget — baked-in vs external boundary decision

After drilling this, the practitioner can classify each rule in a 20+ rule external set as "bake" or "keep external" using the three-axis test (project-variance / cache-position / editability-need), estimate the resulting token budget and per-session cost difference between all-external and hybrid architectures, and identify which rules sit in the cacheable prompt prefix vs the dynamic suffix.

### SC-A-011 — Framework-first check before building new functionality

After drilling this, the practitioner can, given a brief for new functionality, run a mandatory grep of the existing framework for prior-art primitives before writing any code, produce a one-paragraph assessment of what exists vs what is genuinely missing, and rewrite the scope statement so it composes with existing primitives — ending with a scope smaller than the original brief.

### SC-A-012 — Author CLAUDE.md as the Layer 2 master directive

After drilling this, the practitioner can rewrite a bloated 400-line CLAUDE.md into a sub-200-line master directive that contains zero restated rules, names the repo's actual identity, lists absolute directives, classifies the rule set, and passes the completeness check that an agent reading only the directive can find every subordinate artifact it needs.

### SC-A-013 — Design agent specialisation routing as explicit Layer 1 Intent

After drilling this, the practitioner can write an explicit routing table mapping each of ten request types to exactly one of four specialist agents with no type mapped to two agents, resolve ambiguous cases by either splitting the request or adding a specialist (with justification), and encode the table as an authored artifact (agent description frontmatter or dedicated routing rule file) rather than leaving it emergent.

### SC-A-014 — Decompose milestones with hard-blocking approval gates

After drilling this, the practitioner can decompose a 12-15 requirement feature brief into milestones with an explicit dependency graph, identify the maximum parallelisation subset, write concrete evidence-based pass criteria for each gate ("all four vacancy paths enforce the check" not "tests pass"), and identify the convergence milestone that depends on all others.

### SC-A-015 — Structure the Layer 2 institutional knowledge corpus by artifact type

After drilling this, the practitioner can classify each of eight pieces of new institutional knowledge into rule (always-loaded constraint) / skill (on-demand reference) / agent (persona with own context) / command (workflow phase with gate) based on load semantics, state the load trigger for each classification, and identify any pieces currently misplaced in the practitioner's project.

## Practitioner layer (34 atoms)

### SC-P-001 — Identify adjacent-but-disconnected modules as spec conformance gaps

After drilling this, the practitioner can audit a pair of module directories for adjacent-but-disconnected spec conformance gaps by counting cross-module imports, enumerating every normative 'MUST' in the spec that requires cross-module calls, and producing a grep-verified finding for each missing integration — reporting the exact count of cross-module import lines rather than "a few".

### SC-P-002 — Run empirical model-vs-rule ablation in a clean-room environment

After drilling this, the practitioner can set up a clean-room directory (`mkdir /tmp/ablation-drill && git init`), run `claude -p` in it with zero loaded rules, execute scenario prompts that would trigger a violation for each of five rules, compare the model's response against their prior prediction, and explain each prediction error in one sentence — without resorting to in-session "ignore the rules" ablation.

### SC-P-003 — Write cross-feature interaction tests before individual feature tests

After drilling this, the practitioner can list the shared mutable state between two features in a module, state each feature's safety invariant in one sentence, write a seam test where Feature A's output becomes Feature B's input and the combined safety property must hold, and identify the missing guard when the test fails — without writing the interaction test as an afterthought to per-feature testing.

### SC-P-004 — Label every file with a disposition before starting implementation

After drilling this, the practitioner can walk 10-15 source files before writing implementation code and produce a table with file path / line count / disposition label (SOLID / NEEDS MIGRATION / EXTEND / ENHANCE / REWRITE / IMPLEMENT) / one-sentence rationale grounded in file contents, compare the table against an architecture document to list every discrepancy, and revise the effort estimate based on the table.

### SC-P-005 — Diagnose friction-gradient inversions where the best API has the worst discoverability

After drilling this, the practitioner can map an API module's friction gradient by recording discovery cost and usage quality for each entry point, identify the inversion where the highest-quality API has the worst discoverability, trace the root cause to a specific COC layer failure (Context/Guardrails/Learning), and write a one-paragraph intervention naming the exact COC artifact change required.

### SC-P-006 — Decide whether new functionality is a module inside an existing package or a separate package

After drilling this, the practitioner can apply the three-question test (own release cycle? / distinct consumers? / distinct dependencies?) to each of three proposed features, write a one-paragraph rationale per decision, state which direction an ambiguous case was resolved plus what would flip it, and ground the extraction-cost assessment in specifics (API surface area, internal coupling depth) rather than hand-waving.

### SC-P-007 — Define session completion state before starting, not after convergence is claimed

After drilling this, the practitioner can write a pre-execution completion checklist whose items are individually verifiable ("test_auth_expired passes" not "implement auth"), identify parallel vs sequential dependencies among items, name what is explicitly NOT in scope with a one-sentence exclusion reason, and evaluate a post-session narrative against the checklist to catch silently-dropped deliverables.

### SC-P-008 — Trace deep dependency chains to find where a single broken link breaks the install path

After drilling this, the practitioner can trace a transitive dependency tree of 8-12 packages, identify each fragility point that fails all three questions (single maintainer? / source compilation required? / API broken across pinned range?), propose tightened version bounds with specific rationale per bound, and describe a CI version-matrix test covering minimum and maximum of each tightened range.

### SC-P-009 — Treat architecture documents as testable claims against the actual codebase

After drilling this, the practitioner can read each factual claim in an architecture document, classify each as CONFIRMED / CORRECTED / ABSENT by reading the relevant source files, produce a numbered correction list with specific discrepancies for every CORRECTED item (e.g., "doc says polars 0.39, codebase uses 0.53"), and revise the effort estimate directionally based on the corrections.

### SC-P-010 — Red team CI, auth, and release processes for timing-dependent failures

After drilling this, the practitioner can identify timing assumptions across three categories — CI release race conditions, authentication timing side-channels (`.any()` / `.find()` short-circuit over constant-time comparison), and resource Drop paths lacking ambient runtime — construct a concrete attack scenario for each, propose a fix that eliminates the timing dependency rather than the symptom, and describe how to test that the fix actually resolves the race, side-channel, or runtime gap.

### SC-P-011 — Detect when an audit categorization method is structurally blind to the architecture it audits

After drilling this, the practitioner can, given three audit findings with high orphan/dead-code rates, name the architecture's discovery mechanism (explicit imports / glob-based plugin loading / progressive-disclosure trigger-based SKILL.md), name the audit's detection mechanism, state whether the two are compatible, and propose a matching detection method for each incompatibility rather than deferring to the reported number.

### SC-P-012 — Decide whether to demote or strengthen a rule when ablation shows partial redundancy

After drilling this, the practitioner can, for each of five rules with ablation data, decide demote vs strengthen using failure cost (not compliance percentage) as the criterion, give a one-sentence rationale citing the specific failure consequence, name a concrete monitoring mechanism for each demoted rule, and name a concrete additional enforcement mechanism (e.g., pre-commit hook) for each strengthened rule.

### SC-P-013 — Treat internal estimate inconsistency as a high-confidence signal that at least one estimate is wrong

After drilling this, the practitioner can identify two effort estimates within a single plan document that disagree by more than 2x, trace the divergence to its root cause (which section compressed detail, which section incorporated it), state which estimate they distrust with reasoning about what each did or did not account for, and write a one-sentence gate rejection naming the analytical failure rather than averaging or picking a number.

### SC-P-014 — Distinguish pre-existing failures from pre-existing gaps when applying zero-tolerance Rule 1

After drilling this, the practitioner can classify each of six discovered issues as FAILURE (something supposed to work but broken — fix now) or GAP (something never built — file as separate issue), state the concrete next-action for each classification, and avoid both scope explosion (treating every gap as a failure) and failure normalization (treating every failure as a gap).

### SC-P-015 — End complex prompts with "what would invalidate this?" rather than "is this right?"

After drilling this, the practitioner can, for each of three agent-produced deliverables, write a specific falsification question ("which estimate has the weakest evidence base, and what would change if it were 2x higher?") rather than a confirmation question, run both against the same model, and articulate why the falsification framing produced a different response that surfaces real weaknesses.

### SC-P-016 — Detect overcorrection in both directions as a single contamination failure

After drilling this, the practitioner can, given three agent outputs (rule-loaded / in-session "ignore rules" / clean-room), identify the in-session defiant output as contaminated rather than baseline, explain why conspicuous anti-compliance is evidence of rule awareness not rule absence, and articulate the clean-room methodology that would have caught the contamination — treating over-complying and over-defying outputs as the same failure class.

### SC-P-017 — Detect false-positive learning pipelines that report healthy metrics but produce noise

After drilling this, the practitioner can audit a learning pipeline at all three stages (observation capture, processing, promotion) to identify input-quality noise (e.g., 99% session-start events), processing-quality problems (hardcoded 0.9 confidence, near-duplicate instincts), and promotion-quality gaps (empty evolved/ directory, /codify never reads the outputs), and propose fixes addressing all three stages rather than input alone.

### SC-P-018 — Split a multi-session blocker into fast-unblock and parallel-completion halves

After drilling this, the practitioner can, given a dependency graph with a single blocker item of 4+ sessions fanning out to 5+ downstream items, identify which downstream items depend on which parts of the blocker, propose a split that maximizes items unblocked by the fast half, define independent gates for each half (with the second half NOT depending on the first completing), and redraw the critical path showing the time saved.

### SC-P-019 — Verify session-start continuity reaches the model, not just the terminal

After drilling this, the practitioner can, at every session cold start, test whether session notes reach Claude's context by asking the model to quote them before providing content, diagnose a broken injection path (which hook loads the notes, does it write to stderr or stdout/context, does the path resolution match), and write session notes as imperative instructions-to-next-self rather than past-tense log narrative.

### SC-P-020 — Treat the second occurrence of a drift class as a structural signal, not another incident

After drilling this, the practitioner can, given a sequence of three journal entries reporting dangling references after rename/delete, identify that all three are the same drift class (not separate incidents), explain why the second entry — not the third — is the moment to shift from instance-fix to mechanism-fix, and propose the mechanism-level prevention (post-rename reference sweep, pre-commit dangling-path check, or atomic rename tool).

### SC-P-021 — Grep the entire repo for dangling references immediately after every rename or delete

After drilling this, the practitioner can perform a same-commit reference sweep after every rename or delete, write a grep pattern that catches both exact path matches and partial matches (including YAML frontmatter `references:` lists and markdown link `[text](path)` syntax), update every hit in the same commit as the rename, and refuse to treat the rename as done until the sweep returns zero hits.

### SC-P-022 — Author rules for the weakest model in the deployment envelope, not the strongest

After drilling this, the practitioner can, given a rule set with ablation data for two models, identify rules where the models diverge, classify each as "keep for the weaker model" even if removable for the stronger, propose a variant architecture where the stronger model gets a compressed set and the weaker model gets the full set, and explain why a security-critical rule must be kept at full verbosity for the weaker model regardless of token cost.

### SC-P-023 — Insert a gating workspace at integration seams rather than adding assertions to existing tests

After drilling this, the practitioner can, given four workspaces where three produce components and one builds a client against all three, identify the missing integration seams, propose a dedicated gating workspace (not assertions sprinkled into existing workspaces), define what the gate tests (reference app exercising all seams), and specify that downstream work is structurally blocked until the gate passes.

### SC-P-024 — Intervene when a temporary workaround accumulates its second consumer

After drilling this, the practitioner can, given a timeline where a temporary workaround gains its second consumer, identify Day 2 (not Day 3 or Day 5) as the critical intervention point, explain why the second consumer — not age or backlog status — is the structural signal, and choose between either fast-completing the proper implementation to cap consumers at one or explicitly promoting the workaround to production-grade with tests and documentation.

### SC-P-025 — Run a tier-ranked literature search with source verification

After drilling this, the practitioner can run a systematic literature search across 5-7 topic areas and 3+ databases per area, record for each paper a full citation, VERIFIED/UNVERIFIED status against publisher sources, TIER 1/2/3 ranking relative to the author's argument (not the paper's general importance), what it argues, what it does NOT claim, and its connection to the argument — plus identify at least two contribution-space gaps and one DO NOT CITE paper with a positioning-risk rationale.

### SC-P-026 — Detect misquotation and specification drift between source and citation

After drilling this, the practitioner can, for each of three citations in a research paragraph, read the actual source and classify the attribution as VERIFIED at a specific section/page or NOT FOUND, classify any error as misquotation / fabrication / specification drift, and identify what the source actually says — using the source text directly rather than asking the AI whether a paper supports a claim.

### SC-P-027 — Run hostile reviewer simulation as preemptive defense

After drilling this, the practitioner can specify a target venue and reviewer persona (discipline, seniority, venue-specific biases), run a hostile simulation that identifies at least 1 FATAL, 2 MAJOR findings, and 2 desk-rejection triggers, write for each finding the attack + what a referee report would say + a defense or revision path, and classify each finding as fixable-in-revision vs requires-fundamental-rethink — rejecting polite-hedged review that concludes "the argument is interesting but could be strengthened".

### SC-P-028 — Synthesize multiple hostile critiques into a minimal publishable model

After drilling this, the practitioner can, given three hostile review documents for the same argument, classify each claim as agreed-works / agreed-fails / contested across all reviewers, list convergent strengths and convergent failures, propose a minimal publishable model that preserves all convergent strengths and eliminates all convergent failures, and justify a chosen revision path against that synthesis — without trying to satisfy contradictory single-reviewer concerns.

### SC-P-029 — Run a post-publication gap check for recent literature

After drilling this, the practitioner can, for each of three specific claims in a draft, define 3-4 narrow search categories, search for papers published in the last three years, classify each found paper as MUST CITE / SHOULD CITE / MAY CITE / DO NOT CITE with a positioning-risk rationale for the DO NOT CITE category, and update the tier-ranked literature base with the findings — treating "zero new must-cites" as a valid result rather than citation-padding.

### SC-P-030 — Calibrate academic register with detection-bias awareness

After drilling this, the practitioner can, given a 500-word draft paragraph, identify the target venue's register conventions, find at least three detection-flag patterns (parallel triads, smooth transitions, hedging phrases, uniform sentence length), rewrite the paragraph eliminating those patterns without replacing them with equally-detectable alternatives, and verify the rewrite preserves the argument while matching the venue.

### SC-P-031 — Frame venue strategy as a constraint envelope for argument scope

After drilling this, the practitioner can, for each of three candidate venues, list the venue's constraint envelope (methodology requirements, scope boundaries, audience, format, citation density), identify what the argument must ADD and what it must REMOVE to satisfy each venue, and assess whether the argument survives the reshaping or whether a venue's constraint envelope breaks it — treating the break discovery as a valuable finding rather than a failure.

### SC-P-032 — Document writing choices as margin-note deliberation artifacts

After drilling this, the practitioner can, for a 200-word paragraph, identify three non-obvious writing choices and write a margin note for each that records what was chosen + at least two genuine alternatives + why this choice serves the argument, producing notes that a peer can use to reconstruct the reasoning without requiring the author — distinguishing margin notes (explain why) from summaries (describe what).

### SC-P-033 — Diagnose reflexivity in self-referential research

After drilling this, the practitioner can, given a research paper where the author formalizes their own prior work, identify the reflexive coupling between design choices and model specification, list three alternative model specifications the author did NOT use and predict whether the result survives each, assess whether the paper's reflexivity acknowledgment addresses specific model choices, and propose a sensitivity analysis that converts the reflexivity from vulnerability to strength.

### SC-P-034 — Qualify every superlative — prevent overclaims in research writing

After drilling this, the practitioner can identify every superlative or novelty claim in a research paragraph ("first", "only", "novel", "unique", "no prior work") and rewrite each with three qualifications: a specific falsifiable scope, cited gap-check evidence for the emptiness of that scope, and a temporal qualifier ("as of [date]") — rejecting generic hedges like "to the best of our knowledge" that lack scope or evidence.
