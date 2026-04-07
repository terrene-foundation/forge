---
atom_id: SC-P-011
name: Detect when an audit categorization method is structurally blind to the architecture it audits
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC, COR, COE]
spec_lineage:
  - CO §15 — Progressive Disclosure
  - CO §49 — Quality Criteria — Enforcement Reliability
journal_evidence:
  - path: loom/journal/0005-GAP-redteam-audit-gaps.md
    polarity: NEGATIVE
    quote: "59% of skill files are orphans (204/350 in kailash-py) — Never referenced by any agent or command."
  - path: loom/journal/0011-RISK-redteam-r1-findings.md
    polarity: POSITIVE
    quote: "The numbered skill directories (01-core-sdk through 30-claude-code-patterns) ARE the distilled Kailash documentation. They are discovered by Claude Code's skill system via SKILL.md trigger descriptions — agents don't need to explicitly reference them by name. These skills are NOT orphans. TODO-3.1 is deleted."
practice_modality: case
status: verified
---

## The move

Before trusting an audit finding, check whether the audit's detection method can see the architectural pattern it claims to measure. Specifically: if the architecture uses indirect discovery (progressive disclosure, trigger-based loading, convention-over-configuration), a "referenced by name" grep will report false negatives. The practitioner must match the audit's detection mechanism to the architecture's wiring mechanism and reject any finding where the two are incompatible.

## When it fires

An audit or red team produces a categorical finding with a large percentage ("59% orphan rate," "80% dead code," "no consumers for module X"). The percentage is suspiciously high for a codebase that is functioning in production. The trigger is the mismatch between the audit's confidence and the system's observed behavior: if the system works, and the audit says most of its components are unused, the audit's detection method is the first suspect.

## What it serves

CO §15 (Progressive Disclosure) defines a structural pattern where "minimum context [is provided] by default, deeper reference on demand." This means files do not need to be explicitly referenced by name to be in active use — they are discovered at runtime through trigger descriptions, directory conventions, or index files. CO §49 (Quality Criteria — Enforcement Reliability) requires that enforcement audits reliably detect violations: "Intentionally attempt to violate critical rules and verify hard enforcement catches it." An audit method that cannot see progressive-disclosure wiring produces unreliable enforcement data, violating this quality criterion.

## Evidence walkthrough

- **0005-GAP-redteam-audit-gaps** (NEGATIVE) — The red team audit used "is this file referenced by any agent or command?" as the orphan-detection method and reported 204 of 350 skill files as orphans. The entry itself raised the right question in its "For Discussion" section — "could mean skills are designed for progressive disclosure" — but the finding was already published as a categorical gap with a specific number (59%). The detection method was blind to SKILL.md trigger-based discovery, where Claude Code's skill system reads trigger descriptions to decide when to load a skill, without any agent needing to reference the file by name.

- **0011-RISK-redteam-r1-findings** (POSITIVE) — The next red team round explicitly retracted the finding. The correction identified that numbered skill directories ARE the distilled documentation, discovered via SKILL.md triggers, not by explicit name references. TODO-3.1 (the orphan audit) was deleted entirely. This is the correction loop completing: the audit's structural blindness was recognized and the false finding was withdrawn before it drove a misguided cleanup.

## How to drill it

Present the practitioner with three audit findings, each reporting a high orphan/dead-code rate for a different architectural pattern:

1. A conventional codebase with explicit imports — the audit's grep-for-imports method is appropriate.
2. A plugin system with convention-based discovery (e.g., `plugins/*.py` loaded by glob) — the audit greps for `import plugin_name` and finds nothing.
3. A progressive-disclosure system where an index file (SKILL.md) describes triggers and the runtime loads files based on trigger match.

For each finding, the practitioner must: (a) name the architecture's discovery mechanism, (b) name the audit's detection mechanism, (c) state whether the two are compatible, and (d) if incompatible, propose a detection method that matches the architecture. Score on whether the practitioner catches the mismatch in cases 2 and 3 and proposes a valid alternative detection method (e.g., "scan SKILL.md for trigger descriptions that map to directory names" for case 3).

## Common failure mode

The practitioner trusts the number because it came from a red team. Red team findings carry implicit authority — they are adversarial, rigorous, skeptical. But adversarial rigor in the interpretation of results does not compensate for a structurally wrong detection method. The failure is not in the red team's skepticism but in the assumption that "referenced by name" is the only valid wiring mechanism. A practitioner who defers to the number ("59% is bad, we should clean up") instead of questioning the method ("how does the system actually discover these files?") will delete functioning components.

## Related atoms

- SC-P-002 (model-vs-rule ablation) — another case where the measurement method must match the thing being measured; in-session ablation fails because the detection environment is contaminated.
- SC-B-006 (multi-round red team to numeric convergence) — the retraction in 0011 is an example of a multi-round red team correcting a prior round's finding.
- SC-A-009 (progressive disclosure architecture) — the audit method must understand the 4-level hierarchy's discovery mechanisms; counting "referenced by name" misses skills discovered via globs or directory-listing.
- SC-P-016 (detect overcorrection) — a structurally blind audit method can produce false data that drives overcorrection; detecting the measurement flaw prevents the downstream overcorrection.
