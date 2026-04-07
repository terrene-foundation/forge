---
atom_id: SC-B-008
name: Convergence-as-validation across independent reds
craft_layer: brokerage
destination: co-codegen
craft_areas: [attend-how, attend-when]
applications: [COC, COR, COE]
spec_lineage:
  - CO §5.4 — Defense in Depth
  - CO §49 — Quality Criteria — Enforcement Reliability
journal_evidence:
  - path: loom/kz-engage/workspaces/herald/journal/0009-DISCOVERY-redteam-convergence-findings.md
    polarity: POSITIVE
    quote: "Two independent red team agents (deep analyst + security reviewer) converged on the same 5 critical issues without coordination. Convergence on 3/5 critical findings validates the multi-agent red team approach. The 2 non-overlapping findings justify running both perspectives."
  - path: loom/kailash-rs/workspaces/cross-sdk-governance/journal/0008-RISK-cross-feature-vacancy-bridge.md
    polarity: POSITIVE
    quote: "Two independent red team agents (security-reviewer and deep-analyst) discovered that `approve_bridge()` and `reject_bridge()` did not check whether the approver/rejector role was vacant."
  - path: loom/journal/0042-RISK-kailash-ml-rs-redteam-round1.md
    polarity: POSITIVE
    quote: "Three red team agents attacked the 500KB analysis corpus from three angles: architecture gaps (contradictions/missing pieces), gradient boosting engine design (algorithmic correctness/features), and feasibility risks (practical implementation threats)."
practice_modality: observation
status: draft
---

## The move

Run two or more independent red team agents against the same artifact, each from a different angle or with a different specialization (e.g., security reviewer vs. deep analyst vs. feasibility assessor). Do not share findings between agents until both complete. Compare the finding sets: convergent findings (independently discovered by multiple agents) are high-confidence issues; non-overlapping findings justify the cost of running multiple perspectives. The convergence rate itself validates or invalidates the audit method — if two agents find zero overlap, the method is not comprehensive.

## When it fires

When a single-perspective red team has completed and the practitioner needs to assess whether the audit was thorough enough. Also fires when the stakes of a missed finding are high enough that a single viewpoint is insufficient — governance modules, security-critical paths, architectural decisions that constrain future work. The trigger is "how confident am I that this one red team caught everything?" — if the answer requires evidence rather than assertion, run a second independent perspective.

## What it serves

CO §5.4 (Defense in Depth) requires multiple independent enforcement layers for critical rules. Convergence-as-validation applies the same principle to the audit layer: multiple independent red team perspectives are the defense-in-depth for the validation process itself. CO §49 (Quality Criteria — Enforcement Reliability) demands that enforcement be verified by intentional violation attempts. When two independent agents both find the same violation, the finding's validity is corroborated. When each agent finds a violation the other missed, the combined coverage exceeds what either could achieve alone.

## Evidence walkthrough

- **kz-engage 0009** (POSITIVE) — Two independent agents (deep analyst, security reviewer) converged on 3 of 5 critical findings in the Herald codebase: unbounded rate limiter dicts, SQLite audit thread safety, and health endpoint DoS. The two non-overlapping findings (null byte passthrough found only by the security reviewer; model fallback race found only by the deep analyst) demonstrate that each perspective contributes unique coverage. The entry names this explicitly: "The 2 non-overlapping findings justify running both perspectives."
- **kailash-rs 0008** (POSITIVE) — Two independent agents both discovered that `approve_bridge()` and `reject_bridge()` failed to check vacancy status. The root cause was that vacancy enforcement (issue #107) and bridge approval (issue #106) were developed as independent features without cross-feature interaction testing. The convergent finding proved the gap was real, not an artifact of one agent's interpretation.
- **loom-meta 0042** (POSITIVE) — Three red team agents attacked the kailash-ml-rs analysis from three different angles. The multi-angle approach produced 34 findings across architecture, algorithmic correctness, and feasibility. This is convergence-as-validation at the architecture level: three independent perspectives ensured that the analysis corpus was stress-tested from all relevant directions before implementation began.

## How to drill it

Take a completed red team report (single-perspective). Without reading the findings, assign a second agent with a different specialization to red-team the same artifact. Compare the two finding sets. Score the drill on three measures: (a) convergence rate (what fraction of findings appear in both sets), (b) unique coverage (findings in one set but not the other), and (c) whether the practitioner used the comparison to assess audit completeness rather than just merging the lists. The kz-engage 0009 pattern (3/5 convergent, 2/5 unique) is the reference calibration.

## Common failure mode

Practitioner runs two red team agents but briefs them with the same prompt and the same attack angle, producing cosmetic independence (different sessions) with substantive convergence (same perspective). The result looks like validation but is actually redundancy. The fix is mandating different specializations — one security-focused, one architecture-focused, one feasibility-focused — so the non-overlapping findings are genuinely different rather than stochastic variations of the same viewpoint.

## Related atoms

- SC-B-006 (multi-round red team) — convergence-as-validation is orthogonal to multi-round convergence. Multi-round measures whether findings decrease over time; convergence-as-validation measures whether independent perspectives agree.
- SC-B-005 (spec-to-code conformance) — a conformance audit is one specialization that can be combined with a security audit for convergence measurement.
