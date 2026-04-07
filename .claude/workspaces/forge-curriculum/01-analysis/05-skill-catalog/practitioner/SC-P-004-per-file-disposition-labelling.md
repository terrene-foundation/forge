---
atom_id: SC-P-004
name: Label every file with a disposition before starting implementation
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - CO §33 — Phase — Analyze
  - CO §29 — Evidence-Based Completion
journal_evidence:
  - path: loom/kaizen-cli-rs/workspaces/claude-code-analysis/journal/0014-DISCOVERY-existing-codebase-architecture.md
    polarity: POSITIVE
    quote: "Pre-implementation audit of what exists. Key finding: **solid foundation, needs extension not rewrite**."
  - path: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md
    polarity: NEGATIVE
    quote: "The existing `architecture.md` was written before codebase verification. After auditing the actual kailash-rs workspace, 12 corrections were identified."
  - path: loom/kailash-rs/workspaces/nexus-parity/journal/0001-DISCOVERY-nexus-already-exceeds-python-b0.md
    polarity: POSITIVE
    quote: "The estimated effort drops from '2 sessions' to **1 session** (approximately 4 hours). The primary deliverable shifts from 'implementing parity' to 'documenting how Rust already exceeds parity, plus implementing one small gap.'"
practice_modality: drill
status: draft
---

## The move

Before writing a single line of implementation code, walk every source file in the target module and label it with a disposition: SOLID (do not touch), NEEDS MIGRATION (structural change required), EXTEND (add new functionality to existing code), ENHANCE (improve existing functionality), REWRITE (replace entirely), or IMPLEMENT (file does not exist yet, must be created). Record the disposition in a table with file path, line count, status, and a one-sentence rationale. This table becomes the implementation plan's ground truth — agents stop re-deriving file status mid-edit, and effort estimates are grounded in the actual codebase rather than in an architecture document that may predate the code.

## When it fires

Two trigger conditions:

1. An implementation phase begins on an existing codebase (not greenfield). The signal is that the target directory already contains source files, and the brief or architecture document makes claims about those files' state.
2. An effort estimate in a brief or todo disagrees with what a quick audit of the codebase reveals. The entry 0001 (nexus-parity) shows the most dramatic version: the brief estimated 2 sessions of work, but a pre-implementation audit showed 4 of 5 abstractions already exceeded parity, dropping the estimate to 1 session.

## What it serves

CO §33 (Phase — Analyze) defines the analyze phase as "research the problem space." For an existing codebase, the problem space IS the codebase — its current state, not the architecture document's description of its intended state. Per-file disposition labelling is the concrete move that makes "analyze the existing codebase" actionable rather than aspirational. CO §29 (Evidence-Based Completion) requires "evidence for completion claims — not just assertions, but verifiable proof." When an implementation phase claims "this file is done," the disposition label is the baseline: if the label said EXTEND and the diff shows only additions to the existing structure, the claim is consistent. If the label said SOLID but the file was rewritten, something went wrong in planning.

## Evidence walkthrough

- **0014-DISCOVERY-existing-codebase-architecture** (POSITIVE) — The entry is a worked example of per-file disposition labelling applied to 21 source files in kaizen-cli-rs. Every file gets a status (SOLID, NEEDS MIGRATION, EXTEND, ENHANCE, REWRITE, IMPLEMENT, COMPLETE, PARTIAL, REDEFINE) and a one-sentence rationale. The result: "solid foundation, needs extension not rewrite" — a finding that prevents wasteful rewrites of working code and focuses implementation on the 6 files that actually need change. The table also preserves architecture decisions already made (e.g., "LLM via callback, not HTTP client in loop") that an agent without this table would re-derive or contradict.
- **0001-DISCOVERY-architecture-doc-has-12-corrections** (NEGATIVE) — The architecture document for kailash-ml was written before anyone audited the actual codebase. When the audit ran, 12 corrections surfaced — including 3 critical ones (polars version 14 minor behind, ndarray version mismatch at the tract boundary, ort pre-release semver issue). The disposition labelling move was NOT made before the architecture doc was written; it was made retroactively, and the cost was an entire audit session to fix what should have been caught before the first todo was drafted.
- **0001-DISCOVERY-nexus-already-exceeds-python-b0** (POSITIVE) — The brief framed Nexus parity as a 2-session effort. A pre-implementation audit of 30 source files (21,835 lines) revealed that Rust already exceeded Python in 4 of 5 abstractions, with only a ~360-line gap remaining. The disposition labelling (implicit in the audit) cut the effort estimate in half and shifted the deliverable from "implement parity" to "document existing superiority plus one small addition." Without this audit, the practitioner would have spent a full session implementing features that already existed.

## How to drill it

Give the practitioner a module with 10-15 source files plus an architecture document or brief that describes the intended implementation. The architecture document should contain at least 3 inaccuracies about the current codebase state (version mismatches, features described as missing that already exist, features described as complete that are stubs). The practitioner must:

1. Read each source file and assign a disposition (SOLID / EXTEND / ENHANCE / REWRITE / IMPLEMENT / NEEDS MIGRATION).
2. Produce a table with file path, line count, disposition, and one-sentence rationale.
3. Compare their table against the architecture document and list every discrepancy.
4. Revise the effort estimate based on the table, not the document.

Score on the number of architecture-vs-codebase discrepancies found (target: all 3+) and on whether the dispositions are justified by file content rather than by the architecture document's claims. A practitioner who assigns dispositions by reading the architecture document instead of the source files is failing the drill even if the labels happen to be correct.

## Common failure mode

The practitioner reads the architecture document or brief, trusts its description of the codebase, and starts implementing. Mid-implementation, they discover that File X is labeled "needs rewrite" but is actually solid, or that File Y is labeled "complete" but has a critical version mismatch. Each discovery forces a context switch, a re-plan, and often a revert. The kaizen-kz entry (0014) shows the corrective: a 21-file pre-implementation audit that took one session but saved multiple sessions of mid-implementation re-derivation.

## Related atoms

- SC-P-001 (adjacent-but-disconnected modules) — disposition labelling surfaces integration gaps between modules, not just individual file status.
- SC-P-005 (friction-gradient inversion diagnosis) — a disposition audit can reveal friction-gradient inversions by showing which files are well-documented but broken vs which are undocumented but solid.
