---
atom_id: SC-B-006
name: Multi-round red team to numeric convergence
craft_layer: brokerage
destination: co-codegen
craft_areas: [attend-when, intervene-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §28 — Approval Gate
  - CO §49 — Quality Criteria — Enforcement Reliability
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: POSITIVE
    quote: "Red team Round 1 produced 4 CRITICAL, 6 HIGH, 5 MEDIUM findings across kailash-ml, Nexus transport refactor, and dependency sequencing."
  - path: loom/journal/0039-DECISION-redteam-round3-convergence.md
    polarity: POSITIVE
    quote: "Round 1: 15 findings → all resolved in journals 0033, 0035. Round 2: 8 findings → all resolved in journal 0038. Round 3: 3 gaps → all resolved in this journal (0039)."
  - path: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0003-RISK-kailash-ml-rs-redteam-round1.md
    polarity: POSITIVE
    quote: "Three red team agents attacked the 500KB analysis corpus from three angles: architecture gaps (contradictions/missing pieces), gradient boosting engine design (algorithmic correctness/features), and feasibility risks (practical implementation threats)."
  - path: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0005-DECISION-kailash-ml-rs-redteam-convergence.md
    polarity: POSITIVE
    quote: "Red team Round 1 produced 34 findings (2 CRITICAL, 14 HIGH, 16 MEDIUM, 2 LOW). Eight architectural decisions were made to resolve them. Round 2 verified each finding against the decisions."
practice_modality: brokerage-rep
status: draft
---

## The move

Run red team rounds against the same artifact or plan, count the findings by severity, resolve them, then run the next round. Define a numeric convergence threshold before the first round (e.g., zero CRITICAL + zero HIGH unresolved, or fewer than 3 new findings total). Refuse to declare "converged" or advance to the next phase until the threshold is met. Each round produces a journal entry with the count; the series is the convergence evidence.

## When it fires

At any CO §28 Approval Gate that precedes implementation — typically after analysis or after todos. The trigger is not "we feel confident" but "the finding count has monotonically decreased across rounds to below the threshold." Also fires mid-implementation when a red team round surfaces new findings that were not present in prior rounds, resetting convergence.

## What it serves

CO §28 (Approval Gate) requires human judgment at structural transitions. Multi-round red teaming operationalizes that judgment: the human sees a numeric trend (15 findings → 8 → 3 → 0) and can make a go/no-go decision on evidence rather than intuition. CO §49 (Quality Criteria — Enforcement Reliability) demands that critical rules be verified by intentional violation attempts; each red team round is exactly such an attempt, and the finding count measures how many violations still succeed.

## Evidence walkthrough

- **loom-meta 0034** (POSITIVE) — Round 1 of the terrene-stack-gaps red team. Three CRITICAL, six HIGH, and five MEDIUM findings across the 17-workspace plan. The entry does not declare convergence; it opens the series.
- **loom-meta 0039** (POSITIVE) — Round 3 of the same red team. Finding counts: Round 1 → 15, Round 2 → 8, Round 3 → 3. All three Round 3 gaps were resolved in the same entry. The entry explicitly declares "CONVERGED. Ready for /todos." The numeric monotonic decrease is the proof.
- **kailash-rs 0003** (POSITIVE) — Round 1 of the kailash-ml-rs red team, independent of the loom-meta series. Three agents attacked from three angles (architecture gaps, algorithmic correctness, feasibility). Total: 2 CRITICAL, 14 HIGH, 16 MEDIUM, 2 LOW. The multi-angle attack is what makes the round comprehensive.
- **kailash-rs 0005** (POSITIVE) — Round 2 convergence for kailash-ml-rs. Of the original 34 findings, 32 were resolved by eight architectural decisions; 2 remained partially resolved. The entry declares convergence and names the carry-forward items explicitly.

## How to drill it

Take an analysis artifact (a plan, an architecture document, or a set of todos). Run a single red-team pass and count findings by severity. Resolve each finding with an explicit decision. Run a second pass. The drill succeeds only if the practitioner (a) defined the convergence threshold before Round 1, (b) journaled each round with the count, and (c) refused to declare convergence while any CRITICAL or HIGH finding remained unresolved. The loom-meta 0034-0039 series (three rounds, three journal entries, one convergence declaration) is the reference pattern.

## Common failure mode

Practitioner runs one red team round, resolves the findings, and declares "done" without a second round to verify that the resolutions did not introduce new gaps. The single-round pattern catches existing issues but cannot detect resolution-introduced issues. The cure is the second round: it verifies the fixes and surfaces anything the first round missed because the attack surface changed.

## Related atoms

- SC-B-008 (convergence-as-validation) — uses convergence of independent red team agents to validate the audit method itself, rather than the artifact.
- SC-B-005 (spec-to-code conformance) — a conformance audit can be one angle within a multi-round red team.
