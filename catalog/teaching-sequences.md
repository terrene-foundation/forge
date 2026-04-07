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
