---
drill_id: DR-SC-P-013-FULL
source_atom: SC-P-013
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Treat Internal Estimate Inconsistency as a High-Confidence Signal That at Least One Estimate Is Wrong

## Setup

The practitioner receives a plan document (2 pages) produced by a single analytical session for a project called "kailash-nexus transport refactor." The plan reads smoothly and contains no explicit caveats or flags. Two effort estimates for the same deliverable are embedded in different sections of the plan. The estimates disagree by approximately 3x. The practitioner is not told that there is an inconsistency — they must find it.

**Plan Document:**

> **Executive Summary**
>
> The Nexus transport layer requires a refactor to support three simultaneous channel types (HTTP, CLI, MCP) with a unified handler registry and event bus. The primary work items are:
>
> - Extract `HandlerRegistry` into a standalone module with type-safe parameter validation
> - Implement `EventBus` with filtered subscription and broadcast semantics
> - Migrate HTTP, CLI, and MCP transports to the new handler registry
> - Add authentication middleware (JWT, API key, rate limiting)
> - Implement background service lifecycle management
>
> The scope is well-understood from the Python SDK implementation. Estimated effort: **2-3 sessions** to reach parity with the Python transport layer. The Rust type system will enforce invariants that Python handles at runtime, so the migration should be straightforward.
>
> **Detailed Breakdown**
>
> _Phase 1 — HandlerRegistry extraction (1 session):_
> Extract the handler registration logic from the monolithic `nexus.rs` into `handler.rs`. The current handler system uses string-based dispatch; the refactored version must use Rust's type system for compile-time parameter validation. This requires designing a `HandlerFn` trait, implementing `ClosureHandler` and `HandlerDef` adapters, and building a registry that resolves handlers by name with type checking. The Python SDK uses runtime introspection for this; the Rust equivalent needs a different design (trait objects with downcasting or enum dispatch). Estimated: 1 full session.
>
> _Phase 2 — EventBus and transport migration (2-3 sessions):_
> Build the `EventBus` with 256-capacity broadcast channel, publish/subscribe/subscribe_filtered APIs, and a bridge to DataFlow for persistent event storage. Migrate each of the three transports (HTTP with FastAPI-equivalent routing, CLI with clap argument parsing, MCP with stdio/SSE/HTTP sub-transports) to use the new HandlerRegistry instead of direct dispatch. Each transport migration is approximately 1 session due to transport-specific edge cases (MCP alone has 3 sub-transports, each with its own connection lifecycle). Estimated: 2-3 sessions.
>
> _Phase 3 — Auth and background services (2 sessions):_
> Implement `NexusAuthPlugin` with basic_auth, JWT, RBAC, API key validation, and rate limiting (global and per-user). Build CORS middleware with permissive/strict factories. Implement `BackgroundService` trait with lifecycle management (start, stop, health check). Add `SessionStore` with TTL-based expiry. Estimated: 2 sessions.
>
> **Dependencies and Risks**
>
> The transport refactor sits on the critical path for 3 downstream packages that depend on the Nexus API surface. A slip in this refactor cascades to all downstream consumers. The Python SDK's transport layer is approximately 2,400 lines; the Rust implementation will likely be similar in scope given the additional type-system ceremony. The 2-3 session estimate assumes no significant design divergence from the Python implementation.
>
> **Timeline**
>
> Target completion: end of sprint 4 (2 weeks from plan approval). With 2-3 sessions allocated, this fits comfortably within the sprint boundary.

## Task

1. Read the plan document. Identify the two inconsistent estimates and state the disagreement ratio.
2. Trace the inconsistency to its root cause. Which section incorporated more detail? Which section compressed or abstracted away complexity?
3. State which estimate you distrust and why. Your reasoning must reference what each estimate accounted for that the other did not — not simply "the higher one is safer" or "the lower one is optimistic."
4. Write a one-sentence gate rejection: "This plan cannot proceed because [specific inconsistency] indicates [specific analytical failure]."

## Model Answer

**1. The two inconsistent estimates:**

- **Executive Summary** estimates the total effort at **2-3 sessions** ("to reach parity with the Python transport layer").
- **Detailed Breakdown** estimates the total effort at **5-6 sessions** (Phase 1: 1 session + Phase 2: 2-3 sessions + Phase 3: 2 sessions = 5-6 sessions).

Disagreement ratio: the detailed estimate is approximately **2-2.5x** the summary estimate. The summary says 2-3 sessions; the breakdown says 5-6.

**2. Root cause of the inconsistency:**

The Executive Summary was written as a compressed narrative that frames the work as "straightforward parity migration." The phrase "the Rust type system will enforce invariants that Python handles at runtime, so the migration should be straightforward" is doing the compression — it implies that the Rust version is simpler than the Python version, which contradicts the Detailed Breakdown's own observation that "the Rust equivalent needs a different design" and "MCP alone has 3 sub-transports, each with its own connection lifecycle."

The Detailed Breakdown incorporated the actual scope: three distinct phases with independent work items, transport-specific edge cases, and a full auth stack. The Executive Summary compressed all of this into a single "parity migration" framing that discarded the phase-level detail.

The root cause is that the Executive Summary was written BEFORE the Detailed Breakdown (or independently of it), using a top-down estimate ("Python parity is probably 2-3 sessions"), and the Detailed Breakdown was written bottom-up by enumerating actual work items. The two estimation methods were never reconciled. The plan was assembled by concatenation, not by revision.

**3. Which estimate to distrust:**

Distrust the Executive Summary's 2-3 sessions. The Detailed Breakdown accounts for three specific scope items that the summary's estimate does not:

- The `HandlerFn` trait design work (1 full session), which the summary dismisses as "straightforward" because of the type system — but the breakdown acknowledges requires "a different design" from Python's runtime introspection approach.
- The MCP transport migration with 3 sub-transports (included in the 2-3 session Phase 2 estimate), which the summary rolls into "migration should be straightforward."
- The full auth stack — JWT, RBAC, API key, rate limiting, CORS, session store, background services (2 sessions in Phase 3), which the summary does not mention at all. The summary's "2-3 sessions" appears to cover only the handler and transport work, silently excluding auth and background services.

The Detailed Breakdown may itself be an underestimate (the journal evidence from loom-meta 0034 shows that detailed estimates also compress), but the Executive Summary is demonstrably an underestimate because it accounts for less scope than the plan's own breakdown.

**4. Gate rejection:**

"This plan cannot proceed because the Executive Summary estimates 2-3 sessions while the Detailed Breakdown totals 5-6 sessions for the same scope, indicating that the summary was written without incorporating the phase-level analysis and the plan was never internally reconciled."

## Scoring

| Criterion                                                                                                                                                  | Points |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| Both estimates correctly identified with specific numbers (2-3 vs 5-6) and source sections named                                                           | 2      |
| Disagreement ratio stated as approximately 2-2.5x (accept 2x-3x range)                                                                                     | 1      |
| Root cause traced to compression: summary written top-down, breakdown written bottom-up, never reconciled                                                  | 2      |
| Distrust rationale names specific scope items the summary omits (auth stack, MCP sub-transports, trait design) — not just "the lower number is optimistic" | 2      |
| Gate rejection names the specific inconsistency AND the analytical failure (not just "estimates disagree")                                                 | 2      |
| Practitioner caught the inconsistency without being told it exists (no hint-seeking)                                                                       | 1      |
| **Total**                                                                                                                                                  | **10** |

## Extensions

1. **Hidden unit conversion variant (hard).** Replace the Executive Summary's estimate with "approximately 3-4 working days" and the Detailed Breakdown's estimate with "5-6 sessions." The plan never defines the conversion factor between "working days" and "sessions." If the practitioner assumes 1 session = 1 day, the estimates appear roughly consistent. If sessions are half-days (the actual convention in the journal corpus), the real disagreement is 3-4 days vs 2.5-3 days — which looks consistent but is actually an AGREEMENT produced by two different estimation methods arriving at similar numbers via different reasoning. The drill tests whether the practitioner demands explicit unit definitions before comparing estimates.

2. **Cascading re-estimation variant.** After rejecting the plan, the practitioner receives a revised plan where the Executive Summary now says "5-6 sessions." However, the Dependencies section still says "with 2-3 sessions allocated, this fits comfortably within the sprint boundary." The revision patched the summary but did not propagate the change to the timeline. The practitioner must catch the second-order inconsistency: the estimate was updated but the schedule was not, meaning the sprint boundary is now blown by 2-3 sessions.

3. **Multi-document variant.** The Executive Summary and Detailed Breakdown live in separate documents (a project brief and a technical design doc). The practitioner receives both and must cross-reference them. This tests whether the inconsistency-detection skill transfers from within-document reading to across-document comparison, where the two estimates are never seen on the same page.
