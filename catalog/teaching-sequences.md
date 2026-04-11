---
title: Teaching Sequences — atom clusters that compose into workflows
date: 2026-04-07
destination: forge
---

# Teaching Sequences

Some atoms are genuinely standalone — a practitioner can learn them in isolation. Others compose into **teaching sequences** where atom A's output feeds atom B's input. This file maps the known sequences.

Downstream course builders (`lyceum/courses/`) should present atoms within a sequence in order, not arbitrarily. Individual atoms are still self-contained (each has its own drill + evidence), but the sequence reveals how the moves chain together in practice.

## Sequence 1: Ablation / Rule-Optimization Workflow

**5 atoms, workflow order:**

```
SC-P-002 (ablation methodology)
    ↓ produces: baseline findings (which rules are model-native)
SC-P-016 (detect overcorrection)
    ↓ produces: contamination diagnosis (is the baseline itself compromised?)
SC-A-008 (rule classification: critical vs advisory)
    ↓ produces: classification (each rule is tiered)
SC-P-012 (demotion vs strengthening judgment)
    ↓ produces: disposition per rule (demote / strengthen / keep)
SC-P-022 (cross-model gap reading)
    ↓ produces: minimum-viable rule set for the weakest model in scope
```

**Spec thread**: CO §1 (Institutional Knowledge Thesis) → CO §22 (Advisory Rules) → CO §19 (Guardrails) → CO §5 (Deterministic Enforcement) → CO §3.3 (Safety Blindness at the model boundary)

**Teaching note**: SC-P-002 must come before SC-P-016 — you cannot detect contamination if you haven't run the clean-room first. SC-P-022 must come last — the cross-model gap is only meaningful after classification and demotion decisions.

## Sequence 2: Sync / Drift / Authority Chain Workflow

**7 atoms, layered order:**

```
SC-B-010 (drift detection audit)
    ↓ produces: numbered drift-gap list
SC-B-001 (read-then-merge sync)
    ↓ produces: per-file diff with conflict identification
SC-B-003 (variant classification)
    ↓ produces: global / variant / skip disposition per artifact
SC-B-002 (authority-chain escalation)
    ↓ produces: upstream proposal for defects found in synced artifacts
SC-B-014 (explicit supersession journaling)
    ↓ produces: decision-reversal record when upstream changes the prior decision
SC-B-011 (version tracking cascade)
    ↓ produces: version stamps that make future drifts detectable
SC-B-012 (manifest parity automation)
    ↓ produces: automated check that distribution manifest matches filesystem
```

**Spec thread**: CO §17 (Single Source of Truth) → CO §28 (Approval Gate at sync) → CO §49 (Enforcement Reliability via automation) → PACT §10 (Monotonic Tightening across the chain)

**Teaching note**: SC-B-010 (audit) and SC-B-012 (automation) bookend the sequence — one is the manual diagnostic, the other is the automated prevention. The middle atoms are the craft that bridges them.

## Sequence 3: Red Team / Convergence Workflow

**4 atoms, workflow order:**

```
SC-B-005 (spec-to-code conformance audit)
    ↓ produces: gap list (what the spec says MUST exist vs what the code has)
SC-B-006 (multi-round red team to numeric convergence)
    ↓ produces: finding counts by severity across rounds
SC-B-008 (convergence-as-validation via independent reds)
    ↓ produces: overlap rate between independent agents (validates audit quality)
SC-P-011 (audit categorization blindness)
    ↓ produces: diagnosis of when the audit method itself has a blind spot
```

**Spec thread**: CO §49 (Enforcement Reliability) → CO §28 (Approval Gate) → CO §5.4 (Defense in Depth) → CO §15 (Progressive Disclosure revealing what grep-based audits miss)

**Teaching note**: SC-P-011 is the meta-move — it teaches the practitioner that the entire audit chain (SC-B-005 → SC-B-006 → SC-B-008) can have a structural blind spot. It should come after the practitioner has experienced the audit chain, not before.

## Sequence 4: CO 5-Layer Architecture Walkthrough

**6 atoms, one per layer + integration:**

```
SC-A-013 (Layer 1: Intent — agent specialization routing)
SC-A-012 (Layer 2: Context — CLAUDE.md authoring)
SC-A-008 (Layer 3: Guardrails — rule classification critical vs advisory)
SC-A-014 (Layer 4: Instructions — milestone decomposition)
SC-A-001 (Layer 5: Learning — codify-with-explicit-NOT-codified)
SC-A-015 (Layer 2+5: Context corpus structuring — how Layer 2 and Layer 5 interact)
```

**Spec thread**: CO §9 → CO §13 → CO §19 → CO §26 → CO §39 → CO §14 (Master Directive as the integrator)

**Teaching note**: This sequence teaches the 5-layer architecture through concrete artifact-authoring moves, not through spec recitation. Each atom is "how to author this layer" not "what this layer is." SC-A-015 comes last because it teaches the interaction between Context and Learning — which requires understanding both.

## Sequence 5: Attention-Timing Progression

**5 atoms, ordered by when in the session lifecycle they fire:**

```
SC-P-019 (cold-start continuity — at session open)
SC-P-004 (per-file disposition labelling — at analysis start)
SC-P-020 (second occurrence structural signal — during implementation)
SC-P-024 (fallback permanence detection — during implementation)
SC-P-009 (architecture doc as contract — at review gates)
```

**Spec thread**: CO §39 (Learning persistence) → CO §33 (Phase Analyze) → CO §3.2 (Convention Drift) → CO §29 (Evidence-Based Completion)

**Teaching note**: This sequence maps to a session timeline. SC-P-019 fires at minute 0 (session start). SC-P-004 fires during the first phase (analysis). SC-P-020 and SC-P-024 fire during the implementation loop. SC-P-009 fires at review gates. The progression teaches practitioners to pay attention at the right moment in the workflow, not just to the right things.

---

## COR Application Sequences

The following sequences cover atoms from the COR (CO for Research) layer. They are listed separately because they teach research-as-scholarship craft, not codegen craft, but follow the same sequencing logic: each atom's output feeds the next atom's input.

## Sequence 6: Literature Integrity Workflow

**4 atoms, ordered by the research lifecycle:**

```
SC-P-025 (tier-ranked literature search — builds the evidence base)
    ↓ produces: ranked literature base with VERIFIED citations per area
SC-P-026 (citation integrity audit — verifies the evidence base)
    ↓ produces: verified citations with misquotation/drift flags resolved
SC-P-029 (post-publication gap check — maintains currency)
    ↓ produces: MUST CITE / SHOULD CITE / DO NOT CITE classification for recent work
SC-P-034 (overclaim prevention — qualifies claims derived from the evidence)
    ↓ produces: qualified novelty claims with scope + evidence + temporal qualifier
```

**Spec thread**: CO §1 (Institutional Knowledge Thesis — the literature IS the institutional knowledge) → CO §49 (Verification requirements) → COR § Layer 3 (no unverified citations, no fabricated references, no overclaims) → COR § Layer 1 (literature-researcher + claims-verifier agents)

**Teaching note**: SC-P-025 must come first — everything downstream depends on having a verified literature base. SC-P-026 is the integrity check on that base. SC-P-029 is the temporal maintenance move (the base ages; this atom keeps it current). SC-P-034 comes last because the qualification discipline depends on having both the base (025) and the gap check evidence (029) to cite as proof that the claimed scope is empty. A practitioner who tries overclaim prevention without a verified literature base produces the generic hedges ("to the best of our knowledge") that the atom explicitly rejects.

## Sequence 7: Peer Review Resilience

**2 atoms, ordered by the critique lifecycle:**

```
SC-P-027 (hostile reviewer simulation — generates the attack surface)
    ↓ produces: 3+ hostile reviews from different disciplinary perspectives
SC-P-028 (multi-perspective synthesis — resolves the attacks into a revision path)
    ↓ produces: convergent strengths/failures + minimal publishable model + revision path
```

**Spec thread**: CO §28 (Convergence requirements) → CO §5.4 (Independent verification — multiple hostile perspectives are defense-in-depth for the argument) → COR § Layer 4 (/challenge command gates: FATAL findings block section continuation)

**Teaching note**: SC-P-027 generates the critiques; SC-P-028 synthesizes them. Running 028 without 027 produces a synthesis from a single perspective that merely rearranges concerns rather than resolving them. Running 027 without 028 produces a collection of hostile reviews that the practitioner responds to individually (whack-a-mole), which is the failure mode 028 explicitly corrects. The two-atom sequence mirrors the COC Red Team / Convergence sequence (SC-B-005 → SC-B-006 → SC-B-008 → SC-P-011) at a smaller scale: generate critiques → synthesize to convergence. This parallel can be used in courses that teach both COC and COR to show how the same convergence pattern appears in different CO applications.
