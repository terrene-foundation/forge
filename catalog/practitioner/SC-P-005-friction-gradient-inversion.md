---
atom_id: SC-P-005
name: Diagnose friction-gradient inversions where the best API has the worst discoverability
craft_layer: practitioner
destination: forge
craft_areas: [attend-how, intervene-when]
applications: [COC]
spec_lineage:
  - CO §1 — Institutional Knowledge Thesis
  - CO §3.2 — Convention Drift (Failure Mode)
  - CO §64 — COC Skill Hierarchy
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash/journal/0006-DISCOVERY-engine-layer-broken-across-projects.md
    polarity: NEGATIVE
    quote: "The engine layer is supposed to be the primary developer interface — the thing that makes Kailash productive."
  - path: loom/kailash-py/workspaces/kailash/journal/0007-RISK-agent-api-unused-but-trap.md
    polarity: NEGATIVE
    quote: "The Agent API is documented in `patterns.md` (6 lines) and accepts tools silently."
practice_modality: case
status: verified
---

## The move

Map the friction gradient of a module's API surface: for each API entry point, measure (a) discovery cost (how many clicks/searches/docs pages to find it) and (b) usage quality (does it actually work as documented, how many lines of boilerplate, does it handle errors correctly). When the easiest-to-discover API has the lowest usage quality, or the highest-quality API has the highest discovery cost, diagnose the inversion. Then trace the root cause through the COC layers: is the inversion a Layer 5 (Learning/codify) failure (institutional knowledge not updated), a Layer 2 (Context/CLAUDE.md) failure (master directive routes to the wrong entry point), or a Layer 3 (Guardrails/rules) failure (no rule prevents the broken path)?

## When it fires

Three trigger conditions:

1. Multiple independent consumers of the same SDK or module converge on the same suboptimal pattern (e.g., three dev teams all using 60-line BaseAgent boilerplate instead of the 2-line Delegate API). The signal is convergence on a worse path despite a better path existing.
2. A quick-start or patterns document routes users to an API that silently fails or silently drops parameters. The signal is that the documented "happy path" is a trap.
3. A codebase audit finds high adoption of a low-level API alongside zero adoption of the high-level API that wraps it. The signal is that the wrapper is either broken, undocumented, or harder to find than the thing it wraps.

## What it serves

CO §1 (Institutional Knowledge Thesis) states that "the primary determinant of AI output quality is not model capability but the institutional context surrounding the model." A friction-gradient inversion is an institutional knowledge failure: the institution (via its documentation, COC artifacts, and API surface) actively routes practitioners toward the worse tool. CO §3.2 (Convention Drift) names the downstream effect: practitioners follow generic patterns from training data (use the base class, write boilerplate) instead of organizational conventions (use the engine API) because the organizational conventions are either broken or invisible. CO §64 (COC Skill Hierarchy) defines the progressive-disclosure structure that SHOULD prevent inversions — `CLAUDE.md` routes to skill indices, skill indices route to topic files, topic files route to the correct API. When this hierarchy fails to mention the correct API or actively mentions the broken one, the skill hierarchy is the root cause of the inversion.

## Evidence walkthrough

- **0006-DISCOVERY-engine-layer-broken-across-projects** (NEGATIVE) — Three independent development teams (Arbor, Pact, ImpactVerse) converged on the same suboptimal pattern: building on Layer 2 primitives (BaseAgent, 60+ lines per agent) instead of the engine layer (Delegate, 2 lines). Investigation revealed the engine layer was broken — the Agent API had 3 critical bugs including silent tool-dropping and success fabrication on failure. The friction gradient was inverted: the simplest working API (Delegate) had the highest discovery cost, while the most-discoverable API (Agent, documented in patterns.md) was a trap. The COC artifacts reinforced the inversion by teaching only primitives.
- **0007-RISK-agent-api-unused-but-trap** (NEGATIVE) — Red team of the engine deficiency analysis confirmed zero downstream adoption of the broken Agent API — all consumers had independently discovered the problem and routed around it to Delegate or BaseAgent. But the Agent API remained documented in `patterns.md` (6 lines, easy to discover) and accepted tools silently (appearing to work until real behavior was tested). The inversion persisted because the COC was never updated to route developers away from Agent toward Delegate, and the Agent API's silent success fabrication masked the bug from automated testing.

## How to drill it

Present the practitioner with a module that has three API entry points at different abstraction levels (e.g., a low-level primitive, a broken engine, and a working engine). Include a patterns document that mentions the broken engine and the primitive but not the working one. The practitioner must:

1. Map the friction gradient: for each entry point, record discovery cost (is it in the quick-start? in CLAUDE.md? buried in a subdirectory?) and usage quality (does it work? how many lines? does it handle errors?).
2. Identify the inversion: which entry point has the worst discoverability-to-quality ratio?
3. Trace the root cause through COC layers: which layer (Context, Guardrails, Learning) is responsible for the inversion persisting?
4. Write a one-paragraph intervention: what specific COC artifact change would correct the gradient?

Score on whether the practitioner identifies the inversion AND traces it to a COC layer failure, not just to "bad docs." The drill fails if the practitioner recommends "write better documentation" without specifying which COC artifact (CLAUDE.md, a skill file, a rule, a hook) should change and how.

## Common failure mode

The practitioner identifies the broken API and files a bug against the engine code, but does not address the friction gradient. Fixing the engine bug is necessary but insufficient — if the COC still routes users to the formerly-broken API (now fixed but with a reputation problem) and still omits the alternative, the inversion persists in discovery cost even after the quality gap is closed. The journal entries show that all three teams had already routed around the broken engine; the friction-gradient inversion was the ongoing problem, not the bugs themselves.

## Related atoms

- SC-P-004 (per-file disposition labelling) — disposition labelling can surface friction-gradient inversions by revealing files that are well-documented but broken vs undocumented but solid.
- SC-A-001 (codify-with-explicit-NOT-codified) — the codify move that would catch "we did NOT update the COC to deprecate the broken API" as an explicit gap.
- SC-A-009 (progressive disclosure architecture) — the progressive disclosure hierarchy should route users to the best API first; a friction inversion means the hierarchy routes them to the worst.
- SC-P-001 (adjacent-but-disconnected modules) — friction-gradient inversions often live at module boundaries where the integration is structurally adjacent but functionally absent.
