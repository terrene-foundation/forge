---
destination: forge
application: COC
date: 2026-04-07
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

| Case | Title | Key Skill |
|------|-------|-----------|
| CASE-04 | The Token Ablation Clean-Room Experiment | Empirical rule necessity testing, contamination detection |

### Artifact (Layer 2)

Cases teaching artifact-authoring and artifact-validation skills.

| Case | Title | Key Skill |
|------|-------|-----------|
| CASE-03 | Shipped Mock Data After 11 Red Team Rounds | Frontend mock detection, spec-coverage audit |
| CASE-05 | The Enforcers Must Obey Their Own Rules | Layer 3 hook security audit, defense-in-depth |
| CASE-06 | Three Critical API Mismatches Before a Line of Code | Todos red team, API surface verification |
| CASE-08 | Ten Wrong Assumptions About the SDK | Specialist delegation, SDK API verification |
| CASE-10 | 4,579 Observations, 8 Duplicate Instincts, Zero Learning | Learning pipeline diagnosis, signal filtering |

### Brokerage (Layer 3)

Cases teaching authority-chain enforcement, variant management, and cross-system governance.

| Case | Title | Key Skill |
|------|-------|-----------|
| CASE-01 | The 85% Drift Audit | Variant classification, drift detection |
| CASE-02 | Sync Must Merge, Not Copy | Read-then-merge sync, authority chain |
| CASE-07 | The GLOBAL BUG in Loom Source | Authority chain escalation, multi-round red team |
| CASE-09 | The Vacant Approver Who Could Approve Bridges | Cross-feature interaction red team, governance integrity |

## Index by Spec Concept

### CO Concepts

| Concept | Cases |
|---------|-------|
| CO §1 (Institutional Knowledge Thesis) | CASE-04 |
| CO §3.2 (Convention Drift) | CASE-01 |
| CO §3.3 (Safety Blindness) | CASE-03, CASE-04, CASE-05, CASE-09 |
| CO §5 (Deterministic Enforcement) | CASE-04, CASE-05 |
| CO §7 (Knowledge Compounds) | CASE-10 |
| CO §9 (Layer 1 -- Intent) | CASE-08 |
| CO §15 (Progressive Disclosure) | CASE-01 |
| CO §16 (Framework-First) | CASE-08 |
| CO §17 (Single Source of Truth) | CASE-01, CASE-02, CASE-07 |
| CO §19 (Layer 3 -- Guardrails) | CASE-03, CASE-04, CASE-05 |
| CO §22 (Advisory Rules) | CASE-04 |
| CO §28 (Approval Gate) | CASE-02 |
| CO §29 (Evidence-Based Completion) | CASE-03, CASE-06, CASE-08 |
| CO §36 (Phase -- Vet) | CASE-06 |
| CO §39 (Layer 5 -- Learning) | CASE-10 |
| CO §40 (Observe-Capture-Evolve) | CASE-10 |
| CO §49 (Enforcement Reliability) | CASE-03, CASE-07, CASE-09 |
| CO §50 (Workflow Discipline) | CASE-07 |
| CO §51 (Learning Velocity) | CASE-10 |

### CARE Concepts

| Concept | Cases |
|---------|-------|
| CARE §6 (Trust Verification Bridge) | CASE-02 |
| CARE §44 (Failure Modes) | CASE-05 |

### EATP Concepts

| Concept | Cases |
|---------|-------|
| EATP §16 (Delegation Record) | CASE-06 |

### PACT Concepts

| Concept | Cases |
|---------|-------|
| PACT §10 (Monotonic Tightening) | CASE-07 |
| PACT §35 (Bridges) | CASE-06, CASE-09 |
| PACT §51 (Vacant Roles) | CASE-09 |

## Index by Recommended Use

| Use | Cases |
|-----|-------|
| **Teaching** | CASE-01, CASE-02, CASE-05, CASE-06, CASE-09 |
| **Discussion** | CASE-04, CASE-07, CASE-10 |
| **Assessment** | CASE-03, CASE-08 |

## Coverage Notes

The 10 exemplar cases cover:

- **19 distinct CO concepts** out of 187 total (10%), concentrated on the heavy-cited load-bearing concepts (CO §17, §3.3, §49, §29, §19)
- **2 CARE concepts** (§6, §44) -- expected low coverage since CARE is philosophical, not engineering craft
- **1 EATP concept** (§16) -- engineering evidence for EATP is sparse in the COC-layer corpus
- **3 PACT concepts** (§10, §35, §51) -- concentrated on governance primitives where Rust SDK work provides direct evidence
- **All three craft layers**: 1 practitioner, 5 artifact, 4 brokerage

The 25-candidate list in `case-candidates.md` includes 15 additional cases that can be expanded using the same format when coverage needs to broaden.
