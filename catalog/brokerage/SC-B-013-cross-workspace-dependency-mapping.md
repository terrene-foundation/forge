---
atom_id: SC-B-013
name: Map cross-workspace dependency surface before parallelizing
craft_layer: brokerage
destination: co-codegen
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - CO §27 — Structured Workflow
  - CO §17 — Single Source of Truth
journal_evidence:
  - path: loom/kailash-py/workspaces/dataflow-enhancements/journal/0003-CONNECTION-six-features-share-engine-bottleneck-and-cross-workspace-dependencies.md
    polarity: MIXED
    quote: "The critical insight is that all features converge on engine.py (6,400 lines). Every feature modifies DataFlow.__init__ or @db.model or adds properties. Four features also modify express.py (TSG-102, TSG-103, TSG-104, TSG-105). This creates two bottleneck files that every feature must touch."
  - path: loom/kailash-py/workspaces/kailash/journal/0018-DECISION-five-workspace-parallel-implementation.md
    polarity: POSITIVE
    quote: "Cross-workspace dependencies were mapped in Phase 0 (kailash-ml-protocols \u2192 kailash-align, Nexus EventBus \u2192 DataFlow events, MCP deletions \u2192 Nexus B0b sequencing)."
practice_modality: brokerage-rep
status: verified
---

## The move

Before launching parallel implementation across multiple workspaces, map the shared-file and shared-abstraction surface. For each pair of workspaces, identify: (a) files both will modify, (b) abstractions one produces that the other consumes, (c) sequencing constraints that prevent true parallelism. Produce a dependency graph with bottleneck files marked. Declare which workspaces can run in parallel and which must be sequenced — and which files require module extraction before parallel work is safe.

## When it fires

Whenever the plan calls for parallel execution across two or more workspaces that touch overlapping code. The trigger is "we plan to run N workspaces concurrently" — the dependency map is the check that concurrency is safe and the bottlenecks are managed.

## What it serves

CO §27 (Structured Workflow) requires at least two phases and at least one approval gate, and explicitly requires that phase-skipping be prevented. Launching parallel workspaces without a dependency map is phase-skipping at the coordination level — the analysis phase (what depends on what?) is skipped in favour of jumping straight to implementation. CO §17 (Single Source of Truth) applies because a shared bottleneck file modified by two parallel workspaces produces two competing truths that must be reconciled at merge time; the dependency map surfaces this cost up front.

## Evidence walkthrough

- **dataflow-enhancements 0003-CONNECTION** (MIXED) — Six features all converge on engine.py (6,400 lines) and four also modify express.py. The entry explicitly notes that the brief's claimed parallelisation to "~2 sessions wall clock" is optimistic because it requires careful coordination of engine.py changes across branches. The mitigation (separate modules, minimal engine.py hooks) is sound but requires discipline. Additionally, the entry maps cross-workspace connections: DataFlow's TSG-201 and Nexus's B0a both introduce EventBus integrations, and if their event types diverge, the eventual bridge becomes an adapter layer. This is a concrete example of the dependency map surfacing a design coordination need.
- **kailash 0018-DECISION-five-workspace-parallel-implementation** (POSITIVE) — Five workspaces were implemented in a single session, but only after cross-workspace dependencies were mapped in Phase 0. The entry names the specific dependency chains: kailash-ml-protocols must exist before kailash-align can consume them, Nexus EventBus must exist before DataFlow events can bridge to it, MCP deletions must happen before Nexus B0b refactors the gateway. The dependency map enabled parallel execution by identifying what could and could not overlap.

## How to drill it

Take three mock workspaces (W1, W2, W3) with a shared codebase of 10 files. W1 modifies files A, B, C. W2 modifies files B, D, E. W3 modifies files C, E, F. W2 also consumes an abstraction W1 produces. Produce a dependency graph: which pairs conflict on shared files? Which have a producer-consumer sequencing constraint? Which can truly run in parallel? Present a concrete execution plan (sequential, parallel, or mixed) with justification for each decision. Score: does the practitioner identify B and E as merge-conflict risks, C as a separate conflict, and the W1-before-W2 sequencing constraint?

## Common failure mode

Practitioner identifies the producer-consumer dependency (W1 before W2) but misses the shared-file bottleneck (files B, C, E). The workspaces launch in parallel, produce clean results individually, and then produce substantial merge conflicts when integrated. The dependency map must cover both abstraction-level and file-level dependencies.

## Related atoms

- SC-B-001 (read-then-merge sync) — when dependency mapping spans repos, the sync mechanism is how shared artifacts are reconciled after parallel execution.
- SC-B-007 (worktree-per-agent) — the mechanism that enables safe parallel execution once the dependency map confirms it is safe.
- SC-B-006 (multi-round red team convergence) — the validation that catches integration issues the dependency map missed.
- SC-B-010 (drift detection audit) — the drift audit surfaces shared-file conflicts that the dependency map should have predicted; a post-parallel drift audit validates the map's completeness.
- SC-P-018 (split the long pole) — splitting a multi-session blocker into parallel halves requires dependency mapping to confirm the halves are truly independent.
