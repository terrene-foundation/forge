---
drill_id: DR-SC-A-011-FULL
source_atom: SC-A-011
difficulty: intermediate
time_box_minutes: 30
---

# Framework-First Check Before Building New Functionality

## Setup

The practitioner receives a feature brief and access to a codebase.

**Feature brief:** "Build a local model serving bridge for the alignment framework. The bridge should let users fine-tune a model with kailash-align and then serve it locally via Ollama or vLLM for inference. The bridge needs to: (a) detect which local serving backend is available, (b) create the correct adapter configuration, (c) return a configured Delegate agent that can use the local model. Estimated scope: new package `kailash-align-serve` with adapter infrastructure, backend detection, and Delegate integration. Estimated effort: 3-4 sessions."

**Codebase provided:** A simplified version of the Kaizen Delegate package (modelled on `packages/kaizen-agents/src/kaizen_agents/delegate/`), containing:

- `delegate.py` -- the main Delegate class, accepting an `adapter` parameter
- `adapters/ollama.py` -- `OllamaStreamAdapter` with full streaming, tool calling, and token tracking against Ollama's `/api/chat` endpoint
- `adapters/openai.py` -- `OpenAIStreamAdapter` that works with any OpenAI-compatible API (including vLLM) via configurable `base_url`
- `config.py` -- `KzConfig` supporting `provider = "ollama"` via TOML config or `KZ_PROVIDER` environment variable
- `adapters/__init__.py` -- adapter registry with `get_adapter(provider_name)` function

The codebase also contains a `README.md` that does not mention local model serving, making it easy to miss the existing capability if the practitioner reads only documentation.

## Task

1. Before writing any code or designing any module, search the existing codebase for primitives that handle the core functionality described in the brief. Record every relevant file found, with a one-sentence description of what it does.
2. For each capability in the brief (backend detection, adapter configuration, Delegate integration), write whether it already exists, partially exists, or is genuinely missing.
3. Write a revised scope statement that composes with existing primitives instead of rebuilding. The revised scope must be smaller than the original brief.
4. Estimate the revised effort in sessions, explaining why it is lower than the original 3-4 session estimate.
5. Identify any capability that IS genuinely new and cannot be satisfied by existing primitives. Write a one-paragraph justification for why this capability requires new code rather than composition.

## Model Answer

**Search results:**

| File | Relevance |
|---|---|
| `adapters/ollama.py` | OllamaStreamAdapter already provides full Ollama integration -- streaming, tool calling, token tracking. This IS the Ollama adapter the brief asks to build. |
| `adapters/openai.py` | OpenAIStreamAdapter works with vLLM by setting `base_url` to the vLLM endpoint. This IS the vLLM adapter the brief asks to build. |
| `config.py` | KzConfig already supports `provider = "ollama"` via TOML or environment variable. Backend detection is partially present. |
| `delegate.py` | Delegate accepts `adapter=OllamaStreamAdapter(...)` directly. Integration is already possible. |
| `adapters/__init__.py` | Adapter registry can resolve providers by name. |

**Capability assessment:**

| Capability | Status | Evidence |
|---|---|---|
| Detect local serving backend | PARTIALLY EXISTS | `config.py` supports `provider = "ollama"` but does not auto-detect which backends are running. |
| Create adapter configuration | EXISTS | `OllamaStreamAdapter` and `OpenAIStreamAdapter` already handle all configuration. |
| Return configured Delegate | EXISTS | `Delegate(adapter=OllamaStreamAdapter(...))` works today. |

**Revised scope:** The "local model serving bridge" is not a new package. It is a 150-250 line convenience layer that: (a) looks up adapter metadata from the adapter registry, (b) determines deployment target (Ollama or vLLM) by checking local endpoints, (c) constructs the appropriate existing adapter with the correct `base_url` and model name, and (d) returns a configured Delegate. No changes to the Delegate or adapter codebase are required.

**Revised effort:** 1 session (down from 3-4). The brief implied building adapter infrastructure from scratch. The search revealed that the adapters already exist and are production-tested. The only genuinely new work is the convenience wrapper and endpoint auto-detection.

**Genuinely new capability:** The `local_hf` strategy (direct in-process HuggingFace inference) has no existing adapter and would require implementing streaming generation from `transformers.pipeline`. This is explicitly a development-only feature and can be deferred. Everything else composes from existing primitives.

This matches the real outcome from journal entry `0005-DISCOVERY-delegate-already-supports-local-models`: "KaizenModelBridge is simpler than expected. It does not need to create new adapters or modify the Delegate system. It is a pure convenience layer."

## Scoring Criteria

1. **Search before build** (pass/fail): The practitioner must produce search results BEFORE any design or code. A practitioner who sketches a module architecture first and then checks for conflicts is failing the drill even if they eventually find the existing primitives.
2. **Scope reduction** (pass/fail): The revised scope must be strictly smaller than the original brief. If the practitioner's revised scope is the same size or larger, they either missed the existing primitives or did not compose with them.
3. **Specificity of search results** (pass: 4/5 files found): The practitioner must find at least 4 of the 5 relevant files. Missing `adapters/openai.py` (which handles vLLM) is the most common omission because the filename does not mention "vLLM."
4. **Honest gap identification** (pass/fail): The practitioner must identify at least one genuinely new capability (auto-detection of running backends or `local_hf` support) rather than claiming "everything already exists." Over-fitting to existing primitives by ignoring real gaps is as wrong as ignoring existing primitives by rebuilding.

## Common Mistakes

1. **Skipping the search because of assumed knowledge.** The practitioner reads the brief, sees "local model serving," and begins designing a new adapter system because they assume the codebase does not already support local models. The MCP consolidation case (journal 0033) shows the extreme version of this failure: six separate MCP server implementations existed because each session assumed no prior art existed and built from scratch. The fix is making the search mandatory and explicit -- not a mental check but a codebase search with results written down.

2. **Searching documentation instead of code.** The `README.md` does not mention local model serving. A practitioner who searches only documentation will conclude the capability is missing. The actual evidence lives in the adapter source files. The framework-first check must search code, not docs.

3. **Finding the existing primitive but building a wrapper that reimplements it.** The practitioner finds `OllamaStreamAdapter` but then designs a `LocalModelAdapter` that wraps it while duplicating its configuration logic. The correct composition is to construct the existing adapter with the correct parameters, not to wrap it in a new abstraction that re-derives what the adapter already knows. The data-fabric-engine case (journal 0005) shows the correct outcome: the entire new feature reduced to three new concepts on top of unchanged existing primitives.

## Extensions

1. **Consolidation variant.** Instead of a brief that asks to build one new thing, give the practitioner a codebase that already contains three implementations of the same concept (modelled on the six MCP servers from journal 0033). The task: identify all three, evaluate which is the best, and write a consolidation plan that replaces the other two with thin wrappers around the canonical implementation. Score on whether the practitioner identifies all three and consolidates to one rather than building a fourth.

2. **Framework-first across repos.** Give the practitioner access to two related repositories (e.g., kailash-py and kailash-rs). The brief asks to build a feature in one repo, but the feature already exists in the other. The practitioner must search both repos, identify the cross-repo prior art, and decide whether to port, wrap, or re-implement. This tests whether the framework-first instinct extends across repository boundaries.

3. **False positive.** Give the practitioner a codebase with a file whose name suggests it handles the brief's functionality but whose implementation is an empty stub or an incompatible abstraction. The practitioner must distinguish between "this primitive exists and can be composed with" and "this primitive exists in name only and must be replaced." Score on whether the practitioner reads the implementation rather than trusting the filename.
