---
destination: forge
application: COC
date: 2026-04-09
type: case-library-index
---

# FORGE Case Library

Teaching cases drawn from the COC-layer journal corpus (232 entries across loom/, kailash-py, kailash-rs, kaizen-cli, and kz-engage). Each case has a clear narrative arc, is self-contained, and connects to multiple spec concepts from CARE/EATP/CO/PACT.

## Structure

```
cases/
  case-candidates.md          Screening of 232 entries; 25 candidates identified
  README.md                   This file
  exemplars/                  Full teaching cases (6-section format)
    CASE-01-drift-audit.md
    CASE-02-sync-merge-not-copy.md
    CASE-03-spec-coverage-gate.md
    CASE-04-token-ablation.md
    CASE-05-session-start-command-injection.md
    CASE-06-pact-todos-red-team.md
    CASE-07-artifact-redteam-convergence.md
    CASE-08-sdk-api-corrections.md
    CASE-09-cross-feature-vacancy-bridge.md
    CASE-10-learning-system-broken.md
    CASE-11-pact-spec-conformance-four-gaps.md
    CASE-12-human-approves-agent-performs.md
    CASE-13-pub-fields-security-holes.md
    CASE-14-deny-bypass-nobody-tested.md
    CASE-15-rule-plus-hook.md
    CASE-16-silent-drift-divergence.md
    CASE-17-layer5-not-aspirational.md
    CASE-18-vacancy-asymmetric-enforcement.md
    CASE-19-orphans-stales-misclassified.md
    CASE-20-adjacent-disconnected.md
    CASE-21-orphan-audit-wrong.md
    CASE-22-decision-reversal.md
    CASE-23-audited-side-ahead.md
    CASE-24-146kb-rules-tokens.md
    CASE-25-timing-sidechannel.md
```

## Case Format

Every exemplar case has six sections:

1. **The Situation** -- project, phase, what was happening
2. **The Trigger** -- what the practitioner noticed (the attentional pattern)
3. **The Move** -- what was done or should have been done (names the skill atom)
4. **The Outcome** -- what happened as a result
5. **The Spec Connection** -- which CARE/EATP/CO/PACT concepts, with concrete enumeration
6. **Discussion Questions** -- 2-3 probing questions (at least one counterfactual)

Each case carries frontmatter with `case_id`, `source_path`, `atoms_served`, `spec_concepts`, `craft_layer`, and `recommended_use`.

## Index by Craft Layer

### Practitioner (Layer 1)

Cases teaching practitioner-level attentional and diagnostic skills.

| Case    | Title                                    | Key Skill                                                        |
| ------- | ---------------------------------------- | ---------------------------------------------------------------- |
| CASE-04 | The Token Ablation Clean-Room Experiment | Empirical rule necessity testing, contamination detection        |
| CASE-12 | Human Approves, Agent Performs           | MUST-rule reinterpretation, human-on-the-loop operationalization |

### Artifact (Layer 2)

Cases teaching artifact-authoring and artifact-validation skills.

| Case    | Title                                                    | Key Skill                                                              |
| ------- | -------------------------------------------------------- | ---------------------------------------------------------------------- |
| CASE-03 | Shipped Mock Data After 11 Red Team Rounds               | Frontend mock detection, spec-coverage audit                           |
| CASE-05 | The Enforcers Must Obey Their Own Rules                  | Layer 3 hook security audit, defense-in-depth                          |
| CASE-06 | Three Critical API Mismatches Before a Line of Code      | Todos red team, API surface verification                               |
| CASE-08 | Ten Wrong Assumptions About the SDK                      | Specialist delegation, SDK API verification                            |
| CASE-10 | 4,579 Observations, 8 Duplicate Instincts, Zero Learning | Learning pipeline diagnosis, signal filtering                          |
| CASE-14 | The Deny Bypass Nobody Tested                            | Negative test for deny-default, auth enforcement verification          |
| CASE-15 | Rule Plus Hook                                           | Rule-plus-hook combination, soft-to-hard enforcement escalation        |
| CASE-17 | Layer 5 Is Not Aspirational                              | Institutional knowledge mandate, structural-vs-functional completeness |
| CASE-21 | The Orphan Audit That Was Wrong                          | Audit categorization error detection, progressive disclosure awareness |
| CASE-24 | 146KB of Rules at 36K Tokens Per Turn                    | Token budget analysis, external-vs-baked tradeoff                      |
| CASE-25 | Timing Side-Channel in API Key Comparison                | Timing sidechannel detection, security primitive audit                 |

### Brokerage (Layer 3)

Cases teaching authority-chain enforcement, variant management, and cross-system governance.

| Case    | Title                                         | Key Skill                                                        |
| ------- | --------------------------------------------- | ---------------------------------------------------------------- |
| CASE-01 | The 85% Drift Audit                           | Variant classification, drift detection                          |
| CASE-02 | Sync Must Merge, Not Copy                     | Read-then-merge sync, authority chain                            |
| CASE-07 | The GLOBAL BUG in Loom Source                 | Authority chain escalation, multi-round red team                 |
| CASE-09 | The Vacant Approver Who Could Approve Bridges | Cross-feature interaction red team, governance integrity         |
| CASE-11 | Four Gaps Between Spec and Code               | Spec-to-code conformance audit, gap-list methodology             |
| CASE-13 | Public Fields as Security Holes               | Encapsulation as security boundary, signed struct integrity      |
| CASE-16 | 24 Directories of Silent Drift                | Drift detection audit, variant classification                    |
| CASE-18 | Vacancy Enforcement Only Works One Way        | Governance primitive integrity, asymmetric enforcement detection |
| CASE-19 | Orphans, Stales, and Misclassifications       | Manifest integrity audit, orphan-stale-misclassified taxonomy    |
| CASE-20 | Adjacent But Disconnected                     | Adjacent module gap detection, spec wiring verification          |
| CASE-22 | Decision Reversal in One Day                  | Decision reversal journaling, supersede with reference           |
| CASE-23 | The Audited Side Was Ahead                    | Bidirectional parity audit, assumption reversal                  |

## Index by Spec Concept

### CO Concepts

| Concept                                | Cases                                                                           |
| -------------------------------------- | ------------------------------------------------------------------------------- |
| CO §1 (Institutional Knowledge Thesis) | CASE-04                                                                         |
| CO §3.2 (Convention Drift)             | CASE-01, CASE-16                                                                |
| CO §3.3 (Safety Blindness)             | CASE-03, CASE-04, CASE-05, CASE-09                                              |
| CO §4 (Human-on-the-Loop)              | CASE-12                                                                         |
| CO §5 (Deterministic Enforcement)      | CASE-04, CASE-05, CASE-11, CASE-13, CASE-14, CASE-15, CASE-18, CASE-24, CASE-25 |
| CO §5.1 (Hard Enforcement)             | CASE-15                                                                         |
| CO §5.2 (Soft Enforcement)             | CASE-15                                                                         |
| CO §7 (Knowledge Compounds)            | CASE-10, CASE-17                                                                |
| CO §9 (Layer 1 -- Intent)              | CASE-08                                                                         |
| CO §13 (Layer 2 -- Context)            | CASE-24                                                                         |
| CO §14 (Master Directive)              | CASE-24                                                                         |
| CO §15 (Progressive Disclosure)        | CASE-01, CASE-16, CASE-21, CASE-24                                              |
| CO §16 (Framework-First)               | CASE-08, CASE-22                                                                |
| CO §17 (Single Source of Truth)        | CASE-01, CASE-02, CASE-07, CASE-16, CASE-19, CASE-20, CASE-22, CASE-23          |
| CO §19 (Layer 3 -- Guardrails)         | CASE-03, CASE-04, CASE-05, CASE-15, CASE-25                                     |
| CO §20 (Rule Classification)           | CASE-21                                                                         |
| CO §22 (Advisory Rules)                | CASE-04                                                                         |
| CO §28 (Approval Gate)                 | CASE-02, CASE-12, CASE-22                                                       |
| CO §29 (Evidence-Based Completion)     | CASE-03, CASE-06, CASE-08, CASE-11, CASE-23                                     |
| CO §33 (Phase -- Analyze)              | CASE-23                                                                         |
| CO §36 (Phase -- Vet)                  | CASE-06                                                                         |
| CO §39 (Layer 5 -- Learning)           | CASE-10, CASE-17                                                                |
| CO §40 (Observe-Capture-Evolve)        | CASE-10                                                                         |
| CO §42 (Knowledge Growth)              | CASE-17                                                                         |
| CO §49 (Enforcement Reliability)       | CASE-03, CASE-07, CASE-09, CASE-11, CASE-14, CASE-19, CASE-21, CASE-25          |
| CO §50 (Workflow Discipline)           | CASE-07                                                                         |
| CO §51 (Learning Velocity)             | CASE-10                                                                         |

### CARE Concepts

| Concept                             | Cases   |
| ----------------------------------- | ------- |
| CARE §4 (Human-on-the-Loop)         | CASE-12 |
| CARE §6 (Trust Verification Bridge) | CASE-02 |
| CARE §42 (Constraint Resolution)    | CASE-19 |
| CARE §44 (Failure Modes)            | CASE-05 |

### EATP Concepts

| Concept                         | Cases            |
| ------------------------------- | ---------------- |
| EATP §14 (Trust Lineage Chain)  | CASE-20          |
| EATP §16 (Delegation Record)    | CASE-06, CASE-13 |
| EATP §33 (Cascade Revocation)   | CASE-13          |
| EATP §36 (Graceful Degradation) | CASE-14, CASE-25 |

### PACT Concepts

| Concept                                   | Cases                     |
| ----------------------------------------- | ------------------------- |
| PACT §10 (Monotonic Tightening)           | CASE-07, CASE-11, CASE-13 |
| PACT §13 (Effective Envelope)             | CASE-18                   |
| PACT §19 (Verification Gradient)          | CASE-12                   |
| PACT §23 (Blocked Zone)                   | CASE-14                   |
| PACT §33 (Knowledge Cascade)              | CASE-18                   |
| PACT §35 (Bridges)                        | CASE-06, CASE-09, CASE-11 |
| PACT §40 (EATP Integration)               | CASE-11, CASE-20          |
| PACT §42 (Delegation Record EATP Backing) | CASE-20                   |
| PACT §44 (Organizational Compilation)     | CASE-11                   |
| PACT §51 (Vacant Roles)                   | CASE-09, CASE-18          |

## Index by Recommended Use

| Use            | Cases                                                                                             |
| -------------- | ------------------------------------------------------------------------------------------------- |
| **Teaching**   | CASE-01, CASE-02, CASE-05, CASE-06, CASE-09, CASE-13, CASE-15, CASE-16, CASE-17, CASE-19, CASE-22 |
| **Discussion** | CASE-04, CASE-07, CASE-10, CASE-12, CASE-20, CASE-21, CASE-23, CASE-24                            |
| **Assessment** | CASE-03, CASE-08, CASE-11, CASE-14, CASE-18, CASE-25                                              |

## Coverage Notes

The 25 exemplar cases cover:

- **28 distinct CO concepts** out of 187 total (15%), concentrated on the heavy-cited load-bearing concepts (CO §17, §5, §49, §15, §29, §19)
- **4 CARE concepts** (§4, §6, §42, §44) -- expected low coverage since CARE is philosophical, not engineering craft
- **4 EATP concepts** (§14, §16, §33, §36) -- engineering evidence for EATP is sparse in the COC-layer corpus
- **10 PACT concepts** (§10, §13, §19, §23, §33, §35, §40, §42, §44, §51) -- strong governance coverage from Rust SDK cross-sdk-governance work
- **All three craft layers**: 2 practitioner, 11 artifact, 12 brokerage
- **All three uses**: 11 teaching, 8 discussion, 6 assessment

All 25 candidates from `case-candidates.md` are now fully expanded.
