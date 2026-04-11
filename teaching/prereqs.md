---
title: Atom prerequisite graph
date: 2026-04-10
destination: forge
---

# FORGE Atom Prerequisite Graph

Explicit dependency edges between atoms. Downstream courses can topologically sort this graph to produce any sequence consistent with atom dependencies. Two edge types:

- **hard** — the source atom must come before the target atom because the target uses the source's output or depends on skills the source teaches
- **soft** — the source atom reinforces the target atom but is not required to come first

This file is derived from each atom's `## Related atoms` section and from `catalog/teaching-sequences.md`. If you think an edge is wrong, check the source atom file — do not patch this file without updating the underlying relationship description.

Extraction rules applied:

1. Atoms appearing in `teaching-sequences.md` contribute hard prereqs in sequence order (later ← earlier).
2. Explicit "X produces the input this move halts on," "X is the prerequisite that prevents…," "X fires before the decision this atom teaches," and "X is what to do with the results of this atom" → hard.
3. "Related," "composes with," "complementary," "reinforces," "sibling pattern," "the other side of the same event" → soft.
4. When a Related atoms note suggested a hard edge that would have created a cycle with an authoritative teaching sequence, the weaker edge was reclassified as soft (see verification checks for the three cases this applied to).

## Topological layer view

Rough layering by dependency depth (atoms at Layer 0 have no hard prereqs; atoms at Layer N have hard prereqs only at layers 0..N-1).

### Layer 0 — foundational (no hard prereqs, 37 atoms)

Brokerage (9): SC-B-004, SC-B-005, SC-B-009, SC-B-010, SC-B-013, SC-B-015, SC-B-016, SC-B-017, SC-B-018

Artifact (10): SC-A-002, SC-A-003, SC-A-004, SC-A-005, SC-A-006, SC-A-007, SC-A-009, SC-A-010, SC-A-011, SC-A-013

Practitioner (18): SC-P-001, SC-P-002, SC-P-003, SC-P-005, SC-P-007, SC-P-008, SC-P-010, SC-P-013, SC-P-014, SC-P-015, SC-P-017, SC-P-019, SC-P-025, SC-P-027, SC-P-030, SC-P-031, SC-P-032, SC-P-033

### Layer 1 — one hard prereq deep (13 atoms)

- SC-B-001 (requires: SC-B-010)
- SC-B-006 (requires: SC-B-005)
- SC-B-007 (requires: SC-B-013)
- SC-A-012 (requires: SC-A-013)
- SC-P-004 (requires: SC-P-019)
- SC-P-006 (requires: SC-A-011)
- SC-P-016 (requires: SC-P-002)
- SC-P-018 (requires: SC-B-013, SC-P-013)
- SC-P-023 (requires: SC-B-013)
- SC-P-026 (requires: SC-P-025)
- SC-P-028 (requires: SC-P-027)
- SC-P-029 (requires: SC-P-025)
- SC-P-034 (requires: SC-P-025)

### Layer 2 — two hard prereqs deep (5 atoms)

- SC-B-003 (requires: SC-B-001 [→ SC-B-010])
- SC-B-008 (requires: SC-B-006 [→ SC-B-005])
- SC-P-020 (requires: SC-P-004 [→ SC-P-019], SC-B-010)
- SC-P-021 (requires: SC-P-004 [→ SC-P-019])
- SC-A-008 (requires: SC-P-016 [→ SC-P-002], SC-A-012 [→ SC-A-013])

### Layer 3 — three hard prereqs deep (5 atoms)

- SC-B-002 (requires: SC-B-003)
- SC-P-011 (requires: SC-B-008)
- SC-P-024 (requires: SC-P-020)
- SC-A-014 (requires: SC-A-008)
- SC-P-012 (requires: SC-A-008)

### Layer 4 — four hard prereqs deep (4 atoms)

- SC-B-014 (requires: SC-B-002)
- SC-P-009 (requires: SC-P-024)
- SC-A-001 (requires: SC-A-014)
- SC-P-022 (requires: SC-P-012)

### Layer 5 — five hard prereqs deep (2 atoms)

- SC-B-011 (requires: SC-B-014)
- SC-A-015 (requires: SC-A-001)

### Layer 6 — six hard prereqs deep (1 atom)

- SC-B-012 (requires: SC-B-011)

## Full edge list

### Hard prereqs

| Target atom | Required prereq | Reason                                                                                                                                                                                            |
| ----------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SC-P-016    | SC-P-002        | Teaching sequence 1: detect-overcorrection protects the ablation methodology; per atom, "the upstream methodology this atom protects"                                                             |
| SC-A-008    | SC-P-016        | Teaching sequence 1: "the prerequisite check: classification built on contaminated ablation data will misclassify rules in both directions"                                                       |
| SC-P-012    | SC-A-008        | Teaching sequence 1: "the upstream classification that this atom revisits when ablation reveals partial redundancy"                                                                               |
| SC-P-022    | SC-P-012        | Teaching sequence 1 terminus: "the immediate predecessor: demote-or-strengthen judgments must be re-evaluated when a new model is added"                                                          |
| SC-B-001    | SC-B-010        | Teaching sequence 2: SC-B-010 produces the numbered drift-gap list SC-B-001 halts on                                                                                                              |
| SC-B-003    | SC-B-001        | Teaching sequence 2: per-file diff surfaces global-vs-variant questions that SC-B-003 resolves                                                                                                    |
| SC-B-002    | SC-B-003        | Teaching sequence 2: variant classification determines whether the upstream fix lands as global or variant                                                                                        |
| SC-B-014    | SC-B-002        | Teaching sequence 2: authority-chain escalation produces the decision reversal that supersession journaling records                                                                               |
| SC-B-011    | SC-B-014        | Teaching sequence 2: supersession journaling captures version-cascade decisions                                                                                                                   |
| SC-B-012    | SC-B-011        | Teaching sequence 2: version-pin consistency is one dimension manifest parity must check                                                                                                          |
| SC-B-006    | SC-B-005        | Teaching sequence 3: conformance audit is one angle within a multi-round red team                                                                                                                 |
| SC-B-008    | SC-B-006        | Teaching sequence 3: multi-round convergence is the structural mechanism; convergence-as-validation measures independent agreement within it                                                      |
| SC-P-011    | SC-B-008        | Teaching sequence 3: "SC-P-011 is the meta-move — should come after the practitioner has experienced the audit chain"                                                                             |
| SC-A-012    | SC-A-013        | Teaching sequence 4: Layer 1 Intent (routing) precedes Layer 2 Context (master directive)                                                                                                         |
| SC-A-008    | SC-A-012        | Teaching sequence 4: Layer 2 Context precedes Layer 3 Guardrails (rule classification)                                                                                                            |
| SC-A-014    | SC-A-008        | Teaching sequence 4: Layer 3 Guardrails precedes Layer 4 Instructions (milestone decomposition); classification structures the milestone decomposition move                                       |
| SC-A-001    | SC-A-014        | Teaching sequence 4: Layer 4 Instructions precedes Layer 5 Learning (codify)                                                                                                                      |
| SC-A-015    | SC-A-001        | Teaching sequence 4: "SC-A-015 comes last because it teaches the interaction between Context and Learning — which requires understanding both"                                                    |
| SC-P-004    | SC-P-019        | Teaching sequence 5: cold-start continuity fires at session open; disposition labelling fires at analysis start                                                                                   |
| SC-P-020    | SC-P-004        | Teaching sequence 5: disposition labelling at analysis start precedes second-occurrence recognition during implementation                                                                         |
| SC-P-024    | SC-P-020        | Teaching sequence 5: parallel principle — "the second occurrence of a drift class triggers structural intervention, just as the second consumer of a workaround triggers permanence intervention" |
| SC-P-009    | SC-P-024        | Teaching sequence 5: fallback permanence detection during implementation precedes architecture-doc-as-contract review at review gates                                                             |
| SC-B-007    | SC-B-013        | "the dependency map determines which worktrees can run concurrently; worktree isolation is the mechanism, dependency mapping is the governance"                                                   |
| SC-P-006    | SC-A-011        | "the framework-first check fires before the package boundary decision; if the functionality already exists in a framework, the boundary question is moot"                                         |
| SC-P-018    | SC-B-013        | "mapping dependencies is the prerequisite move that reveals which blocker to split"                                                                                                               |
| SC-P-018    | SC-P-013        | "the estimate contradiction that surfaces the blocker's true duration is what motivates the split; without catching the 2-3 vs 7 session disagreement, the split would never be proposed"         |
| SC-P-023    | SC-B-013        | "dependency mapping reveals which seams need gating; this atom teaches the structural intervention once the seams are identified"                                                                 |
| SC-P-020    | SC-B-010        | "the audit that surfaces the first and second occurrences; this atom teaches the practitioner what to do at the second occurrence"                                                                |
| SC-P-021    | SC-P-004        | "disposition labelling before implementation catches files that will be renamed or deleted, enabling proactive reference sweeps"                                                                  |
| SC-P-026    | SC-P-025        | Explicit arrow in SC-P-025 Related atoms: "the search produces citations; the audit verifies them against sources"                                                                                |
| SC-P-029    | SC-P-025        | Explicit arrow in SC-P-025 Related atoms: "the initial search establishes the base; the gap check maintains it"                                                                                   |
| SC-P-034    | SC-P-025        | "the search provides the evidence for the qualification"                                                                                                                                          |
| SC-P-028    | SC-P-027        | "the input — synthesis requires at least 3 hostile reviews to produce meaningful convergence"                                                                                                     |

**Total hard edges: 33**

### Soft prereqs (reinforces)

These edges come from `## Related atoms` notes where the relationship is complementary, parallel, sibling-pattern, or mutually reinforcing without a strict "X must precede Y" signal. A downstream course can freely sequence both sides of any soft edge.

| Atom     | Reinforced by | Note                                                                                                              |
| -------- | ------------- | ----------------------------------------------------------------------------------------------------------------- |
| SC-B-001 | SC-B-002      | What to do when the per-file diff reveals a drift the local fix cannot resolve                                    |
| SC-B-001 | SC-B-011      | Version-mismatch check that runs before per-file merge (classified soft to avoid cycle with teaching sequence 2)  |
| SC-B-001 | SC-B-012      | Automated pre-flight check (classified soft to avoid cycle with teaching sequence 2)                              |
| SC-B-001 | SC-B-013      | When sync writes span workspaces, dependency mapping prevents merge from breaking shared bottleneck files         |
| SC-B-001 | SC-B-014      | When a sync resolves a drift by superseding an earlier decision, the supersession is journaled at sync time       |
| SC-B-002 | SC-B-001      | Structural protection that catches the same class of error from the sync side                                     |
| SC-B-002 | SC-B-003      | The test that decides whether the upstream fix lands as global or as a variant                                    |
| SC-B-002 | SC-B-004      | Specialist delegation reduces these failures upstream                                                             |
| SC-B-002 | SC-B-009      | Cross-SDK upstream brokerage is authority-chain escalation across SDK boundaries                                  |
| SC-B-002 | SC-B-015      | When a rule's literal text blocks the escalation path, reinterpretation amends the rule                           |
| SC-B-002 | SC-B-017      | The zone assigned determines whether the upstream fix proposal requires held approval                             |
| SC-B-003 | SC-B-001      | Operational layer where classification halts a sync at numbering or content collisions                            |
| SC-B-003 | SC-B-002      | Takes a misclassification discovered downstream and pushes the fix to the authoring layer                         |
| SC-B-003 | SC-B-010      | Drift gaps surface when classified artifact disagrees with its global counterpart                                 |
| SC-B-003 | SC-B-016      | Variant classification is single-file; cross-language parity is cross-repo audit                                  |
| SC-B-003 | SC-P-002      | Ablation data reveals which rules are model-native (global) vs model-specific (variant)                           |
| SC-B-004 | SC-B-002      | What to do when the specialist finds the SDK source itself has a defect                                           |
| SC-B-004 | SC-B-009      | When specialist review reveals a cross-SDK defect, upstream brokerage is the escalation path                      |
| SC-B-004 | SC-B-017      | Delegating to a specialist with a narrow Role Envelope may warrant a wider zone                                   |
| SC-B-004 | SC-A-001      | The codify move that captures the corrections list as institutional knowledge                                     |
| SC-B-004 | SC-A-013      | Artifact-authoring counterpart: routing rules determine which specialist handles which framework                  |
| SC-B-005 | SC-B-001      | Sync operations can propagate a conformant implementation from one repo to a non-conformant one                   |
| SC-B-005 | SC-B-003      | Conformance audits surface global-vs-variant questions                                                            |
| SC-B-005 | SC-B-009      | Cross-SDK conformance gaps should be filed upstream                                                               |
| SC-B-005 | SC-B-016      | Parity audits complement conformance audits                                                                       |
| SC-B-005 | SC-P-014      | Conformance gaps are failures (MUST violated) not gaps (MUST not attempted)                                       |
| SC-B-006 | SC-B-005      | A conformance audit can be one angle within a multi-round red team (also hard via teaching sequence 3)            |
| SC-B-006 | SC-B-008      | Orthogonal: multi-round measures finding decrease; convergence-as-validation measures independent agreement       |
| SC-B-006 | SC-B-014      | Red team rounds are the primary source of decision reversals requiring supersession                               |
| SC-B-006 | SC-P-015      | Ending red team prompts with "what would invalidate this?" produces adversarial rather than confirmatory findings |
| SC-B-007 | SC-B-004      | Specialists often run in parallel, making worktree isolation a prerequisite for specialist delegation at scale    |
| SC-B-007 | SC-B-006      | Red team rounds can be parallelized across angles in separate worktrees                                           |
| SC-B-007 | SC-B-017      | Worktree isolation is mechanical prerequisite; zone assignment is governance prerequisite                         |
| SC-B-008 | SC-B-005      | Conformance audit is one specialization that can be combined with a security audit for convergence                |
| SC-B-008 | SC-B-006      | Orthogonal measures (also hard via teaching sequence 3)                                                           |
| SC-B-008 | SC-B-007      | Independent red team agents need worktree isolation                                                               |
| SC-B-008 | SC-P-003      | Cross-feature interactions are a common source of convergent findings                                             |
| SC-B-009 | SC-B-002      | Upstream brokerage is a form of authority-chain escalation                                                        |
| SC-B-009 | SC-B-004      | Specialists are the first to discover cross-SDK defects                                                           |
| SC-B-009 | SC-B-005      | Cross-SDK upstream issues often surface during conformance audits                                                 |
| SC-B-009 | SC-B-016      | Parity audits reveal which SDK's implementation should be the upstream reference                                  |
| SC-B-009 | SC-B-015      | When a rule in the consumer repo blocks the upstream-first approach, reinterpretation amends the rule             |
| SC-B-010 | SC-B-001      | The sync mechanism that the drift audit validates                                                                 |
| SC-B-010 | SC-B-003      | The classification step that the drift-gap list depends on for each file                                          |
| SC-B-010 | SC-B-011      | Version drift is a special case of cross-repo divergence                                                          |
| SC-B-010 | SC-B-012      | Automated check that operationalises the numbered drift-gap list                                                  |
| SC-B-010 | SC-B-014      | When drift audit reveals a prior sync decision was wrong, correction is journaled with supersession               |
| SC-B-010 | SC-P-020      | Audit surfaces drift occurrences; at the second occurrence the practitioner escalates to structural intervention  |
| SC-B-011 | SC-B-001      | Sync mechanism that version check gates                                                                           |
| SC-B-011 | SC-B-003      | Version mismatches can cause rule misclassification                                                               |
| SC-B-011 | SC-B-010      | Version drift is a special case of cross-repo divergence                                                          |
| SC-B-011 | SC-B-012      | Manifest parity checks should include version-pin consistency                                                     |
| SC-B-012 | SC-B-001      | Sync command that the parity check gates                                                                          |
| SC-B-012 | SC-B-003      | Orphan variants are often misclassified files                                                                     |
| SC-B-012 | SC-B-010      | Audit that parity check operationalises at the manifest level                                                     |
| SC-B-012 | SC-B-011      | Manifest parity should verify version pins in distributed files                                                   |
| SC-B-013 | SC-B-001      | When dependency mapping spans repos, sync mechanism reconciles shared artifacts                                   |
| SC-B-013 | SC-B-006      | Validation that catches integration issues the dependency map missed                                              |
| SC-B-013 | SC-B-007      | Mechanism that enables safe parallel execution once dependency map confirms it is safe (reverse of hard edge)     |
| SC-B-013 | SC-B-010      | Post-parallel drift audit validates the map's completeness                                                        |
| SC-B-013 | SC-P-018      | Splitting a multi-session blocker requires dependency mapping (also hard)                                         |
| SC-B-014 | SC-B-001      | When a sync resolves a drift by superseding, the supersession is journaled at sync time                           |
| SC-B-014 | SC-B-005      | Conformance audits may trigger reversals that need supersession journaling                                        |
| SC-B-014 | SC-B-006      | Red team rounds are the primary source of decision reversals                                                      |
| SC-B-014 | SC-B-010      | Drift audits surface discrepancies that trace back to decisions that should have been superseded                  |
| SC-B-014 | SC-B-015      | Rule reinterpretation is a form of decision reversal requiring the same supersession discipline                   |
| SC-B-015 | SC-B-002      | If the reinterpretation is contested, escalation determines who has authority to amend                            |
| SC-B-015 | SC-B-003      | The specific Gate 1 task that was the subject of the canonical reinterpretation                                   |
| SC-B-015 | SC-B-014      | A rule reinterpretation is a decision reversal                                                                    |
| SC-B-015 | SC-B-017      | Same Gate 1 delegation event from two angles: rule amendment and zone assignment                                  |
| SC-B-015 | SC-B-009      | Rules that block upstream-first approaches are prime candidates for reinterpretation                              |
| SC-B-016 | SC-B-003      | Parity audits surface global-vs-variant questions                                                                 |
| SC-B-016 | SC-B-005      | The spec is the shared reference both SDKs implement                                                              |
| SC-B-016 | SC-B-009      | Governance mechanism for deciding which SDK becomes the upstream reference                                        |
| SC-B-016 | SC-B-014      | When parity audit reverses assumed reference direction, prior assumption is superseded                            |
| SC-B-017 | SC-B-002      | Zone assigned determines whether upstream fix requires held approval                                              |
| SC-B-017 | SC-B-004      | Specialist with narrow Role Envelope may warrant a wider zone                                                     |
| SC-B-017 | SC-B-007      | Worktree isolation is mechanical; zone assignment is governance                                                   |
| SC-B-017 | SC-B-015      | Same Gate 1 delegation event from two angles                                                                      |
| SC-B-017 | SC-B-018      | Zone assignment determines PDP policy; PEP must match zone's strictness                                           |
| SC-B-017 | SC-A-013      | Routing determines which specialist handles work; zone assignment determines trust level                          |
| SC-B-018 | SC-B-005      | Conformance audit would catch single-gate bypass; PDP/PEP separation is one specific check                        |
| SC-B-018 | SC-B-017      | Zone assignment determines PDP policy; PEP must match                                                             |
| SC-B-018 | SC-A-006      | PEP's deny path must be tested with negative tests                                                                |
| SC-B-018 | SC-A-008      | Whether a rule is critical or advisory determines whether PEP must be strict or shadow                            |
| SC-B-018 | SC-P-010      | Timing attacks exploit the gap between PDP decision and PEP enforcement                                           |
| SC-A-001 | SC-B-002      | Upstream side: when codified item is in the wrong repo, NOT-codified section tells you                            |
| SC-A-001 | SC-B-004      | Generates corrections list that often has both codify-worthy patterns and one-off bugs                            |
| SC-A-001 | SC-A-003      | Complementary: defense-in-depth is about breadth; NOT-codified is about precision of exclusion                    |
| SC-A-001 | SC-P-017      | The explicit gap "/codify never reads from the learning system" is the kind of NOT-codified fact                  |
| SC-A-002 | SC-A-001      | What to record after the fix lands; a fix that crosses six artifacts produces six codify candidates               |
| SC-A-002 | SC-A-003      | The fix landed across six artifacts; this is defense-in-depth codification applied to a detection gap             |
| SC-A-002 | SC-A-006      | Once the hook blocks mock data, negative tests must verify the deny path                                          |
| SC-A-002 | SC-B-002      | Relevant when the fix must land in loom/scripts/hooks/ before sync                                                |
| SC-A-003 | SC-A-001      | Complementary: breadth of landing vs precision of exclusion                                                       |
| SC-A-003 | SC-A-004      | Hooks are one enforcement surface; this ensures the hook is present, SC-A-004 ensures hook is secure              |
| SC-A-003 | SC-A-005      | Encapsulation is a struct-level enforcement surface that complements rule-level and hook-level                    |
| SC-A-003 | SC-A-006      | Each enforcement surface needs negative testing                                                                   |
| SC-A-003 | SC-A-007      | Zero-config paths are the enforcement surface most likely missing from the stack                                  |
| SC-A-003 | SC-B-002      | When multi-artifact landing crosses repo boundaries, authority-chain escalation determines ownership              |
| SC-A-004 | SC-A-003      | Defense-in-depth requires multiple enforcement surfaces; this atom audits each surface                            |
| SC-A-004 | SC-A-005      | Hooks manipulating signed structs must respect encapsulation boundaries                                           |
| SC-A-004 | SC-A-006      | Both address enforcement mechanisms assumed to work but never tested for failure cases                            |
| SC-A-004 | SC-A-007      | Hooks invoked during zero-config startup inherit permissive defaults                                              |
| SC-A-004 | SC-P-010      | Timing side-channels in hooks are a security concern the hook audit should cover                                  |
| SC-A-005 | SC-A-003      | Encapsulation boundary is one enforcement surface in the defense-in-depth stack                                   |
| SC-A-005 | SC-A-004      | Hooks manipulating signed structs must respect encapsulation                                                      |
| SC-A-005 | SC-A-006      | Both address default-open vs default-closed semantics                                                             |
| SC-A-005 | SC-A-007      | Zero-config constructors defaulting signed-struct fields to permissive values                                     |
| SC-A-005 | SC-P-010      | Timing attacks on signed structs are a cross-cutting security concern                                             |
| SC-A-006 | SC-A-003      | Each enforcement surface needs negative testing                                                                   |
| SC-A-006 | SC-A-004      | Hooks are enforcement mechanisms that assume correct behavior                                                     |
| SC-A-006 | SC-A-005      | Struct-level default-private vs middleware-level default-deny                                                     |
| SC-A-006 | SC-A-007      | Once zero-config default is changed to deny, negative tests verify the deny posture holds                         |
| SC-A-006 | SC-P-010      | Enforcement mechanisms with deny semantics can be bypassed via timing                                             |
| SC-A-007 | SC-A-003      | Zero-config paths are likely missing from the defense-in-depth stack                                              |
| SC-A-007 | SC-A-004      | Hooks invoked during zero-config startup inherit permissive defaults                                              |
| SC-A-007 | SC-A-005      | Zero-config constructors defaulting signed-struct fields to permissive values                                     |
| SC-A-007 | SC-A-006      | Testing counterpart                                                                                               |
| SC-A-007 | SC-P-010      | Zero-config paths skipping explicit auth may also skip constant-time comparison                                   |
| SC-A-008 | SC-A-003      | Critical rules require multiple enforcement mechanisms (next structural move after classification)                |
| SC-A-008 | SC-A-009      | Classification determines which level a rule lands at in the 4-level hierarchy                                    |
| SC-A-008 | SC-A-010      | Token budget pressure is the trigger for classification work                                                      |
| SC-A-008 | SC-B-018      | For critical rules, PDP/PEP separation is the prerequisite for actionable strict-vs-shadow enforcement            |
| SC-A-009 | SC-A-001      | Codify decision of what enters institutional knowledge is upstream of where it lands                              |
| SC-A-009 | SC-A-008      | Classification determines which level a rule belongs at                                                           |
| SC-A-009 | SC-A-010      | 4-level hierarchy is external side; baked rules sit below as compiled infrastructure                              |
| SC-A-009 | SC-A-015      | 4-level hierarchy is vertical structure; SC-A-015 covers horizontal classification at each level                  |
| SC-A-009 | SC-P-002      | Ablation experiment produces data that drives KEEP/COMPRESS/REMOVE/DEMOTE classification                          |
| SC-A-009 | SC-P-011      | Audit method must understand hierarchy's discovery mechanisms                                                     |
| SC-A-010 | SC-A-008      | Critical/advisory classification is complementary                                                                 |
| SC-A-010 | SC-A-009      | 4-level hierarchy is external side of boundary; baked is below as compiled infrastructure                         |
| SC-A-010 | SC-A-012      | Master directive is primary baked-in artifact; its size affects token budget                                      |
| SC-A-010 | SC-P-022      | Multi-model envelopes drive the variant architecture decision                                                     |
| SC-A-011 | SC-P-006      | Three-question test for module-vs-package is downstream of framework-first check (also hard edge)                 |
| SC-A-011 | SC-A-009      | Existing primitives must be placed at correct level in disclosure hierarchy                                       |
| SC-A-011 | SC-B-004      | Specialist is the agent most likely to know whether framework provides primitive                                  |
| SC-A-011 | SC-B-010      | Framework-first applies across repos; divergence audit may reveal primitive exists in sibling repo                |
| SC-A-012 | SC-A-009      | CLAUDE.md is level 1 of the 4-level hierarchy                                                                     |
| SC-A-012 | SC-A-010      | Master directive is primary baked-in artifact                                                                     |
| SC-A-012 | SC-A-013      | Master directive indexes agents; routing artifact is the detailed map (also hard via seq 4)                       |
| SC-A-012 | SC-A-015      | Master directive indexes corpus; SC-A-015 covers how to structure the corpus                                      |
| SC-A-012 | SC-P-019      | Master directive is one injection point for session-continuity information                                        |
| SC-A-013 | SC-B-004      | Delegation is runtime execution of routing; this atom covers authoring                                            |
| SC-A-013 | SC-B-017      | Routing determines which specialist handles work; zone assignment determines trust level                          |
| SC-A-013 | SC-A-009      | Agent descriptions are index-level artifacts; routing rules determine which context gets loaded                   |
| SC-A-013 | SC-A-012      | Master directive indexes agents; routing artifact is the detailed map                                             |
| SC-A-014 | SC-P-007      | Completion state defines "done"; milestone decomposition defines gate checkpoints                                 |
| SC-A-014 | SC-B-006      | Convergence gate at end of milestone requires numeric convergence threshold                                       |
| SC-A-014 | SC-B-013      | When milestones span workspaces, dependency mapping feeds milestone decomposition                                 |
| SC-A-014 | SC-P-018      | Splitting blocker restructures milestone graph                                                                    |
| SC-A-015 | SC-A-009      | Vertical structure vs horizontal classification                                                                   |
| SC-A-015 | SC-A-012      | Master directive indexes corpus                                                                                   |
| SC-A-015 | SC-A-001      | Codify decision determines what enters the corpus                                                                 |
| SC-A-015 | SC-A-008      | Sub-classification within the rule artifact type                                                                  |
| SC-P-001 | SC-A-001      | Codify-phase move that would have caught the gap                                                                  |
| SC-P-001 | SC-B-001      | Sync-level manifestation: two artifacts that should compose but are maintained independently                      |
| SC-P-001 | SC-B-005      | Conformance audit catches adjacent-but-disconnected modules                                                       |
| SC-P-001 | SC-P-003      | Intra-module version: features within a module that should constrain each other                                   |
| SC-P-002 | SC-P-016      | Immediate next move: ablation results are unreliable until contamination is distinguished (hard via seq 1)        |
| SC-P-002 | SC-A-008      | Artifact-side classification this atom's data feeds (hard via seq 1+4 chain)                                      |
| SC-P-002 | SC-P-012      | Post-ablation decision atom                                                                                       |
| SC-P-002 | SC-P-022      | Multi-model extension                                                                                             |
| SC-P-002 | SC-A-001      | Ablation data feeds codify decision about rules                                                                   |
| SC-P-002 | SC-B-003      | Ablation informs variant classification                                                                           |
| SC-P-003 | SC-P-001      | Module-level manifestation of the same pattern                                                                    |
| SC-P-003 | SC-A-002      | Case where per-feature validation misses cross-boundary issues                                                    |
| SC-P-003 | SC-B-008      | Cross-feature interactions are a common source of convergent findings                                             |
| SC-P-003 | SC-P-010      | Timing failures are a specific class of cross-feature interaction                                                 |
| SC-P-004 | SC-P-001      | Disposition labelling surfaces integration gaps between modules                                                   |
| SC-P-004 | SC-P-005      | Disposition audit can reveal friction-gradient inversions                                                         |
| SC-P-004 | SC-P-009      | File-level ground truth that architecture documents must be tested against                                        |
| SC-P-004 | SC-P-021      | Files labelled for rename/deletion need proactive reference sweeps (also hard edge)                               |
| SC-P-005 | SC-P-004      | Disposition labelling can surface friction-gradient inversions                                                    |
| SC-P-005 | SC-A-001      | Codify move that would catch "we did NOT update the COC to deprecate the broken API"                              |
| SC-P-005 | SC-A-009      | Progressive disclosure hierarchy should route users to the best API first                                         |
| SC-P-005 | SC-P-001      | Friction-gradient inversions often live at module boundaries                                                      |
| SC-P-006 | SC-A-003      | Package boundary decisions affect where guardrails are codified                                                   |
| SC-P-006 | SC-A-011      | Framework-first check fires before package boundary decision (also hard edge)                                     |
| SC-P-006 | SC-P-001      | Wrong package boundary decision can create adjacent-but-disconnected modules                                      |
| SC-P-006 | SC-P-008      | Package boundaries determine which dependency chains are exposed                                                  |
| SC-P-007 | SC-P-004      | Disposition labelling is a specific instance of completion-state definition                                       |
| SC-P-007 | SC-A-014      | Milestones with gates are the structural counterpart                                                              |
| SC-P-007 | SC-B-006      | Red team convergence is a completion claim                                                                        |
| SC-P-007 | SC-P-019      | Completion-state definition is precondition for useful session notes                                              |
| SC-P-008 | SC-P-006      | Package boundaries determine which dependency chains are exposed                                                  |
| SC-P-008 | SC-A-006      | Dependency fragility tests are a form of negative testing                                                         |
| SC-P-008 | SC-B-011      | Version cascade must track SDK dependency pins                                                                    |
| SC-P-008 | SC-P-017      | Broken learning pipeline is a dependency chain                                                                    |
| SC-P-009 | SC-P-004      | Disposition labelling is the file-level version                                                                   |
| SC-P-009 | SC-P-007      | Completion-state built on unverified architecture doc inherits errors                                             |
| SC-P-009 | SC-P-013      | Architecture docs with inconsistent claims are a source of estimate contradictions                                |
| SC-P-009 | SC-B-016      | Nexus-parity audit found claimed-missing capabilities already existed                                             |
| SC-P-010 | SC-A-003      | Timing-sensitive enforcement requires multiple independent verification surfaces                                  |
| SC-P-010 | SC-A-004      | Timing side-channels in auth are security concerns for hooks                                                      |
| SC-P-010 | SC-A-005      | Timing attacks on signed structs are cross-cutting                                                                |
| SC-P-010 | SC-A-006      | Enforcement mechanisms with deny semantics can be bypassed via timing                                             |
| SC-P-010 | SC-A-007      | Zero-config paths skipping explicit auth may skip constant-time comparison                                        |
| SC-P-010 | SC-B-006      | Timing bugs often require multiple red team rounds                                                                |
| SC-P-010 | SC-B-018      | Timing attacks exploit the PDP/PEP gap                                                                            |
| SC-P-010 | SC-P-003      | Timing failures emerge from interaction of two correct subsystems                                                 |
| SC-P-011 | SC-P-002      | Another case where measurement method must match the thing being measured                                         |
| SC-P-011 | SC-B-006      | Retraction in 0011 is a multi-round red team correcting prior round (also hard via seq 3)                         |
| SC-P-011 | SC-A-009      | Audit method must understand hierarchy's discovery mechanisms                                                     |
| SC-P-011 | SC-P-016      | Structurally blind audit can drive overcorrection                                                                 |
| SC-P-012 | SC-P-002      | Provides methodology that produces data (transitively via seq 1)                                                  |
| SC-P-012 | SC-P-016      | Guards against acting on contaminated ablation data                                                               |
| SC-P-012 | SC-A-008      | Upstream classification this atom revisits (also hard via seq 1)                                                  |
| SC-P-012 | SC-P-022      | Cluster terminus: never demote based on strongest model alone                                                     |
| SC-P-012 | SC-A-001      | Demotion rationale should be codified in NOT-codified section                                                     |
| SC-P-012 | SC-A-003      | For rules classified as "strengthen," defense-in-depth is the implementation mechanism                            |
| SC-P-012 | SC-A-015      | Demotion is a relocation from rule to skill or command artifact type                                              |
| SC-P-013 | SC-B-006      | Red team that caught the estimate inconsistency was in a multi-round convergence loop                             |
| SC-P-013 | SC-P-004      | Both require examining each item individually rather than accepting batch characterization                        |
| SC-P-013 | SC-P-009      | Architecture docs with inconsistent claims propagate into plans                                                   |
| SC-P-013 | SC-P-015      | Falsification question is the prompt-level move that surfaces estimate contradictions                             |
| SC-P-014 | SC-P-004      | Failure/gap classification is a per-item disposition label                                                        |
| SC-P-014 | SC-B-005      | Conformance audit may surface both failures and gaps                                                              |
| SC-P-014 | SC-B-009      | Zero-tolerance Rule 4 requires filing upstream                                                                    |
| SC-P-014 | SC-P-024      | Temporary workaround accumulating consumers becomes zero-tolerance violation                                      |
| SC-P-015 | SC-P-013      | Falsification question is what surfaced the estimate contradiction                                                |
| SC-P-015 | SC-P-009      | Applied to architecture documents, surfaces hidden-assumption errors                                              |
| SC-P-015 | SC-B-006      | Red team rounds are structural mechanism; this atom teaches prompt-level skill                                    |
| SC-P-015 | SC-B-008      | Falsification from independent perspectives increases convergence signal                                          |
| SC-P-016 | SC-P-002      | Upstream methodology this atom protects (also hard via seq 1)                                                     |
| SC-P-016 | SC-A-008      | Next step in cluster: only after contamination ruled out (also hard via seq 1)                                    |
| SC-P-016 | SC-P-012      | Demotion decisions depend on accurate baseline                                                                    |
| SC-P-016 | SC-P-022      | Cluster terminus                                                                                                  |
| SC-P-016 | SC-P-011      | Sibling pattern: measurement method must match thing measured                                                     |
| SC-P-017 | SC-P-008      | Broken learning pipeline is a dependency chain                                                                    |
| SC-P-017 | SC-A-001      | Explicit gap: "/codify never reads from the learning system" is a NOT-codified fact                               |
| SC-P-017 | SC-A-003      | Learning pipeline with one processing stage is single point of failure                                            |
| SC-P-017 | SC-P-011      | Both teach distrust of structurally plausible metrics                                                             |
| SC-P-018 | SC-P-013      | Estimate contradiction motivates the split (also hard)                                                            |
| SC-P-018 | SC-A-014      | Splitting restructures milestone graph                                                                            |
| SC-P-018 | SC-B-013      | Dependency mapping reveals which blocker to split (also hard)                                                     |
| SC-P-018 | SC-P-023      | Integration gate at the seam validates the two compose correctly                                                  |
| SC-P-018 | SC-P-024      | Fast-unblock half becoming permanent workaround                                                                   |
| SC-P-019 | SC-P-007      | Completion-state definition is precondition for useful session notes                                              |
| SC-P-019 | SC-A-003      | Session-notes injection should be defense-in-depth                                                                |
| SC-P-019 | SC-A-004      | Session-start hooks must be audited for security vulnerabilities                                                  |
| SC-P-019 | SC-A-012      | Master directive is one injection point for session-continuity                                                    |
| SC-P-020 | SC-B-010      | Audit that surfaces first and second occurrences (also hard)                                                      |
| SC-P-020 | SC-B-012      | Mechanism-level fix that should have triggered                                                                    |
| SC-P-020 | SC-P-021      | Specific mechanism-level fix for one common drift class                                                           |
| SC-P-020 | SC-P-024      | Parallel principle: second occurrence triggers structural intervention                                            |
| SC-P-021 | SC-P-020      | Dangling references after rename are a drift class that recurs                                                    |
| SC-P-021 | SC-B-002      | When rename is in upstream repo, sweep must be escalated                                                          |
| SC-P-021 | SC-B-010      | Cross-repo version of the same problem                                                                            |
| SC-P-021 | SC-P-004      | Disposition labelling enables proactive reference sweeps (also hard)                                              |
| SC-P-022 | SC-P-002      | Empirical methodology for per-model gap profiles (transitively via seq 1)                                         |
| SC-P-022 | SC-P-016      | Overcorrection contamination can mask gap profile                                                                 |
| SC-P-022 | SC-P-012      | Immediate predecessor (also hard via seq 1)                                                                       |
| SC-P-022 | SC-A-008      | Classification is model-dependent                                                                                 |
| SC-P-022 | SC-A-010      | Multi-model envelopes drive variant architecture                                                                  |
| SC-P-022 | SC-B-003      | Variant placement considers model gaps                                                                            |
| SC-P-023 | SC-P-018      | Integration gate is structural complement to splitting                                                            |
| SC-P-023 | SC-A-014      | Gating workspaces are milestones in decomposition graph                                                           |
| SC-P-023 | SC-B-013      | Dependency mapping reveals which seams need gating (also hard)                                                    |
| SC-P-023 | SC-P-013      | Same journal entry's Finding 1 shows estimate contradictions                                                      |
| SC-P-024 | SC-P-018      | B0a/B0b split prevents fallback from activating                                                                   |
| SC-P-024 | SC-P-020      | Parallel principle applied to drift classes (also hard via seq 5)                                                 |
| SC-P-024 | SC-P-014      | Zero-tolerance applies to workaround consumers                                                                    |
| SC-P-024 | SC-B-014      | Workaround promoted to permanent requires supersession journaling                                                 |
| SC-P-025 | SC-P-026      | Search produces citations; audit verifies them (reverse of hard edge)                                             |
| SC-P-025 | SC-P-029      | Initial search vs maintenance (reverse of hard edge)                                                              |
| SC-P-025 | SC-B-006      | Literature search convergence                                                                                     |
| SC-P-026 | SC-P-025      | Search finds papers; audit verifies them (also hard)                                                              |
| SC-P-026 | SC-B-005      | Codegen analogue: audit code against spec                                                                         |
| SC-P-026 | SC-P-015      | "What would invalidate this citation?"                                                                            |
| SC-P-027 | SC-B-006      | Run multiple hostile reviewers with different personas                                                            |
| SC-P-027 | SC-B-008      | Two independent hostile reviewers finding same flaw is stronger                                                   |
| SC-P-027 | SC-P-015      | Hostile review prompt should end with "what would make you reject this?"                                          |
| SC-P-027 | SC-P-033      | Hostile review is how reflexivity problems are discovered                                                         |
| SC-P-028 | SC-P-027      | The input (also hard)                                                                                             |
| SC-P-028 | SC-B-008      | General principle; this is research instantiation                                                                 |
| SC-P-028 | SC-P-013      | Minimal model must be internally consistent                                                                       |
| SC-P-029 | SC-P-025      | Initial search; this atom maintains it (also hard)                                                                |
| SC-P-029 | SC-P-026      | Gap check finds papers; citation audit verifies them                                                              |
| SC-P-029 | SC-P-034      | Gap check discovers that "first to formalize" claim is no longer true                                             |
| SC-P-030 | SC-P-032      | Register decisions documented in margin notes                                                                     |
| SC-P-030 | SC-P-034      | Register includes vocabulary precision                                                                            |
| SC-P-031 | SC-P-007      | Venue defines paper's completion state                                                                            |
| SC-P-031 | SC-P-027      | Reviewer persona should match target venue                                                                        |
| SC-P-031 | SC-P-030      | Register must match venue                                                                                         |
| SC-P-032 | SC-A-001      | Margin notes are a codify move                                                                                    |
| SC-P-032 | SC-P-030      | Register decisions documented as margin notes                                                                     |
| SC-P-032 | SC-P-027      | Defense notes from hostile review feed margin notes                                                               |
| SC-P-033 | SC-P-027      | Reflexivity is typically discovered during hostile review                                                         |
| SC-P-033 | SC-P-015      | Reflexivity-specific falsification question                                                                       |
| SC-P-033 | SC-P-034      | Reflexive research must be careful about novelty claims                                                           |
| SC-P-034 | SC-P-025      | Search provides evidence for qualification (also hard)                                                            |
| SC-P-034 | SC-P-029      | Gap check maintains temporal validity                                                                             |
| SC-P-034 | SC-P-033      | Self-referential research must be careful about novelty claims                                                    |
| SC-P-034 | SC-P-026      | Overclaim detection is specific type of citation integrity                                                        |

**Total soft edges: approximately 190**

## Per-atom view

Each entry lists the atom's hard and soft prereqs (what the atom requires or is reinforced by). For atoms with no hard prereqs, only the soft list is shown.

### Brokerage atoms (18)

#### SC-B-001 — Read-then-merge sync semantics

**Hard prereqs:** SC-B-010
**Soft prereqs:** SC-B-002, SC-B-003, SC-B-011, SC-B-012, SC-B-013, SC-B-014

#### SC-B-002 — Authority-chain escalation

**Hard prereqs:** SC-B-003
**Soft prereqs:** SC-B-001, SC-B-004, SC-B-009, SC-B-015, SC-B-017

#### SC-B-003 — Variant classification

**Hard prereqs:** SC-B-001
**Soft prereqs:** SC-B-002, SC-B-010, SC-B-016, SC-P-002

#### SC-B-004 — Specialist delegation as required first move

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-002, SC-B-009, SC-B-017, SC-A-001, SC-A-013

#### SC-B-005 — Spec-to-code conformance audit

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-001, SC-B-003, SC-B-009, SC-B-016, SC-P-014

#### SC-B-006 — Multi-round red team to numeric convergence

**Hard prereqs:** SC-B-005
**Soft prereqs:** SC-B-008, SC-B-014, SC-P-015

#### SC-B-007 — Worktree-per-agent

**Hard prereqs:** SC-B-013
**Soft prereqs:** SC-B-004, SC-B-006, SC-B-017

#### SC-B-008 — Convergence-as-validation across independent reds

**Hard prereqs:** SC-B-006
**Soft prereqs:** SC-B-005, SC-B-007, SC-P-003

#### SC-B-009 — Cross-SDK upstream brokerage

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-002, SC-B-004, SC-B-005, SC-B-015, SC-B-016

#### SC-B-010 — Cross-repo divergence audit with numbered drift-gap list

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-001, SC-B-003, SC-B-011, SC-B-012, SC-B-014, SC-P-020

#### SC-B-011 — Version tracking cascade

**Hard prereqs:** SC-B-014
**Soft prereqs:** SC-B-001, SC-B-003, SC-B-010, SC-B-012

#### SC-B-012 — Manifest parity automation

**Hard prereqs:** SC-B-011
**Soft prereqs:** SC-B-001, SC-B-003, SC-B-010, SC-B-011

#### SC-B-013 — Cross-workspace dependency mapping

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-001, SC-B-006, SC-B-007, SC-B-010, SC-P-018

#### SC-B-014 — Explicit supersession journaling

**Hard prereqs:** SC-B-002
**Soft prereqs:** SC-B-001, SC-B-005, SC-B-006, SC-B-010, SC-B-015

#### SC-B-015 — MUST rule semantic reinterpretation

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-002, SC-B-003, SC-B-009, SC-B-014, SC-B-017

#### SC-B-016 — Bidirectional cross-language parity

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-003, SC-B-005, SC-B-009, SC-B-014

#### SC-B-017 — Verification-gradient zone assignment

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-002, SC-B-004, SC-B-007, SC-B-015, SC-B-018, SC-A-013

#### SC-B-018 — PDP/PEP separation

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-005, SC-B-017, SC-A-006, SC-A-008, SC-P-010

### Artifact atoms (15)

#### SC-A-001 — Codify-with-explicit-NOT-codified

**Hard prereqs:** SC-A-014
**Soft prereqs:** SC-B-002, SC-B-004, SC-A-003, SC-P-017

#### SC-A-002 — Frontend mock data detection

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-001, SC-A-003, SC-A-006, SC-B-002

#### SC-A-003 — Defense-in-depth codification

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-001, SC-A-004, SC-A-005, SC-A-006, SC-A-007, SC-B-002

#### SC-A-004 — Layer 3 hook security audit

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-003, SC-A-005, SC-A-006, SC-A-007, SC-P-010

#### SC-A-005 — Encapsulation as security boundary

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-003, SC-A-004, SC-A-006, SC-A-007, SC-P-010

#### SC-A-006 — Negative tests for deny-by-default

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-003, SC-A-004, SC-A-005, SC-A-007, SC-P-010

#### SC-A-007 — Zero-config security audit

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-003, SC-A-004, SC-A-005, SC-A-006, SC-P-010

#### SC-A-008 — Rule classification critical vs advisory

**Hard prereqs:** SC-P-016, SC-A-012
**Soft prereqs:** SC-A-003, SC-A-009, SC-A-010, SC-B-018

#### SC-A-009 — Progressive disclosure architecture

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-001, SC-A-008, SC-A-010, SC-A-015, SC-P-002, SC-P-011

#### SC-A-010 — Token budget baked vs external

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-008, SC-A-009, SC-A-012, SC-P-022

#### SC-A-011 — Framework-first check

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-009, SC-B-004, SC-B-010, SC-P-006

#### SC-A-012 — CLAUDE.md as Layer 2 context

**Hard prereqs:** SC-A-013
**Soft prereqs:** SC-A-009, SC-A-010, SC-A-015, SC-P-019

#### SC-A-013 — Agent specialisation routing

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-004, SC-B-017, SC-A-009, SC-A-012

#### SC-A-014 — Milestone decomposition

**Hard prereqs:** SC-A-008
**Soft prereqs:** SC-P-007, SC-B-006, SC-B-013, SC-P-018

#### SC-A-015 — Layer 2 context structuring

**Hard prereqs:** SC-A-001
**Soft prereqs:** SC-A-009, SC-A-012, SC-A-008

### Practitioner atoms (34)

#### SC-P-001 — Adjacent-disconnected modules

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-001, SC-B-001, SC-B-005, SC-P-003

#### SC-P-002 — Model-vs-rule ablation

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-016, SC-A-008, SC-P-012, SC-P-022, SC-A-001, SC-B-003

#### SC-P-003 — Cross-feature interaction red team

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-001, SC-A-002, SC-B-008, SC-P-010

#### SC-P-004 — Per-file disposition labelling

**Hard prereqs:** SC-P-019
**Soft prereqs:** SC-P-001, SC-P-005, SC-P-009, SC-P-021

#### SC-P-005 — Friction-gradient inversion

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-004, SC-A-001, SC-A-009, SC-P-001

#### SC-P-006 — Package boundary decision

**Hard prereqs:** SC-A-011
**Soft prereqs:** SC-A-003, SC-P-001, SC-P-008

#### SC-P-007 — Session completion state

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-004, SC-A-014, SC-B-006, SC-P-019

#### SC-P-008 — Dependency chain fragility

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-006, SC-A-006, SC-B-011, SC-P-017

#### SC-P-009 — Architecture doc as contract

**Hard prereqs:** SC-P-024
**Soft prereqs:** SC-P-004, SC-P-007, SC-P-013, SC-B-016

#### SC-P-010 — Timing race condition red team

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-003, SC-A-004, SC-A-005, SC-A-006, SC-A-007, SC-B-006, SC-B-018, SC-P-003

#### SC-P-011 — Audit categorization progressive disclosure

**Hard prereqs:** SC-B-008
**Soft prereqs:** SC-P-002, SC-B-006, SC-A-009, SC-P-016

#### SC-P-012 — Rule demotion judgment

**Hard prereqs:** SC-A-008
**Soft prereqs:** SC-P-002, SC-P-016, SC-P-022, SC-A-001, SC-A-003, SC-A-015

#### SC-P-013 — Estimate self-contradiction signal

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-006, SC-P-004, SC-P-009, SC-P-015

#### SC-P-014 — Zero-tolerance scope discipline

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-004, SC-B-005, SC-B-009, SC-P-024

#### SC-P-015 — Ask for contradictions

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-013, SC-P-009, SC-B-006, SC-B-008

#### SC-P-016 — Detect overcorrection

**Hard prereqs:** SC-P-002
**Soft prereqs:** SC-A-008, SC-P-012, SC-P-022, SC-P-011

#### SC-P-017 — False-positive learning detection

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-008, SC-A-001, SC-A-003, SC-P-011

#### SC-P-018 — Split the long pole

**Hard prereqs:** SC-B-013, SC-P-013
**Soft prereqs:** SC-A-014, SC-P-023, SC-P-024

#### SC-P-019 — Cold-start continuity

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-007, SC-A-003, SC-A-004, SC-A-012

#### SC-P-020 — Second-occurrence structural signal

**Hard prereqs:** SC-P-004, SC-B-010
**Soft prereqs:** SC-B-012, SC-P-021, SC-P-024

#### SC-P-021 — Reference sweep after rename

**Hard prereqs:** SC-P-004
**Soft prereqs:** SC-P-020, SC-B-002, SC-B-010

#### SC-P-022 — Cross-model gap reading

**Hard prereqs:** SC-P-012
**Soft prereqs:** SC-P-002, SC-P-016, SC-A-008, SC-A-010, SC-B-003

#### SC-P-023 — Insert gate not check

**Hard prereqs:** SC-B-013
**Soft prereqs:** SC-P-018, SC-A-014, SC-P-013

#### SC-P-024 — Fallback permanence detection

**Hard prereqs:** SC-P-020
**Soft prereqs:** SC-P-018, SC-P-014, SC-B-014

#### SC-P-025 — Tier-ranked literature search

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-026, SC-P-029, SC-B-006

#### SC-P-026 — Citation integrity audit

**Hard prereqs:** SC-P-025
**Soft prereqs:** SC-B-005, SC-P-015

#### SC-P-027 — Hostile reviewer simulation

**Hard prereqs:** (none)
**Soft prereqs:** SC-B-006, SC-B-008, SC-P-015, SC-P-033

#### SC-P-028 — Multi-perspective synthesis

**Hard prereqs:** SC-P-027
**Soft prereqs:** SC-B-008, SC-P-013

#### SC-P-029 — Post-publication gap check

**Hard prereqs:** SC-P-025
**Soft prereqs:** SC-P-026, SC-P-034

#### SC-P-030 — Academic register calibration

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-032, SC-P-034

#### SC-P-031 — Venue strategy as constraint envelope

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-007, SC-P-027, SC-P-030

#### SC-P-032 — Margin note as deliberation

**Hard prereqs:** (none)
**Soft prereqs:** SC-A-001, SC-P-030, SC-P-027

#### SC-P-033 — Reflexivity diagnosis

**Hard prereqs:** (none)
**Soft prereqs:** SC-P-027, SC-P-015, SC-P-034

#### SC-P-034 — Overclaim prevention

**Hard prereqs:** SC-P-025
**Soft prereqs:** SC-P-029, SC-P-033, SC-P-026

## Teaching sequences (from catalog/teaching-sequences.md)

For reference, the 5 curated sequences that informed the hard edges above:

1. **Ablation / Rule-Optimization** (5 atoms): SC-P-002 → SC-P-016 → SC-A-008 → SC-P-012 → SC-P-022
2. **Sync / Drift / Authority Chain** (7 atoms): SC-B-010 → SC-B-001 → SC-B-003 → SC-B-002 → SC-B-014 → SC-B-011 → SC-B-012
3. **Red Team / Convergence** (4 atoms): SC-B-005 → SC-B-006 → SC-B-008 → SC-P-011
4. **CO 5-Layer Walkthrough** (6 atoms): SC-A-013 → SC-A-012 → SC-A-008 → SC-A-014 → SC-A-001 → SC-A-015
5. **Attention-Timing Progression** (5 atoms): SC-P-019 → SC-P-004 → SC-P-020 → SC-P-024 → SC-P-009

Note that SC-A-008 appears in two sequences (seq 1 via SC-P-016, seq 4 via SC-A-012), so it carries two independent hard-prereq chains. This is the only atom that participates in two sequences.

## Verification checks

- **Total hard edges:** 33
- **Total soft edges:** ~190 (counted from the soft prereq table; some atoms appear in each other's soft lists, producing undirected pairs)
- **Atoms with at least one hard prereq:** 30 (67 − 37 Layer-0 atoms)
- **Atoms with no hard prereqs (Layer 0):** 37
- **Atoms that appear as hard prereq for others (load-bearing foundations):** 26
- **Atoms with out-degree ≥ 3 (flagged load-bearing):** 2 — SC-B-013 (→ SC-B-007, SC-P-018, SC-P-023); SC-P-025 (→ SC-P-026, SC-P-029, SC-P-034)
- **Atoms with in-degree ≥ 2 (flagged convergence points):** 3 — SC-A-008 (from SC-P-016 and SC-A-012); SC-P-018 (from SC-B-013 and SC-P-013); SC-P-020 (from SC-P-004 and SC-B-010)
- **Max dependency depth:** 7 layers (SC-B-012 is 6 hard prereqs from any Layer 0 atom via SC-B-011 → SC-B-014 → SC-B-002 → SC-B-003 → SC-B-001 → SC-B-010)
- **Cycles detected:** 0

### Cycles broken during extraction

Three cycle-candidates were identified during extraction and resolved by reclassifying the weaker edge as soft:

1. **SC-B-001 ↔ SC-B-011 via "runs before this move."** SC-B-001's Related atoms note: "SC-B-011 (version tracking cascade) — the version-mismatch check that runs before this move." If taken as hard (SC-B-001 requires SC-B-011), this creates a cycle with teaching sequence 2 where SC-B-011 requires SC-B-014 which eventually requires SC-B-001. **Resolution:** Teaching sequence 2 is authoritative; the "runs before" phrasing in SC-B-001 is reclassified as soft (reinforces).

2. **SC-B-001 ↔ SC-B-012 via "pre-flight check this move depends on."** SC-B-001's Related atoms: "SC-B-012 (manifest parity automation) — the automated pre-flight check that this move depends on." Same cycle risk with teaching sequence 2. **Resolution:** Reclassified as soft.

3. **SC-A-011 ↔ SC-P-006 potential inverse.** SC-A-011's Related atoms note: "SC-P-006 (package boundary decision) — the three-question test for 'module vs separate package' is a downstream decision that fires only after the framework-first check has confirmed the functionality is genuinely new." And SC-P-006's Related atoms note: "SC-A-011 (framework-first check) — the framework-first check fires before the package boundary decision; if the functionality already exists in a framework, the boundary question is moot." Both point in the same direction (SC-A-011 first, then SC-P-006). **Resolution:** Not a cycle — one hard edge in the direction both atoms agree on: SC-P-006 requires SC-A-011.

### Load-bearing atoms (for course builders)

**Must-teach-early atoms** (out-degree ≥ 2 or subsidiary chain length ≥ 3):

- **SC-B-010** (Cross-repo divergence audit) — anchors the entire Sync sequence (7 downstream atoms via transitive closure) and directly prereqs SC-P-020. Any sync-related or drift-related course must start here.
- **SC-B-013** (Cross-workspace dependency mapping) — direct hard prereq for SC-B-007, SC-P-018, SC-P-023. Also soft prereq for SC-A-014. Any parallel-execution, integration, or milestone course must include this early.
- **SC-P-025** (Tier-ranked literature search) — anchors the research-writing cluster (SC-P-026, SC-P-029, SC-P-034). Any research-writing course must start here.
- **SC-P-002** (Model-vs-rule ablation) — anchors sequence 1 (5 atoms deep to SC-P-022).
- **SC-A-013** (Agent specialization routing) — anchors sequence 4 (6 atoms deep to SC-A-015).
- **SC-P-019** (Cold-start continuity) — anchors sequence 5 (5 atoms deep to SC-P-009).
- **SC-B-005** (Spec-to-code conformance audit) — anchors sequence 3 (4 atoms deep to SC-P-011).
- **SC-A-008** (Rule classification critical vs advisory) — the only atom that sits in two sequences; high in-degree (2 hard). Feeds SC-P-012, SC-A-014, and onward.

**Terminal atoms** (no downstream hard prereqs, pure leaves):

SC-P-022, SC-A-015, SC-B-012, SC-P-009, SC-P-011, SC-P-006, SC-B-007, SC-P-023, SC-P-021, SC-P-026, SC-P-028, SC-P-029, SC-P-034 — these are "capstone" atoms for their respective clusters; courses can end a track at any of these.
