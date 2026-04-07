---
atom_id: SC-P-020
name: Treat the second occurrence of a drift class as a structural signal, not another incident
craft_layer: practitioner
destination: forge
craft_areas: [attend-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §3.2 — Convention Drift (Failure Mode)
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/journal/0014-DISCOVERY-drift-audit.md
    polarity: NEGATIVE
    quote: "Only 15% of artifacts were truly shared (identical across all three). 66% of skill files had drifted."
  - path: loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md
    polarity: NEGATIVE
    quote: "12 numbers had different meanings (e.g., 05=enterprise in rs vs 05=kailash-mcp globally). 14 directories were Rust-specific versions of globals at different numbers."
  - path: loom/journal/0028-RISK-manifest-integrity-gaps.md
    polarity: NEGATIVE
    quote: "3 orphan py variant files — existed on disk but not in manifest, never synced to templates."
practice_modality: case
status: draft
---

## The move

When you see the same class of drift for the second time — even in a different repo, different artifact type, or different session — stop fixing the instance and find the mechanism that produces it. Once is an incident (fix and move on). Twice is structural (the system generates this class of drift faster than you can fix individual instances). At the second occurrence, the intervention shifts from instance-level repair to mechanism-level prevention: add a validation check, an automated parity scan, or a gate that blocks the drift-producing action.

## When it fires

The practitioner encounters a drift, divergence, or integrity gap that matches the pattern of a previous finding. The match is on the class of failure, not the specific artifact. Examples: "artifact exists on disk but not in manifest" is a class; it does not matter whether the artifact is a py variant, an rs variant, or a skill file. "Numbering scheme diverges between repos" is a class; it does not matter whether it is skill numbers, rule numbers, or version numbers. The trigger is recognition of class recurrence, which requires the practitioner to have cataloged the first occurrence's class, not just its specifics.

## What it serves

CO §3.2 (Convention Drift) warns that "the AI follows generic practices instead of organizational conventions" and that drift "MUST be addressed through explicit institutional knowledge encoding." Fixing individual drift instances without addressing the production mechanism is itself a form of convention drift: the convention is "we fix drift when we find it" instead of "we prevent drift from occurring." CO §17 (Single Source of Truth) requires that "each piece of institutional knowledge lives in exactly one place, with no contradictions." Repeated drift in the same class proves that the single-source-of-truth property is not enforced — the system allows contradictions to accumulate silently. The second occurrence is the evidence that enforcement is missing.

## Evidence walkthrough

- **0014-DISCOVERY-drift-audit** (NEGATIVE) — The first occurrence: a deep audit of py and rs COC templates revealed 85% artifact drift. 66% of skill files had diverged. The root cause was identified: "no mechanism existed to distinguish intentional (language-specific) from accidental (sync failure) drift." This led to the variant architecture decision, which was the mechanism-level fix.
- **0020-DISCOVERY-rs-skill-numbering-divergence** (NEGATIVE) — The second occurrence of the same drift class in a different repo: kailash-rs had 24 skill directories using a completely different numbering scheme from the canonical one. 12 numbers had different meanings across repos. The root cause was the same class: `/codify` sessions created artifacts without reference to a global scheme, and no gate checked alignment. This was the same structural gap — no enforcement of single-source numbering — in a different manifestation.
- **0028-RISK-manifest-integrity-gaps** (NEGATIVE) — The third occurrence: red team found 3 orphan py variant files existing on disk but not in the manifest, plus 5 misclassified rs variants. Same class again: artifacts exist in one location but are invisible to the system that should track them. By this third occurrence, the journal explicitly recommended automated pre-sync validation — the mechanism-level fix that the second occurrence should have triggered.

## How to drill it

Present the practitioner with a sequence of three journal entries from the same project, spaced across sessions. Entry 1: "found 5 agents referencing deleted skill directories, fixed the references." Entry 2 (different session): "found 3 rules referencing a renamed command, fixed the references." Entry 3 (different session): "found 2 skills referencing a moved file path, fixed the references." The practitioner must: (1) identify that all three are the same class (dangling references after rename/delete), (2) explain why entry 2 — not entry 3 — was the moment to shift from instance-fix to mechanism-fix, and (3) propose the mechanism (a post-rename reference sweep, a pre-commit hook that checks for dangling paths, or a structural rename tool that updates references atomically). Score on timing: the practitioner who waits until entry 3 to recognize the pattern has already let one preventable instance through.

## Common failure mode

The practitioner treats each occurrence as unique because the specifics differ (skill files vs variant files vs numbering schemes). The class-level pattern ("artifact divergence between locations without enforcement") is invisible because the practitioner catalogs findings by artifact type rather than by failure class. The correction is to maintain a running list of drift classes, not just drift instances, so that the second occurrence of a class triggers an automatic escalation from fix to prevent.

## Related atoms

- SC-B-010 (cross-repo divergence audit) — the audit that surfaces the first and second occurrences; this atom teaches the practitioner what to do at the second occurrence
- SC-B-012 (manifest-to-filesystem parity check) — the mechanism-level fix that the second or third occurrence should have triggered
- SC-P-021 (reference sweep after rename) — a specific mechanism-level fix for one common drift class
