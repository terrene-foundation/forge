---
title: Upstream Flags — stale artifacts in template-synced rules
date: 2026-04-07
authority: flag-only, do not edit template-synced rules from this repo
destination: forge
---

# Upstream Flags

This file documents artifacts in this repository's template-synced rules that are stale with respect to the current FORGE framing. Per `rules/forge-scope.md` §1: **flag the discrepancy, do not silently edit template-synced rules from this repo.** Fixes must land at the upstream authoring layer (loom/ for COC artifacts, atelier/ for CC+CO methodology) and propagate through the next sync pass.

_(No active flags. Flag 1 resolved 2026-04-09 — upstream loom/ terrene-naming.md and behavioral-guidelines.md updated from "trinity" to "quartet"; forge local copy synced manually.)_

---

## How to add a new flag

When a future session discovers another stale upstream artifact:

1. Do NOT edit the stale file directly
2. Add a new flag section below with: file path, line, stale text, why stale, required fix, upstream authoring layer
3. Note any FORGE compensating guidance that already handles the discrepancy
4. When the upstream fix lands and propagates, remove the flag from this file (with a commit message referencing the upstream commit hash)
