---
case_id: CASE-08
source_path: loom/kz-engage/workspaces/herald/journal/0008-DISCOVERY-sdk-api-corrections.md
title: "Ten Wrong Assumptions About the SDK"
atoms_served: [specialist-delegation-as-first-move, sdk-api-verification]
spec_concepts: [CO §9, CO §16, CO §29]
craft_layer: artifact
recommended_use: assessment
destination: both
application: COC
---

# CASE-08: Ten Wrong Assumptions About the SDK

## 1. The Situation

The Herald project (part of kz-engage) had completed its analysis and planning phases. Architecture plans were written, milestones defined, and todos specified. The plans described how Herald would use two Kailash SDK frameworks: Kaizen (for AI agents) and Nexus (for multi-channel deployment). The plan was detailed enough to specify individual method calls, constructor parameters, and lifecycle patterns.

Before implementation began, the plans were reviewed by SDK specialist agents -- a Kaizen specialist and a Nexus specialist -- as part of the standard COC specialist-delegation process.

## 2. The Trigger

The specialist reviews revealed 10 API corrections needed across the two frameworks. The architectural patterns described in the plans were sound -- the right frameworks were chosen for the right purposes, the module boundaries were correct, the data flow was logical. But the specific API details were wrong in ways that would have caused failures during implementation.

**Kaizen (5 corrections)**:
1. `run_async()` requires `use_async_llm=True` in config -- without it, raises `ValueError`. The plan assumed it worked by default.
2. `_extract_str` and `_extract_float` are built-in `BaseAgent` methods. The plan redefined them, which would shadow the SDK's tested implementations.
3. `write_to_memory()` silently no-ops without `shared_memory=SharedMemoryPool()`. The plan used it for inter-agent communication without initializing shared memory.
4. `read_from_memory()` does not exist as an API. The plan called it. The actual API is `shared_memory.read_insights()`.
5. Fallback model swap must mutate `self.config.model` (on `BaseAgentConfig`), not `self._config.model`. The plan used the wrong attribute path.

**Nexus (5 corrections)**:
1. `Nexus.start()` is blocking. There is no `start_async()`. The plan assumed non-blocking startup.
2. `session_backend` and `session_timeout` constructor parameters do not exist. Sessions are in-memory dictionaries only.
3. Plugin protocol uses `install(app)` (`NexusPluginProtocol`), not `apply()` (legacy). The plan used the legacy name.
4. No pre/post-delegate hooks exist in the plugin protocol. Governance enforcement must be embedded in the handler or implemented as FastAPI middleware.
5. `@app.health_check_handler()` does not exist. The actual API is `ProbeManager.add_readiness_check()`.

## 3. The Move

The move is **specialist-delegation-as-first-move** combined with **sdk-api-verification**. The critical skill is consulting framework specialists before implementation, not during or after. The specialist review happened at the plan level, where corrections are cheap -- changing a method name in a todo specification costs nothing. The same corrections discovered during implementation would have cost debugging time, partial rollbacks, and morale damage from "why doesn't this work?"

The specialists did not redesign the architecture. The architecture was correct. They corrected the API surface: method names, parameter names, constructor signatures, lifecycle semantics. This is a different kind of review from code review or design review -- it is **API assumption verification**.

## 4. The Outcome

All 10 corrections were incorporated into the todo files for milestones M2, M4, and M5 before implementation began. The architectural patterns were confirmed correct -- single BaseAgent, Nexus as gateway, governance enforcement. Only the API-level details needed adjustment.

The lesson the journal explicitly records: "Always validate SDK API assumptions against actual source before implementation. Plans based on documentation/skills may reference aspirational APIs that don't match the current SDK version."

This last point is subtle. The plan author likely consulted documentation or skill files that described the API. But the documentation described the intended API, which in some cases differed from the implemented API (e.g., `read_from_memory()` was perhaps planned but never built, `session_backend` was perhaps designed but not implemented). The specialist caught the gap between documented intent and implemented reality.

## 5. The Spec Connection

**CO §9 (Layer 1 -- Intent)** defines the intent layer as the human's statement of what the system should do. The Herald plans were intent artifacts -- they described what the system should do and how it should use the SDK. §9 says the intent layer must be grounded in reality. The 10 corrections are cases where intent was grounded in documentation rather than implementation, and the two had diverged.

**CO §16 (Framework-First)** requires that technical work use existing frameworks rather than building from scratch. Herald correctly chose Kaizen and Nexus rather than building custom agent and deployment infrastructure. But §16's requirement does not stop at "choose the framework" -- it extends to "use the framework correctly." The 5 Kaizen corrections and 5 Nexus corrections are all cases where the framework was chosen but its actual API was not verified.

**CO §29 (Evidence-Based Completion)** requires evidence that claims are true. The plan claimed "we will use `run_async()` for agent execution." The evidence would be: does `run_async()` work as described? The specialist provided the evidence: it does, but only with `use_async_llm=True`. §29 applied to the plan's API assumptions, not just to the final code.

## 6. Discussion Questions

1. Five of the ten corrections involve APIs that do not exist or do not work as documented (`read_from_memory()`, `session_backend`, `@app.health_check_handler()`, `start_async()`, `apply()`). If the documentation had been accurate, the specialist review would have found fewer issues. Is the root cause the plan author's failure to verify, or the SDK's failure to maintain accurate documentation? Who bears the cost of documentation drift?

2. The `write_to_memory()` call silently no-ops without `SharedMemoryPool()`. This is a silent-failure API design: it accepts the call, does nothing, and reports no error. If the API had raised an exception on misconfiguration, the plan author would have discovered the issue during a quick prototype. Should SDK APIs fail loudly on misconfiguration, or is silent degradation a valid design choice for optional features?

3. The specialist review happened at the plan level, where corrections are cheap. But specialist review consumes specialist agent tokens and time. If you had a project with 50 todos using 5 different frameworks, the specialist review overhead could be significant. At what scale does specialist review become a bottleneck, and what would you do about it -- skip the review, batch the reviews, or restructure the plans to minimize framework-crossing?
