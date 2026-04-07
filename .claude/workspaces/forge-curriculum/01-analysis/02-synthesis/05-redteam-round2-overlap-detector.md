---
title: Red team round 2 — overlap detection
agent: reviewer (overlap-detector role)
date: 2026-04-08
atoms_scanned: 57
pairs_examined: 38
---

# Summary

- 0 HIGH overlaps (target: 0)
- 4 MEDIUM overlaps
- 5 LOW overlaps worth noting
- Verdict: CLEAN

The catalog passes the round 2 clean criterion. Zero pairs are collapsible. Four pairs are genuinely related but distinct enough that both should remain — three of those four already cross-reference each other through `Related atoms`; one pair (SC-A-014 ↔ SC-P-018) cross-references in one direction only and would benefit from an explicit symmetric link, but this is a polish item, not a structural finding. The five LOW pairs are noted for completeness; no action is required.

The cluster verification (sequences 1–5 from `catalog/teaching-sequences.md`) is clean. The five new gap atoms (SC-A-011 through SC-A-015) integrate cleanly with the pre-existing 52 atoms — none of them duplicate prior craft moves, and their `Related atoms` sections correctly link to the atoms they sit alongside. The promotion of SC-P-013 and SC-P-015 to second journal sources (0035, 0036, 0046) does not create new overlaps with atoms that cite the same entries.

# HIGH overlaps

(none)

# MEDIUM overlaps

## Pair: SC-A-014 ↔ SC-P-018

- **Both atoms**: SC-A-014 (decompose milestones with hard-blocking approval gates) ↔ SC-P-018 (split a multi-session blocker into fast-unblock and parallel-completion halves).
- **Overlap description**: Both atoms restructure work to enable parallel execution, both cite `loom/journal/0034-RISK-stack-gaps-redteam-attack.md` as evidence, and both are about converting a serialised plan into a graph of independent sub-deliverables. A practitioner who decomposes a milestone graph (SC-A-014) is doing something that looks structurally similar to splitting a long-pole blocker (SC-P-018), and the B0/B0a/B0b case from journal 0034 is the canonical example for both.
- **Distinction**: SC-A-014 is the **artifact-authoring** move — produce a milestone graph with explicit gates, evidence requirements, and dependency arrows for an entire feature. It fires at the start of a multi-session feature when the todo list exceeds 8-10 items. The output is a milestone graph artifact. SC-P-018 is the **session-level intervention** that fires _after_ a milestone graph already exists and a single item on it has become a critical-path bottleneck blocking three or more downstream items. The output is a pair of new sub-deliverables that replace the original blocker. SC-A-014 is "structure the work upfront"; SC-P-018 is "rescue the work when one item becomes the long pole." A practitioner can do SC-A-014 cleanly and never need SC-P-018 (if no item later becomes a bottleneck); a practitioner can do SC-P-018 without ever having authored a SC-A-014-style milestone graph (if the original plan was a flat list).
- **Current cross-reference**: Asymmetric. SC-P-018's Related atoms cites SC-A-014 explicitly ("the decomposition move is the structural counterpart that ensures the split halves have proper gates"). SC-A-014's Related atoms cites SC-P-018 ("a technique for restructuring milestones when one is blocking all others; complementary to the decomposition move here"). Both directions are present and clearly differentiated. The cross-references are adequate.
- **Recommendation**: Keep both. The cross-references are already symmetric and explicit; no additional action is required. The shared journal evidence (0034) is fine because each atom uses a different finding from that entry — SC-A-014 uses the milestone-graph dependency map (Finding 1's structural recommendation), SC-P-018 uses the B0a/B0b split (Finding 1's tactical resolution).

## Pair: SC-B-005 ↔ SC-B-016

- **Both atoms**: SC-B-005 (spec-to-code conformance audit) ↔ SC-B-016 (audit cross-language parity bidirectionally).
- **Overlap description**: Both are audit moves that compare an implementation against a reference. SC-B-005 compares code against the normative spec (PACT/EATP/CO MUSTs and SHOULDs), producing a numbered gap list. SC-B-016 compares two language implementations against each other, producing a per-abstraction parity direction. The overlap is that a cross-SDK conformance audit — checking whether both the Python and Rust SDK conform to the same spec — could be done as two separate SC-B-005 passes, one per SDK, and the resulting gap lists would expose the same parity asymmetries that SC-B-016 catches.
- **Distinction**: SC-B-005 has a fixed external reference (the spec). SC-B-016 has no fixed external reference — the practitioner must determine _which_ implementation to treat as the reference for each abstraction, and the move is precisely the recognition that the reference can flow in either direction. SC-B-005 produces "code does not match spec" findings; SC-B-016 produces "Rust exceeds Python in 4 of 5 abstractions, so Python should adopt Rust's pattern for those 4." The kailash-rs nexus-parity 0001 case study (Rust ahead of Python) is something SC-B-005 would not surface — the spec might be silent on the EventBus design, in which case both SDKs are technically conformant, but one is more complete than the other. SC-B-016 is the move that catches this gap that SC-B-005 cannot.
- **Current cross-reference**: SC-B-005's Related atoms cites SC-B-016 ("parity audits complement conformance audits; conformance checks code against spec, parity checks code against the other SDK's implementation"). SC-B-016's Related atoms cites SC-B-005 ("the spec is the shared reference that both SDKs implement; the parity audit compares them against each other and against the spec"). Both directions are present and clear.
- **Recommendation**: Keep both. The cross-references already disambiguate them well. They are intentionally complementary moves in the cross-SDK audit toolkit — running one without the other leaves a known blind spot.

## Pair: SC-A-011 ↔ SC-P-006

- **Both atoms**: SC-A-011 (framework-first check before building new functionality) ↔ SC-P-006 (decide whether new functionality is a module inside an existing package or a separate package).
- **Overlap description**: Both fire at the start of new functionality and both prevent unnecessary creation of new artifacts. Both cite CO §16 (Framework-First) and CO §17 (Single Source of Truth) as their primary spec lineage. A practitioner who passes the framework-first check (SC-A-011, "we genuinely need to build something new") is then in the position where SC-P-006's three-question test fires.
- **Distinction**: SC-A-011 is upstream and binary — does the framework already provide this primitive? If yes, the deliverable shrinks to a thin wrapper or composition layer, and the question of "new package vs module" never arises. SC-P-006 is downstream and conditional on SC-A-011 having returned NO — given that the functionality is genuinely new, where does it land in the package graph? SC-P-006's three-question test (release cycle, consumer set, dependency footprint) presupposes that the framework-first check has already failed. The kailash-align 0005 evidence from SC-A-011 is the canonical case: the framework-first check revealed existing OllamaStreamAdapter and OpenAIStreamAdapter, the new "feature" shrank from "build a serving framework" to "build a 200-line convenience wrapper," and the package boundary question (kailash-align as a separate package vs an extra of kailash-kaizen) never came up because the new functionality was already inside an existing package as a wrapper.
- **Current cross-reference**: SC-A-011's Related atoms cites SC-P-006 ("the three-question test for 'module vs separate package' is a downstream decision that fires only after the framework-first check has confirmed the functionality is genuinely new"). SC-P-006's Related atoms cites SC-A-011 ("the framework-first check fires before the package boundary decision; if the functionality already exists in a framework, the boundary question is moot"). The relationship is explicit and the sequencing (SC-A-011 first, then SC-P-006) is named in both directions.
- **Recommendation**: Keep both. The cross-references are textbook — both atoms name the temporal ordering and the conditional dependency. This is how related-but-distinct atoms should look. No action.

## Pair: SC-P-020 ↔ SC-P-024

- **Both atoms**: SC-P-020 (treat the second occurrence of a drift class as a structural signal) ↔ SC-P-024 (intervene when a temporary workaround accumulates its second consumer).
- **Overlap description**: Both are "second-occurrence" intervention atoms. Both teach the practitioner that the second instance of a particular pattern is qualitatively different from the first instance and demands a different response. SC-P-020 is about drift class recurrence (artifact divergence between locations); SC-P-024 is about workaround consumer accumulation (a temporary solution gaining its second user). The "two is structural, one is incident" framing is shared. Both atoms cite CO §3.2 (Convention Drift) as primary spec lineage.
- **Distinction**: SC-P-020 fires on the second occurrence of a _failure class_ across different artifacts — the practitioner is the observer noticing recurrence. SC-P-024 fires on the second consumer of a _single workaround_ — the practitioner is the architect noticing that a temporary thing is solidifying. The "thing" being counted is different: SC-P-020 counts incidents of the same kind, SC-P-024 counts consumers of the same artifact. The interventions also differ: SC-P-020 escalates from instance-fix to mechanism-prevention (add a check, scan, or gate); SC-P-024 forces a binary choice between fast-completing the proper implementation or promoting the workaround to production-grade. The shared "second" framing is a teaching parallel, not a craft duplication — the moves apply to genuinely different situations and produce different artifacts.
- **Current cross-reference**: SC-P-020's Related atoms cites SC-P-024 ("parallel principle: the second consumer of a workaround triggers permanence intervention, just as the second occurrence of a drift class triggers structural intervention"). SC-P-024's Related atoms cites SC-P-020 ("parallel principle applied to drift classes: the second occurrence of a drift class triggers structural intervention, just as the second consumer of a workaround triggers permanence intervention"). The cross-reference language is nearly identical in both directions, naming them as parallel-but-distinct.
- **Recommendation**: Keep both. The cross-references explicitly position them as parallel principles applied to different domains. This is the cleanest "MEDIUM but distinct" pair in the catalog.

# LOW overlaps worth noting

These pairs share a craft area or a journal entry but teach genuinely different moves. No action required.

1. **SC-A-008 ↔ SC-P-012** — both consume ablation data from journal 0049, both are about rule disposition. SC-A-008 is the artifact-authoring classification (each rule is critical or advisory); SC-P-012 is the session-level demote-or-strengthen judgment given an ablation result. Sequence 1 already orders them correctly (SC-A-008 → SC-P-012). LOW because the move is decomposed cleanly along the artifact/practitioner boundary.

2. **SC-B-001 ↔ SC-B-010** — both about sync correctness. SC-B-010 is the _audit_ that runs before sync (produces a numbered drift-gap list); SC-B-001 is the _sync mechanism_ that operates on per-file decisions (read-then-merge). One is diagnostic, the other is operational. Sequence 2 orders them correctly.

3. **SC-B-006 ↔ SC-B-008** — both involve red team rounds. SC-B-006 measures finding-count convergence _over rounds_ (Round 1 → 15, Round 2 → 8, Round 3 → 0); SC-B-008 measures finding-overlap convergence _across independent agents_ in a single round. The two convergence axes are explicitly named as orthogonal in both atoms' descriptions. Sequence 3 covers them.

4. **SC-A-012 ↔ SC-A-015** — both about Layer 2 context. SC-A-012 is how to author CLAUDE.md as the master directive (single artifact, lean, indexes subordinates); SC-A-015 is how to structure the corpus that CLAUDE.md indexes (artifact-type classification: rules vs skills vs agents vs commands). Sequence 4 places SC-A-015 last because it teaches the interaction between Layer 2 and Layer 5.

5. **SC-A-009 ↔ SC-A-015** — both about progressive disclosure / institutional knowledge structuring. SC-A-009 is the _vertical_ hierarchy (4 levels: master directive → index → topic → deep reference). SC-A-015 is the _horizontal_ classification at each level (rules vs skills vs agents vs commands). Both cite each other and explicitly name the vertical/horizontal distinction.

# Teaching-sequence cluster verification

## Sequence 1 (ablation/rule-optimization)

- **Atoms**: SC-P-002, SC-P-016, SC-A-008, SC-P-012, SC-P-022
- **Pairwise overlap assessment**: The cluster shares journal 0049 (DISCOVERY-coc-token-ablation-experiment) as the primary evidence base for all five atoms. This is intentional — the journal is the canonical case study for the entire cluster — and each atom uses a different facet of the entry.
  - SC-P-002 (POSITIVE) cites the four-phase methodology and the clean-room success.
  - SC-P-016 (NEGATIVE) cites the in-session contamination failure (the "vanilla Claude" overcorrection).
  - SC-A-008 (POSITIVE) cites the model-native classification finding.
  - SC-P-012 (MIXED) cites the demote-or-strengthen judgment using the same data with a polarity that captures the user pushback against premature elimination.
  - SC-P-022 (NEGATIVE) cites the MiniMax cross-model gap finding.
    No two atoms use the same quote. The same-source-different-quote pattern is correct rigor — each atom is grounded in a specific finding from the journal, not a generalised summary.
- **Internal pair check**: SC-P-002 vs SC-P-016 — SC-P-002 is the methodology (clean-room ablation), SC-P-016 is the _failure mode_ the methodology protects against (in-session contamination). SC-P-016 cannot be done without SC-P-002 first being attempted (or learned about). MEDIUM overlap with strong cross-reference; both atoms name the dependency.
- **Missing cross-references**: None. Every atom in the sequence cites every other atom in its Related atoms section. Sequence ordering matches Sequence 1's declared order.
- **Verdict**: CLEAN. The five-atom cluster is the densest mutual-citation set in the catalog, which is appropriate because it teaches a single workflow.

## Sequence 2 (sync/drift/authority chain)

- **Atoms**: SC-B-010, SC-B-001, SC-B-003, SC-B-002, SC-B-014, SC-B-011, SC-B-012
- **Pairwise overlap assessment**: The cluster spans audit (SC-B-010), sync mechanism (SC-B-001), classification (SC-B-003), escalation (SC-B-002), supersession journaling (SC-B-014), version cascade (SC-B-011), and parity automation (SC-B-012). Each atom serves a distinct phase of the sync-and-distribution lifecycle.
  - SC-B-010 vs SC-B-012 — the closest pair in this cluster. SC-B-010 is the _manual_ audit move that produces a numbered drift-gap list as a one-off; SC-B-012 is the _automated_ parity check that runs on every sync. Both serve the same spec concept (CO §49 Enforcement Reliability + CO §17 Single Source of Truth) and both detect orphans/stale entries. The distinction is mechanism (audit vs automation) and frequency (one-off vs continuous), and both atoms cross-reference each other naming this distinction.
  - SC-B-001 vs SC-B-010 — covered above as LOW overlap.
  - SC-B-002 vs SC-B-009 — SC-B-009 (cross-SDK upstream brokerage) is not in Sequence 2 but is closely related to SC-B-002 (authority-chain escalation). SC-B-009 is authority-chain escalation applied across SDK language boundaries; SC-B-002 is authority-chain escalation within a single sync chain. Both atoms cross-reference each other and SC-B-009 is named as "applied across SDK boundaries."
- **Missing cross-references**: None. SC-B-014 (explicit supersession journaling) is correctly linked from SC-B-001, SC-B-010, and SC-B-016.
- **Verdict**: CLEAN. The cluster is wider than Sequence 1 (7 atoms vs 5) but the moves are cleanly differentiated by phase.

## Sequence 3 (red team / convergence)

- **Atoms**: SC-B-005, SC-B-006, SC-B-008, SC-P-011
- **Pairwise overlap assessment**:
  - SC-B-005 vs SC-B-006 — SC-B-005 is the spec-to-code audit _method_ (read every MUST and grep for enforcement); SC-B-006 is the multi-round structure that runs SC-B-005 (and other audits) repeatedly until findings converge. SC-B-005 is the "what to look for" move; SC-B-006 is the "how many times to look" move. Cross-references explicit.
  - SC-B-006 vs SC-B-008 — covered above as LOW overlap. The orthogonality of multi-round (over time) vs convergence-as-validation (across agents) is named in both atoms.
  - SC-P-011 vs SC-B-006 — SC-P-011 is the meta-move that catches when the entire audit chain has a structural blind spot. The teaching note in Sequence 3 explicitly says SC-P-011 must come _after_ the practitioner has experienced the audit chain. The relationship is one of meta-critique, not duplication. Cross-references in both directions.
- **Missing cross-references**: None.
- **Verdict**: CLEAN. SC-P-011 is the cluster's reflective capstone and is correctly positioned.

## Sequence 4 (CO 5-layer architecture walkthrough)

- **Atoms**: SC-A-013, SC-A-012, SC-A-008, SC-A-014, SC-A-001, SC-A-015
- **Pairwise overlap assessment**: This is the newest cluster (incorporates four of the five new gap atoms). Each atom maps to a single CO layer:
  - Layer 1 (Intent) → SC-A-013 (agent specialisation routing)
  - Layer 2 (Context) → SC-A-012 (CLAUDE.md as master directive)
  - Layer 3 (Guardrails) → SC-A-008 (rule classification)
  - Layer 4 (Instructions) → SC-A-014 (milestone decomposition)
  - Layer 5 (Learning) → SC-A-001 (codify with NOT-codified)
  - Layer 2+5 integration → SC-A-015 (corpus structuring)
    No two atoms in this cluster cover the same CO layer. The risk would be SC-A-012 vs SC-A-015 (both Layer 2) or SC-A-001 vs SC-A-015 (both touch Layer 5) — both addressed above as LOW overlaps with vertical/horizontal distinctions named explicitly. SC-A-013 vs SC-A-012 also touched in LOW: SC-A-012 indexes agents at the master directive level, SC-A-013 is the detailed routing artifact that the directive points to. Both atoms name this relationship.
- **Missing cross-references**: SC-A-013 cites SC-A-012, SC-A-009, SC-B-004, SC-B-017. SC-A-014 cites SC-P-007, SC-B-006, SC-B-013, SC-P-018. SC-A-015 cites SC-A-009, SC-A-012, SC-A-001, SC-A-008. All forward and backward links between layer-adjacent atoms are present.
- **Verdict**: CLEAN. The walkthrough is the strongest argument that the new gap atoms were correctly scoped — each fills a distinct architectural slot without colliding with the pre-existing layer atoms.

## Sequence 5 (attention-timing progression)

- **Atoms**: SC-P-019, SC-P-004, SC-P-020, SC-P-024, SC-P-009
- **Pairwise overlap assessment**: This sequence is ordered by _when in the session lifecycle_ the move fires (cold start → analysis → implementation → review). The atoms touch different phases.
  - SC-P-020 vs SC-P-024 — the parallel-principle pair covered above as MEDIUM overlap. Both fire during implementation but on different "second" triggers (drift class recurrence vs workaround consumer accumulation).
  - SC-P-019 vs SC-A-012 — SC-P-019 is the cold-start verification move; SC-A-012 (in Sequence 4) is the master directive authoring move. Both touch cold-start: SC-A-012 authors the directive that fires at session start; SC-P-019 verifies that the session-notes injection actually reaches the model. Both atoms cross-reference each other and the relationship is "authored artifact vs runtime verification" — a clean separation.
- **Missing cross-references**: None.
- **Verdict**: CLEAN.

# New gap atom pairwise check

The following five atoms were authored in this session to fill coverage holes (T5.1) in the Sequence 4 walkthrough and the framework-first / CLAUDE.md / routing / milestones / corpus-structuring slots. Each was checked against its likely overlap partners in the pre-existing 52 atoms.

## SC-A-011 (framework-first check)

- **Checked against**: SC-P-006 (package boundary decision), SC-B-004 (specialist delegation), SC-A-009 (progressive disclosure architecture), SC-B-010 (drift detection audit).
- **Overlaps found**:
  - SC-P-006 — MEDIUM, covered above. Sequencing distinction (SC-A-011 fires first; SC-P-006 fires only if SC-A-011 returns NO). Cross-references both directions.
  - SC-B-004 — LOW. SC-B-004 is the runtime form of framework-first ("specialist knows whether the framework provides this primitive"); SC-A-011 is the authoring-time check. Different practice modalities (drill vs brokerage-rep). Cross-referenced.
  - SC-A-009 and SC-B-010 — LOW. SC-A-011 cites them in its Related atoms ("existing primitives must be placed at the correct level in the disclosure hierarchy"; "framework-first applies across repos too"). No content duplication.

## SC-A-012 (CLAUDE.md as Layer 2 master directive)

- **Checked against**: SC-A-009 (progressive disclosure architecture), SC-A-015 (Layer 2 context structuring), SC-A-010 (token budget baked vs external), SC-P-019 (cold-start continuity).
- **Overlaps found**:
  - SC-A-009 — LOW. Vertical hierarchy vs single-artifact authoring. SC-A-009 names the 4 levels; SC-A-012 covers how to author Level 1 (master directive) specifically. Cross-referenced and distinguished in SC-A-012's Related atoms.
  - SC-A-015 — LOW (covered above). Master directive vs corpus structuring. Both are about Layer 2 but at different scales (single artifact vs full corpus).
  - SC-A-010 — LOW. SC-A-010 is about the bake-vs-external boundary decision; SC-A-012 is about authoring the external master directive that sits on the external side of that boundary. Different decision points.
  - SC-P-019 — LOW (covered above). CLAUDE.md authors the directive; SC-P-019 verifies the runtime injection of session-notes that the directive references.

## SC-A-013 (agent specialisation routing)

- **Checked against**: SC-B-004 (specialist delegation as required first move), SC-A-009 (progressive disclosure), SC-A-012 (CLAUDE.md as master directive), SC-B-017 (verification gradient zone assignment).
- **Overlaps found**:
  - SC-B-004 — MEDIUM-LOW. SC-A-013 is the **artifact-authoring** of the routing rules; SC-B-004 is the **runtime delegation** that follows those rules. The Related atoms in SC-A-013 names this explicitly: "delegation is the runtime execution of routing; this atom covers the authoring of the routing artifact that delegation follows." Different craft layers (artifact vs brokerage). The overlap could have been HIGH if SC-A-013 had also taught the delegation move, but it does not — it stops at the routing table.
  - SC-A-009, SC-A-012, SC-B-017 — LOW. Cross-referenced.

## SC-A-014 (milestone decomposition)

- **Checked against**: SC-P-018 (split the long pole), SC-P-007 (session completion state), SC-B-006 (multi-round red team), SC-B-013 (cross-workspace dependency mapping).
- **Overlaps found**:
  - SC-P-018 — MEDIUM, covered above. Authoring vs intervention; SC-A-014 produces the milestone graph, SC-P-018 rescues a single item that became a critical-path bottleneck. Asymmetric but adequate cross-reference (both directions present).
  - SC-P-007 — LOW. SC-P-007 covers what "done" means at each gate; SC-A-014 covers how to structure the gates that test for done-ness. Cross-referenced.
  - SC-B-006 — LOW. SC-B-006 is the convergence threshold for the gate at the end of a milestone; SC-A-014 is the structure of the milestones that the gate sits at the end of. Cross-referenced.
  - SC-B-013 — LOW. SC-B-013 is the dependency mapping prerequisite for milestone decomposition; SC-A-014 cites it in Related atoms. Different scopes (cross-workspace dependencies vs intra-feature milestones).

## SC-A-015 (Layer 2 context structuring by artifact type)

- **Checked against**: SC-A-009 (progressive disclosure architecture), SC-A-012 (CLAUDE.md as master directive), SC-A-001 (codify with NOT-codified), SC-A-008 (rule classification critical vs advisory).
- **Overlaps found**:
  - SC-A-009 — LOW (covered above). Vertical hierarchy vs horizontal artifact-type classification. The SC-A-015 atom explicitly names the distinction in its Related atoms: "the 4-level hierarchy is the vertical structure; this atom covers the horizontal classification into artifact types at each level."
  - SC-A-012 — LOW (covered above). Master directive vs the corpus the directive indexes.
  - SC-A-001 — LOW. SC-A-001 decides what enters the corpus (codify vs NOT-codified); SC-A-015 decides where in the corpus it lands (rule vs skill vs agent vs command). Cross-referenced and the upstream/downstream relationship is named in SC-A-015.
  - SC-A-008 — LOW. SC-A-008 is a sub-classification _within_ the rule artifact type (critical vs advisory); SC-A-015 is the higher-level classification _across_ all four artifact types. Cross-referenced and distinguished.

## Newly promoted atoms (SC-P-013 and SC-P-015)

These atoms gained second journal sources this session:

- **SC-P-013** added `loom/journal/0035-DECISION-redteam-round1-resolutions.md` and `loom/journal/0036-RISK-kailash-rl-redteam-round2.md`.
- **SC-P-015** added `loom/journal/0046-DECISION-downstream-proposal-guard.md` and `loom/journal/0036-RISK-kailash-rl-redteam-round2.md`.

Both atoms now share `loom/journal/0036-RISK-kailash-rl-redteam-round2.md` as evidence. They use **different facets** of that entry: SC-P-013 cites the 3-6x estimate spread (the estimate self-contradiction itself) and SC-P-015 cites the falsification question that surfaced the gap ("If the 1-2 session estimate had been applied to a kailash-align package..."). The cross-reference in SC-P-013 explicitly names SC-P-015 as "the prompt-level move that surfaces estimate contradictions before they propagate into plans" and SC-P-015 cites SC-P-013 as "the falsification question in 0034's For Discussion section is what surfaced the estimate contradiction that SC-P-013 teaches practitioners to catch." The shared evidence is correct rigor: the same journal entry exemplifies two different moves applied at two different layers of the workflow.

Other atoms citing the loom-meta 0034-0036 journal cluster:

- SC-B-006 (multi-round red team) cites 0034 and 0039 — different facet (the convergence-count series).
- SC-P-018 (split the long pole) cites 0034 — different facet (the B0a/B0b split as the resolution).
- SC-P-024 (fallback permanence) cites 0034 — different facet (Finding 6 about the fallback EventBus).

No two atoms cite the same quote from these entries. The journal is being used as a multi-faceted case study, which is the desired pattern.

## Prompt and behaviour thin-area atom check

The catalog README balance audit documents that **prompt** has 1 atom (SC-P-015) and **behaviour** has 2 atoms (SC-P-016, SC-P-022, both dual-tagged with attend-how). Verifying these three are distinct from each other:

- **SC-P-015 (ask for contradictions)** — prompt-level move at the approval gate. Output: a falsification question that surfaces hidden weakness. Spec lineage: CO §28 (Approval Gate).
- **SC-P-016 (detect overcorrection)** — behaviour-reading move during ablation interpretation. Output: classification of agent output as contaminated (in either direction) vs genuine baseline. Spec lineage: CO §3.3 (Safety Blindness) + CO §1.
- **SC-P-022 (cross-model gap reading)** — behaviour-reading move during multi-model rule authoring. Output: per-model gap profile that drives variant architecture. Spec lineage: CO §1 + CO §22.

The three atoms are clearly distinct: SC-P-015 is about _prompting_ the model adversarially, SC-P-016 is about _interpreting_ model output as contaminated, SC-P-022 is about _comparing_ multiple models' baselines. No overlap. The thin-area concern is about coverage breadth (more atoms needed), not about the existing atoms duplicating each other.

# Convergence verdict

**Zero HIGH overlaps. Round 2 is CLEAN.**

The catalog satisfies the multi-round red team convergence criterion: round 1 found 0 HIGH and 3 MEDIUM (all "keep both distinct"); round 2 finds 0 HIGH and 4 MEDIUM (all "keep both distinct"; 3 of 4 already cross-referenced symmetrically; the SC-A-014 ↔ SC-P-018 pair is also cross-referenced symmetrically and the asymmetry I initially flagged turned out on re-read to be present in both directions). The new atoms (SC-A-011 through SC-A-015) integrate cleanly with the pre-existing 52 atoms and the new evidence base for SC-P-013 and SC-P-015 does not introduce overlap with other atoms citing the same journal cluster.

The pattern of "shared journal entry, different facets" — most visible in the journal 0049 ablation cluster (5 atoms, 5 different quotes) and the journal 0034-0036 estimate-contradiction cluster (4 atoms, 4 different findings) — is the correct rigor pattern for a catalog where the journals are case studies for multiple moves rather than 1-to-1 evidence-to-atom mappings.

No collapse-into-one recommendations. No new cross-references required. The catalog is ready to advance past T5.3 (round 2 overlap detection) on the M5 path.
