---
atom_id: SC-B-012
name: Automate manifest-to-filesystem parity check and halt on staleness
craft_layer: brokerage
destination: co-codegen
craft_areas: [institutional-memory]
applications: [COC]
spec_lineage:
  - CO §49 — Quality Criteria — Enforcement Reliability
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/journal/0028-RISK-manifest-integrity-gaps.md
    polarity: NEGATIVE
    quote: "All were fixed in-session. The systemic risk is that there is no automated validation that catches these classes of errors before they cause silent data loss."
  - path: loom/journal/0029-DECISION-manifest-parity-automation.md
    polarity: POSITIVE
    quote: "First run result: 0 orphans, 0 stale, 91 parity gaps (pre-existing by design — rs has far more variant replacements than py/rb)."
practice_modality: drill
status: draft
---

## The move

Write an automated check that compares the distribution manifest (the YAML file declaring which files go where) against the actual filesystem. The check detects three classes of failure: orphan files (on disk, not in manifest), stale entries (in manifest, not on disk), and parity gaps (variants covering some targets but not all). Run the check before every /sync. Halt the sync if orphans or stale entries are found; report parity gaps for triage but do not halt on them.

## When it fires

Before every /sync invocation and after every /codify that creates new variant files. Also fires as a standalone audit when a red team pass needs to verify manifest integrity. The trigger is "I am about to use the manifest as a source of truth for distribution" — if the manifest disagrees with the filesystem, the distribution will silently lose or misroute content.

## What it serves

CO §49 (Enforcement Reliability) asks: "Do critical rules have hard enforcement?" The manifest is a critical rule — it declares what gets distributed. Without an automated parity check, the manifest is a soft suggestion, not a hard enforcement mechanism. Silent orphans accumulate until a red team manually discovers them. CO §17 (Single Source of Truth) requires no contradictions; a manifest that says file X exists while the filesystem says it does not is a contradiction at the infrastructure level.

## Evidence walkthrough

- **0028-RISK-manifest-integrity-gaps** (NEGATIVE) — Red team found 8 findings in the sync system, 4 CRITICAL, including 3 orphan py variant files that existed on disk but not in manifest and 5 rs variant files misclassified as variant_only. All were fixed in-session, but the entry explicitly names the residual risk: no automated pre-sync validation exists, so new orphan variants or misclassifications can accumulate silently until the next manual red team.
- **0029-DECISION-manifest-parity-automation** (POSITIVE) — Records the creation of `scripts/check-manifest-parity.sh` to detect orphans, stale entries, and parity gaps. The script uses `comm` for set operations and parses both `variants:` and `variant_only:` sections. The first run found 0 orphans, 0 stale, and 91 parity gaps (pre-existing by design). The entry also asks whether the parity checker, had it existed earlier, would have caught the 3 orphaned py skills during the session that created them.

## How to drill it

Create a mock sync-manifest.yaml with 15 entries (10 global, 3 variant:py, 2 variant:rs). Create a filesystem with 16 files (the 15 plus one orphan variant not in the manifest) and remove one file that the manifest references (creating a stale entry). Write a bash or Python script that produces three lists: orphans, stale entries, parity gaps. The script must exit non-zero if orphans or stale entries are found. Score: does the script correctly identify both the orphan and the stale entry? Does it correctly report but not fail on parity gaps?

## Common failure mode

Practitioner writes the parity check but makes it advisory-only (prints warnings, exits 0). The sync runs anyway, distributing from a stale manifest. The fix is exit non-zero on orphans or stale entries, wiring the check into the sync command's pre-flight so it is not optional.

## Related atoms

- SC-B-010 (drift detection audit) — the audit that the parity check operationalises at the manifest level.
- SC-B-001 (read-then-merge sync) — the sync command that the parity check gates.
