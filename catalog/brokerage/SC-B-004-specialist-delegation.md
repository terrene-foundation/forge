---
atom_id: SC-B-004
name: Specialist delegation as required first move (framework work)
craft_layer: brokerage
destination: both
craft_areas: [intervene-when, institutional-memory]
applications: [COC]
spec_lineage:
  - CO §9 — Layer 1 — Intent
  - CO §11 — Routing Rules
  - PACT §11 — Role Envelope
journal_evidence:
  - path: loom/kz-engage/workspaces/herald/journal/0008-DISCOVERY-sdk-api-corrections.md
    polarity: NEGATIVE
    quote: "Specialist reviews of the Herald plans against actual kailash-py SDK source revealed 10 corrections needed. The architectural patterns are sound but specific API details were wrong."
  - path: loom/kailash-py/workspaces/kailash/journal/0017-RISK-pact-todos-red-team-three-critical-findings.md
    polarity: NEGATIVE
    quote: "TrustStore API mismatch — The todos assumed synchronous, per-record emission to TrustStore. But TrustStore is async and stores complete TrustLineageChain objects, not individual records."
practice_modality: brokerage-rep
status: verified
---

## The move

When the next planned action touches a Kailash framework (Core SDK, DataFlow, Nexus, Kaizen, MCP platform, PACT primitives, ML, Align), route the work through the relevant framework specialist _before_ writing the todo or the implementation. The specialist's job is to read the actual SDK source against the planned API surface and produce a corrections list. The corrections feed the todo before any code is written; they do not arrive as red team findings after implementation.

## When it fires

Two trigger conditions: (1) a todo enumerates SDK API calls (constructor parameters, method names, return types, async/sync semantics), or (2) a session is about to write code that imports from a framework module the practitioner has not opened in the last hour. The second is the cheaper trigger and the one that catches more drift — assuming "I know this API" is the most common cause of a wrong-API-shape todo.

## What it serves

CO §9 (Layer 1 — Intent) names the requirement that intent be expressed in terms the executor can act on. A todo that names a method that does not exist is not intent — it is fiction the implementer must reverse-engineer. CO §11 (Routing Rules) is the structural reason — routing decisions are part of the layer-1 contract, and "route framework work to a framework specialist" is exactly that kind of rule. PACT §11 (Role Envelope) is the accountability reason — the framework specialist operates inside a known role envelope (read SDK source, validate against actual code, produce corrections list), and any other agent operating outside that envelope on framework work is taking unaudited action.

## Evidence walkthrough

- **0008-DISCOVERY-sdk-api-corrections** in herald (NEGATIVE) — A plan was authored without specialist review. Specialist review afterwards turned up 10 corrections, all API-level: `run_async()` requires a config flag the plan didn't set, plugin protocol used the wrong method name, sessions had constructor parameters that don't exist, health check decorator doesn't exist. Each of these was a runtime failure waiting in the todo list. The "lesson" line in the entry is the explicit form of the move: "Always validate SDK API assumptions against actual source before implementation."
- **0017-RISK-pact-todos-red-team-three-critical-findings** in kailash-py (NEGATIVE) — A more dramatic version of the same failure on PACT spec-conformance todos. Three critical findings, all of which would have caused runtime failures: TrustStore API was async (todos assumed sync); DelegationRecord used `delegated_at` (todos used `created_at`); `create_bridge()` had a breaking signature change against 14+ existing call sites. All three were pure API-shape mistakes that a 30-minute specialist read against source would have prevented.

The polarity is NEGATIVE in both cases because the specialist review _did_ happen — but it happened after the plan was written, not before. The skill atom is the move that would have made the corrections list a todo input rather than a todo revision, saving the time-cost difference between the two.

## How to drill it

Pick a feature plan from the corpus that already has a corrections list attached (entry 0017 is ideal because it lists C1/C2/C3/H1-H5 explicitly). Strip the corrections out and hand the practitioner the original plan + a one-line context ("this touches PACT spec conformance — TrustStore, DelegationRecord, GovernanceEngine"). They have 20 minutes to:

1. Identify which API surfaces the plan depends on.
2. Open the actual SDK source for each one in the corpus cache.
3. Produce their own corrections list.
4. Compare against the recorded list (the entry's findings section is the answer key).

Score on (a) coverage — did they catch all 8 findings, (b) discrimination — did they raise the criticals as criticals, and (c) source-citation — did each correction reference the actual source file/line they read, or did they hallucinate from API shape memory?

## Common failure mode

Practitioner reasons "I have used this API before, so I do not need to re-read it." The Herald entry is the empirical refutation — the plan was written by an experienced practitioner and still produced 10 corrections against current source. The cure is the second trigger condition above: the gating question is not "do I know this API?" but "have I opened the source for this API in the last hour?" The first question is unfalsifiable; the second is binary.

## Related atoms

- SC-B-002 (authority-chain escalation) — what to do when the specialist finds the SDK source itself has a defect that the plan was correctly compensating for.
- SC-B-009 (cross-SDK upstream brokerage) — when specialist review reveals a cross-SDK defect, upstream brokerage is the escalation path.
- SC-B-017 (verification-gradient zone assignment) — delegating to a specialist with a narrow Role Envelope may warrant a wider zone than delegating to a generalist.
- SC-A-001 (codify-with-explicit-NOT-codified) — the codify move that captures the corrections list as institutional knowledge so the next session does not re-derive it.
- SC-A-013 (agent specialisation routing) — the artifact-authoring counterpart: routing rules determine which specialist handles which framework, and this atom is the runtime execution of that routing.
