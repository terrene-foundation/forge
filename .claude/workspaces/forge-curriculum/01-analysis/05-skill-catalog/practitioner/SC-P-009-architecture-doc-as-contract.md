---
atom_id: SC-P-009
name: Treat architecture documents as testable claims against the actual codebase
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - CO §29 — Evidence-Based Completion
  - CO §16 — Framework-First
journal_evidence:
  - path: loom/kailash-rs/workspaces/kailash-ml-crate/journal/0001-DISCOVERY-architecture-doc-has-12-corrections.md
    polarity: NEGATIVE
    quote: "The existing `architecture.md` was written before codebase verification. After auditing the actual kailash-rs workspace, 12 corrections were identified."
  - path: loom/kailash-rs/workspaces/nexus-parity/journal/0003-DISCOVERY-issues-overstate-gaps.md
    polarity: NEGATIVE
    quote: "The 5 Nexus issues claim capabilities are 'missing' from the Rust core, but code audit reveals ~60% already exists."
practice_modality: drill
status: draft
---

## The move

Read every factual claim in an architecture document, brief, or issue tracker, then verify each claim against the actual codebase by reading the relevant source files. Record each claim as CONFIRMED (code matches the document), CORRECTED (code contradicts the document, with the specific discrepancy), or ABSENT (code does not address the claim at all). Produce a numbered list of corrections. The architecture document is now a testable contract: implementation work proceeds from the corrected document, not the original.

## When it fires

Two trigger conditions:

1. An implementation phase begins and the brief or architecture document was written in a prior session (or by a different agent). The gap between writing and implementing is the window where the codebase can drift from the document's claims.
2. An issue tracker or gap analysis asserts that capabilities are "missing" or "need implementation." The assertion itself is a claim to be verified -- the capability may already exist, may partially exist, or may exist under a different name.

## What it serves

CO §29 (Evidence-Based Completion) requires evidence for completion claims, not just assertions. An architecture document IS a set of completion claims about the codebase's intended state. When those claims are unverified, they are assertions masquerading as evidence. The move converts assertions to evidence by checking each one. CO §16 (Framework-First) says to prefer composition of existing building blocks before building from scratch. An unverified architecture document that says "X is missing" when X already exists causes the practitioner to rebuild what already works -- the opposite of framework-first.

## Evidence walkthrough

- **0001-DISCOVERY-architecture-doc-has-12-corrections** (NEGATIVE) -- The kailash-ml-rs architecture document was written before anyone audited the actual codebase. A verification pass found 12 corrections: 3 critical (polars 14 minor versions behind, ndarray version mismatch at the tract boundary, ort using pre-release semver that cargo does not resolve), 4 high (candle is NOT an ONNX backend as the doc implies, PyO3 binding submodule pattern is unprecedented, Arrow zero-copy code example is wrong, ndarray-linalg should be optional), and 5 medium. The 12-correction list became the ground truth for implementation. Without this audit, practitioners would have implemented against wrong dependency versions, built an ONNX backend for a library that does not support ONNX, and used a code pattern (Arrow IPC) that does not work. Each correction is a case of an architecture-document claim that was testable and was found to be false.
- **0003-DISCOVERY-issues-overstate-gaps** (NEGATIVE) -- Five GitHub issues claimed Nexus capabilities were "missing" from the Rust core. A code audit revealed that approximately 60% already existed: auth plugin with JWT+RBAC+APIKey+RateLimit, CORS config with factories, rate limiting at both global and per-user levels, EventBus with pub/sub, session store with TTL, and a scheduler with cron expression parser. The genuine gaps were narrower: route-style endpoint registration, per-handler auth guards, convenience methods, and binding exposure. The scope shrank from "greenfield effort" to "40% new code, 30% wiring, 30% binding exposure." Without the code audit, the session would have estimated 2x the actual work and potentially reimplemented features that already existed.

## How to drill it

Provide the practitioner with a 2-page architecture document describing a module of 8-12 source files. The document should contain: 2 claims about dependency versions that are wrong (e.g., says "uses library X 2.0" when the code uses 1.8), 2 claims about capabilities that already exist but are described as "to be implemented," and 1 claim about a code pattern that is described incorrectly (e.g., says "uses pattern A" when the code actually uses pattern B). The practitioner must:

1. Read each factual claim in the architecture document.
2. For each claim, read the relevant source file and classify the claim as CONFIRMED, CORRECTED, or ABSENT.
3. Produce a numbered correction list with the specific discrepancy for each CORRECTED claim.
4. Revise the effort estimate based on the corrections (should decrease if capabilities already exist, should increase if dependency versions require migration).

Score on the number of discrepancies found (target: all 5), the specificity of the corrections (vague corrections like "version is wrong" score zero; specific corrections like "doc says polars 0.39, codebase uses 0.53, 14 minor versions of API changes" score full), and whether the effort estimate revision is directionally correct.

## Common failure mode

The practitioner treats the architecture document as authoritative and proceeds directly to implementation. Mid-implementation, they discover that a dependency version in the code does not match the document, or that a feature described as missing already exists. Each discovery forces a pause, a re-investigation, and often a re-plan. The kailash-ml-crate entry shows the extreme case: 12 corrections including 3 critical ones that would have caused compilation failures. The nexus-parity entry shows the effort-estimation version: issues claimed 5 capabilities were missing, but 60% already existed, meaning the session would have been planned for 2x the actual scope.

## Related atoms

- SC-P-004 (per-file disposition labelling) -- disposition labelling is the file-level version of this move; architecture-doc-as-contract is the document-level version. Both produce a ground-truth table that replaces assumptions with verified facts.
- SC-P-007 (session completion state) -- a completion-state definition built on an unverified architecture document inherits the document's errors. This move ensures the completion definition is built on corrected claims.
