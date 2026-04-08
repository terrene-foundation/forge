---
atom_id: SC-P-031
name: Frame venue strategy as a constraint envelope for argument scope
craft_layer: practitioner
destination: forge
craft_areas: [decide]
applications: [COR]
spec_lineage:
  - COR § EATP Integration — Constraint envelope defining paper scope boundaries
  - COR § Layer 4 — /publish command (venue-specific preparation)
  - CARE §02-architecture/02 — Constraint Envelopes
journal_evidence:
  - path: terrene/foundation/workspaces/care-thesis/journal/0010-DELIBERATION-multi-venue-strategy.md
    polarity: POSITIVE
    quote: "Dr. Hong is a Financial Economics PhD (Malatesta, UW). He can write formal models at AER/JPE quality. The CARE contribution touches 6 fields simultaneously. The strategy is: stake claim first (SSRN + AIES), then formalize for top economics journals, then empirics for management journals, then the big societal claim for Science/Nature."
practice_modality: case
status: verified
---

## The move

Treat venue selection not as a marketing decision (where will this paper be accepted?) but as a constraint envelope that shapes what the argument must and must not contain. Each venue defines constraints across multiple dimensions: methodology (does it require formal proofs, empirical evidence, or both?), scope (single-paper contribution or research-program announcement?), audience (disciplinary specialists or interdisciplinary?), format (page limits, citation density minimums, required sections). Choosing a venue first constrains the argument; choosing the argument first constrains the venue. The practitioner must decide which direction to work in and document the trade-offs.

## When it fires

Two trigger conditions:

1. A research contribution spans multiple fields and could target multiple venues. Without explicit venue-as-constraint framing, the argument oscillates between venues, producing prose that satisfies none.
2. A paper has been rejected at one venue and must be retargeted. The retargeting is not just formatting — it requires restructuring the argument to fit the new venue's constraint envelope.

## What it serves

COR § EATP Integration defines a "constraint envelope defining paper scope boundaries" — the venue IS the constraint envelope for academic research. CARE §02-architecture/02 (Constraint Envelopes) defines envelope dimensions (Financial, Operational, Temporal, Data Access, Communication). In research, these map to: page budget (Financial), methodology requirements (Operational), submission deadlines and review cycles (Temporal), access to data/empirics (Data Access), and citation density and cross-disciplinary reach (Communication).

## Evidence walkthrough

- **0010-DELIBERATION-multi-venue-strategy** (POSITIVE) — The entry documents a multi-venue strategy for the CARE thesis. The contribution "touches 6 fields simultaneously" (economics, management, philosophy, CS/AI, public policy, organizational design). The strategy uses sequential venues as progressive constraint tightening: SSRN + AIES first (wide scope, moderate formality, establish priority), then JFE/AER (tight scope, formal proofs required, empirical implications needed), then management journals (empirical validation), then Science/Nature (broadest impact). Each venue imposes different constraints that reshape the argument. The entry identifies the key insight: "The CARE paper is no longer 'an AIES paper.' It is the priority-establishing paper for a cross-disciplinary research program." This reframing changed what the paper must contain: "Be written at JMS/JFE quality, contain a formal model section with propositions, cite entry papers across all 6 fields, introduce 'functional agency' as a formal concept."

## How to drill it

The practitioner has a research contribution that could target 3 different venues. For each venue:

1. List the venue's constraint envelope: methodology requirements, scope boundaries, audience expectations, format constraints, citation density.
2. Identify what the argument must ADD to satisfy this venue (e.g., formal proofs, empirical section, broader impact statement).
3. Identify what the argument must REMOVE or deemphasize for this venue (e.g., philosophical motivation for an economics journal, mathematical proofs for a policy journal).
4. Assess: does the argument survive the addition and removal? Or does the venue's constraint envelope break the argument?

Scoring: completeness of constraint mapping, quality of add/remove analysis, and whether the practitioner identified a venue that breaks the argument (this is a valuable finding, not a failure).

## Common failure mode

The practitioner treats venue selection as "where will this paper get accepted?" rather than "what does this venue force me to add or cut?" The result is a paper submitted to the highest-prestige venue without restructuring, which gets desk-rejected because it does not meet the venue's methodology constraints. The 0010-DELIBERATION entry avoids this by explicitly identifying what each venue requires: AER needs formal proofs; AIES needs broader impact; Science needs accessibility.

## Related atoms

- SC-P-007 (session completion state): the venue defines the paper's completion state — what "done" means is venue-specific.
- SC-P-027 (hostile reviewer simulation): the reviewer persona should match the target venue's reviewer pool.
- SC-P-030 (academic register calibration): register must match the venue.
