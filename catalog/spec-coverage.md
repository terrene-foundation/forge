---
title: Spec Coverage Report — 57 atoms against 417 spec concepts
date: 2026-04-07
destination: forge
---

# Spec Coverage Report

How the 57-atom catalog maps against the 417 spec concepts enumerated in the spec index (CARE 69 + EATP 94 + CO 187 + PACT 67).

## CO 5-Layer Coverage (T5.6 requirement)

Every CO layer must have ≥2 dedicated atoms.

| CO Layer                   | §§                                | Dedicated atoms                                                                                                                                                      | Count | Status          |
| -------------------------- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----- | --------------- |
| **Layer 1 — Intent**       | §9, §10, §11                      | SC-A-013 (agent routing), SC-B-004 (specialist delegation), SC-P-019 (cold-start)                                                                                    | 3     | Covered         |
| **Layer 2 — Context**      | §13, §14, §15, §16, §17           | SC-A-012 (CLAUDE.md), SC-A-015 (corpus structuring), SC-A-009 (progressive disclosure), SC-A-011 (framework-first)                                                   | 4     | Well covered    |
| **Layer 3 — Guardrails**   | §19, §20, §21, §22, §23, §24, §25 | SC-A-002 (mock detection), SC-A-003 (defense-in-depth), SC-A-004 (hook security), SC-A-006 (deny-by-default), SC-A-007 (zero-config), SC-A-008 (rule classification) | 6     | Heavily covered |
| **Layer 4 — Instructions** | §26, §27, §28, §29                | SC-A-014 (milestone decomposition), SC-P-007 (session completion), SC-B-006 (convergence), SC-P-023 (gate not check)                                                 | 4     | Covered         |
| **Layer 5 — Learning**     | §39, §47                          | SC-A-001 (codify NOT-codified), SC-A-015 (corpus structuring), SC-P-012 (rule demotion), SC-P-017 (false-positive learning), SC-P-019 (cold-start continuity)        | 5     | Well covered    |

All 5 layers have ≥2 dedicated atoms. Layer 3 (Guardrails) is the most densely covered (6 atoms) — unsurprising since the corpus is engineering work where guardrails are the highest-friction layer.

## Top 15 Heavy-Cited Concepts Coverage

| #   | Concept                          | Evidence entries | Dedicated atoms              | Gap? |
| --- | -------------------------------- | ---------------- | ---------------------------- | ---- |
| 1   | CO §17 Single Source of Truth    | 47               | 15 atoms cite it             | No   |
| 2   | CO §16 Framework-First           | 45               | SC-A-011 + 3 others          | No   |
| 3   | CO §19 Layer 3 Guardrails        | 38               | 6 atoms                      | No   |
| 4   | CO §29 Evidence-Based Completion | 24               | 5 atoms                      | No   |
| 5   | CO §28 Approval Gate             | 21               | 7 atoms                      | No   |
| 6   | CO §3.3 Safety Blindness         | 21               | 8 atoms                      | No   |
| 7   | CO §13 Layer 2 Context           | 19               | SC-A-012, SC-A-015 + 2       | No   |
| 8   | CO §49 Enforcement Reliability   | 17               | 7 atoms                      | No   |
| 9   | CO §5 Deterministic Enforcement  | 16               | 7 atoms                      | No   |
| 10  | CO §9 Layer 1 Intent             | 16               | SC-A-013 + 2                 | No   |
| 11  | CO §39 Layer 5 Learning          | 15               | 5 atoms                      | No   |
| 12  | CO §26 Layer 4 Instructions      | 14               | SC-A-014 + 3                 | No   |
| 13  | CO §15 Progressive Disclosure    | 13               | SC-A-009, SC-P-011           | No   |
| 14  | CO §14 Master Directive          | 12               | SC-A-012, SC-A-015           | No   |
| 15  | CO §27 Structured Workflow       | 12               | SC-A-014, SC-B-013, SC-P-018 | No   |

**0 gaps in the top 15.** All have at least 1 dedicated atom after the red team gap-fill pass (D26).

## PACT Coverage

| Concept                        | Entries | Atoms                        | Gap? |
| ------------------------------ | ------- | ---------------------------- | ---- |
| PACT §10 Monotonic Tightening  | 11      | SC-A-005, SC-B-002, SC-B-009 | No   |
| PACT §19 Verification Gradient | 6       | SC-B-017                     | No   |
| PACT §11 Role Envelope         | 5       | SC-B-002, SC-B-004, SC-B-017 | No   |
| PACT §35 Bridges               | 5       | SC-P-003                     | No   |
| PACT §51 Vacant Roles          | 5       | SC-P-003                     | No   |
| PACT §40 EATP Integration      | 3       | SC-B-005, SC-P-001           | No   |

## EATP Coverage

| Concept                              | Entries | Atoms    | Gap?                                                           |
| ------------------------------------ | ------- | -------- | -------------------------------------------------------------- |
| EATP §16 Delegation Record           | 6       | SC-A-005 | No                                                             |
| EATP §40 PDP/PEP Enforcement         | 5       | SC-B-018 | No                                                             |
| EATP §15 Genesis Record              | 4       | —        | Expected gap (trust-protocol internals, not engineering craft) |
| EATP §24 Dimension-Scoped Delegation | 4       | —        | Expected gap                                                   |

## CARE Coverage

| Concept                           | Entries | Atoms    | Gap?                                                                   |
| --------------------------------- | ------- | -------- | ---------------------------------------------------------------------- |
| CARE §44 Failure Modes            | 9       | —        | Expected gap (architectural resilience theory, not practitioner craft) |
| CARE §6 Trust Verification Bridge | 8       | SC-B-001 | No                                                                     |
| CARE §30 Constraint Envelopes     | 8       | —        | Expected gap (philosophy)                                              |
| CARE §4 Human-on-the-Loop         | 7       | SC-B-015 | No                                                                     |

## Concepts Without Atoms — Expected Gaps

The 417 spec concepts include many that are philosophy (CARE), trust-protocol internals (EATP), or domain-application specifics (CO for research/education/compliance/etc.). These are NOT failures — they are expected gaps for a COC-layer catalog.

- **CARE philosophy concepts** (§1-§29, §30-§69): 52 without atoms. These are Dual Plane theory, Mirror Thesis, 8 principles. Practitioner craft atoms touch them indirectly but don't teach the philosophy — that's a CARE-facing pass, not a COC pass.
- **EATP interoperability concepts** (§40-§94): 76 without atoms. These are trust-protocol technical details (chain resolution, verification processes, MCP integration patterns). The 2 atoms (SC-A-005, SC-B-018) cover the highest-signal engineering concepts.
- **CO domain-application concepts** (§73-§187): 122 without atoms. These are COR, COE, compliance, finance, governance, learners application-specific concepts. Out of COC-layer scope by design — queued for future passes in `future-passes.md`.
- **PACT classification semantics** (§25-§39): 41 without atoms. These are detailed governance classification rules that don't surface as engineering craft.

## Summary

- **57 atoms** covering **~126 of 417 spec concepts** (30%) — same coverage rate as the reverse index (D21)
- **All 5 CO layers** have ≥2 dedicated atoms
- **All top 15 heavy-cited concepts** have ≥1 dedicated atom
- **0 unexplained gaps** — every uncovered concept has a documented reason (philosophy, protocol internals, out-of-scope application, classification semantics)
- **18 atoms cross-tagged** for COR/COE, providing a foundation for future application passes
