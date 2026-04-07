---
atom_id: SC-P-021
name: Grep the entire repo for dangling references immediately after every rename or delete
craft_layer: practitioner
destination: forge
craft_areas: [intervene-how]
applications: [COC]
spec_lineage:
  - CO §17 — Single Source of Truth
  - CO §15 — Progressive Disclosure
journal_evidence:
  - path: loom/journal/0011-RISK-redteam-r1-findings.md
    polarity: NEGATIVE
    quote: "RT3-1 | HIGH | coc-claude-rs agents reference deleted skills (10-governance, 32-kaizen-agents) | Updated 3 agent files to reference existing skills"
practice_modality: drill
status: draft
---

## The move

Immediately after removing, renaming, or moving any artifact (file, directory, function, command, skill), grep the entire repo for references to the old name or path. Update every reference in the same commit. Do not treat the rename as "done" until the sweep returns zero hits. This is a same-turn intervention, not a follow-up task: if the sweep is deferred, new content will be written against the stale references in the interval, compounding the debt.

## When it fires

Any time the practitioner performs a rename, delete, or move of a named artifact. The trigger is the action itself, not a discovered problem. This is a proactive sweep, not a reactive fix. It fires on every structural change, including: deleting a skill directory, renaming a rule file, moving a command from one location to another, renaming a function that is referenced in CLAUDE.md or agent definitions, and removing a workspace.

## What it serves

CO §17 (Single Source of Truth) requires that "each piece of institutional knowledge lives in exactly one place, with no contradictions." A rename that updates the artifact but not its references creates a contradiction: the artifact's new name is the truth, but references still point to the old name. The repo now contains two versions of the artifact's identity. CO §15 (Progressive Disclosure) structures knowledge as "master directive to index to topic to deep reference." When an index or agent definition references a deleted skill directory, the progressive-disclosure chain is broken: the reader follows the reference and finds nothing, or worse, finds a different artifact that now occupies the old name's slot.

## Evidence walkthrough

- **0011-RISK-redteam-r1-findings** (NEGATIVE) — Red team round 1 found that 3 agent files in kailash-coc-claude-rs still referenced deleted skill directories: `10-governance` and `32-kaizen-agents`. These directories had been removed in a prior session, but the agent definitions that referenced them were not updated. The finding was classified as HIGH severity because the agent definitions are the routing layer (Layer 1): an agent configured to reference a nonexistent skill directory cannot load the skills it needs, silently degrading its capability. The fix was straightforward (update 3 agent files), but the debt existed across multiple sessions between the delete and the red team discovery. A same-turn reference sweep would have caught it immediately.

## How to drill it

Give the practitioner a repo with 5 skill directories, 3 agent files, and a CLAUDE.md that references all 5 skills. Instruct them to delete one skill directory and rename another. After the practitioner makes the changes, run `grep -r` for the old names across the repo. If any hits remain, the drill fails. Then introduce a subtlety: one reference is in a YAML frontmatter `references:` list (not a prose mention), and one is in a markdown link `[text](path)`. Score on whether the practitioner's grep pattern catches both structured and unstructured references, and whether they update all hits in the same commit as the rename.

## Common failure mode

The practitioner greps for the exact filename but misses partial matches. A skill directory named `10-governance` might be referenced as `10-governance`, `skills/10-governance/`, `10-governance/SKILL.md`, or even just `governance` in a prose description. The sweep must cover both exact path matches and plausible substring matches. The second failure mode is treating the sweep as a separate task ("I'll clean up references in the next session") — by then, new content has been written that references the stale paths, and the sweep's scope has grown.

## Related atoms

- SC-P-020 (second-occurrence structural signal) — dangling references after rename are a drift class that recurs; the reference sweep is the mechanism-level prevention
- SC-B-010 (cross-repo divergence audit) — the cross-repo version of the same problem; this atom handles the single-repo case
- SC-P-004 (per-file disposition labelling) — disposition labelling before implementation catches files that will be renamed or deleted, enabling proactive reference sweeps
