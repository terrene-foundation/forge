---
title: Mastery rubrics per atom
date: 2026-04-10
destination: forge
count: 67
---

# FORGE Mastery Rubrics

Four-level mastery rubric for each of the 67 skill atoms. Every level describes an observable behavior — what the practitioner does, not what they know. Derived from each atom's "Common failure mode" (novice), "When it fires" (developing), "The move" (proficient), and "Related atoms" (advanced) sections.

Use these rubrics when:

- Designing assessment criteria for a downstream course
- Evaluating practitioner growth across multiple atoms
- Calibrating drill scoring thresholds (novice = 0-3/10, developing = 4-6, proficient = 7-8, advanced = 9-10)

## Brokerage layer (18 atoms)

### SC-B-001 — Read-then-merge sync semantics

**Novice**: Accepts the source repo's state as authoritative because it is upstream and runs a bulk overwrite sync, reasoning "the BUILD repo is upstream, so its state wins," erasing target-only content without producing a per-file diff.
**Developing**: Produces a per-file diff when instructed to audit before sync but does not independently recognize that writing into a tree they do not own requires halting at numbering collisions for human classification.
**Proficient**: Independently inventories the target's current state before any cross-repo distribution, computes a per-file diff with source/target/decision columns, and halts the run at every numbering collision or content conflict until the human classifies each as global/variant/skip.
**Advanced**: Orchestrates read-then-merge across a multi-repo chain by composing with SC-B-010 (drift audit) as a pre-sync pass, SC-B-012 (manifest parity) as a pre-flight check, and SC-B-003 (variant classification) at every collision point, producing a single coherent sync plan that no individual atom could produce alone.

### SC-B-002 — Authority-chain escalation when local fix gets reverted

**Novice**: Fixes a bug in a synced artifact where they found it, runs tests, and ships — without tracing the artifact back to its authoring layer, so the fix is silently overwritten by the next sync.
**Developing**: Recognizes when prompted that a fix in a USE repo will be overwritten by sync and traces the artifact back to its authoring layer, but does not independently notice when a previously applied fix has been silently reverted.
**Proficient**: Independently detects that a prior fix is missing after a sync, traces the artifact through the authority chain to the correct authoring layer (USE template, BUILD repo, or upstream methodology source), files the upstream fix as a separate deliverable, and marks the local fix as a time-bounded tourniquet with an explicit re-check date.
**Advanced**: Combines authority-chain escalation with SC-B-003 (variant classification) to determine whether the upstream fix lands as global or variant, and with SC-B-009 (cross-SDK upstream brokerage) when the fix crosses SDK boundaries, producing a multi-repo fix proposal without being prompted.

### SC-B-003 — Variant classification — single-sentence test rule

**Novice**: Classifies artifacts by file location instead of by content — reasoning "it's in kailash-rs/.claude/rules/ so it must be a variant" — which would manufacture unnecessary variants and double the drift surface area for subsequent syncs.
**Developing**: Applies the single-sentence counterfactual test ("Does this rule apply if the user is NOT using Kailash?") when explicitly told to classify an artifact but does not independently invoke the test at authoring time or during Gate 1 sync reviews.
**Proficient**: Independently runs the single-sentence counterfactual test before opening any directory when placing a new CC artifact into the variant overlay system, classifies global/variant-py/variant-rs/skip in under 30 seconds per artifact, and self-corrects when subsidiary tests (file size, sync convenience, author) were applied before the primary test.
**Advanced**: Uses ablation data from SC-P-002 (model-vs-rule ablation) to inform the classification test with empirical evidence about which rules are model-native across all models versus model-specific, and validates classification consistency across both SDKs via SC-B-016 (bidirectional cross-language parity) without being prompted.

### SC-B-004 — Specialist delegation as required first move

**Novice**: Reasons "I have used this API before, so I do not need to re-read it" and writes todos or implementation code that references SDK methods, constructor parameters, or return types that do not exist in the current SDK version, producing runtime failures embedded in the plan.
**Developing**: Routes framework work to a specialist when instructed but does not independently check whether the last time they opened the relevant SDK source was within the current session.
**Proficient**: Independently routes every planned action that touches a Kailash framework through the relevant specialist before writing the todo or implementation, produces a corrections list by reading actual SDK source, and ensures corrections feed the todo as input rather than arriving as red team findings after implementation.
**Advanced**: Composes specialist delegation with SC-B-009 (cross-SDK upstream brokerage) when specialist review reveals a cross-SDK defect, and with SC-A-001 (codify-with-explicit-NOT-codified) to capture the corrections list as institutional knowledge so the next session does not re-derive it.

### SC-B-005 — Spec-to-code conformance audit

**Novice**: Reads the spec, spots three or four obvious gaps, and files issues without systematic coverage — missing the remaining gaps that survive until the next red team or cross-SDK comparison.
**Developing**: Produces a checklist of spec MUST/SHOULD statements when prompted to audit but does not independently check all code paths for each requirement, catching only the obvious enforcement path and missing asymmetric enforcement across alternative paths.
**Proficient**: Independently reads every MUST and SHOULD in the normative spec, greps for each one's implementation across all code paths, produces a numbered gap list where each gap names the spec section, the expected enforcement, and the actual behavior found (or not found), and files each gap as a trackable issue.
**Advanced**: Extends the conformance audit across SDK boundaries by composing with SC-B-016 (bidirectional cross-language parity) to check whether gaps found in one SDK are already addressed in the other, and feeds gap findings into SC-B-009 (cross-SDK upstream brokerage) for coordinated resolution.

### SC-B-006 — Multi-round red team to numeric convergence

**Novice**: Runs one red team round, resolves the findings, and declares "done" without a second round to verify that the resolutions did not introduce new gaps — catching existing issues but missing resolution-introduced issues.
**Developing**: Runs a second red team round when coached to do so but does not independently define a numeric convergence threshold before the first round or journal each round with a finding count.
**Proficient**: Independently defines a convergence threshold (e.g., zero CRITICAL + zero HIGH unresolved) before the first round, journals each round with severity-counted findings, runs successive rounds until the threshold is met, and refuses to declare "converged" or advance to the next phase while any finding above the threshold remains unresolved.
**Advanced**: Composes multi-round convergence with SC-B-008 (convergence-as-validation) by running independent red team agents from different specializations within each round, and feeds decision reversals surfaced by red team rounds into SC-B-014 (explicit supersession journaling) without being prompted.

### SC-B-007 — Worktree-per-agent for safe parallel execution

**Novice**: Launches parallel agents in the same working directory, reasoning "the files they touch don't overlap," which breaks when two agents both modify a shared file like `__init__.py` or `conftest.py`, producing partial writes and unrelated test failures.
**Developing**: Creates git worktrees when instructed before launching parallel agents but does not independently set up per-worktree environments (PYTHONPATH, virtualenv) or handle merge conflicts as a separate human-gated step after all agents complete.
**Proficient**: Independently creates one git worktree per agent before any parallel execution, configures each worktree with its own branch, test environment, and PYTHONPATH overrides, runs tests independently in each worktree, and handles merge as a separate conflict-resolution step after all agents complete.
**Advanced**: Composes worktree isolation with SC-B-013 (cross-workspace dependency mapping) to determine which worktrees can run concurrently, and with SC-B-017 (verification-gradient zone assignment) to set governance-level trust for each parallel agent's output before launching.

### SC-B-008 — Convergence-as-validation across independent reds

**Novice**: Runs two red team agents but briefs them with the same prompt and same attack angle, producing cosmetic independence (different sessions) with substantive redundancy (same perspective) that looks like validation but is actually duplication.
**Developing**: Assigns different specializations to red team agents when prompted but does not independently compare finding sets to measure convergence rate or assess audit completeness from the comparison.
**Proficient**: Independently runs two or more red team agents with genuinely different specializations (e.g., security reviewer vs. deep analyst vs. feasibility assessor), withholds findings between agents until both complete, then compares finding sets to measure convergence rate and uses the comparison to assess audit thoroughness — with convergent findings as high-confidence issues and non-overlapping findings as justification for the multi-perspective cost.
**Advanced**: Uses convergence rates to calibrate the audit method itself, composing with SC-B-005 (spec-to-code conformance) as one specialization angle and SC-P-003 (cross-feature interaction red team) as another, and adjusts the number of perspectives based on prior convergence data.

### SC-B-009 — Cross-SDK upstream brokerage

**Novice**: Discovers a bug that traces to an SDK component, applies a local workaround that fixes their immediate problem, and never files the upstream issue because "someone else will notice it" — leaving every other consumer to independently discover and work around the same bug.
**Developing**: Files an upstream issue against the SDK when coached but does not independently check whether the same defect exists in the other SDK or tag the local mitigation as temporary with a link to the upstream issue number.
**Proficient**: Independently files issues against all affected SDK repositories when a discovered bug crosses SDK boundaries, applies a temporary local mitigation tagged with the upstream issue number and a time-bounded re-check date, and notes the filing in the session journal.
**Advanced**: Combines upstream brokerage with SC-B-004 (specialist delegation) to route the cross-SDK fix through the appropriate specialist, and with SC-B-016 (bidirectional parity) to determine which SDK's implementation should be the upstream reference for the shared abstraction.

### SC-B-010 — Cross-repo divergence audit with numbered drift-gap list

**Novice**: Runs the sync without auditing first, relying on the sync command's own conflict resolution to surface problems — which misses orphans entirely (invisible to sync because they have no manifest entry) and misses stale entries (manifest points to nothing, so sync silently skips them).
**Developing**: Produces a numbered drift-gap list when instructed to audit before sync but does not independently classify each divergence as intentional-variant, unintentional-drift, or orphan, or halt the distribution when unintentional drift exceeds a threshold.
**Proficient**: Independently audits every downstream repo against upstream before any distribution pass, produces a numbered drift-gap list with classification (orphan/stale/variant/drift/aligned) and one-line description for each file, and halts the distribution if unintentional drift exceeds the declared threshold.
**Advanced**: Feeds drift audit findings into SC-B-012 (manifest parity automation) for institutionalized prevention, and uses SC-P-020 (second-occurrence structural signal) to escalate from instance-level repair to mechanism-level prevention when the same drift class recurs.

### SC-B-011 — Version tracking cascade across the repo chain

**Novice**: Maintains the VERSION file but forgets to link it to the SDK version in pyproject.toml, creating two independent version tracks that drift silently — so artifacts reference API features that do not exist in the SDK version the downstream project actually uses.
**Developing**: Checks the VERSION file at session start when prompted but does not independently verify that the COC artifact version, SDK release version, and dependency pins are all consistent across the three trigger points (session start, /codify, /sync gates).
**Proficient**: Independently wires version awareness across all three tracks (COC artifact version in .claude/VERSION, SDK release version in pyproject.toml, and dependency pins in USE templates), surfaces mismatches at session start and at sync gates, and stamps the SDK version into /codify proposals so that stale artifacts are never silently applied to a newer SDK.
**Advanced**: Extends the version cascade to include SC-B-012 (manifest parity) by verifying that version pins in distributed files match the cascade's expected values, and connects version mismatches to SC-B-003 (variant classification) when a rule authored against a newer SDK needs different classification for projects on older SDKs.

### SC-B-012 — Automate manifest-to-filesystem parity check and halt on staleness

**Novice**: Writes the parity check but makes it advisory-only (prints warnings, exits 0), so the sync runs anyway from a stale manifest that may contain orphan files invisible to distribution or stale entries pointing to nothing.
**Developing**: Runs a manifest parity check when instructed before sync but does not independently wire the check into the sync command's pre-flight as a hard gate that exits non-zero on orphans or stale entries.
**Proficient**: Independently writes and maintains an automated parity check that compares the distribution manifest against the actual filesystem, detects orphan files, stale entries, and parity gaps, exits non-zero on orphans or stale entries (halting sync), and reports parity gaps for triage without halting.
**Advanced**: Composes the parity check with SC-B-011 (version tracking cascade) to include version-pin consistency as one of the parity dimensions, and with SC-B-003 (variant classification) to catch orphan variants that the classification test would have caught at authoring time.

### SC-B-013 — Map cross-workspace dependency surface before parallelizing

**Novice**: Identifies producer-consumer dependencies between workspaces (W1 before W2) but misses shared-file bottlenecks (files B, C, E modified by multiple workspaces), so workspaces launch in parallel, produce clean results individually, and then produce substantial merge conflicts when integrated.
**Developing**: Maps shared-file dependencies when coached but does not independently produce a full dependency graph covering both abstraction-level dependencies (one workspace produces what another consumes) and file-level bottlenecks (shared files both modify).
**Proficient**: Independently maps the shared-file and shared-abstraction surface before launching parallel workspaces, produces a dependency graph with bottleneck files marked, declares which workspaces can run in parallel and which must be sequenced with justification, and identifies files requiring module extraction before parallel work is safe.
**Advanced**: Composes dependency mapping with SC-B-007 (worktree-per-agent) for safe parallel execution and SC-P-018 (split the long pole) to restructure the dependency graph when a single workspace blocks multiple others.

### SC-B-014 — Journal decision reversals with explicit supersession references

**Novice**: Creates a new decision entry that reverses a prior decision but omits the supersession reference, treating the reversal as a fresh decision — so six months later a different agent finds two contradictory entries with no link between them and cannot tell which is current.
**Developing**: Includes a supersession reference in the new entry when reminded to do so but does not independently check whether the new decision contradicts any existing journal entry before committing it.
**Proficient**: Independently creates a new journal entry whenever a decision reversal occurs that names the prior entry by number and title, states "this supersedes entry NNNN" explicitly, explains why the prior decision was wrong or incomplete, and records the new decision as a standalone entry readable without the prior one.
**Advanced**: Applies supersession discipline not only to journal entries but also to rule-text amendments (composing with SC-B-015) and to drift audit corrections (composing with SC-B-010), maintaining a navigable supersession chain across artifact types without being prompted.

### SC-B-015 — Reinterpret MUST rules from "human performs" to "human approves"

**Novice**: Reinterprets the rule verbally during the session but does not amend the rule text, so the next session reads the original text, encounters the same bottleneck, and either re-derives the reinterpretation or follows the literal text and recreates the bottleneck.
**Developing**: Amends the rule text when instructed after a reinterpretation but does not independently distinguish rules where the human's value is in approval (reinterpretable) from rules where the human's value is in the execution itself (not reinterpretable, e.g., aesthetic decisions, stakeholder empathy).
**Proficient**: Independently identifies when a MUST rule's literal text puts the human in-the-loop as a throughput bottleneck rather than on-the-loop as an approver, reinterprets the rule to "human approves" after verifying that agent proposals are trustworthy, and amends the rule text in the same session so the reinterpretation is durable and visible to future sessions.
**Advanced**: Composes rule reinterpretation with SC-B-017 (verification-gradient zone assignment) to set the appropriate trust zone for the agents now performing the task, and with SC-B-014 (explicit supersession journaling) to journal the rule amendment as a decision reversal.

### SC-B-016 — Audit cross-language parity bidirectionally

**Novice**: Audits only in the assumed direction (what does SDK-B need from SDK-A?) and produces a gap list that omits abstractions where SDK-A is behind SDK-B, leading to wasted work implementing features SDK-B already has while leaving SDK-A's gaps unaddressed.
**Developing**: Checks whether the assumed "follower" SDK exceeds the "leader" when prompted but does not independently record parity direction per-abstraction rather than per-SDK.
**Proficient**: Independently audits cross-language parity bidirectionally for each abstraction, determines which implementation is more complete, type-safe, or featureful, records per-abstraction parity direction (not per-SDK), and adjusts effort estimates based on the actual gap rather than the assumed direction.
**Advanced**: Feeds bidirectional parity findings into SC-B-009 (cross-SDK upstream brokerage) to decide which SDK's implementation becomes the upstream reference for shared abstractions, and into SC-B-014 (explicit supersession journaling) to supersede prior assumptions about parity direction.

### SC-B-017 — Assign verification-gradient zone explicitly before delegating

**Novice**: Delegates to multiple agents without any zone differentiation — all agents operate at the same implicit trust level — so when one agent's task escalates in scope, the escalation is invisible because no zone boundary exists to trigger re-evaluation.
**Developing**: Assigns a verification-gradient zone when prompted before delegation but defaults all agents to the same zone rather than differentiating based on task risk and reversibility.
**Proficient**: Independently names which of the four verification-gradient zones (auto-approved, flagged, held, blocked) each agent operates in before delegation, states the zone in the delegation brief, differentiates zones based on task risk and reversibility, and re-evaluates the zone assignment when task scope changes mid-session.
**Advanced**: Composes zone assignment with SC-B-004 (specialist delegation) to calibrate zones based on specialist expertise, and with SC-B-018 (PDP/PEP separation) to verify that the enforcement point matches the assigned zone's strictness level for each delegated agent.

### SC-B-018 — Separate the policy decision point from the policy enforcement point

**Novice**: Separates the PDP and PEP at the primary call site but leaves a secondary code path (admin endpoint, batch job, migration script) that calls the governed operation directly — bypassing the PEP entirely and making the PDP's verdict irrelevant for that path.
**Developing**: Refactors a merged authorize-and-execute function into separate PDP and PEP modules when instructed but does not independently audit all call sites of the governed operation to verify that every path passes through a PEP.
**Proficient**: Independently identifies when a function both decides and enforces policy in the same body, refactors into a PDP that returns a verdict data structure (not a raised exception) and a PEP that is the only call site for the governed action, verifies that no secondary code path bypasses the PEP, and writes tests for the PDP that verify blocked verdicts without triggering enforcement side effects.
**Advanced**: Composes PDP/PEP separation with SC-B-017 (verification-gradient zone assignment) to ensure zone assignments map to PEP strictness levels, and with SC-A-006 (negative tests for deny-by-default) to verify the deny path of every PEP with inputs the enforcement has never seen.

## Artifact layer (15 atoms)

### SC-A-001 — Codify-with-explicit-NOT-codified

**Novice**: Records only the wins in the codify entry — produces a "What was codified" section and nothing else — so the next session re-examines the same exclusion candidates from scratch because nobody recorded that they were previously rejected and why.
**Developing**: Includes a NOT-codified section when prompted but uses aesthetic reasons ("out of scope," "low priority") instead of one of the four structural reasons (code-is-authority, established-science, one-off-bug, already-captured-elsewhere).
**Proficient**: Independently produces both a "What Was Codified" and a mandatory "What Was NOT Codified (and why)" section in every codify entry, classifies each excluded item using one of the four reasons with a specific citation, and re-examines any exclusion that does not fit the four reasons.
**Advanced**: Composes the NOT-codified practice with SC-A-003 (defense-in-depth codification) to ensure codified items land in multiple enforcement surfaces, and feeds the NOT-codified section into SC-P-017 (false-positive learning detection) to surface gaps like "/codify never reads from the learning system."

### SC-A-002 — Frontend mock data detection

**Novice**: Adds frontend mock data patterns to an advisory rule file and stops there, leaving detection probabilistic — the model reads the rule most of the time and ignores it sometimes — so mock data ships when the rule is not loaded or the context window overflows.
**Developing**: Extends the detection to the hook layer when instructed but does not independently recognize that detection in only one layer (rule OR hook) is the failure mode, and that defense in depth (rule + hook + command-step) is required.
**Proficient**: Independently extends no-stubs detection to catch JS/TS frontend equivalents (MOCK*\*, FAKE*_, DUMMY\__, SAMPLE\__ constants; generate_() and mock\*() functions; Math.random() for display values) at the hook layer as a BLOCK (not warn), with regex specificity that avoids false positives on legitimate utilities like generateUUID().
**Advanced**: Composes detection with SC-A-003 (defense-in-depth codification) to land the fix across rule + hook + command-step simultaneously, and with SC-A-006 (negative tests for deny-by-default) to verify the deny path with real mock-data examples, producing a detection stack that no single-layer failure can bypass.

### SC-A-003 — Defense-in-depth codification across multiple artifact locations

**Novice**: Codifies a new rule into the artifact type they are most familiar with (usually a rule file) and stops, so when a session does not trigger that rule (wrong file scope, context window overflow, or advisory-not-deterministic), the constraint vanishes.
**Developing**: Lands a rule in two artifact types when prompted but does not independently count the distinct enforcement surfaces or identify the failure mode if any one is missing.
**Proficient**: Independently lands critical rules in at least four distinct artifact locations (rule file for constraint text, hook for deterministic blocking, specialist agent for contextual guidance, skill for teaching material), explains what each artifact type contributes that the others cannot, and identifies the specific failure mode for each missing landing.
**Advanced**: Extends the defense-in-depth stack by composing with SC-A-004 (Layer 3 hook security audit) to verify that each enforcement surface is itself secure, and with SC-A-006 (negative tests for deny-by-default) to test the deny path of each surface independently.

### SC-A-004 — Layer 3 hook security audit

**Novice**: Treats hooks as infrastructure rather than application code, so hooks that were written once and "work" are never revisited by red team passes — leaving the highest-privilege code in the system with the least security scrutiny.
**Developing**: Audits hooks for command injection when specifically asked but does not independently check other vulnerability classes (path traversal, denial of service, unbounded reads) or trace every external input to its consumption point.
**Proficient**: Independently audits every Layer 3 hook for the same vulnerability classes those hooks are designed to prevent — tracing each external input (files, environment variables, git state) to its consumption point, classifying each as safe or unsafe with a one-sentence rationale, and correctly dismissing false positives where external inputs are not dangerous.
**Advanced**: Composes hook auditing with SC-A-007 (zero-config security audit) to catch hooks invoked during zero-config startup that inherit permissive defaults, and with SC-P-010 (timing race condition red team) to probe for timing side-channels in variable-time hook validation.

### SC-A-005 — Encapsulation as security boundary in signed structs

**Novice**: Defaults all fields to `pub` for convenience during initial implementation, intending to tighten visibility later — which never arrives because the struct works, callers read fields they need, and the security boundary remains invisible until a red team or attacker discovers unrestricted mutation.
**Developing**: Makes security-sensitive fields private when prompted but does not independently design an approved-mutation path (constructor or builder) or write tests that demonstrate both the bypass (if public) and the fix (if private).
**Proficient**: Independently defaults every field on signed or governance-chain structs to private with read-only accessors, provides a single approved-mutation constructor path, and writes both a bypass test (clone, flip revocation, confirm chain accepts) and a fix test (confirm chain rejects the modified record).
**Advanced**: Composes encapsulation with SC-A-003 (defense-in-depth codification) to ensure signature verification and chain validation are independent enforcement surfaces alongside struct visibility, and with SC-P-010 (timing race condition red team) to check for timing side-channels in signature verification.

### SC-A-006 — Negative tests for deny-by-default

**Novice**: Writes positive tests for all mapped routes and known roles, sees 100% pass, and declares enforcement complete — never exercising the deny path with an unmapped route or unrecognized role, so a `deny_by_default` flag that executes identical code in both branches goes undetected.
**Developing**: Writes a negative test for the deny path when prompted but does not independently write the test FIRST (before reading the implementation) or assert both the HTTP status and the presence of a denial log entry.
**Proficient**: Independently writes the negative test first for every enforcement point with deny-by-default semantics — confirming that unmapped routes, unrecognized roles, and unvalidated inputs are blocked — asserts both the denial response and the audit log entry, and runs the test before considering the enforcement complete.
**Advanced**: Extends negative testing to cover timing-aware bypass paths by composing with SC-P-010 (timing race condition red team), and verifies that every enforcement surface in a SC-A-003 (defense-in-depth) stack independently blocks the denied case.

### SC-A-007 — Zero-config security audit

**Novice**: Audits the explicit configuration surface (flags, env vars, config files) but treats zero-config convenience paths as "the simple case" excluded from the threat model — when they are actually the highest-risk case because they are the path most developers will take first and many will never leave.
**Developing**: Enumerates network-accessible resources created by a zero-config `start()` call when instructed but does not independently classify each as read-only vs. read-write and localhost-only vs. network-reachable, or identify which security parameters were defaulted to permissive values.
**Proficient**: Independently audits every zero-config convenience path by enumerating what is exposed and to whom, classifying each resource by access type and reachability, identifying permissive defaults on security-sensitive parameters, and proposing a fix that makes the default deny-by-default without breaking the one-line usability promise.
**Advanced**: Composes zero-config auditing with SC-A-006 (negative tests for deny-by-default) to verify the deny posture holds across all convenience entry points, and with SC-A-004 (Layer 3 hook security audit) to catch hooks invoked during zero-config startup that inherit the permissive defaults.

### SC-A-008 — Rule classification — critical vs advisory

**Novice**: Classifies rules by importance rather than by failure consequence — labeling a code-style rule as critical (consuming hook complexity) and a no-secrets rule as advisory (leaving it probabilistic) — which is exactly backwards because style drift is correctable in review while secret leakage is irreversible.
**Developing**: Classifies rules by failure consequence when prompted but does not independently use ablation data from SC-P-002 to distinguish model-native rules (advisory candidates) from load-bearing rules (critical candidates).
**Proficient**: Independently classifies every rule as critical (deterministic hook enforcement, irreversible failure mode) or advisory (probabilistic instruction-following, tolerable deviation), using ablation data to ground the classification and naming the enforcement mechanism for each critical rule.
**Advanced**: Extends classification across multiple models by composing with SC-P-022 (cross-model gap reading) to recognize that a rule advisory for Claude may be critical for MiniMax, and feeds classification results into SC-A-009 (progressive disclosure architecture) to determine which level of the 4-level hierarchy each rule occupies.

### SC-A-009 — Progressive disclosure architecture for CC artifacts

**Novice**: Flattens the hierarchy by putting everything at the index level "so it is always available," driven by anxiety about the model not finding a rule when it matters — producing the 48K always-loaded token overhead that degrades conversation quality.
**Developing**: Adds `paths:` frontmatter to scope domain-specific rules when instructed but does not independently classify each artifact into the 4-level hierarchy (master directive, index, topic, deep reference) or extract oversized agents into agent-stub-plus-skill pairs.
**Proficient**: Independently structures CC artifacts in a 4-level hierarchy matching CO's progressive disclosure principle, classifies every artifact into exactly one level, scopes topic-level rules with `paths:` frontmatter, extracts oversized agents into stub-plus-skill pairs, and measures the before/after always-loaded token count to verify measurable reduction without losing institutional knowledge.
**Advanced**: Composes progressive disclosure with SC-A-008 (rule classification) to move critical rules to hooks (outside the hierarchy entirely) and with SC-P-002 (ablation) to produce the KEEP/COMPRESS/REMOVE/DEMOTE data that drives the hierarchy placement, achieving a token-optimized artifact set that still enforces all critical constraints.

### SC-A-010 — Token budget — baked-in vs external boundary decision

**Novice**: Either bakes everything into a monolithic compiled prompt that cannot be updated without recompilation (violating CO §13's living-documents requirement) or refuses to bake anything and accepts the 36K token/turn cost as the price of transparency — treating the boundary as a philosophical stance rather than a quantitative decision.
**Developing**: Evaluates individual rules for baking when prompted using one axis (e.g., project-variance) but does not independently apply all three axes (project-variance, cache-position, editability-need) to each rule individually.
**Proficient**: Independently evaluates the baked-vs-external boundary for each rule on three axes — does it change between projects (external), is it in the cacheable prefix or dynamic suffix (baked gets caching), and does it need to be user-visible and editable (external) — producing a classified rule set with per-category token estimates and a measurable per-session cost reduction.
**Advanced**: Extends the boundary decision across model envelopes by composing with SC-P-022 (cross-model gap reading) where weaker models need full rule sets baked while stronger models ship compressed, and with SC-A-009 (progressive disclosure) to ensure the external side of the boundary is properly hierarchized.

### SC-A-011 — Framework-first check before building new functionality

**Novice**: Skips the search for existing primitives because they assume they know the codebase or because the existing primitive has a different name than the concept in the brief, and writes a new implementation that duplicates what already exists — as in the MCP case where six implementations existed because each was built without searching for prior art.
**Developing**: Searches for existing primitives when instructed to check the framework but does not independently record the search results or write a revised scope statement that composes with existing primitives instead of rebuilding.
**Proficient**: Independently greps the framework for existing primitives before writing any new code, produces a one-paragraph assessment of what exists vs. what is genuinely missing, writes a revised scope statement that composes with existing primitives, and when multiple competing implementations are found, makes the deliverable consolidation into one canonical primitive rather than another variant.
**Advanced**: Composes framework-first checking with SC-P-006 (package boundary decision) to determine whether genuinely new functionality should be a module or separate package, and with SC-B-004 (specialist delegation) to route the framework search through the specialist most likely to know whether the primitive already exists.

### SC-A-012 — Author CLAUDE.md as the Layer 2 master directive

**Novice**: Treats CLAUDE.md as a documentation file for human readers — with prose paragraphs, copy-pasted rule summaries "for convenience," and no index of subordinate artifacts — producing a directive that is both too long (restated rules waste tokens) and too incomplete (missing artifact pointers mean the agent never discovers skills and commands).
**Developing**: Removes restated rules from CLAUDE.md when prompted but does not independently audit whether the directive correctly names the repo's identity, lists absolute directives, classifies rules, and indexes all subordinate artifacts.
**Proficient**: Independently authors CLAUDE.md as a machine-readable master directive under 200 lines that names the repo identity, lists absolute directives, classifies rules as CO-general vs COC-specific, indexes all subordinate artifacts by purpose without restating their content, and earns every line against the always-in-context token budget.
**Advanced**: Composes master directive authoring with SC-A-009 (progressive disclosure) to ensure CLAUDE.md is level 1 of the 4-level hierarchy pointing to the correct subordinate levels, and with SC-P-019 (cold-start continuity) to include workspace-state references that enable session continuity.

### SC-A-013 — Design agent specialisation routing as explicit Layer 1 Intent

**Novice**: Writes detailed agent descriptions but no routing rules, assuming the orchestrator will infer routing from descriptions — which works for obvious cases but fails for ambiguous cases ("fix a failing test" — code task or test task?), producing non-deterministic specialist selection across runs.
**Developing**: Writes a routing table when prompted but does not independently ensure every request type is mapped to exactly one agent or propose specialist additions for request types that do not cleanly map.
**Proficient**: Independently authors the routing decision as an explicit artifact (agent description frontmatter or dedicated routing rule file) before any agent executes, maps every request type to exactly one specialist, identifies request types that do not cleanly map and proposes either splitting the request or adding a specialist, and treats an unmapped request as evidence of a missing specialist rather than a cue for the orchestrator to guess.
**Advanced**: Composes routing with SC-B-017 (verification-gradient zone assignment) to assign trust levels to each specialist's output, and with SC-A-012 (CLAUDE.md as master directive) to ensure the master directive indexes agents and their specialties as a navigable routing map.

### SC-A-014 — Decompose milestones with hard-blocking approval gates

**Novice**: Lists milestones in a flat sequence (M1 then M2 then M3) without dependency analysis, forcing serial execution of work that could run in parallel, and defines gates without evidence requirements ("M1 complete" instead of specific acceptance criteria).
**Developing**: Identifies milestone dependencies when prompted and adds acceptance criteria but does not independently identify which milestones can run in parallel or identify the convergence milestone that depends on all others.
**Proficient**: Independently decomposes multi-session features into milestones where every milestone names a single approval gate with concrete evidence requirements, maps the dependency graph with explicit parallelization opportunities, identifies the convergence milestone, and prevents phase-skipping by making each gate hard-blocking on the next milestone's start.
**Advanced**: Composes milestone decomposition with SC-B-013 (cross-workspace dependency mapping) when milestones span workspaces, and with SC-P-018 (split the long pole) to restructure milestones when a single one blocks all downstream work.

### SC-A-015 — Structure the Layer 2 institutional knowledge corpus by artifact type

**Novice**: Dumps everything into rules because rules are always loaded and therefore "safest" — the knowledge is always present — increasing the always-in-context token budget with every new piece of knowledge, diluting signal with noise and degrading conversation quality.
**Developing**: Classifies institutional knowledge by artifact type (rule, skill, agent, command) when prompted but bases the classification on topic grouping rather than load semantics (always-loaded vs. on-demand vs. delegation-scoped vs. phase-gated).
**Proficient**: Independently places each piece of institutional knowledge into the correct artifact type based on load semantics — rules for always-loaded behavioral constraints, skills for on-demand domain knowledge, agents for specialist personas, commands for workflow phases with explicit gates — and verifies that the classification matches how CC's discovery mechanism actually loads each type.
**Advanced**: Composes artifact-type structuring with SC-A-009 (progressive disclosure architecture) for vertical hierarchy within each type, and with SC-A-008 (rule classification) to sub-classify within the rule type as critical (hook-enforced) vs. advisory (loaded in context).

## Practitioner layer (34 atoms)

### SC-P-001 — Identify adjacent-but-disconnected modules as spec conformance gaps

**Novice**: Sees that both modules exist and are individually well-tested (one had 1,139 tests), concludes they are "integrated" because they share a package namespace, and misses that zero cross-module imports exist despite the spec requiring integration at every governance action.
**Developing**: Counts cross-module import lines when prompted to check integration but does not independently enumerate every normative "MUST" in the spec that requires cross-module calls, or produce grep results for each.
**Proficient**: Independently checks whether structurally adjacent modules actually call each other by counting cross-module imports, listing every spec "MUST" that requires cross-boundary calls, producing grep results for each, and writing a finding for each missing integration that names the spec section, expected call site, and consequence of the gap.
**Advanced**: Composes adjacent-module detection with SC-B-005 (spec-to-code conformance audit) for systematic cross-module coverage, and with SC-P-003 (cross-feature interaction red team) to test the intra-module version of the same pattern — features that should constrain each other but do not.

### SC-P-002 — Run empirical model-vs-rule ablation in a clean-room environment

**Novice**: Runs the ablation inside the current rule-loaded session by telling agents to "ignore the rules," producing contaminated results where agents overcorrect in both directions — the zero-tolerance test agent says "file a ticket and move on" as contaminated defiance, not actual baseline behavior.
**Developing**: Sets up the clean-room directory when instructed but does not independently decide to run ablation before classifying rules as model-native or load-bearing.
**Proficient**: Independently initiates a clean-room ablation in `/tmp/` with zero loaded context before any rule-classification decision, records prediction vs actual for each tested rule, and writes error explanations for mispredictions.
**Advanced**: Extends the ablation methodology across multiple models (Claude, MiniMax, etc.) to produce a model-specific variant architecture, and feeds ablation findings into SC-A-008 (rule classification) and SC-P-012 (demotion judgment) without being prompted.

### SC-P-003 — Write cross-feature interaction tests before individual feature tests

**Novice**: Writes thorough tests for Feature A (20 tests, all pass) and Feature B (15 tests, all pass) and reports "all green" — but each feature's test suite uses its own fixtures with no crossover, so a scenario where Feature A's output enters Feature B's logic (e.g., a vacant role approving a bridge) exists in neither fixture set.
**Developing**: Writes a cross-feature interaction test when prompted but does not independently identify shared mutable state between features or write the interaction test BEFORE the individual feature tests.
**Proficient**: Independently identifies shared mutable state between two governance features, states each feature's safety invariant in one sentence, and writes the cross-feature interaction test before writing individual feature tests — constructing a scenario where one feature's output becomes the other's input and verifying the combined safety property at the seam.
**Advanced**: Composes cross-feature testing with SC-B-008 (convergence-as-validation) by probing different feature boundaries from independent red team perspectives, and with SC-P-010 (timing race condition red team) to test timing-dependent interactions between individually correct subsystems.

### SC-P-004 — Label every file with a disposition before starting implementation

**Novice**: Reads the architecture document or brief, trusts its description of the codebase, and starts implementing — discovering mid-implementation that files labeled "needs rewrite" are actually solid, or files labeled "complete" have critical version mismatches, forcing context switches and reverts.
**Developing**: Assigns dispositions to source files when instructed but derives labels from the architecture document rather than from reading the actual source files, missing discrepancies between the document and the codebase.
**Proficient**: Independently walks every source file in the target module before writing any implementation code, labels each with a disposition (SOLID/EXTEND/ENHANCE/REWRITE/IMPLEMENT/NEEDS MIGRATION) with a one-sentence rationale based on file content, produces a comparison table against the architecture document listing every discrepancy, and revises the effort estimate based on the table.
**Advanced**: Composes disposition labeling with SC-P-009 (architecture doc as contract) to systematically verify architecture-document claims, and with SC-P-021 (reference sweep after rename) to proactively plan reference sweeps for files labeled for rename or deletion.

### SC-P-005 — Diagnose friction-gradient inversions

**Novice**: Identifies the broken API and files a bug against the engine code but does not address the friction gradient — the COC still routes users to the formerly-broken API while omitting the working alternative, so the inversion persists in discovery cost even after the quality gap is closed.
**Developing**: Maps the friction gradient when prompted (discovery cost vs. usage quality per API entry point) but traces the root cause only to "bad docs" without specifying which COC artifact (CLAUDE.md, skill file, rule, hook) should change and how.
**Proficient**: Independently maps the friction gradient for each API entry point measuring discovery cost and usage quality, identifies inversions where the easiest-to-discover API has the lowest quality, and traces the root cause through COC layers (Layer 2 Context, Layer 3 Guardrails, Layer 5 Learning) to produce a specific COC artifact change that corrects the gradient.
**Advanced**: Composes friction-gradient diagnosis with SC-A-009 (progressive disclosure architecture) to restructure the disclosure hierarchy so users encounter the best API first, and with SC-A-001 (codify-with-explicit-NOT-codified) to surface "we did NOT update the COC to deprecate the broken API" as an explicit gap.

### SC-P-006 — Decide whether new functionality is a module or a separate package

**Novice**: Defaults to "make everything a separate package" because it feels cleaner, without accounting for the overhead of new CI pipeline, PyPI publisher, version tracking, CLAUDE.md listing, and sync target — or conversely, crams everything into one package without checking whether the new functionality has its own release cycle, consumers, and dependencies.
**Developing**: Applies the three-question test (release cycle, distinct consumers, dependency footprint) when coached but does not independently assess extraction cost for ambiguous cases or identify what would flip the decision.
**Proficient**: Independently applies the three-question test before writing the first line of code — (1) own release cycle? (2) distinct consumers? (3) unique heavyweight dependencies? — writes a one-paragraph rationale for each decision, and for ambiguous cases states the chosen direction plus what would flip it and whether extraction cost is low or high.
**Advanced**: Composes the package boundary decision with SC-A-011 (framework-first check) to confirm the functionality is genuinely new before the boundary question arises, and with SC-P-008 (dependency chain fragility) to evaluate which dependency chains a wrong boundary decision would expose to consumers.

### SC-P-007 — Define session completion state before starting

**Novice**: Starts implementing immediately, makes good progress, and declares the session "done" based on the feeling that the major work is complete — silently dropping 2-3 items (a test never written, a doc never updated, an edge case acknowledged in analysis but never implemented) because no pre-defined checklist exists to catch the gaps.
**Developing**: Writes a completion checklist when prompted but uses vague items ("tests pass," "implementation done") rather than specific verifiable criteria ("test_auth_expired passes," "auth middleware returns 401 for expired tokens").
**Proficient**: Independently writes a concrete completion-state checklist before the first implementation action — with verifiable items (named tests, specific functions, explicit documentation updates), parallel vs. sequential identification, and explicit NOT-in-scope exclusions with reasons — and evaluates completion claims against the checklist rather than narrative summaries.
**Advanced**: Composes completion-state definition with SC-A-014 (milestone decomposition) for multi-session programs, and with SC-P-019 (cold-start continuity) to ensure session notes carry actionable completion-state information to the next session.

### SC-P-008 — Trace deep dependency chains for fragility

**Novice**: Writes `>=X` for every dependency because "we want the latest" and moves on — so the range admits versions where the required API does not exist, or transitive dependencies require source compilation on specific platforms, or the dependency is maintained by a single person with irregular releases.
**Developing**: Tightens version pins for the primary dependency when prompted but does not independently trace the transitive chain 2+ levels deep or apply the three fragility questions (maintenance bus factor, binary wheel coverage, API stability across pinned range) to each link.
**Proficient**: Independently traces the full transitive dependency chain of every heavyweight dependency, answers three fragility questions at each link, pins fragile links tightly with rationale, adds CI version-matrix tests covering the minimum and maximum of each pinned range, and documents the fragility in the package's dependency notes.
**Advanced**: Composes dependency chain analysis with SC-B-011 (version tracking cascade) to ensure cascade-level version tracking includes dependency pin consistency, and with SC-P-006 (package boundary decision) to evaluate which dependency chains a boundary decision would expose to consumers.

### SC-P-009 — Treat architecture documents as testable claims

**Novice**: Treats the architecture document as authoritative and proceeds directly to implementation — discovering mid-session that dependency versions in the code do not match the document, or features described as missing already exist, forcing pauses, re-investigations, and re-plans.
**Developing**: Verifies individual claims when prompted but does not independently read every factual claim in the document, classify each as CONFIRMED/CORRECTED/ABSENT, or produce a numbered correction list.
**Proficient**: Independently reads every factual claim in an architecture document or issue tracker, verifies each against the actual codebase by reading relevant source files, classifies each as CONFIRMED (code matches), CORRECTED (code contradicts, with specific discrepancy), or ABSENT (code does not address), produces a numbered correction list, and revises the effort estimate based on the corrections.
**Advanced**: Composes architecture-as-contract with SC-P-004 (per-file disposition labeling) for file-level ground truth, and with SC-P-013 (estimate self-contradiction) to catch internally inconsistent claims within the architecture document itself.

### SC-P-010 — Red team for timing-dependent failures

**Novice**: Reviews code for logical correctness and concludes it is correct — because it IS logically correct — but misses the timing dimension entirely: the `.any()` call returns the right boolean, the Drop calls the right rollback, the tag push creates the right tags, but none behave correctly under concurrent or interrupted conditions.
**Developing**: Identifies one category of timing failure (e.g., CI race conditions) when prompted but does not independently probe all three categories (CI/release race conditions, authentication timing side-channels, transaction/resource lifecycle drops) or construct concrete scenarios where the timing assumption fails.
**Proficient**: Independently probes for timing-dependent failures across all three categories during red team, constructs concrete scenarios where timing assumptions fail for each (e.g., "attacker measures response time for key at index 0 vs index 5"), proposes fixes that eliminate the timing dependency rather than the symptom, and describes how to test each fix.
**Advanced**: Composes timing red team with SC-A-003 (defense-in-depth) to verify that timing-sensitive enforcement has multiple independent verification surfaces, and with SC-P-003 (cross-feature interaction) to detect timing failures that emerge from the interaction of two individually correct subsystems.

### SC-P-011 — Detect when an audit method is structurally blind to the architecture

**Novice**: Trusts the red team's finding number ("59% orphan rate") because red team findings carry implicit authority, without questioning whether the detection method can see the architecture's actual wiring mechanism — deferring to the number and preparing to delete functioning components that were only "orphans" because the audit used name-grep on a trigger-based discovery system.
**Developing**: Questions whether the audit method matches the architecture when prompted but does not independently propose an alternative detection method that matches the architecture's actual discovery mechanism.
**Proficient**: Independently checks whether an audit's detection method can see the architectural pattern it claims to measure, matches the audit's detection mechanism to the architecture's wiring mechanism (e.g., name-grep vs. SKILL.md trigger-based discovery), rejects findings where the two are incompatible, and proposes a detection method that matches the architecture.
**Advanced**: Composes audit-method scrutiny with SC-P-016 (detect overcorrection) to prevent false audit data from driving overcorrection, and with SC-A-009 (progressive disclosure architecture) to ensure audit methods understand the 4-level hierarchy's discovery mechanisms.

### SC-P-012 — Decide whether to demote or strengthen a rule

**Novice**: Averages compliance percentages and demotes everything above a threshold (e.g., "anything above 90% is model-native"), ignoring the asymmetry between low-cost failures (communication style drift) and high-cost failures (security breaches) — so a 99%-compliant rule with catastrophic failure mode gets demoted alongside a 95%-compliant rule with trivial failure mode.
**Developing**: Considers failure cost when coached but does not independently define a monitoring mechanism for demoted rules or name a specific enforcement mechanism to add for strengthened rules.
**Proficient**: Independently makes an explicit demote-or-strengthen judgment for each partially-redundant rule, citing failure cost (not compliance percentage) as the rationale, defines a concrete monitoring mechanism for each demoted rule (e.g., "if the model starts producing human-weeks estimates, re-enter the always-in-context set"), and names a specific additional enforcement mechanism for each strengthened rule.
**Advanced**: Extends demotion judgment across the model envelope by composing with SC-P-022 (cross-model gap reading) to never demote on the strongest model's compliance alone if a weaker model still needs it, and feeds demotion rationale into SC-A-001 (codify-with-explicit-NOT-codified) to prevent future re-addition without data.

### SC-P-013 — Treat internal estimate inconsistency as a signal

**Novice**: Averages two inconsistent estimates from the same analytical process ("the truth is probably somewhere in the middle") or picks the pessimistic one, treating the inconsistency as noise rather than as a signal that the analysis has a flaw affecting all its outputs.
**Developing**: Identifies the inconsistency when prompted but traces it only to "different levels of detail" without specifying which estimate compressed away complexity and what that compression omitted.
**Proficient**: Independently detects when a single analysis produces two effort estimates that disagree by more than 2x, stops the plan, traces the divergence to the specific compression step that omitted information, states which estimate to distrust and why, and writes a gate rejection naming the analytical failure rather than picking a number.
**Advanced**: Composes estimate-contradiction detection with SC-P-018 (split the long pole) to subdivide the scope until both estimates can be true of different sub-deliverables, and with SC-P-015 (ask for contradictions) to surface estimate inconsistencies before they propagate into plans.

### SC-P-014 — Zero-tolerance scope discipline: failures vs gaps

**Novice**: Either treats every gap as a failure and attempts to build all missing functionality in the current session (scope explosion, original task incomplete, gap work half-done) or treats every failure as a gap and files them all as separate issues (failure ratchet, broken code survives another session).
**Developing**: Classifies discovered issues as failure or gap when prompted but does not independently state the action that matches the classification — failures get fixed now, gaps get filed with explicit scope boundaries.
**Proficient**: Independently classifies every pre-existing issue discovered during a session as either FAILURE (something was supposed to work but does not — fix in this session) or GAP (something was never built — file as separate issue with scope description), and matches the action to the classification without scope explosion or failure normalization.
**Advanced**: Composes failure/gap classification with SC-B-005 (spec-to-code conformance) to distinguish spec violations (failures) from unimplemented spec requirements (gaps), and with SC-B-009 (cross-SDK upstream brokerage) to route SDK-level failures to upstream rather than working around them locally.

### SC-P-015 — End complex prompts with falsification questions

**Novice**: Ends approval-gate prompts with confirmation questions ("is this right?" or "does this look good?") which bias the model toward agreement and bury weaknesses, producing rubber-stamp approvals on plans with hidden contradictions.
**Developing**: Writes a falsification question when prompted but makes it too broad ("is there anything wrong with this?"), which the model treats as equivalent to a confirmation question and answers with a token list of minor issues followed by "overall the analysis is sound."
**Proficient**: Independently ends every complex-deliverable review with a specific falsification question ("which estimate has the weakest evidence base, and what would change if it were 2x higher?") that forces the model to commit to a concrete vulnerability rather than hedging, and defaults to falsification over confirmation at every approval gate.
**Advanced**: Composes falsification prompting with SC-B-006 (multi-round red team) to make each round produce adversarial rather than confirmatory output, and with SC-B-008 (convergence-as-validation) to use convergent invalidation attempts from independent perspectives as high-confidence findings.

### SC-P-016 — Detect overcorrection as contamination

**Novice**: Trusts the "ignore rules" agent as a genuine baseline and concludes rules are essential because the no-rules agent produced bad output — when the bad output was actually contaminated defiance (performing anti-compliance) rather than genuine baseline behavior.
**Developing**: Recognizes bidirectional contamination when coached (both over-comply and over-defy are contamination) but does not independently set up a clean-room comparison to distinguish genuine baseline from contaminated output.
**Proficient**: Independently detects when an agent's output reflects performed compliance or performed defiance rather than genuine reasoning, diagnoses both extremes as the same contamination failure, and compares against a clean-room baseline (isolated instance with no access to the rules in question) to calibrate which outputs are trustworthy.
**Advanced**: Extends contamination detection across models by composing with SC-P-022 (cross-model gap reading) to run clean-room comparisons against every model in the deployment envelope, and feeds clean baselines into SC-A-008 (rule classification) to produce contamination-free classification data.

### SC-P-017 — Detect false-positive learning pipelines

**Novice**: Inspects only the observation-capture stage, sees 5,000 logged observations, and concludes the learning pipeline is healthy — without checking that the processing stage produces 8 duplicate instincts at hardcoded 0.9 confidence and the evolved directory is empty, meaning zero observations have ever been promoted to institutional knowledge.
**Developing**: Checks the processing stage output when prompted but does not independently audit all three stages (capture, processing, promotion) or identify that fixing input quality alone is insufficient when the processing script itself cannot distinguish between genuinely different patterns.
**Proficient**: Independently inspects the output end of a learning pipeline (not just the input), checks three diagnostic questions — are outputs distinct or near-duplicates, do confidence scores vary or are they hardcoded, has any output been promoted to institutional knowledge — and identifies failures at all three pipeline stages (capture quality, processing discrimination, promotion pathway).
**Advanced**: Composes pipeline diagnosis with SC-A-003 (defense-in-depth codification) to add independent validation between observation capture and instinct generation, and with SC-P-008 (dependency chain fragility) to analyze the pipeline as a dependency chain where the weakest link determines the value of the entire system.

### SC-P-018 — Split a multi-session blocker into fast-unblock and parallel-completion halves

**Novice**: Splits the blocker but makes the second half depend on the first half completing, reintroducing the sequential bottleneck — so the split saves zero time on the critical path and only adds gate overhead.
**Developing**: Identifies that a blocker should be split when prompted but does not independently determine which downstream items depend on which parts of the blocker or verify that the two halves are independently completable.
**Proficient**: Independently identifies when a single work item sits on the critical path blocking multiple downstream items, splits it into a minimum-subset fast-unblock half (gated independently) and a remainder half that proceeds in parallel with now-unblocked downstream work, verifies both halves are independently completable, and defines independent gates for each.
**Advanced**: Composes the split with SC-P-013 (estimate self-contradiction) to catch the estimate disagreement that reveals the blocker's true duration, and with SC-P-024 (fallback permanence detection) to ensure the fast-unblock half does not become a permanent workaround when the second half is deferred.

### SC-P-019 — Verify session-start continuity reaches the model

**Novice**: Writes session notes as a past-tense log ("Session 12: implemented auth module, ran into timeout issue, fixed by increasing pool size") and assumes that terminal output of "[WORKSPACE] Session notes: 3h ago" means the model has the information — confusing their own awareness with the model's, since hook stderr goes to the terminal not to the model's context.
**Developing**: Writes session notes as imperative directives when prompted ("resume at X, do NOT re-run Y") but does not independently verify that the injection channel delivers the notes into the model's context by testing whether the model can quote them.
**Proficient**: Independently verifies at every session start that (1) session notes exist and are written as instructions-to-next-self (imperative directives, not past-tense narrative), and (2) the injection channel delivers notes into the model's context by testing whether the model can reference them — and when the injection is broken, traces the path (hook registration, stderr vs stdout, path resolution) and fixes it within the first 5 minutes.
**Advanced**: Composes session-start continuity with SC-A-003 (defense-in-depth codification) to make the injection defense-in-depth (hook + prompt reminder + CLAUDE.md reference), and with SC-P-007 (session completion state) to ensure the notes carry actionable completion-state information.

### SC-P-020 — Treat the second occurrence of a drift class as structural

**Novice**: Treats each drift occurrence as unique because the specifics differ (skill files vs. variant files vs. numbering schemes), catalogs findings by artifact type rather than by failure class, and fixes individual instances without recognizing that the system generates this class of drift faster than instances can be repaired.
**Developing**: Recognizes a recurring drift class when prompted at the third occurrence but does not independently escalate from instance-fix to mechanism-fix at the second occurrence.
**Proficient**: Independently recognizes the second occurrence of a drift class (regardless of whether the artifact type differs) as a structural signal, shifts the intervention from instance-level repair to mechanism-level prevention (validation check, automated parity scan, or gate that blocks the drift-producing action), and maintains a running list of drift classes to enable this recognition.
**Advanced**: Composes second-occurrence recognition with SC-B-012 (manifest parity automation) as the mechanism-level fix for one common drift class, and with SC-P-021 (reference sweep after rename) as the mechanism-level fix for another, building a portfolio of prevention mechanisms matched to drift classes.

### SC-P-021 — Grep the entire repo for dangling references after every rename or delete

**Novice**: Greps for the exact filename after a rename but misses partial matches (the skill referenced as `10-governance`, `skills/10-governance/`, `10-governance/SKILL.md`, or `governance` in prose), or treats the sweep as a separate task for a future session — by which time new content has been written against stale references.
**Developing**: Runs a reference sweep when prompted after a rename but does not independently cover both structured references (YAML frontmatter, markdown links) and unstructured references (prose mentions, partial path matches), or include all hits in the same commit as the rename.
**Proficient**: Independently greps the entire repo for references to the old name or path immediately after every rename, delete, or move — covering exact paths, partial substrings, structured references (YAML, markdown links), and prose mentions — updates every reference in the same commit, and does not consider the rename "done" until the sweep returns zero hits.
**Advanced**: Composes reference sweeps with SC-P-020 (second-occurrence structural signal) to recognize dangling references as a recurring drift class and build a structural rename tool that updates references atomically, and with SC-B-002 (authority-chain escalation) to escalate sweeps to upstream repos when a rename affects synced artifacts.

### SC-P-022 — Author rules for the weakest model in the deployment envelope

**Novice**: Optimizes the rule set for the primary model (typically the strongest, used most often) and treats the weaker model as an edge case — producing a lean rule set that works 90% of the time but allows the weaker model to produce critical security violations (plaintext password comparison, hardcoded API keys) in the remaining 10%.
**Developing**: Checks the weaker model's gap profile when prompted but does not independently re-evaluate every existing rule against the new model or propose a variant architecture where weaker models carry fuller rule sets.
**Proficient**: Independently identifies every model in the deployment envelope, runs ablation against each model's baseline (not just the strongest), keeps rules needed by any model even if the primary model does not need them, and proposes a variant architecture where weaker models carry full rule sets and stronger models ship compressed.
**Advanced**: Composes cross-model gap reading as the terminus of the rule-optimization cluster (SC-P-002 -> SC-P-016 -> SC-A-008 -> SC-P-012 -> SC-P-022), forcing the entire chain to re-execute against every model, and feeds per-model gap profiles into SC-B-003 (variant classification) to inform global-vs-variant placement with model-specific evidence.

### SC-P-023 — Insert a gating workspace at integration seams

**Novice**: Adds integration assertions to one domain's workspace (e.g., the API layer) because it is "closest to the consumer" — making that workspace responsible for validating other domains' contracts without the context to do so, and hiding integration tests inside one domain's test suite where they are invisible to the other domains.
**Developing**: Proposes a gating workspace when prompted but defines vague gate criteria ("integration tests pass") rather than specifying a reference application or integration test suite that exercises all identified seams with concrete acceptance criteria.
**Proficient**: Independently identifies implicit integration seams where domains' outputs must compose but no workspace owns the seam, adds a dedicated gating workspace or phase that exercises all integration points with a reference application, specifies concrete acceptance criteria for each seam, and blocks downstream work structurally until the gate passes.
**Advanced**: Composes gating workspaces with SC-B-013 (cross-workspace dependency mapping) to ensure all seams identified by dependency mapping have corresponding gates, and with SC-P-018 (split the long pole) to validate that split halves compose correctly at the seam between fast-unblock and parallel-completion.

### SC-P-024 — Intervene when a temporary workaround gains its second consumer

**Novice**: Treats the workaround as "technical debt to address later" and adds it to a backlog — so the workaround stays "temporary" while consumers accumulate, and by the time "later" arrives the refactoring cost exceeds what any single session can absorb, silently promoting the workaround to permanent architecture.
**Developing**: Recognizes the permanence risk when prompted at the third or fourth consumer but does not independently intervene at the second consumer when the replacement cost is still manageable.
**Proficient**: Independently detects when a temporary workaround gains its second consumer, recognizes this as the critical intervention point (not the third or fifth), and chooses one of two actions: complete the proper implementation before the third consumer arrives, or accept the workaround as permanent and invest in making it production-grade with proper tests and documentation.
**Advanced**: Composes permanence detection with SC-P-018 (split the long pole) to fast-track the proper implementation before the workaround activates, and with SC-B-014 (explicit supersession journaling) to journal the decision when a workaround is promoted to permanent, superseding the original "temporary" framing.

### SC-P-025 — Run a tier-ranked literature search with source verification

**Novice**: Delegates the literature search entirely to the AI and accepts the results without verification — producing plausible-sounding citations that may be fabricated, misattributed, or stale, with tier rankings based on the AI's training data rather than on the actual papers' content.
**Developing**: Verifies citations against publisher sites when prompted but does not independently organize the search by topic area, tier-rank each paper relative to the specific argument (not general importance), or identify gaps where no paper occupies a relevant intersection.
**Proficient**: Independently runs a systematic literature search organized by 5-7 topic areas, records full citation with VERIFIED status (confirmed against publisher/database), assigns TIER 1/2/3 relative to the paper's argument, writes structured assessments (what it argues, what it does NOT claim, connection to argument), and identifies gap intersections as potential contribution spaces.
**Advanced**: Composes the literature search with SC-P-029 (post-publication gap check) to maintain the base over time, and uses convergence from SC-B-006 (multi-round convergence) to determine when successive search rounds produce no new TIER 1 papers, signaling search saturation.

### SC-P-026 — Detect misquotation and specification drift between source and citation

**Novice**: Verifies citations by asking the AI "does this paper say X?" — which is circular since the AI may have generated the misattribution in the first place — or accepts plausible-sounding derivative content without cross-checking against the authoritative source document, missing cases where "correct-sounding content from the wrong layer" (SDK vs spec) is the error.
**Developing**: Reads the actual source text when prompted to verify but does not independently check whether the source also says something that qualifies or contradicts the attributed claim, or classify errors by type (misquotation, fabrication, drift).
**Proficient**: Independently reads the actual source for every citation, verifies that the attributed claim appears at a specific section/page, checks for qualifications or contradictions the citation omits, classifies each discrepancy as misquotation (wrong claim attributed), fabrication (paper does not exist), or drift (derivative diverged from source), and records the verification status with the source location.
**Advanced**: Composes citation integrity with SC-B-005 (spec-to-code conformance audit) as the codegen analogue — auditing code against spec the same way this atom audits citations against sources — and with SC-P-015 (ask for contradictions) to apply "what would invalidate this citation?" as a falsification question.

### SC-P-027 — Run hostile reviewer simulation as preemptive defense

**Novice**: Runs a "review" that is actually a summary with hedged praise ("the argument is interesting but could be strengthened by...") — producing politeness theater rather than adversarial examination that identifies fatal flaws, desk-rejection triggers, and mechanisms that do not operate.
**Developing**: Adopts a hostile reviewer persona when prompted but does not independently specify the venue-specific reviewer background, identify FATAL vs MAJOR findings, or provide constructive revision paths alongside the critique.
**Proficient**: Independently commissions a hostile reviewer simulation with a specified venue-appropriate persona (discipline, seniority, known biases), identifies at least one fatal flaw (claims asserted rather than derived, mechanisms that do not operate), multiple major issues (missing comparisons, insufficient formalization), and desk-rejection triggers, with each finding including the attack, why it matters, and a suggested defense or revision path.
**Advanced**: Composes hostile review with SC-B-008 (convergence across independent reds) by running multiple hostile reviewers with different disciplinary personas and treating convergent weaknesses as confirmed findings, and feeds reflexivity problems discovered during hostile review into SC-P-033 (reflexivity diagnosis).

### SC-P-028 — Synthesize multiple hostile critiques into a minimal publishable model

**Novice**: Treats each hostile critique independently and tries to address all critics' individual concerns, including contradictory ones (Critic A says "too formal" while Critic B says "not formal enough"), producing incoherent revisions that satisfy neither.
**Developing**: Separates convergent from non-convergent critiques when prompted but does not independently propose a minimal publishable model that preserves all convergent strengths while eliminating all convergent failures.
**Proficient**: Independently separates what all critics agree works from what all critics agree does not work, produces the minimal publishable model (smallest argument surviving all attacks), identifies viable revision paths that address convergent failures while preserving convergent strengths, and treats non-convergent critiques as style preferences rather than structural failures.
**Advanced**: Composes multi-critique synthesis with SC-B-008 (convergence-as-validation) as the general principle applied to research, and verifies the minimal model's internal consistency using SC-P-013 (estimate self-contradiction) to catch contradictions between the model's own claims.

### SC-P-029 — Run a post-publication gap check for recent literature

**Novice**: Runs the gap check as a citation-padding exercise ("find more papers to cite") rather than verifying whether the field has moved under the argument — adding 10 new citations without questioning whether the argument needs revision, not just more references.
**Developing**: Searches for recent publications when prompted but does not independently distinguish MUST CITE (empirical evidence reviewers will expect) from DO NOT CITE (papers that would reposition the argument in a diluting way).
**Proficient**: Independently runs a focused gap check for recent publications targeting specific claims in the draft, classifies each paper found as MUST CITE, SHOULD CITE, MAY CITE, or DO NOT CITE with positioning rationale, and verifies that novelty claims ("first to formalize") are still true as of the submission date.
**Advanced**: Composes the gap check with SC-P-025 (tier-ranked literature search) to maintain the literature base over time, and with SC-P-034 (overclaim prevention) to update temporal qualifiers on novelty claims when the gap check finds new prior work.

### SC-P-030 — Calibrate academic register with detection-bias awareness

**Novice**: Eliminates AI-detection-flag patterns but replaces them with equally detectable alternatives — swapping "Furthermore" for "Building on this" (still a smooth transition) — restructuring surface markers without earning transitions through argumentative necessity.
**Developing**: Identifies detection-flag patterns (parallel triads, smooth transitions, hedging phrases, uniform sentence length) when prompted but does not independently calibrate register to both the target venue's conventions and detection-bias awareness simultaneously.
**Proficient**: Independently calibrates prose register on two axes — (1) matching venue conventions so reviewers recognize the paper as belonging to their field, and (2) eliminating stylistic patterns that trigger AI detection false positives — by restructuring transitions to be earned by argumentative necessity rather than inserted as connective tissue, while preserving the argument's content.
**Advanced**: Composes register calibration with SC-P-032 (margin note as deliberation) to document why specific formality levels were chosen, and with SC-P-034 (overclaim prevention) to ensure vocabulary precision ("first formal model" not "novel") aligns with the calibrated register.

### SC-P-031 — Frame venue strategy as a constraint envelope

**Novice**: Treats venue selection as "where will this paper be accepted?" and submits to the highest-prestige venue without restructuring — getting desk-rejected because the paper does not meet the venue's methodology constraints (formal proofs, empirical evidence, required sections).
**Developing**: Lists a venue's requirements when prompted but does not independently map what the argument must ADD and what it must REMOVE for that venue, or assess whether the venue's constraint envelope breaks the argument.
**Proficient**: Independently maps each candidate venue's constraint envelope across methodology, scope, audience, and format dimensions, identifies what the argument must add and remove for each venue, assesses whether the constraint envelope breaks the argument (a valuable finding), and documents trade-offs between venues as explicit decisions.
**Advanced**: Composes venue strategy with SC-P-027 (hostile reviewer simulation) to ensure the reviewer persona matches the target venue's reviewer pool, and with SC-P-030 (academic register calibration) to match register to the chosen venue's conventions.

### SC-P-032 — Document writing choices as margin-note deliberation artifacts

**Novice**: Writes margin notes that describe what the text says ("this paragraph introduces the governance dilemma") rather than why it says it that way — producing summaries redundant with the text instead of institutional knowledge about the writing process that prevents future sessions from relitigating settled decisions.
**Developing**: Documents the chosen option when prompted but does not independently record alternatives considered or explain why this choice serves the argument better than alternatives.
**Proficient**: Independently documents non-obvious writing choices as margin-note artifacts recording what was chosen, what alternatives were considered (at least 2), and why this choice serves the argument — producing institutional knowledge about the writing process that a future editor or AI can use to understand and preserve the deliberate choices.
**Advanced**: Composes margin notes with SC-A-001 (codify-with-explicit-NOT-codified) to treat writing deliberation as codifiable institutional knowledge, and with SC-P-027 (hostile reviewer simulation) to feed defense notes from hostile review into margin notes explaining why specific phrasings were chosen.

### SC-P-033 — Diagnose reflexivity in self-referential research

**Novice**: Confuses reflexivity with bias — treating a structural circularity (the modeler already knew the answer) as a systematic error (the model consistently overestimates one variable) and proposing bias correction instead of sensitivity analysis showing results survive under alternative specifications.
**Developing**: Identifies that the author formalized their own prior design when prompted but does not independently list the specific model specification choices that map suspiciously well to the desired result or propose alternative specifications that could falsify the result.
**Proficient**: Independently diagnoses reflexivity when a researcher formalizes their own prior design, identifies the specific model specification choices made by someone who already knew the answer, lists 3 alternative specifications the author did NOT use, and proposes a sensitivity analysis that would convert the reflexivity from a vulnerability to a strength.
**Advanced**: Composes reflexivity diagnosis with SC-P-027 (hostile reviewer simulation) where hostile review is the discovery tool for reflexivity, and with SC-P-015 (ask for contradictions) to apply "what model specification would falsify this result?" as the reflexivity-specific falsification question.

### SC-P-034 — Qualify every superlative — prevent overclaims

**Novice**: Qualifies novelty claims with generic hedging ("to the best of our knowledge, this is the first paper to address AI governance") without specifying the scope within which the claim holds or citing evidence of a search — so the "qualification" is still an overclaim because "AI governance" is too broad and "to the best of our knowledge" is not evidence.
**Developing**: Adds a scope qualifier when prompted ("first formal model in the incomplete contracts tradition") but does not independently cite the evidence that the scope is empty (the gap check that found no prior work) or add a temporal qualifier.
**Proficient**: Independently applies a three-step qualification to every superlative in research writing — (1) defines the scope within which the claim holds, (2) cites the evidence that the scope is empty (the gap check, the literature review section), and (3) adds a temporal qualifier ("as of [date]") — producing qualified claims that are stronger because they are falsifiable.
**Advanced**: Composes overclaim prevention with SC-P-029 (post-publication gap check) to maintain temporal validity of novelty claims, and with SC-P-026 (citation integrity audit) to treat "no prior work" as itself a citation of absence that must be verified against sources.

## Summary statistics

- Atoms where the novice level maps cleanly to the documented failure mode: 67/67
- Atoms where the advanced level involves cross-atom composition: 67/67
