---
atom_id: SC-P-026
name: Detect misquotation and specification drift between source and citation
craft_layer: practitioner
destination: both
craft_areas: [judge]
applications: [COR, COC, COE]
spec_lineage:
  - CO §49 — Verification requirements
  - COR § Layer 3 — no fabricated references (FATAL, blocks submission)
  - COR § Layer 3 — no unverified citations in final draft (hard rule)
  - COR § Layer 1 — claims-verifier agent (factual and attributive claim verification)
journal_evidence:
  - path: terrene/foundation/workspaces/care-thesis/journal/0004-TEACH-parasuraman-riley-misuse.md
    polarity: POSITIVE
    quote: "Common miscitation to avoid: Do not treat misuse/disuse/abuse as user failures only. Abuse is explicitly about designer failure — building systems that set humans up to fail."
  - path: terrene/journal/0006-DISCOVERY-pact-skill-thesis-drift.md
    polarity: NEGATIVE
    quote: "The SKILL was rewritten from memory during the knowledge-base refactor (commit 4b4862e) without cross-referencing the actual thesis. The previous version was SDK-level documentation from kailash-pact, so it was correctly rejected per journal 0004. But the replacement was written from recall rather than from the source document."
practice_modality: case
status: verified
---

## The move

When citing a source — whether a published paper, a specification document, or a prior version of your own work — read the actual source and verify that your citation accurately represents what it says. Two specific failure modes to check: (1) misquotation, where a widely-cited claim is attributed to a paper that does not actually make that claim, and (2) specification drift, where a summary or derivative document diverges from the authoritative source because it was written from memory rather than from the source text.

## When it fires

Three trigger conditions:

1. A citation is being used for the first time in a research artifact. The author (or AI) recalls the paper's contribution from memory and attributes a specific claim. Verification catches cases where the claim is misattributed or fabricated.
2. A derivative document (skill file, summary, reference card) is rewritten after a source document changes. Drift between the derivative and the source is invisible without explicit cross-check.
3. A reviewer challenges a specific citation. The author must be able to defend the attribution by pointing to the exact passage in the source.

## What it serves

COR § Layer 3 designates fabricated references as FATAL — the highest severity guardrail in any CO application. This severity is warranted because fabricated or misattributed citations in published research are irretractable: once in arXiv or a journal, the error persists in the scholarly record. CO §49 establishes verification requirements for all institutional knowledge claims. The citation integrity audit is the research-domain instantiation of this requirement.

## Evidence walkthrough

- **0004-TEACH-parasuraman-riley-misuse** (POSITIVE) — The entry documents a specific misquotation pattern: Parasuraman & Riley (1997) is "widely cited for 'misuse and disuse' of automation" but the taxonomy is actually misuse, disuse, AND abuse — where abuse is explicitly about designer failure, not user failure. The entry flags this: "Common miscitation to avoid: Do not treat misuse/disuse/abuse as user failures only. Abuse is explicitly about designer failure — building systems that set humans up to fail." This is the positive case: the author caught the misquotation BEFORE it entered the paper by reading the actual source. The catch matters because the abuse category "is directly relevant to CARE's argument that the problem is architectural, not behavioral" — citing only misuse/disuse would undermine the paper's own thesis.

- **0006-DISCOVERY-pact-skill-thesis-drift** (NEGATIVE) — The entry documents a specification drift failure: after the knowledge-base refactor, PACT SKILL.md was "rewritten from memory" and diverged from the PACT thesis in 7 ways: address format (dots vs dashes), a 5-step algorithm sourced from the SDK instead of the spec, 4 vs 5 element count, missing bridges, missing emergency bypass, missing EATP mapping. Root cause: "The replacement was written from recall rather than from the source document." The discovery was made during red team cross-checking, not during authoring. The entry proposes: "Should all reference skills have a mandatory cross-check step after rewriting — a 'verify against source' gate before committing?" This is the negative case: the absence of a citation integrity audit during authoring produced 7 discrepancies that required a separate red team pass to discover.

## How to drill it

The practitioner receives a research paragraph containing 3 citations. For each citation:

1. Read the actual source (abstract + relevant section at minimum; full paper if the claim is specific).
2. Verify that the attributed claim appears in the source. Record: "Claim X attributed to Author (Year) — VERIFIED at [section/page]" or "NOT FOUND — source actually says [Y]."
3. If the citation is correct, check for completeness: does the source also say something that qualifies or contradicts the claim? Record any qualifications.
4. If the citation is incorrect, classify the error: misquotation (wrong claim attributed), fabrication (paper does not exist), or drift (derivative diverged from source).

Scoring: number of errors caught, accuracy of error classification, and quality of correction (did the practitioner identify what the source actually says, not just that the citation is wrong?).

## Common failure mode

The practitioner verifies citations by asking the AI "does this paper say X?" — which is circular, since the AI may have generated the misattribution in the first place. Verification must use the actual source text (PDF, publisher page, library database), not the AI's recall of the source. The PACT drift case illustrates: the rewrite was plausible and detailed, but "correct-sounding content from the wrong layer" (SDK vs spec) is the hardest class of error to detect without source verification.

## Related atoms

- SC-P-025 (tier-ranked literature search): the search finds papers; this atom verifies the citations are accurate.
- SC-B-005 (spec-to-code conformance audit): the codegen analogue — audit code against spec. This atom audits citations against sources.
- SC-P-015 (ask for contradictions): "what would invalidate this citation?" is a falsification question applied to attribution.
