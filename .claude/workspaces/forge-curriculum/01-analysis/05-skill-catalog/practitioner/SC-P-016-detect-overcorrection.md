---
atom_id: SC-P-016
name: Detect overcorrection in both directions as a single contamination failure
craft_layer: practitioner
destination: forge
craft_areas: [behaviour, attend-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §3.3 — Safety Blindness (Failure Mode)
  - CO §1 — Institutional Knowledge Thesis
journal_evidence:
  - path: loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md
    polarity: NEGATIVE
    quote: "Agents overcorrected to appear 'vanilla.' The zero-tolerance test agent said 'file a ticket and move on' — but this was contamination from trying too hard to ignore visible rules, not actual baseline behavior."
practice_modality: case
status: draft
---

## The move

When an agent produces output that is too confidently aligned with a directive OR too confidently opposed to it, treat both extremes as the same failure: contamination. The practitioner checks whether the output reflects genuine reasoning or is performing compliance (or performing defiance). The diagnostic is to compare against a clean-room baseline — an isolated instance with no access to the rules in question. If the clean-room output differs from both the compliant and the defiant output, both were contaminated.

## When it fires

Two trigger conditions:

1. An agent asked to test a baseline (without rules) produces output that is conspicuously the opposite of the rules it was told to ignore. The "trying too hard to be vanilla" signal: the output reads like a deliberate negation of the rules rather than a genuine absence of them.
2. An agent in a rule-loaded session produces output that parrots rule language verbatim in contexts where the rule does not apply. The "over-comply" signal: the output reads like rule recitation rather than judgment.

Both signals indicate that the rules are distorting the agent's reasoning rather than informing it.

## What it serves

CO §3.3 (Safety Blindness) warns that "the AI deprioritizes safety measures unless explicitly enforced." The overcorrection failure is Safety Blindness inverted: instead of deprioritizing rules, the agent over-indexes on them — either by performing compliance when the rule is present, or performing defiance when told to ignore it. Neither behavior reflects genuine reasoning about the rule's applicability. CO §1 (Institutional Knowledge Thesis) states that output quality depends on institutional context surrounding the model. When the context contaminates rather than informs — when the agent cannot reason independently of whether rules are visible — the institutional knowledge is acting as a distortion field rather than a quality floor.

## Evidence walkthrough

- **0049-DISCOVERY-coc-token-ablation-experiment** (NEGATIVE) — The Phase 1 in-session ablation produced unreliable results because agents told to "respond as vanilla Claude without project rules" overcorrected in both directions. The zero-tolerance test agent said "file a ticket and move on" — the opposite of the zero-tolerance rule — but clean-room testing (Phase 2) revealed that actual baseline Claude without rules says "Fix it. Don't skip, don't mark xfail, don't leave for later" — close to the rule. The in-session agent was not demonstrating baseline behavior; it was performing anti-compliance. The contamination ran symmetrically: some agents performed better than baseline (residual rule influence) and others performed worse (overcorrection). Only the clean-room comparison made the contamination visible.

## How to drill it

Present the practitioner with three agent outputs on the same task: (A) from a rule-loaded session, (B) from an in-session agent told to "ignore the rules," and (C) from a clean-room session with no rules present. Output B says the opposite of the rule; output C partially agrees with the rule on its own merits. The practitioner must: (1) identify that B is contaminated, not baseline, (2) explain why B's defiance is evidence of rule awareness rather than rule absence, and (3) articulate the clean-room methodology that would have caught the contamination before it was trusted. Score on whether the practitioner treats B and A as equally unreliable for measuring the rule's necessity.

## Common failure mode

The practitioner trusts the "ignore rules" agent as a genuine baseline and concludes that the rule is essential because the no-rules agent produced bad output. This is the exact mistake the ablation experiment initially made. The corrected methodology (clean-room isolation) revealed that several rules classified as "essential" based on in-session ablation were actually model-native for Claude. The practitioner who cannot distinguish contaminated defiance from genuine baseline will over-invest in rules the model already follows.

## Related atoms

- SC-P-002 (model-vs-rule ablation) — the empirical methodology for determining which rules are model-native; this atom teaches the practitioner to detect when the methodology itself is compromised by contamination
- SC-P-012 (rule demotion judgment) — demotion decisions depend on accurate baseline measurement, which depends on detecting overcorrection
