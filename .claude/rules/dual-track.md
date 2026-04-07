# Library, Not Tracks

> **REPLACES** the previous "Dual-Track Consistency" rule (F-track / E-track / C-track shared-foundation model). That model was a downstream-distillation sketch leaking into FORGE-program design. As of 2026-04-07, FORGE itself is **not** organised by role tracks at all.

## The Rule

FORGE is a **canonical, contributor-neutral library** for the CO ecosystem — not a course, not a dual-track curriculum, not a sequence of modules. It is organised by:

1. **Responsibilities** — what the practitioner owes upward (to specs, to truth, to the human authority above the loop)
2. **Accountabilities** — what the practitioner owes downward (to artifacts they ship, to operators downstream)
3. **Skillsets** — the moves the practitioner can actually execute (drilled, not just understood)
4. **Spec lineage** — which **CARE / EATP / CO / PACT** principle each skill serves (the four standards as peers; COC is a CO application, not a standard)
5. **Application** — which CO application(s) the skill is evidence for (COC / COR / COE / compliance / finance / governance / learners — all in scope; build order is usage-weighted, not inclusion-gated)
6. **Practice modality** — how the skill is learned (drill, case, observation, build, brokerage rep, governance call)

Role-tailored client courses (AI Engineer, AI Business Consultant, future others) live downstream in `lyceum/courses/`. Different clients mix and match the same FORGE library into different sequences. **FORGE never picks a role.**

## MUST

- Catalog skills by responsibility/accountability/skillset/lineage/modality — never by job title
- Every skill atom is reusable across multiple downstream courses without rewriting
- Cross-reference courses can pull the same atom into different sequences without forking it
- When a downstream course needs a skill that doesn't exist in the library, **add it to the library first**, then reference from the course

```
DO:    skills/
         brokerage/
           authority-chain-enforcement.md     (lineage: COC § Brokerage; modality: rep)
           variant-classification.md          (lineage: COC § Variants; modality: drill)
         practitioner/
           dual-plane-classification.md       (lineage: CARE § Dual Plane; modality: case)
       courses/
         ai-engineer/sequence.md → references skills/brokerage/*, skills/practitioner/*
         ai-consultant/sequence.md → references skills/practitioner/dual-plane-classification.md

DO NOT: forge/
          F1-co-fundamentals.md         ← role-coupled module
          E1-coc-development.md         ← role-coupled module
          C1-dx-strategy.md             ← role-coupled module
```

## MUST NOT

- Organise FORGE content under role names (Engineer, Consultant, Manager, Operator, etc.)
- Put a module in FORGE that only one downstream course will ever use — that belongs in the course, not the library
- Sequence skills into a numbered curriculum (M1 → M2 → M3) at the FORGE level — sequencing is a course concern
- Replicate the F/E/C structure even informally ("the foundation atoms" vs "the engineer atoms") — there are no track distinctions in the library

**Why:** Role-coupled libraries fragment on every new client. The first time someone says "we need a course for AI Operations Managers", a track-organised library forces a fork: rewrite F1-F4 for ops people, redo all examples. A responsibility-organised library composes: the Operations course just selects the relevant brokerage and governance atoms and adds operations-specific case studies. Same library, different distillation. This is also why ASCENT (Kailash SDK / ML programme) is **independent** of FORGE — they teach different skillsets and would only entangle each other under a tracks model.
