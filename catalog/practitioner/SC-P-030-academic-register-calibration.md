---
atom_id: SC-P-030
name: Calibrate academic register with detection-bias awareness
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COR]
spec_lineage:
  - COR § Principle 8 — Authentic Voice Preservation
  - COR § Layer 2 — academic-writing-style.md (Authentic Voice Preservation)
  - COR § Layer 3 — authentic voice preservation (soft rule)
journal_evidence:
  - path: terrene/foundation/workspaces/care-thesis/journal/0024-MARGIN-part1-academic-register-revision.md
    polarity: POSITIVE
    quote: "A conversational opening followed by formal model propositions creates tonal whiplash. The prose must signal from page one that this paper belongs in the organizational economics conversation."
practice_modality: case
status: verified
---

## The move

When co-writing academic prose with AI, calibrate the register (formality level, vocabulary, sentence structure) to the target venue AND to detection-bias awareness. Register calibration is a two-axis problem: (1) match the venue's conventions so reviewers recognize the paper as belonging to their field, and (2) avoid stylistic patterns that AI detection tools flag as false positives — not to hide AI assistance, but to prevent detection tools from wrongly flagging genuinely human-directed work.

## When it fires

Two trigger conditions:

1. The paper's formality level does not match the venue. A practitioner-voice draft targeting AER creates "tonal whiplash" when followed by formal propositions. A hyperformalized draft targeting AI & Society alienates the interdisciplinary audience.
2. The prose exhibits patterns known to trigger AI detection false positives: parallel lists of three, smooth transition words ("Furthermore", "Moreover"), hedging phrases ("It is important to note"), uniform sentence length. These are stylistic proxies, not indicators of intellectual origin — but they trigger automated detection tools.

## What it serves

COR § Principle 8 (Authentic Voice Preservation) requires that AI co-authored research reflect genuine human intellectual direction. The academic-writing-style.md rule file contains constraints grounded in published detection-bias research (Kobak et al. 2024, Reinhart et al. 2025, Liang et al. 2023). COR explicitly distinguishes detection-bias mitigation from concealment: "These are not about hiding AI use. They are about ensuring detection tools (which measure stylistic proxies, not intellectual contribution) do not wrongly flag genuinely human-directed work."

## Evidence walkthrough

- **0024-MARGIN-part1-academic-register-revision** (POSITIVE) — The entry documents a register calibration for the CARE paper. The trigger: "After the theoretical framework crystallized (Hart & Moore, Holmstrom, Fama & Jensen, four propositions), Part I needed to match the intellectual ambition." The calibration applies four specific rules: varied sentence length (short declarative + long formal), domain-specific precision (organizational economics vocabulary, not generic), no transition words between paragraphs ("each paragraph earns its position through argumentative necessity"), and first-person where appropriate. The entry also documents detection-awareness: "Academic AI detectors flag: parallel lists of three, smooth transition words, hedging phrases, generic academic filler, uniform sentence length. The revision avoids all of these." The key writing principle: "Every paragraph in this paper must read as if a financial economist and an AI governance researcher both nod."

## How to drill it

The practitioner receives a 500-word draft paragraph written in generic academic prose. Then:

1. Identify the target venue and its register conventions (e.g., AER: formal-but-direct; AI & Society: interdisciplinary-accessible).
2. Identify 3 detection-flag patterns in the draft (parallel triads, smooth transitions, hedging phrases, uniform sentence length).
3. Rewrite the paragraph eliminating the detection-flag patterns while matching the venue register.
4. Verify: does the rewrite preserve the argument? Does it match the venue? Are the detection-flag patterns gone?

Scoring: detection-flag elimination (did the practitioner find and fix the patterns?), register accuracy (does the rewrite match the venue?), argument preservation (did the rewrite distort the claim?).

## Common failure mode

The practitioner eliminates detection-flag patterns but replaces them with equally detectable alternatives — e.g., replacing "Furthermore" with "Building on this" (still a smooth transition). The goal is not to swap one flag for another but to restructure the prose so transitions are earned by argumentative necessity rather than inserted as connective tissue. The 0024-MARGIN entry demonstrates: "No transition words between paragraphs — each paragraph earns its position through argumentative necessity."

## Related atoms

- SC-P-032 (margin note as deliberation artifact): register decisions should be documented in margin notes explaining why a particular formality level was chosen.
- SC-P-034 (overclaim prevention): register calibration includes vocabulary precision — avoiding "novel" when you mean "first formal model" or "new application."
