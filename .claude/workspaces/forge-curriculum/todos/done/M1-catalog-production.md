---
milestone: 1
title: Catalog Production — from analysis artifact to FORGE library
dependency: none (first milestone)
estimated_sessions: 1
---

# Milestone 1: Catalog Production

Move the 57-atom skill catalog from workspace analysis into the production FORGE program structure. The workspace catalog is an analysis artifact; the production catalog is the library that downstream courses and co-codegen consume.

## Todos

### T1.1 Verification pass — promote draft → verified

Read every cited journal entry for all 57 atoms. For each `quote:` field, confirm verbatim match against the corpus cache. Fix the 3 cosmetic paraphrases flagged by red team (SC-B-003 evidence 3, SC-B-014 evidence 2, SC-P-005 evidence 2). Find a second evidence source for thin atoms SC-P-013 and SC-P-015. Update `status: draft` → `status: verified` for each passing atom.

**Acceptance**: 57/57 atoms at `status: verified`. 0 paraphrases remaining. Both thin atoms have ≥2 evidence entries.

### T1.2 PACT body-text qualification cleanup

Apply authoring rule 8: find all bare "PACT" in body text that is not already concept-number-qualified or in a verbatim quote. Add "(spec)", "(primitives)", or "(platform)" parenthetical as appropriate. Focus on SC-P-001, SC-B-005, SC-B-004, SC-B-017 (flagged by red team).

**Acceptance**: `grep` for bare body-text "PACT" without a concept number or layer qualifier returns 0 hits outside of verbatim quotes.

### T1.3 Cross-reference pass — populate Related Atoms sections

Read all 57 atoms' "Related atoms" sections. Many were populated at authoring time but only cross-reference 2-3 atoms. Now that the full catalog exists, expand each "Related atoms" section to reference all genuinely related atoms across the catalog. The overlap detector's 8 flagged pairs are the starting priority.

**Acceptance**: Every atom has ≥2 related-atom references. The 5-atom ablation/rule-optimization cluster (SC-P-002, SC-P-012, SC-P-016, SC-P-022, SC-A-008) has explicit teaching-sequence links.

### T1.4 Create production catalog directory

Create `lyceum/programs/forge/catalog/` with `brokerage/`, `artifact/`, `practitioner/` subdirs. Copy the 57 verified atoms from the workspace (`01-analysis/05-skill-catalog/`) into the production directory. Write a production README.md that is consumer-facing (not analysis-facing): indexed by craft area, by spec lineage, by practice modality, and by application tag.

**Acceptance**: `catalog/` directory exists at program root with all 57 atoms. Production README has 4 index views. Workspace copy remains as evidence (do not delete).

### T1.5 Update FORGE CLAUDE.md to reference production catalog

Add the catalog to CLAUDE.md's rules index and describe the production directory structure. The catalog is now a first-class FORGE artifact, not a workspace analysis output.

**Acceptance**: CLAUDE.md references `catalog/` with a one-line description of what it is and how downstream consumers use it.

### T1.6 Observation-event mining for thin atoms

The specs-first rule declares three evidence sources: specs, journals, and observations. The 893 high-signal observation events (workflow_pattern, error_occurrence, error_fix, connection_pattern) across 38 `.claude/learning/observations.jsonl` files have not been mined. Read the observation events for the thin atoms (SC-P-013, SC-P-015, and any others with single evidence entries). If high-signal events corroborate the atom's move, add them as supplementary evidence with `source_type: observation`. This is not a full observation mining pass — it is targeted at the atoms flagged as thin.

**Acceptance**: All atoms with single-source evidence have either (a) a second source added, or (b) an explicit note "no corroborating observation found as of YYYY-MM-DD" in the atom's frontmatter.

### T1.7 Flag stale terrene-naming.md "trinity" reference

`rules/terrene-naming.md` line 52 still says "CO sits in the trinity: CARE (philosophy) + EATP (protocol) + CO (methodology)". Per `forge-scope.md` §1: flag the discrepancy but do not silently edit template-synced rules from this repo. Create a file `catalog/upstream-flags.md` documenting this and any other stale upstream artifacts that need correction at loom/ level.

**Acceptance**: `catalog/upstream-flags.md` exists with the terrene-naming.md "trinity" discrepancy documented. No edits to terrene-naming.md itself.
