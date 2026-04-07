---
source: analyst (brokerage and governance lens)
date: 2026-04-07
session: forge-curriculum reframe (craft school) + atelier reorg awareness
destination: atelier/co-codegen (canonical method) + lyceum/programs/forge (curriculum that teaches the operator role)
status: pre-rigor-bar — do not use as curriculum source
note: this corresponds to ~half of co-codegen Phase 2a's intended output per the ecosystem-restructure brief
---

# The Brokerage and Governance Craft

The atelier-loom chain is run by an operator who performs a small set of repeated moves on inbound proposals, outbound merges, and cross-repo drift. This catalog distills those moves, the failure modes when they are skipped, and the judgment calls where the operator must reason from principle rather than rule.

## 1. The Operator Moves

| #   | Move                                              | One-line description                                                                                                                        | Evidence                                                                                         |
| --- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| M1  | Compute expected state                            | For each loom file, apply the correct variant overlay for the target — what the BUILD repo _should_ look like if freshly synced             | `loom/.claude/commands/sync.md` step 4                                                           |
| M2  | Stamp SDK version on proposal                     | `/codify` reads `pyproject.toml`/`Cargo.toml` and writes `sdk_version` into `.proposals/latest.yaml`                                        | `loom/journal/0022-DECISION-sdk-version-awareness.md`                                            |
| M3  | Detect stale proposals                            | At Gate 1, compare proposal's `sdk_version` against BUILD repo's current SDK version; mismatch = stale                                      | `loom/.claude/commands/sync.md` step 6                                                           |
| M4  | Classify inbound change (global / variant / skip) | sync-reviewer agent team reads source + BUILD versions, scans for language-specific tokens, presents consolidated tier                      | `loom/journal/0021-DECISION-autonomous-gate1-classification.md`                                  |
| M5  | Place artifact at canonical path                  | Global → `loom/.claude/{type}/{file}`; variant → `loom/.claude/variants/{lang}/{type}/{file}`                                               | `forge/.claude/rules/artifact-flow.md` Variant Overlay Semantics                                 |
| M6  | Cross-SDK alignment note                          | When a change is classified global, ask whether the _other_ SDK needs an equivalent adaptation                                              | `loom/.claude/commands/sync.md` step 8                                                           |
| M7  | Inventory the target before writing               | Read template's current `.claude/` before computing per-file merge decisions — never bulk-overwrite                                         | `loom/journal/0019-DECISION-sync-merge-not-copy.md`                                              |
| M8  | Preserve template-only files                      | Files that exist only in the target template are kept; sync is additive                                                                     | `loom/.claude/commands/sync.md` step 4 (TEMPLATE-ONLY)                                           |
| M9  | Update SDK pins on Gate 2                         | After merging artifacts, rewrite target's `pyproject.toml`/`Cargo.toml` Kailash pins to BUILD repo versions and run `uv sync`/`cargo check` | `loom/journal/0023-DECISION-sync-sdk-dependency-pins.md`                                         |
| M10 | Cascade VERSION                                   | Set target's `.claude/VERSION` `upstream.build_version` to loom/'s version after distribution                                               | `loom/journal/0015-DECISION-version-tracking-cascade.md`                                         |
| M11 | Verify hooks on disk                              | Every hook in `settings.json` must have a matching script file present after sync                                                           | `loom/.claude/commands/sync.md` step 11                                                          |
| M12 | Drift inspection                                  | `/inspect drift` diffs each BUILD repo against expected loom state to detect untracked divergence                                           | `loom/.claude/commands/inspect.md`                                                               |
| M13 | Manifest parity check                             | Run `scripts/check-manifest-parity.sh` to detect orphan variants, stale entries, and parity gaps                                            | `loom/journal/0029-DECISION-manifest-parity-automation.md`                                       |
| M14 | Guard `/codify` proposal generation               | Detect repo type (BUILD vs downstream); only BUILD repos may write `.proposals/latest.yaml`                                                 | `loom/journal/0046-DECISION-downstream-proposal-guard.md`                                        |
| M15 | Push CC/CO from atelier                           | `/sync-to-coc` mirrors only CC and CO tier files from atelier into loom; loom never edits CC/CO independently                               | `atelier/.claude/commands/sync-to-coc.md` and `loom/journal/0016-DECISION-co-authority-chain.md` |

## 2. Failure Modes (one citation per row)

| Failure                                            | What goes wrong                                                                                                                                  | Skipped move   | Evidence                                                                |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------- | ----------------------------------------------------------------------- |
| Silent template drift                              | BUILD-to-template direct sync produced 85% drift; only 15% of artifacts truly shared                                                             | M4, M7         | `loom/journal/0014-DISCOVERY-drift-audit.md`                            |
| Bulk-overwrite content loss                        | Old rsync semantics overwrote 14 Rust-numbered skill dirs with generic globals, creating 28 duplicate directories                                | M7             | `loom/journal/0019-DECISION-sync-merge-not-copy.md`                     |
| Orphan variants invisible to sync                  | 3 py skills existed on disk but had no manifest entry, so they were never delivered to the template since creation                               | M13            | `loom/journal/0028-RISK-manifest-integrity-gaps.md`                     |
| Misclassified variants overwrite language behavior | 5 rs variant files were marked `variant_only` despite having global equivalents; next sync would have silently overwritten Rust-specific content | M4, M5         | `loom/journal/0028-RISK-manifest-integrity-gaps.md`                     |
| Numbering collisions                               | rs BUILD created 24 BUILD-only skill dirs at numbers that meant something else globally (e.g., 05=enterprise vs 05=kailash-mcp)                  | M1, M4         | `loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md`          |
| Stale proposals against newer SDK                  | Without `sdk_version` stamping, proposals codified against v2.0.0 referenced APIs absent from v2.2.1, breaking template adopters                 | M2, M3         | `loom/journal/0022-DECISION-sdk-version-awareness.md`                   |
| Downstream pins lag releases                       | py template carried `kailash>=2.2.1` while BUILD repo shipped 2.3.0, so scaffolded projects pinned obsolete SDK                                  | M9             | `loom/journal/0023-DECISION-sync-sdk-dependency-pins.md`                |
| Downstream proposals with no upstream path         | `/codify` ran in downstream repos created `.proposals/latest.yaml` files that no operator would ever review, violating the authority chain       | M14            | `loom/journal/0046-DECISION-downstream-proposal-guard.md`               |
| Methodology drift between domains                  | Before atelier authority, loom edited CC/CO independently, drifting research/finance/codegen apart                                               | M15            | `loom/journal/0016-DECISION-co-authority-chain.md`                      |
| BUILD-to-USE wording leak                          | BUILD-only management agents (coc-sync, sync-reviewer, repo-ops) waste context in downstream USE repos                                           | M5 (exclusion) | `forge/.claude/rules/cc-artifacts.md` "No BUILD artifacts in USE repos" |

## 3. Judgment Calls

| #   | Call                                                                  | Criteria the operator uses                                                                                                                                                                                                                                                                                                                          |
| --- | --------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| J1  | Global vs variant for an inbound rule                                 | Does the rule reference language tokens (asyncio, Cargo, RSpec, GIL, PyO3) or only methodology? Language tokens → variant. Pure methodology → global. Reference: `loom/journal/0048-DECISION-gate1-rs-variant-classification.md` — most rs skill dirs that _mentioned_ Rust were still globals because the mentions were incidental, not structural |
| J2  | When to demote a global to per-variant rules                          | When a global rule has accumulated `if py: … if rs: …` branches, or when one language's needs invalidate the rule for another. Source: drift audit found 5 rules where rs gutted the rule to frontmatter-only because Python content was inapplicable (`loom/journal/0014-DISCOVERY-drift-audit.md`)                                                |
| J3  | Whether to refuse an inbound proposal                                 | Refuse if (a) proposal `sdk_version` mismatches BUILD repo's current — proposal is stale (`loom/.claude/commands/sync.md` step 6), (b) the change re-implements something already in atelier's CC/CO authority — return upstream to atelier instead (`loom/journal/0016-DECISION-co-authority-chain.md`)                                            |
| J4  | When to break a global rule into per-variant rules vs one-off variant | Single replacement → variant overlay. Persistent multi-section divergence with separate rationale per language → split. Bootstrap example: rb template took 12 variants of patterns/testing/zero-tolerance rather than splitting the global rule (`loom/journal/0027-DECISION-rb-template-bootstrap.md`)                                            |
| J5  | When to add a new template target vs reuse an existing one            | New target if the _audience_ differs (rs targets Rust SDK internals, rb targets Ruby app developers using Magnus) — even if the global content is shared. Reuse if audience matches (`loom/journal/0027-DECISION-rb-template-bootstrap.md`)                                                                                                         |
| J6  | Whether a flagged Gate 2 file overwrite is legitimate                 | If the template version contains USE-specific adaptations (downstream wording, audience softening), keep template version and re-classify upstream as variant. Reference: sync.md Gate 2 step 4 "MODIFIED" decision, `loom/.claude/commands/sync.md`                                                                                                |
| J7  | When to deprecate or quarantine a downstream repo                     | When `/inspect drift` shows a downstream repo with custom artifacts that no longer trace to any loom source, and the divergence is intentional (forked methodology). Hard signal: parity gaps that grow rather than close. Reference: `loom/journal/0029-DECISION-manifest-parity-automation.md` 91 parity gaps as standing condition               |
| J8  | When agent-team classification needs human override                   | Round-table disagreement (e.g., 3 agents global, 2 variant), or conceptual/architectural files where pattern-matching across language tokens is insufficient — both flagged as open questions in `loom/journal/0021-DECISION-autonomous-gate1-classification.md`                                                                                    |

## Files cited

- `/Users/esperie/repos/atelier/CLAUDE.md`
- `/Users/esperie/repos/loom/CLAUDE.md`
- `/Users/esperie/repos/loom/.claude/commands/sync.md`
- `/Users/esperie/repos/loom/.claude/commands/codify.md`
- `/Users/esperie/repos/loom/.claude/commands/inspect.md`
- `/Users/esperie/repos/loom/.claude/commands/repos.md`
- `/Users/esperie/repos/atelier/.claude/commands/sync.md`
- `/Users/esperie/repos/atelier/.claude/commands/sync-to-coc.md`
- `/Users/esperie/repos/training/forge/.claude/rules/artifact-flow.md`
- `/Users/esperie/repos/loom/journal/0013-DECISION-variant-architecture.md`
- `/Users/esperie/repos/loom/journal/0014-DISCOVERY-drift-audit.md`
- `/Users/esperie/repos/loom/journal/0015-DECISION-version-tracking-cascade.md`
- `/Users/esperie/repos/loom/journal/0016-DECISION-co-authority-chain.md`
- `/Users/esperie/repos/loom/journal/0019-DECISION-sync-merge-not-copy.md`
- `/Users/esperie/repos/loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md`
- `/Users/esperie/repos/loom/journal/0021-DECISION-autonomous-gate1-classification.md`
- `/Users/esperie/repos/loom/journal/0022-DECISION-sdk-version-awareness.md`
- `/Users/esperie/repos/loom/journal/0023-DECISION-sync-sdk-dependency-pins.md`
- `/Users/esperie/repos/loom/journal/0026-DECISION-loom-atelier-naming.md`
- `/Users/esperie/repos/loom/journal/0027-DECISION-rb-template-bootstrap.md`
- `/Users/esperie/repos/loom/journal/0028-RISK-manifest-integrity-gaps.md`
- `/Users/esperie/repos/loom/journal/0029-DECISION-manifest-parity-automation.md`
- `/Users/esperie/repos/loom/journal/0046-DECISION-downstream-proposal-guard.md`
- `/Users/esperie/repos/loom/journal/0048-DECISION-gate1-rs-variant-classification.md`
