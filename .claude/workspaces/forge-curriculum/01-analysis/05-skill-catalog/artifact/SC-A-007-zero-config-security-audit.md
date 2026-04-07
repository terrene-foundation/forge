---
atom_id: SC-A-007
name: Zero-config security audit
craft_layer: artifact
destination: both
craft_areas: [attend-how, intervene-how]
applications: [COC]
spec_lineage:
  - CO §3.3 — Safety Blindness
  - CO §19 — Layer 3 — Guardrails
journal_evidence:
  - path: loom/kailash-py/workspaces/data-fabric-engine/journal/0008-RISK-default-open-endpoints-in-zero-config.md
    polarity: NEGATIVE
    quote: "Option A (internal Nexus, zero-config) creates fabric endpoints including write endpoints (`POST /fabric/User/write`) without any authentication. A developer using the simplest path — `await db.start()` — exposes database write operations to anyone who can reach the server."
  - path: loom/kaizen-cli-py/workspaces/kz-foundation/journal/0002-RISK-security-findings-r1.md
    polarity: MIXED
    quote: "Strong security posture overall: SSRF protection on WebFetchTool, NaN/Inf budget defense, Seatbelt injection prevention, fail-closed governance, no eval/exec, command injection prevention, file permission hardening."
  - path: loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md
    polarity: NEGATIVE
    quote: "`ApiKeyConfig::validate_key()` used `.any()` on the stored key hashes, which short-circuits on the first match. While each individual comparison used `subtle::ConstantTimeEq`, the iterator terminated early when a match was found at index 0 vs index N."
practice_modality: drill
status: draft
---

## The move

Audit every zero-config convenience path for default-open security posture. For each `start()`, `serve()`, or `launch()` call that works without explicit configuration, enumerate what is exposed and to whom. If the default exposes write operations, network-reachable endpoints, or authentication-bypass paths, the zero-config mode has a safety blindness gap that requires either changing the default or adding a mandatory opt-in gate.

## When it fires

Any time a framework, SDK, or platform offers a "just works" entry point that creates network-accessible resources. The trigger is the presence of a convenience API that skips configuration steps — the fewer arguments it requires, the more likely it has defaulted security-sensitive parameters to the permissive value.

## What it serves

CO §3.3 (Safety Blindness) states that AI systems optimize for the most direct path to task completion and deprioritize safety measures unless explicitly enforced. Zero-config APIs are the infrastructure equivalent: the most direct path (`await db.start()`) is also the least secure path. CO §19 (Layer 3 — Guardrails) requires deterministic enforcement of critical rules outside the context window — a guardrail that only fires when the developer remembers to add authentication is not a guardrail.

## Evidence walkthrough

- **data-fabric-engine 0008** (NEGATIVE) — The Data Fabric Engine's zero-config mode (`await db.start()`) exposed write endpoints without authentication. The entry documents the three-part resolution: localhost binding, writes disabled by default, and production requiring explicit auth middleware. The failure was that zero-config optimized for the direct path (working API in one line) and deprioritized the safety measure (authentication) exactly as CO §3.3 predicts.
- **kz-foundation 0002** (MIXED) — A red team of the kaizen-cli-py codebase found 5 HIGH vulnerabilities alongside a strong overall security posture. The pattern is instructive: the codebase had explicit security measures (SSRF protection, injection prevention) but missed implicit exposure paths (unbounded checkpoint records, symlink following, unvalidated CLI params). The mixed polarity shows that even security-conscious code has blind spots where the "direct path" bypasses the intended controls.
- **v3.8-issues 0003** (NEGATIVE) — An API key validation function used constant-time comparison for each individual key but short-circuited across keys via `.any()`. The security mechanism was present at one level (per-comparison) and absent at the next level up (across-comparisons). This is the characteristic shape of a zero-config gap: the developer addressed the obvious threat and missed the structural one.

## How to drill it

Give the practitioner a framework module that offers a zero-config `start()` function. The drill: (a) list every network-accessible resource the default creates, (b) classify each as read-only or read-write, (c) classify each as localhost-only or network-reachable, (d) identify which security parameters were defaulted to permissive values, and (e) write the one-line change that makes the default deny-by-default without breaking the zero-config promise. Score on completeness of the exposure inventory and whether the proposed fix preserves the convenience API's one-line usability. The data-fabric-engine 0008 resolution (localhost + writes-disabled + production-requires-auth) is the answer key pattern.

## Common failure mode

Practitioner audits the explicit configuration surface (flags, env vars, config files) but ignores the implicit defaults. The zero-config path is treated as "the simple case" and excluded from the threat model, when it is actually the highest-risk case because it is the path most developers will take first and many will never leave.

## Related atoms

- SC-A-004 (Layer 3 hook security audit) — hooks are one enforcement mechanism for the guardrails this atom identifies as missing.
- SC-A-006 (negative tests for deny-by-default) — the testing counterpart: once the default is changed to deny, negative tests verify the deny posture holds.
