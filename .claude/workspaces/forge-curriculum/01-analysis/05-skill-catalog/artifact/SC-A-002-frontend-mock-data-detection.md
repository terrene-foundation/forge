---
atom_id: SC-A-002
name: Frontend mock data detection
craft_layer: artifact
destination: both
craft_areas: [attend-how, intervene-how]
applications: [COC]
spec_lineage:
  - CO §3.3 — Safety Blindness
  - CO §29 — Evidence-Based Completion
  - CO §19 — Layer 3 — Guardrails
journal_evidence:
  - path: loom/journal/0047-DECISION-spec-coverage-gate.md
    polarity: NEGATIVE
    quote: "Post-mortem of a project (kz-engage) that passed 11 red team rounds, 94 tests, and 2 consecutive clean convergence rounds — yet shipped with: Frontend pages using `generateHourlyOccupancy()` instead of real API calls… Mock data invisible to hooks — `validate-workflow.js` detects Python stubs but not `MOCK_USERS`, `generateHourly*()` in JS/TS."
practice_modality: build
status: draft
---

## The move

Extend the no-stubs detection that already exists for Python (`raise NotImplementedError`, `TODO`, `pass # placeholder`) to catch the JS/TS frontend equivalents that are invisible to Python-focused validators. The detection MUST be at the hook layer (Layer 3 — Guardrails), not just in code review, because hook enforcement is what gives the rule its anti-amnesia property. Targeted patterns (`MOCK_*`, `FAKE_*`, `DUMMY_*`, `SAMPLE_*` constants; `generate*()` and `mock*()` functions producing synthetic display data; `Math.random()` used for display values) BLOCK the edit, do not warn.

## When it fires

Two trigger conditions:

1. Any new or modified `.tsx`, `.jsx`, `.ts`, `.js` file in a UI tree.
2. Any codify session where the practitioner notices that a frontend page renders without the corresponding backend wiring being merged. The second is the rarer but louder trigger — it is the empirical evidence that the existing detection has a hole, and the fix is to add the pattern, not to file a one-off bug.

## What it serves

CO §3.3 (Safety Blindness) is the structural reason: a stub pattern that is invisible to the validator is exactly the failure mode CO §3.3 names — the practitioner trusts the system because the validator passed, the validator passed because it never saw the stub, and the resulting blind spot is what ships. CO §29 (Evidence-Based Completion) is the workflow reason: the definition of "done" must include "no synthetic data presented as real," and a detection rule that does not see frontend mocks cannot enforce that definition. CO §19 (Layer 3 — Guardrails) is the placement reason: the detection MUST be at the deterministic-enforcement layer because the practitioner who wrote the mock will not flag it themselves.

## Evidence walkthrough

- **0047-DECISION-spec-coverage-gate** (NEGATIVE) — The exact failure post-mortem this skill is built to prevent. The kz-engage project passed 11 red team rounds, 94 tests, and 2 clean convergence rounds — and shipped with `generateHourlyOccupancy()` rendering as if it were real data on the carpark page. The post-mortem names the root cause precisely: the validator detected Python stubs but not the JS/TS equivalents. The fix landed across six artifacts (the `/redteam` command, the `/todos` command, the `/implement` command, `validate-workflow.js`, `no-stubs.md`) — Defense in Depth in operation, with the hook-layer detection being the anti-amnesia mechanism the other artifacts cannot replace.

The polarity is NEGATIVE because the move was missing when the failure shipped; the entry is the post-mortem that established the move. The skill atom's job is to ensure no future practitioner re-derives the lesson from a similar shipment.

## How to drill it

Hand the practitioner a small Next.js/React project with three pages, all of which appear to render real data. One page calls a real API. One page uses `generateHourly*()` as a synthetic data source. One page uses `Math.random()` for the display values that look like a placeholder gauge. Their task:

1. Identify which pages are blocked by the `checkFrontendMockData()` rule and which would pass.
2. For each blocked page, write the regex that catches it and the import statement / variable name that the regex must match against.
3. For each passing page, prove it passes by walking through the network call or props chain.

Score on (a) zero false negatives (every mock pattern caught), (b) regex specificity (no false positives on legitimate `generateUUID()`-style utilities), and (c) correct identification that the cure is hook-layer enforcement, not just review. The 0047 entry's "for discussion" item 1 is where the practitioner should be tested on the false-positive trade-off.

## Common failure mode

Practitioner adds frontend mock data patterns to an advisory rule (`rules/no-stubs.md`) and stops there. The rule is correct, the rule is loaded, and the rule is _probabilistic_ — the model reads it most of the time and ignores it sometimes. The cure is the second placement: the same patterns must also be in the validate hook (`scripts/hooks/validate-workflow.js`) where they BLOCK the edit, not advise against it. Detection in only one layer is the failure mode 0047 names. Defense in depth (rule + hook + command-step) is the move.

## Related atoms

- SC-A-001 (codify-with-explicit-NOT-codified) — what to record after the fix lands; a fix that crosses six artifacts produces six codify candidates and the discrimination matters.
- SC-B-002 (authority-chain escalation) — relevant when the fix must land in loom/scripts/hooks/ before it can be synced to the project that surfaced the bug.
