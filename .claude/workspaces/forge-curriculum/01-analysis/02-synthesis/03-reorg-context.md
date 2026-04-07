---
date: 2026-04-07
status: pointer + FORGE-relevant excerpts from the atelier ecosystem-restructure brief
source: /Users/esperie/repos/atelier/.claude/workspaces/ecosystem-restructure/brief.md
reorg status: PLAN APPROVED (per user, 2026-04-07) — scaffolding and connections will not change
---

# Reorg Context for FORGE

## The hierarchy

```
terrene/                              ← Foundation (specs, governance, strategy, initiatives)
   └── atelier/                       ← CO methodology authority — 6 domains
         ├── co-codegen/    NEW       ← agnostic COC distilled from loom
         ├── co-education/  MERGED    ← COE + COL (instructor + learner methodology)
         ├── co-dx/         MOVED     ← from training/co-ax
         ├── co-research/   unchanged
         ├── co-governance/ MERGED    ← COG + COComp
         │
         ├── loom/                    ← code arm (CC + COC)  ←─── bidirectional sync ──→ co-codegen
         └── lyceum/        NEW       ← education arm
                  ├── programs/       ← canonical content (knowledge source of truth)
                  │     ├── ascent/   ← MOVE from training/ascent
                  │     ├── forge/    ← MOVE from training/forge   ◄── THIS IS US
                  │     └── finance/  ← extracted from atelier/co-finance
                  ├── courses/        ← instructor-facing distillations (lat, mldm, fnce210)
                  └── learners/       ← learner-facing distillations (finance)
```

NOTE: the brief uses `programmes/` (UK spelling). The user has chosen `programs/` (US). Use `programs/` going forward.

## What changes for FORGE

| Item | Before | After |
|---|---|---|
| Path | `repos/training/forge` | `repos/lyceum/programs/forge` |
| Repo identity | A "training" project | A `programs/` entry inside the lyceum education arm |
| Sister content | siblings: ascent, co-ai, co-ax, co-finance, etc. | siblings: ascent, finance (other training/ content moves elsewhere) |
| Authority chain | unclear | atelier → lyceum (peer to atelier → loom) |
| 6-command chain | carried in full | the lyceum move IS the planned cure for the identity drift; do not touch |

## The simple rename (per user)

```bash
# from ~/repos
mv training lyceum
mkdir -p lyceum/programs
mv lyceum/forge lyceum/programs/forge
cd lyceum && git init
# then restart Claude Code in lyceum/programs/forge
```

Other content currently in `training/` (ascent, co-ai, co-ax, co-finance, co-national, etc.) stays at `lyceum/` top level for now and gets sorted into the full reorg structure later.

## The FORGE / co-codegen relationship

- **co-codegen** = atelier's new domain. Agnostic methodology. Stripped of language/stack specifics. Bidirectional sync with loom.
- **FORGE** = lyceum's CO+COC programme. Curriculum + practitioner craft. Built on top of co-codegen.
- **Dual-destination routing**: Reports 05/06/07 from this analysis session populate co-codegen. Reports 01/02/03/04 are uniquely FORGE.

## What remains uncertain

- Exact `lyceum/programs/forge/CLAUDE.md` content (deferred until after rename — see D17)
- Exact structure of `co-codegen/` (Phase 2a of the reorg, not yet executed; this session's reports give it a head start)
- Sequencing between co-codegen Phase 2a and FORGE-program v1 (per D14, fuse them)

## Source

`/Users/esperie/repos/atelier/.claude/workspaces/ecosystem-restructure/brief.md` — full 307-line brief. Status was "Brief written, awaiting discussion" at session start. User confirmed "the reorg plan has been approved and the scaffolding and connections will not change."
