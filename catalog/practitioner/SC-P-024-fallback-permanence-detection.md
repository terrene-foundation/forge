---
atom_id: SC-P-024
name: Intervene when a temporary workaround accumulates its second consumer
craft_layer: practitioner
destination: forge
craft_areas: [attend-when, intervene-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §3.2 — Convention Drift (Failure Mode)
  - CO §5 — Deterministic Enforcement Over Probabilistic Compliance
journal_evidence:
  - path: loom/journal/0034-RISK-stack-gaps-redteam-attack.md
    polarity: NEGATIVE
    quote: "Once 7+ items build against the non-Transport EventBus, the refactoring cost to unify exceeds the cost of waiting for B0 to complete properly. The fallback becomes permanent architecture."
practice_modality: case
status: verified
---

## The move

Watch for the moment a temporary workaround gains its second consumer. The first consumer is expected — that is why the workaround exists. The second consumer is the signal that the workaround is becoming load-bearing infrastructure. At the second consumer, the practitioner must choose: either complete the proper implementation before the third consumer arrives, or accept the workaround as permanent architecture and invest in making it production-grade. The worst outcome is the middle state: the workaround stays "temporary" while accumulating dependents who each increase the cost of eventually replacing it.

## When it fires

A component was introduced as a bridge, fallback, or workaround with an explicit "replace when X is ready" intention. A second, independent component now depends on it. The trigger is not the workaround's age — it can be minutes old. The trigger is the second consumer, because the second consumer proves the workaround is solving a real need that the intended solution has not yet met, and each additional consumer raises the replacement cost linearly (or worse).

## What it serves

CO §3.2 (Convention Drift) warns that "the AI follows generic practices instead of organizational conventions." A workaround that accumulates consumers is an instance of convention drift: the convention (use the proper Transport interface) is being displaced by the generic practice (use the standalone EventBus that is easier to reach). Each consumer that builds against the workaround is drifting further from the intended architecture. CO §5 (Deterministic Enforcement) states that "critical organizational rules MUST be enforced deterministically." The intended architecture (EventBus as Transport) is a critical convention that is not enforced — nothing prevents a new consumer from importing the standalone EventBus instead of waiting for the Transport interface. Without enforcement, the workaround's consumer count will only grow.

## Evidence walkthrough

- **0034-RISK-stack-gaps-redteam-attack** (NEGATIVE) — Finding 6 analyzed the B0 fallback where the EventBus could be built as a standalone pub/sub (instead of as a Transport) if the transport refactor took too long. The journal entry traced the accumulation path: B2 (EventBus) built as standalone, then B7 (DataFlow-Nexus bridge) builds against it, then B8 (webhooks) builds against it, then A1 (on_source_change) builds against it. Once 7+ items depend on the standalone API, "the refactoring cost to unify exceeds the cost of waiting for B0 to complete properly." The fallback becomes permanent. The entry explicitly warns against invoking the fallback and recommends completing B0a (minimum viable proper implementation) before any fallback activates, making B0b the fallback boundary. The debt compounds further through kailash-rs: if Python B2 is standalone, Rust E3 implements standalone too, requiring a second migration when Python eventually unifies.

## How to drill it

Present the practitioner with a timeline: Day 1 — component X is introduced as a workaround for a blocked proper implementation. Day 2 — component Y imports X to solve an unrelated problem (second consumer). Day 3 — component Z imports X (third consumer). Day 5 — the proper implementation is ready, but replacing X now requires updating Y and Z. The practitioner must: (1) identify Day 2 as the intervention point, (2) explain why Day 2 (not Day 3 or Day 5) is the critical moment, (3) propose one of two actions: fast-complete the proper implementation to cap consumers at 1, or promote the workaround to production-grade with proper tests and documentation. Score on whether the practitioner intervenes at the second consumer rather than waiting for the problem to become obvious at Day 5.

## Common failure mode

The practitioner treats the workaround as "technical debt to address later" and adds it to a backlog. This is the drift mechanism the journal entry warns against: the workaround stays "temporary" on the backlog while consumers accumulate. By the time "later" arrives, the refactoring cost exceeds what any single session can absorb, and the workaround is quietly accepted as permanent without the explicit decision to make it production-grade. The intervention must happen at the second consumer, not when the backlog item eventually surfaces.

## Related atoms

- SC-P-018 (split the long pole) — the B0a/B0b split is the intervention that prevents the fallback from activating; this atom teaches the detection signal (second consumer) that motivates that intervention
- SC-P-020 (second-occurrence structural signal) — parallel principle applied to drift classes: the second occurrence of a drift class triggers structural intervention, just as the second consumer of a workaround triggers permanence intervention
- SC-P-014 (zero-tolerance scope discipline) — zero-tolerance applies to the workaround's consumers: each consumer that builds against the workaround without checking whether the proper implementation is available is a zero-tolerance violation of the intended architecture
- SC-B-014 (explicit supersession journaling) — when a workaround is promoted to permanent, that decision must be journaled with explicit supersession of the original "temporary" framing
