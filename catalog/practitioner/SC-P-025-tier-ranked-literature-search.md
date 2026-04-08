---
atom_id: SC-P-025
name: Run a tier-ranked literature search with source verification
craft_layer: practitioner
destination: both
craft_areas: [attend-how]
applications: [COR, COE]
spec_lineage:
  - CO §1 — Institutional Knowledge Thesis
  - CO §49 — Verification requirements
  - COR § Layer 1 — literature-researcher agent (systematic discovery and deep reading)
  - COR § Layer 3 — no unverified citations in final draft (hard rule)
journal_evidence:
  - path: terrene/foundation/workspaces/care-thesis/journal/0009-LITERATURE-ai-agency-tool-vs-agent.md
    polarity: POSITIVE
    quote: "Search conducted 2026-03-15 across seven areas. This is the most important literature gap in the CARE paper. Below are VERIFIED papers with full citations, organized by relevance tier."
  - path: terrene/foundation/workspaces/care-thesis/journal/0052-LITERATURE-part1-para1-post2020-gap-check.md
    polarity: POSITIVE
    quote: "Systematic search for post-2020 publications relevant to the 'What is the human for?' opening and the broader Part I argument."
practice_modality: drill
status: verified
---

## The move

When building a literature base for a research argument, run a systematic search organized by topic area. For each paper found, record: full citation (authors, year, title, journal, DOI), verification status (VERIFIED against publisher/database, or UNVERIFIED), relevance tier (TIER 1: must-cite, reviewers will expect it; TIER 2: should-cite, strengthens argument; TIER 3: contextual, may cite), and a structured assessment: what it argues, what it does NOT claim, and how it connects to your argument. The tier ranking is relative to YOUR paper's argument, not to the paper's general importance. A Nature paper can be TIER 3 if it is tangential to your specific claim.

## When it fires

Two trigger conditions:

1. A research argument needs grounding in prior work. Without systematic search, the literature base is whatever the author (or AI) recalls from training data — which is selection bias dressed as scholarship.
2. A reviewer challenge requests additional citations or questions whether the author is aware of competing work. A tier-ranked search provides the answer: "We considered these 14 papers; here is why we cited 3 and not the other 11."

## What it serves

CO §1 (Institutional Knowledge Thesis) states that output quality depends on institutional context. In research, the literature base IS the institutional knowledge — it is what the author knows about the field. A literature search that is unsystematic or unverified is an institutional knowledge gap that produces fabricated citations, missed competitors, and reviewable omissions. COR § Layer 1 defines a literature-researcher agent specialization whose knowledge includes "academic search protocols, citation verification, source assessment." COR § Layer 3 enforces a hard rule: "no unverified citations in final draft." The tier-ranked search is how both requirements are met.

## Evidence walkthrough

- **0009-LITERATURE-ai-agency-tool-vs-agent** (POSITIVE) — The entry documents a systematic search across seven topic areas for the CARE thesis. Each paper is tagged VERIFIED with publisher confirmation. Papers are organized into TIER 1 (6 must-cite papers including Humberd & Latham 2026, Floridi 2025, Hadfield & Koh 2025), TIER 2 (6 should-cite papers), and TIER 3 (3 contextual papers). Each entry includes: what it argues, what it does NOT claim, connection to CARE, and reviewer notes ("A JMS reviewer will absolutely expect this cited"). The search also identifies 4 gaps where no paper occupies the intersection (e.g., "Hart & Moore + AI governance is an unoccupied intersection"). The gap identification is as valuable as the found papers — it locates the contribution space.

- **0052-LITERATURE-part1-para1-post2020-gap-check** (POSITIVE) — A focused search for post-2020 publications relevant to a specific section. 14 papers evaluated across 7 categories. Three critical gaps identified: Dell'Acqua et al. (2023) as must-cite empirical evidence, Puranam (2021) as framework precedent, Humberd & Latham (2026) for differentiation. Crucially, the entry also identifies papers that should NOT be cited: "Eloundou et al. 2023, Acemoglu 2024 should NOT be cited because they would position the paper as AI-and-labor rather than organizational economics." The positioning insight — which citations to exclude — is a craft move, not just a search result.

## How to drill it

The practitioner selects a research claim they are making (e.g., "AI creates agency costs in organizations"). Then:

1. Define 5-7 topic areas relevant to the claim (e.g., agency theory + AI, organizational economics + automation, human-AI collaboration).
2. For each area, search at least 3 databases or sources (publisher sites, Google Scholar, SSRN).
3. For each paper found, record: full citation, VERIFIED/UNVERIFIED, TIER 1/2/3, what it argues, what it does NOT claim, connection to your argument.
4. Identify at least 2 gaps: intersections where no paper exists. These are potential contribution spaces.
5. Identify at least 1 paper that should NOT be cited and explain why (positioning risk).

Scoring: completeness of tier ranking (did every paper get a tier?), verification status (did you check publisher sites, not just training data?), gap identification quality, and positioning insight (the "do not cite" list).

## Common failure mode

The practitioner delegates the literature search entirely to the AI and accepts the results without verification. The AI produces plausible-sounding citations that may be fabricated, misattributed, or stale. The CARE thesis search explicitly marks each paper VERIFIED with the verification source (e.g., "Wiley Online Library, ResearchGate, JMS Twitter announcement"). Without verification, the tier ranking is unreliable because it is based on the AI's training data, not on the actual paper's content.

## Related atoms

- SC-P-025 (this) → SC-P-026 (citation integrity audit): the search produces citations; the audit verifies them against sources.
- SC-P-025 (this) → SC-P-029 (post-publication gap check): the initial search establishes the base; the gap check maintains it.
- SC-B-006 (multi-round convergence): literature search convergence — when successive search rounds produce no new TIER 1 papers, the search is converging.
