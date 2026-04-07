---
source: synthesis (architect)
date: 2026-04-07
session: forge-curriculum reframe (craft school) — synthesis of 7 specialist reports
destination: lyceum/programs/forge (curriculum spine) + atelier/co-codegen (artifact-level layers)
status: design instruction for next session
---

# The Three-Layer Craft Decomposition

## Master framing — from the value-auditor

> **Specs as the index, craft as the substance.**
>
> Every skill atom is moored in a CARE/EATP/CO/COC concept (the index). Every skill atom is grounded in journal evidence (the substance). Both required. Neither alone works.

This reconciles the apparent tension between "specs as first principles" and "skillset distilled from journal logs." The specs name the *space* of skills that must exist; the journals supply the *content* of those skills.

## The three layers

The body of knowledge FORGE-COC must teach decomposes into three distinct craft layers, each with its own evidence base and its own destination after the lyceum reorg lands.

| Layer | What it is | Source corpus | Captured by | Post-reorg destination |
|---|---|---|---|---|
| **1. Practitioner craft** | Session-level moves: prompting, attention, intervention, reading codegen behavior, keeping institutional memory across sessions | Journal logs (DISCOVERY/RISK/DELIBERATION), session observations | Reports 01 + 02 + 03 + 04 | **lyceum/programs/forge** (uniquely) |
| **2. Artifact craft** | What makes a CC artifact work: 4 quality dimensions, 12 hard limits, 12 micro-patterns, 12 anti-patterns | cc-audit guides + actual artifacts in atelier/loom + drift journals | Report 06 | **atelier/co-codegen** (artifacts) + **lyceum/programs/forge** (curriculum that drills on the artifacts) |
| **3. Brokerage/governance craft** | 15 operator moves, 10 failure modes, 8 judgment calls, 6-command adaptation patterns, sync gates, variant classification, drift detection, authority chain enforcement | loom + atelier commands, BUILD/USE codify journals, drift audits, manifest parity work | Reports 05 + 07 | **atelier/co-codegen** (canonical method) + **lyceum/programs/forge** (curriculum that teaches the operator role) |

## The seven user-named craft areas, plus the seventh derived

The user explicitly named six craft areas:
1. how to prompt
2. how codegen behaves
3. how to intervene
4. when to intervene
5. how to pay attention
6. when to pay attention

The skill-atom taxonomy work (Report 02) revealed a seventh area implicit in the journals:

7. **how to keep institutional memory across sessions** — channel-routing literacy, session-notes-as-instructions-to-next-self, version stamping, manifest parity. This is a *tooling/environment* craft, distinct from the cognitive loop the first six describe.

## The size of what we now have

| Specialist report | Atoms / patterns / failures |
|---|---|
| 01. Failure modes (methodology-only) | 7 failure modes + 7 implied skill atoms |
| 02. Skill atom taxonomy | ~40 skill atoms across 7 craft areas + concept-vs-skill examples |
| 03. Attentional + interventional patterns | 14 attentional patterns + 14 interventional moves + 5-stage prompt-craft progression + CO-general/COC-specific tagging |
| 04. Buyer-side value audit | 6 buyer-side findings + the "specs as index, craft as substance" framing |
| 05. Brokerage and governance craft | 15 operator moves + 10 failure modes + 8 judgment calls |
| 06. CC artifact authoring craft | 4 dimensions + 12 hard limits + 12 micro-patterns + 12 anti-patterns |
| 07. 6-command adaptation craft | 6 invariant substances + 6-repo variant grid + 8 adaptation judgments + 4 fault lines |
| **Total** | **~150 evidence-grounded craft items**, each citing real journal entries or artifact files |

That is the v1 catalog spine. Not the whole library, but enough that "CO/COC are skillsets" is now empirically defensible — we can produce the skill atoms on demand, and they pass the journal-evidence test.

## Dual-destination routing rule

Any FORGE-COC content produced from this point on must be tagged with one of three destinations:

- `destination: lyceum/programs/forge` — practitioner curriculum, drills, exercises, case stories. Layer 1 lives here exclusively.
- `destination: atelier/co-codegen` — agnostic methodology, artifact patterns, operator moves. Layers 2 and 3 live here primarily, stripped of language/stack specifics.
- `destination: both` — for content that exists as canonical methodology in co-codegen *and* as curriculum scaffolding in forge. Most of Layers 2 and 3 will be `both`.

The atelier reorg brief explicitly creates `co-codegen` as a new domain whose Phase 2a is "distill from loom." Reports 05, 06, and 07 are ~50% of that distillation already. Reports 01–04 are uniquely FORGE territory and have no co-codegen counterpart.

## What this means for next-session FORGE-program design

1. **Skill catalog spine.** The next session can begin assembling the FORGE-program skill catalog by indexing every atom from Reports 02 + 03 + 05 + 06 + 07 against the (a) seven craft areas and (b) the spec moorings (CARE Trust Plane / EATP posture / CO 5-layer / etc.). Each catalog entry: name + one-line description + evidence pointer + spec mooring + destination tag + practice modality.
2. **Drill library.** Layer 1 atoms become drills (recurring micro-exercises). Layer 2 atoms become artifact-authoring exercises. Layer 3 atoms become operator scenarios.
3. **Case story library.** The DISCOVERY/RISK journals themselves are pre-formatted case stories. The session can lift them in as primary teaching material, with each case mapping to the skill atoms it instantiates.
4. **No architectural reframes.** The framing is now stable: craft school, COC v1, dual destination, lyceum destination, contributor-neutral. The next session's job is content production, not framing debate.

## Forward references

- The decision log lives in `02-decision-log.md` — read it first if you are picking up this work in a later session.
- The atelier reorg context lives in `03-reorg-context.md` — required reading because FORGE's geography is changing.
- All seven specialist reports live in `../01-research/` and should be treated as the evidence base, not as drafts of curriculum content.
