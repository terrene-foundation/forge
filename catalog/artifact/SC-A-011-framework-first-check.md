---
atom_id: SC-A-011
name: Framework-first check before building new functionality
craft_layer: artifact
destination: both
craft_areas: [intervene-when, attend-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §16 — Framework-First
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/journal/0033-DECISION-mcp-consolidation.md
    polarity: POSITIVE
    quote: "keep to the best one and everybody use that primitive. [...] Six implementations means six places to fix when MCP spec evolves. One primitive, one upgrade path."
  - path: loom/kailash-py/workspaces/data-fabric-engine/journal/0005-DISCOVERY-three-concepts-are-all-you-need.md
    polarity: POSITIVE
    quote: "- Developer learns three new things. Express API, @db.model, caching — all unchanged\n- 100% backward compatible. Existing DataFlow projects add fabric incrementally"
  - path: loom/kailash-py/workspaces/kailash-align/journal/0005-DISCOVERY-delegate-already-supports-local-models.md
    polarity: POSITIVE
    quote: "KaizenModelBridge is simpler than expected. It does not need to create new adapters or modify the Delegate system. It is a pure convenience layer."
practice_modality: drill
status: verified
---

## The move

Before building any new functionality, grep the framework for existing primitives that already do the job. The default action is "find the existing thing and compose with it," not "write a new thing." When the search finds an existing primitive, the deliverable shrinks to a thin composition layer on top of it. When it finds multiple competing implementations, the deliverable becomes consolidation into one canonical primitive, not another variant.

## When it fires

At the start of any implementation session — after analysis, before the first line of new code. The trigger is the impulse to create: a new module, a new package, a new abstraction. The check fires before that impulse becomes a file. A secondary trigger is discovering that multiple implementations of the same concept already exist in the codebase.

## What it serves

CO §16 (Framework-First) prescribes "composition over creation" — check whether existing organisational building blocks already handle the task before building from scratch. CO §17 (Single Source of Truth) prescribes that each piece of institutional knowledge lives in exactly one place. Building a new implementation when one already exists violates both: it creates from scratch (§16) and introduces a second source of truth (§17). The framework-first check is the move that prevents both violations simultaneously.

## Evidence walkthrough

- **loom-meta 0033** (POSITIVE) — The MCP analysis discovered six separate MCP server implementations scattered across kailash-py. The framework-first response was not "build a seventh" but "keep the best one and everybody use that primitive." FastMCP from the official MCP SDK was identified as the canonical server; five other implementations were marked for deletion or refactoring into thin wrappers. The consolidation eliminated six upgrade points and replaced them with one.
- **data-fabric-engine 0005** (POSITIVE) — After two red-team rounds, the entire data fabric engine reduced to exactly three new concepts on top of existing DataFlow. The Express API, @db.model, and caching were all unchanged. The framework-first check revealed that the existing framework already handled the hard parts; the new functionality was a declarative overlay, not a replacement. The result was 100% backward compatibility and a three-concept learning curve instead of a rewrite.
- **kailash-align 0005** (POSITIVE) — An audit of the Kaizen Delegate discovered that full Ollama and vLLM support already existed via OllamaStreamAdapter and OpenAIStreamAdapter. The planned KaizenModelBridge was expected to require new adapter infrastructure; instead it became a 150-250 line convenience layer that constructed existing adapters with correct configuration. The framework had the capability; the team almost rebuilt it.

## How to drill it

Give the practitioner a brief describing a new feature ("add local model serving to the alignment framework"). Before they write any code, they must: (a) grep the existing codebase for primitives that already handle the core functionality, (b) produce a one-paragraph assessment of what exists vs what is genuinely missing, and (c) write a revised scope statement that composes with existing primitives instead of rebuilding. Score on whether the practitioner's revised scope is smaller than the original brief. The kailash-align 0005 case is the canonical example: the brief implied adapter creation, but the check revealed existing adapters, and the scope shrank from "build serving infrastructure" to "build a 200-line convenience wrapper."

## Common failure mode

The practitioner skips the search because they assume they know the codebase, or because the existing primitive has a different name than the concept in the brief. The MCP consolidation case (0033) shows the extreme: six implementations existed because each was built by a session that did not search for prior art. The fix is making the grep mandatory and explicit — not a mental check ("I think we don't have this") but a codebase search with results recorded before the implementation plan is finalised.

## Related atoms

- SC-P-006 (package boundary decision) — the three-question test for "module vs separate package" is a downstream decision that fires only after the framework-first check has confirmed the functionality is genuinely new.
- SC-A-009 (progressive disclosure architecture) — existing primitives discovered by the framework-first check must be placed at the correct level in the disclosure hierarchy, not duplicated at the top level.
- SC-B-004 (specialist delegation) — the specialist is the agent most likely to know whether the framework already provides the needed primitive; delegation before building is the runtime form of framework-first.
- SC-B-010 (cross-repo divergence audit) — framework-first applies across repos too; a divergence audit may reveal that the primitive already exists in a sibling repo.
