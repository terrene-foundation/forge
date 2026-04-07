---
destination: forge
purpose: staging-manifest
generated: 2026-04-07
---

# Co-codegen Staging Manifest

## Routing Summary

| Destination                    | Count  | Action                                                                   |
| ------------------------------ | ------ | ------------------------------------------------------------------------ |
| `co-codegen` only              | 23     | Canonical in `atelier/co-codegen/`                                       |
| `both` (variant created)       | 13     | Canonical in `atelier/co-codegen/`; FORGE retains original with pedagogy |
| `forge` only                   | 21     | Stays in FORGE, not routed                                               |
| **Total routed to co-codegen** | **36** |                                                                          |
| **Total catalog**              | **57** |                                                                          |

## What Goes to `atelier/co-codegen/`

36 skill atoms: 23 co-codegen-only atoms (shipped as-is from `catalog/brokerage/` and `catalog/artifact/`) plus 13 `both` variants (shipped from `catalog/co-codegen-variants/`).

The variants differ from their FORGE originals in two ways:

1. "How to drill it" section replaced with "How to apply" — 2-3 sentences of methodology guidance for a non-FORGE CO ecosystem consumer.
2. FORGE-specific pedagogy language removed. Frontmatter carries `variant_of: {original_atom_id}` and `destination: co-codegen`.

### `both` Variants (13 files in `co-codegen-variants/`)

| Atom ID  | Name                                           | Craft Layer  |
| -------- | ---------------------------------------------- | ------------ |
| SC-B-004 | Specialist delegation as required first move   | brokerage    |
| SC-A-001 | Codify-with-explicit-NOT-codified              | artifact     |
| SC-A-002 | Frontend mock data detection                   | artifact     |
| SC-A-003 | Defense-in-depth codification                  | artifact     |
| SC-A-004 | Layer 3 hook security audit                    | artifact     |
| SC-A-006 | Negative tests for deny-by-default             | artifact     |
| SC-A-007 | Zero-config security audit                     | artifact     |
| SC-A-011 | Framework-first check before building          | artifact     |
| SC-A-012 | Author CLAUDE.md as Layer 2 master directive   | artifact     |
| SC-A-014 | Decompose milestones with approval gates       | artifact     |
| SC-P-013 | Estimate self-contradiction as signal          | practitioner |
| SC-P-015 | End prompts with "what would invalidate this?" | practitioner |
| SC-P-019 | Verify session-start continuity reaches model  | practitioner |

### co-codegen Only Atoms (23 files, shipped as-is)

**Brokerage (17):** SC-B-001 through SC-B-018, excluding SC-B-004 which is `both`.

**Artifact (6):** SC-A-005, SC-A-008, SC-A-009, SC-A-010, SC-A-013, SC-A-015.

Note: The routing manifest summary comment was corrected to 23 / 13 / 21 to match the per-entry tally.

## When

Next sync pass from `atelier/` to `co-codegen/`. FORGE does not write directly to `atelier/co-codegen/` (per `forge-scope.md` section 4 — dual-destination routing). The atoms are staged here; the sync pass picks them up.

## Proposed Directory Structure at Receiving End

```
atelier/co-codegen/
  skills/
    brokerage/
      SC-B-001-read-then-merge-sync.md
      SC-B-002-authority-chain-escalation.md
      SC-B-003-variant-classification.md
      SC-B-004-specialist-delegation.md          (from co-codegen-variants/)
      SC-B-005-spec-to-code-conformance.md
      SC-B-006-multi-round-redteam-convergence.md
      SC-B-007-worktree-per-agent.md
      SC-B-008-convergence-as-validation.md
      SC-B-009-cross-sdk-upstream-brokerage.md
      SC-B-010-drift-detection-audit.md
      SC-B-011-version-tracking-cascade.md
      SC-B-012-manifest-parity-automation.md
      SC-B-013-cross-workspace-dependency-mapping.md
      SC-B-014-explicit-supersession-journaling.md
      SC-B-015-must-rule-semantic-reinterpretation.md
      SC-B-016-bidirectional-cross-language-parity.md
      SC-B-017-verification-gradient-zone-assignment.md
      SC-B-018-pdp-pep-separation.md
    artifact/
      SC-A-001-codify-with-explicit-not-codified.md  (from co-codegen-variants/)
      SC-A-002-frontend-mock-data-detection.md       (from co-codegen-variants/)
      SC-A-003-defense-in-depth-codification.md      (from co-codegen-variants/)
      SC-A-004-layer3-hook-security-audit.md         (from co-codegen-variants/)
      SC-A-005-encapsulation-security-boundary.md
      SC-A-006-negative-tests-deny-by-default.md     (from co-codegen-variants/)
      SC-A-007-zero-config-security-audit.md         (from co-codegen-variants/)
      SC-A-008-rule-classification-critical-vs-advisory.md
      SC-A-009-progressive-disclosure-architecture.md
      SC-A-010-token-budget-baked-vs-external.md
      SC-A-011-framework-first-check.md              (from co-codegen-variants/)
      SC-A-012-claude-md-as-layer2-context.md        (from co-codegen-variants/)
      SC-A-013-agent-specialization-routing.md
      SC-A-014-milestone-decomposition.md            (from co-codegen-variants/)
      SC-A-015-layer2-context-structuring.md
    practitioner/
      SC-P-013-estimate-self-contradiction-signal.md (from co-codegen-variants/)
      SC-P-015-ask-for-contradictions.md             (from co-codegen-variants/)
      SC-P-019-cold-start-continuity.md              (from co-codegen-variants/)
```

## Constraints

- FORGE does NOT write directly to `atelier/co-codegen/` (per `forge-scope.md` section 4).
- Atoms tagged `co-codegen` or `both` are produced here with the tag; routing happens via the next sync pass.
- No duplication: `both` atoms have one canonical location (`atelier/co-codegen/`) and one reference (FORGE catalog original). The FORGE original retains its pedagogy sections; the co-codegen variant strips them.
- `forge`-only atoms (21 practitioner atoms) never leave this repo.
