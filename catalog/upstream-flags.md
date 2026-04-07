---
title: Upstream Flags — stale artifacts in template-synced rules
date: 2026-04-07
authority: flag-only, do not edit template-synced rules from this repo
destination: forge
---

# Upstream Flags

This file documents artifacts in this repository's template-synced rules that are stale with respect to the current FORGE framing. Per `rules/forge-scope.md` §1: **flag the discrepancy, do not silently edit template-synced rules from this repo.** Fixes must land at the upstream authoring layer (loom/ for COC artifacts, atelier/ for CC+CO methodology) and propagate through the next sync pass.

## Flag 1 — "trinity" language in terrene-naming.md

**File**: `rules/terrene-naming.md`
**Line**: 52 (in the Canonical Terminology section)
**Stale text**:

> "CO sits in the trinity: CARE (philosophy) + EATP (protocol) + CO (methodology)"

**Why stale**: Per programme-owner authority (decision log D20, 2026-04-07), the Foundation standards stack is a **quartet**, not a trinity. PACT is the fourth standard, peer of CARE / EATP / CO. The trinity framing is load-bearing for downstream artifacts and cannot be hedged — anyone reading this rule would produce framing that says "PACT is being proposed as a fourth standard" which directly contradicts the programme owner's declared authority.

**Required fix** (at upstream, not here):
Replace the trinity line with:

```
- CO is one of the four Terrene Foundation standards: CARE (philosophy) + EATP (protocol) + CO (methodology) + PACT (governance). All four are peers.
```

**Upstream authoring layer**: loom/ (likely at `loom/.claude/rules/terrene-naming.md` or equivalent variant location). Filing this flag as an issue on loom is the correct path; sync will then carry the fix down to this repo.

**FORGE compensating guidance**: Artifacts produced from this workspace already treat the four standards as peers (see `rules/forge-scope.md` §1 which explicitly supersedes the trinity framing). The `rules/forge-scope.md` §1 text says: "When terrene-naming.md still says 'trinity (CARE + EATP + CO)', that naming rule is **stale** with respect to PACT's elevation — flag the discrepancy but do not silently edit template-synced rules from this repo." That is exactly the guidance followed here.

---

## How to add a new flag

When a future session discovers another stale upstream artifact:

1. Do NOT edit the stale file directly
2. Add a new flag section below with: file path, line, stale text, why stale, required fix, upstream authoring layer
3. Note any FORGE compensating guidance that already handles the discrepancy
4. When the upstream fix lands and propagates, remove the flag from this file (with a commit message referencing the upstream commit hash)
