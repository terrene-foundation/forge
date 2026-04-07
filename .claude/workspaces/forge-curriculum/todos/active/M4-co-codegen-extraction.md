---
milestone: 4
title: Co-codegen Extraction — dual-destination routing of methodology atoms
dependency: M1 (catalog must be verified and destination tags confirmed)
estimated_sessions: 1
---

# Milestone 4: Co-codegen Extraction

22 atoms are tagged `destination: co-codegen` and 11 are tagged `destination: both`. These contain methodology content that any CO ecosystem consumer should have — not just FORGE learners. This milestone extracts them into a form ready for `atelier/co-codegen/` on the next sync pass.

## Todos

### T4.1 Write routing manifest

Produce a machine-readable manifest listing every atom, its destination tag, and its routing action:
- `co-codegen` → canonical lives in co-codegen, FORGE references it
- `both` → canonical lives in co-codegen, FORGE gets a copy with curriculum scaffolding
- `forge` → stays in FORGE only, no co-codegen counterpart

The manifest is the input for the sync pass that distributes content to co-codegen.

**Acceptance**: `catalog/routing-manifest.yaml` with all 57 atoms, their destination, and the routing action. Total: 22 co-codegen + 11 both + 24 forge = 57.

### T4.2 Prepare co-codegen atom format

The co-codegen versions of atoms may need to be stripped of FORGE-specific pedagogy framing. For `co-codegen` atoms, the format is likely identical (they are methodology, not curriculum). For `both` atoms, produce a co-codegen version that removes the "How to drill it" section (drills are FORGE-specific) and replaces it with a "How to apply" section (methodology guidance for any CO consumer).

**Acceptance**: 11 `both` atoms have a co-codegen variant in `catalog/co-codegen-variants/` with drill→apply transformation. 22 pure co-codegen atoms need no transformation (they are already in the right format).

### T4.3 Write co-codegen staging README

Document what will go to `atelier/co-codegen/` and when. This is not the actual sync — FORGE does not write to co-codegen directly (see `rules/forge-scope.md` §4: "do not write content directly into atelier/co-codegen/ from a FORGE session"). The staging README names the content, the destination, and the next sync pass that carries it.

**Acceptance**: `catalog/co-codegen-staging.md` describing the 33 atoms (22 + 11) to be routed, with instructions for the sync pass.
