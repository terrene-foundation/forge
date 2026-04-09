---
case_id: CASE-22
source_path:
  - loom/kailash-py/workspaces/data-fabric-engine/journal/0001-DECISION-dataflow-extension-not-separate-package.md
  - loom/kailash-py/workspaces/data-fabric-engine/journal/0004-DECISION-dataflow-is-the-fabric.md
title: "Decision Reversal in One Day"
atoms_served: [decision-reversal-journaling, supersede-with-reference]
spec_concepts: [CO §16, CO §17, CO §28]
craft_layer: brokerage
recommended_use: teaching
destination: both
application: COC
---

# CASE-22: Decision Reversal in One Day

## 1. The Situation

The Kailash Python SDK needed a data fabric engine -- a way to unify access across databases, APIs, files, and other data sources. The existing DataFlow package handled database operations (zero-config CRUD, model-to-node generation). The question was where to put the new fabric capability. Three options were on the table: a new standalone package (`kailash-fabric`), an extension of DataFlow (`kailash-dataflow[fabric]`), or an evolution of DataFlow itself.

The initial analysis proposed a separate `kailash-fabric` package. Two independent red team passes reviewed this proposal and both recommended against it, converging on the extension approach instead. The platform already had 8 top-level packages; a 9th would add cognitive load, version matrix complexity, and platform dilution.

## 2. The Trigger

On session turn 3, the decision was recorded: ship fabric as `kailash-dataflow[fabric]`, not as a separate package. The rationale was sound -- existing DataFlow users discover fabric organically, no new package to learn, easy to extract later if needed. The journal entry (0001-DECISION) documented the decision, the alternatives considered, and the consequences.

Two turns later, the programme owner challenged the decision. If DataFlow's core abstractions (BaseAdapter, Express API, cache invalidation) were already source-agnostic, why treat fabric as an add-on at all? The attentional skill is **assumption-reversal**: questioning whether the framing ("DataFlow is a database tool that can be extended") is the right framing, or whether a deeper truth ("DataFlow is already a data tool that happens to start with databases") changes the answer. After auditing DataFlow's actual abstractions, the challenge held: BaseAdapter had no SQL in its interface, Express API dispatched on string names and dicts, and cache invalidation was semantic (model + operation), not database-specific.

## 3. The Move

The move is **decision-reversal-journaling** combined with **supersede-with-reference**. Rather than silently updating the first decision or pretending the extension decision had never been made, a new journal entry (0004-DECISION) was created that explicitly stated: "This supersedes 0001-DECISION-dataflow-extension-not-separate-package.md." The new decision -- "DataFlow IS the fabric" -- went further than the extension approach: DataFlow's mission evolves from "zero-config database operations" to "zero-config data operations." No separate package. No extension. The identity of the component changes.

The craft is in how the reversal was handled. The original decision was not deleted or edited. It remains in the journal as a record of the reasoning that led to the extension approach. The superseding entry references it by filename, explains what changed (the programme owner's challenge plus the architectural evidence), and records the new consequences. Anyone reading the journal can trace the full arc: separate package proposed, red team recommends extension, extension decided, programme owner challenges framing, identity evolution decided. The decision trail is complete and legible.

## 4. The Outcome

DataFlow's scope expanded from database operations to data operations. New decorators (`@db.source()` for external sources, `@db.product()` for materialized views) were added alongside the existing `@db.model` for database tables. The change was 100% backward compatible -- all existing `@db.model` code continued to work unchanged. The reversal happened within the same session (turns 3 to 5), with zero wasted implementation work because no code had been written against the extension approach yet. The journal now contains a clean decision chain that any future reader can follow without confusion about which decision is current.

## 5. The Spec Connection

**CO §16 (Framework-First)** requires building on existing frameworks rather than creating parallel ones. The separate-package option violated this principle directly (a new package reimplementing caching and endpoint serving). The extension option partially satisfied it (building on DataFlow but treating fabric as an add-on). The final decision fully satisfied it: DataFlow's existing abstractions already supported the new capability, so the framework was not extended but recognized as already sufficient. Framework-first is not just "use what exists" but also "understand what already exists before deciding it is insufficient."

**CO §17 (Single Source of Truth)** requires that each concept has one canonical location. Under the extension model, "data access" would have had two entry points: `@db.model` for databases and a separate fabric API for everything else. Under the identity-evolution model, DataFlow is the single source of truth for all data operations. The `@db.source()` and `@db.product()` decorators live alongside `@db.model` in the same namespace, discoverable through the same import.

**CO §28 (Approval Gate)** defines the checkpoint where decisions are vetted before implementation proceeds. The first decision passed an approval gate (two red team passes agreed on the extension approach). The second decision was triggered by a programme-owner challenge at an informal gate. The case shows that approval gates are not single-pass -- a decision can pass vetting and still be correctly reversed when new evidence (the architectural audit of BaseAdapter's source-agnosticism) surfaces. The journal makes the reversal auditable rather than opaque.

## 6. Discussion Questions

1. The first decision was supported by two independent red team passes. If both red teams recommended the extension approach, what signal did the programme owner see that the red teams missed? Is there a structural reason why red teams might optimize for the safest incremental option (extension) rather than questioning the framing itself (identity evolution)?

2. Had two days of implementation work been completed against the extension approach before the reversal, what would the cost of the reversal have been? At what point does sunk cost in an implementation make a clean decision reversal impractical, and what does the journal trail look like when a reversal happens after partial implementation?

3. The supersede-with-reference pattern (new entry explicitly names the entry it replaces) preserves the full decision arc. An alternative is to edit the original entry with a "SUPERSEDED" header and a link to the new entry. What are the trade-offs between these two approaches? Does one degrade more gracefully when the journal grows to hundreds of entries?
