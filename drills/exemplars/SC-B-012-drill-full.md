---
drill_id: DR-SC-B-012-FULL
source_atom: SC-B-012
difficulty: beginner
time_box_minutes: 15
---

# Automate Manifest-to-Filesystem Parity Check and Halt on Staleness

## Setup

The practitioner receives a working directory with the following structure:

**`sync-manifest.yaml`** (15 entries):

```yaml
globals:
  rules:
    - zero-tolerance.md
    - security.md
    - git.md
    - communication.md
    - testing.md
    - agents.md
    - autonomous-execution.md
    - cc-artifacts.md
    - documentation.md
    - journal.md

variants:
  py:
    rules:
      - python-environment.md
      - connection-pool.md
  rs:
    rules:
      - trust-plane-security.md

variant_only:
  py:
    skills:
      - nexus-agent-reference/
      - kaizen-multi-provider/
```

**Filesystem** (`.claude/` directory, 16 files):

```
.claude/rules/zero-tolerance.md
.claude/rules/security.md
.claude/rules/git.md
.claude/rules/communication.md
.claude/rules/testing.md
.claude/rules/agents.md
.claude/rules/autonomous-execution.md
.claude/rules/cc-artifacts.md
.claude/rules/documentation.md
.claude/rules/journal.md
.claude/variants/py/rules/python-environment.md
.claude/variants/py/rules/connection-pool.md
.claude/variants/py/skills/nexus-agent-reference/
.claude/variants/py/skills/kaizen-multi-provider/
.claude/variants/py/skills/dataflow-pool-patterns/    ← ORPHAN (on disk, not in manifest)
.claude/variants/rs/rules/trust-plane-security.md
```

**Deliberately planted issues:**

1. **One orphan variant:** `variants/py/skills/dataflow-pool-patterns/` exists on disk but is NOT in the manifest. This file will never be synced to any downstream template.
2. **One stale entry:** Add `framework-first.md` to the `globals.rules` section of the manifest, but do NOT create the file on disk. The manifest references a file that does not exist.
3. **One parity gap:** The `rs` section has `trust-plane-security.md` as a variant rule, but `py` does not have a corresponding variant (this is a legitimate parity gap -- Rust has a trust-plane crate, Python implements it differently).

## Task

1. Write a bash or Python script that reads `sync-manifest.yaml` and compares it against the actual filesystem. The script must detect three classes of failure:
   - **Orphan files:** files on disk under `.claude/variants/` that have no corresponding manifest entry
   - **Stale entries:** manifest entries that point to files that do not exist on disk
   - **Parity gaps:** variant entries that exist for some targets but not all declared targets (report `variants:` only, not `variant_only:` which is intentionally single-target)
2. The script must exit with a non-zero exit code if orphans or stale entries are found. Parity gaps should be reported but NOT cause a non-zero exit.
3. Run the script against the provided directory. Verify that it correctly identifies:
   - 1 orphan: `variants/py/skills/dataflow-pool-patterns/`
   - 1 stale entry: `globals/rules/framework-first.md`
   - 1 parity gap: `trust-plane-security.md` exists in `rs` but not `py`
4. Explain why the script must exit non-zero on orphans and stale entries but not on parity gaps.

## Model Answer

**Script** (`check-manifest-parity.sh`):

```bash
#!/bin/bash
set -euo pipefail

MANIFEST="sync-manifest.yaml"
BASE=".claude"
ERRORS=0

echo "=== Manifest Parity Check ==="

# 1. Detect orphan variant files
echo ""
echo "--- Orphan Variants (on disk, not in manifest) ---"
for target_dir in "$BASE/variants"/*/; do
    target=$(basename "$target_dir")
    # Walk all files under this variant target
    find "$target_dir" -type f -o -type d -mindepth 2 | while read -r path; do
        rel_path="${path#$BASE/variants/$target/}"
        # Check if this relative path appears in the manifest under variants.$target or variant_only.$target
        if ! grep -q "$rel_path" "$MANIFEST" 2>/dev/null; then
            echo "  ORPHAN: variants/$target/$rel_path"
            ERRORS=$((ERRORS + 1))
        fi
    done
done

# 2. Detect stale manifest entries
echo ""
echo "--- Stale Entries (in manifest, not on disk) ---"
# Check globals
for file in $(yq '.globals.rules[]' "$MANIFEST" 2>/dev/null); do
    if [ ! -f "$BASE/rules/$file" ]; then
        echo "  STALE: globals/rules/$file"
        ERRORS=$((ERRORS + 1))
    fi
done

# Check variant entries
for target in $(yq '.variants | keys | .[]' "$MANIFEST" 2>/dev/null); do
    for file in $(yq ".variants.$target.rules[]" "$MANIFEST" 2>/dev/null); do
        if [ ! -f "$BASE/variants/$target/rules/$file" ]; then
            echo "  STALE: variants/$target/rules/$file"
            ERRORS=$((ERRORS + 1))
        fi
    done
done

# 3. Detect parity gaps (report only, no exit code)
echo ""
echo "--- Parity Gaps (variant covers some targets but not all) ---"
TARGETS=$(yq '.variants | keys | .[]' "$MANIFEST" 2>/dev/null)
for target in $TARGETS; do
    for file in $(yq ".variants.$target.rules[]" "$MANIFEST" 2>/dev/null); do
        for other_target in $TARGETS; do
            if [ "$other_target" != "$target" ]; then
                if ! yq ".variants.$other_target.rules[]" "$MANIFEST" 2>/dev/null | grep -q "$file"; then
                    echo "  GAP: $file exists in $target but not $other_target"
                fi
            fi
        done
    done
done

echo ""
if [ $ERRORS -gt 0 ]; then
    echo "FAIL: $ERRORS orphan/stale issues found. Halting."
    exit 1
else
    echo "PASS: 0 orphans, 0 stale entries."
    exit 0
fi
```

**Expected output:**

```
=== Manifest Parity Check ===

--- Orphan Variants (on disk, not in manifest) ---
  ORPHAN: variants/py/skills/dataflow-pool-patterns/

--- Stale Entries (in manifest, not on disk) ---
  STALE: globals/rules/framework-first.md

--- Parity Gaps (variant covers some targets but not all) ---
  GAP: trust-plane-security.md exists in rs but not py

FAIL: 2 orphan/stale issues found. Halting.
```

**Why exit non-zero on orphans/stale but not parity gaps:** Orphans and stale entries are bugs -- they mean the manifest and filesystem disagree about what exists. A sync running against a stale manifest will either error (file not found) or silently lose content (orphans never distributed). Parity gaps are expected design decisions -- Rust has `trust-plane-security.md` because Rust has a trust-plane crate; Python does not need it. Halting on parity gaps would make the script unusable in any repo with intentional language-specific content. The real script (journal 0029) found 91 parity gaps on first run, all pre-existing by design.

## Scoring Criteria

1. **Three-class detection** (pass/fail): The script must detect all three classes (orphans, stale, parity gaps) as separate categories. A script that only checks for stale entries, or that conflates orphans with parity gaps, is incomplete.
2. **Correct exit behaviour** (pass/fail): Exit non-zero on orphans or stale entries. Exit zero when only parity gaps are found. A script that exits zero on all findings (advisory-only) defeats the purpose -- the sync runs anyway from a broken manifest.
3. **Orphan detected** (pass/fail): The script correctly identifies `variants/py/skills/dataflow-pool-patterns/` as an orphan.
4. **Stale entry detected** (pass/fail): The script correctly identifies `globals/rules/framework-first.md` as stale.
5. **variant_only excluded from parity check** (pass/fail): The script does not report `nexus-agent-reference/` or `kaizen-multi-provider/` as parity gaps, because `variant_only` entries are intentionally single-target.

## Common Mistakes

1. **Making the check advisory-only.** The practitioner writes the parity check but has it print warnings and exit 0 regardless of findings. The sync runs anyway, distributing from a stale manifest. Journal 0028 documents the real cost: 3 orphan py skills were invisible to `/sync` since their creation, meaning downstream templates were missing content. The fix is making the exit code non-zero so the check is a hard gate, not a suggestion.

2. **Not distinguishing `variants:` from `variant_only:`.** The practitioner treats all variant entries the same. But `variant_only:` entries are intentionally single-target -- they exist in one language and have no global equivalent. Flagging them as parity gaps produces a flood of false positives. The real script (journal 0029) parsed both sections separately.

3. **Checking only file existence, not directory existence.** Variant entries can be directories (skill directories like `nexus-agent-reference/`), not just files. A script that uses `test -f` for everything will miss orphan directories and false-positive on manifest entries that point to directories.

## Extensions

1. **Wire into pre-sync gate.** Integrate the parity check into the `/sync` command as a pre-flight step. The sync command must call the check script before proceeding and abort if it exits non-zero. Test by introducing an orphan variant and verifying that `/sync` refuses to run until the orphan is resolved. This tests whether the practitioner can wire an automated check into an existing workflow, not just write a standalone script.

2. **Baseline file for expected parity gaps.** The 91 parity gaps from journal 0029 are all expected. Add a `parity-baseline.txt` file that lists known-acceptable gaps. Modify the script to report only NEW parity gaps (gaps not in the baseline). This reduces noise while preserving the ability to detect new gaps introduced by a `/codify` session.

3. **Cross-repo manifest check.** Extend the script to check that the manifest entries are consistent across multiple target repos (py template, rs template, rb template). An entry marked as `global` must exist in all three templates after `/sync`. An entry marked `variant:py` must exist only in the py template. This tests the end-to-end sync integrity, not just the source manifest's internal consistency.
