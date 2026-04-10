---
drill_id: DR-SC-B-002-FULL
source_atom: SC-B-002
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Authority-Chain Escalation When Local Fix Gets Reverted

## Setup

The practitioner is working in a USE template repository (`kailash-coc-claude-rs/.claude/`). During a red team pass, they discover that two skill directories are referenced by wrong numbers in three separate skill files:

**The bug:** Skills reference `07-eatp-reference/` and `08-care-reference/` in their cross-reference links, but the actual directories in the repository are `26-eatp-reference/` and `27-care-reference/`. The skill files that contain the broken references are:

- `skills/05-kailash-mcp/skill.md` — line 42: `"See 07-eatp-reference for trust lineage patterns"`
- `skills/14-code-templates/skill.md` — line 18: `"CARE philosophy constraints are defined in 08-care-reference"`
- `skills/29-pact/skill.md` — line 55: `"EATP integration patterns in 07-eatp-reference"`

**Context the practitioner knows:**

- This is a USE template repo. It receives artifacts via `/sync` from `loom/.claude/`.
- The authority chain is: `atelier/` (CC+CO) --> `loom/` (COC) --> USE templates --> downstream projects.
- The last `/sync` ran 3 days ago. No local edits have been made since.
- The practitioner's immediate task is to finish the red team pass and produce a findings report.

**What the practitioner does NOT know yet (but can deduce):**

- Whether the bug originated in `loom/` (meaning the source itself is wrong) or was introduced during sync (meaning a sync transformation is faulty).
- Whether other USE templates (`kailash-coc-claude-py`, `kailash-coc-claude-rb`) have the same bug.

## Task

1. **Fix locally.** Write the three corrected lines (replacing `07-` with `26-` and `08-` with `27-`). Record each fix with the exact file, line number, old text, and new text.

2. **Trace the authority chain.** For each of the three files, determine where the file is _authored_ — not where you found it. Walk the chain: is this file authored in the USE template, in `loom/`, or in `atelier/`? Write your reasoning. Then state: where must the permanent fix be landed?

3. **Write the upstream proposal.** Draft a concrete artifact — a journal entry, an issue, or a patch file — that lands the fix at the authoring layer you identified. The artifact must include: (a) the exact files and lines affected, (b) the root cause (wrong directory numbers), (c) the blast radius (which downstream repos are affected), and (d) a request for the fix to be included in the next `/sync` run.

4. **Mark the local fix as a tourniquet.** Add a comment to each locally-fixed line that records: (a) this is a temporary local fix, (b) the upstream proposal reference (issue number or journal entry path), and (c) the expected expiry — when the next `/sync` will overwrite this fix. If the upstream fix has not landed by then, the tourniquet must be re-applied.

5. **Predict the revert.** State explicitly: what will happen to your local fixes when the next `/sync` runs from `loom/`? What is the one condition that would make the revert safe (i.e., the fix is no longer needed locally)?

## Model Answer

**Step 1 — Local fixes:**

| File                                | Line | Old text                                                       | New text                                                       |
| ----------------------------------- | ---- | -------------------------------------------------------------- | -------------------------------------------------------------- |
| `skills/05-kailash-mcp/skill.md`    | 42   | `See 07-eatp-reference for trust lineage patterns`             | `See 26-eatp-reference for trust lineage patterns`             |
| `skills/14-code-templates/skill.md` | 18   | `CARE philosophy constraints are defined in 08-care-reference` | `CARE philosophy constraints are defined in 27-care-reference` |
| `skills/29-pact/skill.md`           | 55   | `EATP integration patterns in 07-eatp-reference`               | `EATP integration patterns in 26-eatp-reference`               |

**Step 2 — Authority chain trace:**

All three skill files live in `kailash-coc-claude-rs/.claude/skills/`. This is a USE template repository. USE templates do not author skills — they receive them via `/sync` from `loom/.claude/`. Loom is the COC authority for all `.claude/` artifacts in USE templates. Therefore the files are authored in `loom/.claude/skills/`, and the permanent fix must be landed there.

Checking one level deeper: could `loom/` itself be receiving these files from `atelier/`? Skills are COC artifacts (framework-specific), not CC or CO artifacts. The authority chain rule is: `atelier/` authors CC+CO, `loom/` authors COC. Framework-specific skills are COC. Therefore `loom/` is the authoring layer, not `atelier/`.

**The permanent fix must be landed in `loom/.claude/skills/`.**

**Step 3 — Upstream proposal:**

```markdown
# DISCOVERY: Skill cross-references use stale directory numbers (07/08 instead of 26/27)

## What happened

Three skill files reference `07-eatp-reference/` and `08-care-reference/`, but the
actual directories are `26-eatp-reference/` and `27-care-reference/`. The references
predate the renumbering that moved EATP and CARE reference skills from slots 07/08 to
slots 26/27.

## Affected files (in loom/.claude/skills/)

- `05-kailash-mcp/skill.md` line 42 — references `07-eatp-reference`
- `14-code-templates/skill.md` line 18 — references `08-care-reference`
- `29-pact/skill.md` line 55 — references `07-eatp-reference`

## Blast radius

Every USE template that receives these skills via /sync inherits the broken references:

- kailash-coc-claude-py (confirmed affected by deduction — same source files)
- kailash-coc-claude-rs (confirmed affected by direct observation)
- kailash-coc-claude-rb (confirmed affected by deduction — same source files)
- All downstream projects that sync from these templates

## Fix

Replace `07-eatp-reference` → `26-eatp-reference` and `08-care-reference` →
`27-care-reference` in all three files at the loom/ level. Then run /sync to
propagate the fix to all USE templates.

## Request

Include this fix in the next /sync run so that local tourniquets in downstream
repos can be retired.
```

**Step 4 — Tourniquet annotations:**

Each locally-fixed line gets a comment:

```
See 26-eatp-reference for trust lineage patterns
<!-- TOURNIQUET: local fix for stale 07→26 renumbering. Upstream proposal:
     loom/ DISCOVERY entry pending. Expected expiry: next /sync run.
     If /sync overwrites this line before the upstream fix lands, re-apply. -->
```

The tourniquet annotation serves three purposes: (a) the next developer who sees the comment knows this is not a permanent fix, (b) if `/sync` reverts it, the revert is detectable by searching for the missing comment, and (c) the upstream reference creates accountability — the tourniquet is not "done" until the upstream proposal is resolved.

**Step 5 — Revert prediction:**

When the next `/sync` runs from `loom/`, all three local fixes will be overwritten. The sync reads `loom/.claude/skills/` as the source of truth and writes into `kailash-coc-claude-rs/.claude/skills/`, replacing the locally-fixed files with the (still-broken) loom originals. The tourniquet comments will also be removed.

The one condition that makes the revert safe: **the upstream fix has already landed in `loom/.claude/skills/` before the sync runs.** In that case, the sync overwrites the local tourniquet with the permanent fix from loom, and both the local and upstream versions are now correct. The tourniquet was successfully retired by the sync that would otherwise have reverted it.

## Scoring

| Criterion                                                                                                     | Points |
| ------------------------------------------------------------------------------------------------------------- | ------ |
| All three local fixes are correct (26/27 not 07/08)                                                           | 1      |
| Authority chain correctly identifies loom/ as the authoring layer with explicit reasoning ruling out atelier/ | 3      |
| Upstream proposal is a concrete artifact with affected files, blast radius, and sync-run request              | 2      |
| Tourniquet annotations include upstream reference and expiry condition                                        | 2      |
| Revert prediction is explicit and names the safe-revert condition (upstream fix landed first)                 | 2      |
| **Total**                                                                                                     | **10** |

## Extensions

- **Variant A (deeper chain):** The practitioner discovers that the stale numbering originated in `atelier/co-codegen/`, not in `loom/`. The authority chain has one more hop: `atelier/` authored the cross-reference text, `loom/` received it via `/sync-to-coc`, and the USE template received it via `/sync`. The upstream proposal must now target `atelier/`, and the practitioner must explain why fixing at `loom/` would itself be a tourniquet (loom would be overwritten by the next atelier sync). Does the practitioner walk the full chain, or do they stop at loom?
- **Variant B (silent revert detection):** A week later, the practitioner opens the same USE template and notices the tourniquet comments are gone but the references still say `07-` and `08-`. A sync ran and reverted the fixes. The upstream proposal has not landed. The practitioner must now: (a) re-apply the tourniquet, (b) escalate the upstream proposal's priority, and (c) propose a mechanism to detect future silent reverts (e.g., a post-sync hook that greps for known tourniquet markers).
