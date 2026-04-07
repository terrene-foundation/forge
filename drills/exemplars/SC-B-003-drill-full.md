---
drill_id: DR-SC-B-003-FULL
source_atom: SC-B-003
difficulty: beginner
time_box_minutes: 15
---

# Variant Classification -- Single-Sentence Test Rule

## Setup

The practitioner receives a working directory containing 10 CC artifact files drawn from a real COC template repository. The files are a mix of rules, agents, and skills, presented as plain filenames with no directory context -- the practitioner does not know whether each file originally lived in a global or variant directory. The files are:

1. `zero-tolerance.md` -- a rule demanding no stubs, no placeholders, fix what you find
2. `connection-pool.md` -- a rule prescribing Kailash DataFlow connection pool patterns
3. `security-reviewer.md` -- an agent that performs security audits on any codebase
4. `dataflow-specialist.md` -- an agent for Kailash DataFlow database operations
5. `testing-strategies/` -- a skill covering 3-tier testing methodology
6. `kailash-mcp/` -- a skill covering Kailash MCP server integration
7. `git.md` -- a rule requiring conventional commits and branch naming
8. `python-environment.md` -- a rule covering Python venv setup for Kailash projects
9. `communication.md` -- a rule for outcome-focused language in reports
10. `trust-plane-security.md` -- a rule for Kailash trust-plane crate patterns

The practitioner also receives a one-page reference sheet containing:
- The variant overlay architecture: globals live in the base `.claude/` tree; variants live in `variants/py/` or `variants/rs/` and override or extend globals for language-specific targets.
- The single-sentence test: **"Does this rule apply if the user is NOT using Kailash?"** YES means global. NO means variant.
- A note that subsidiary tests (language specificity, tier membership, project-locality) apply only after the primary test resolves.

An answer key is available from journal entry `0012-DECISION-core-vs-plugin-classification` (kaizen-cli-rs), which contains the full classification table for 19 rules, ~30 agents, and ~38 skill directories with disposition and rationale for each.

## Task

1. For each of the 10 artifacts, write down your classification (GLOBAL or VARIANT) within 30 seconds. Cite the single-sentence test explicitly: state the counterfactual ("If the user is not using Kailash, does this rule still apply?") and record the answer.
2. After classifying all 10 from the name alone, open each file and read its content.
3. For any artifact where reading the content changes your classification, write one sentence explaining why your first instinct was wrong. Was it a content surprise, or did you apply a secondary test (file size, apparent author, sync convenience, directory association) before the primary test?
4. Produce a final table with columns: Artifact, Initial Classification, Final Classification, Rationale.
5. Compare your final table against the answer key from `0012-DECISION-core-vs-plugin-classification`. Count how many you got right. For any disagreement, write one sentence on what the answer key's reasoning teaches you.

## Model Answer

The correct classifications, derived from journal entry 0012 and the single-sentence test:

| Artifact | Classification | Reasoning |
|---|---|---|
| `zero-tolerance.md` | GLOBAL | "Fix what you find, no stubs" applies to any project, not just Kailash projects. |
| `connection-pool.md` | VARIANT | Connection pool patterns are specific to Kailash DataFlow. Without Kailash, there is no connection pool to manage. |
| `security-reviewer.md` | GLOBAL | Security audits apply to any codebase. The agent does not reference Kailash-specific APIs. |
| `dataflow-specialist.md` | VARIANT | DataFlow is a Kailash framework. Without Kailash, there is no DataFlow to specialise in. |
| `testing-strategies/` | GLOBAL | 3-tier testing (unit, integration, E2E) applies universally. The real-infrastructure policy is not Kailash-specific. |
| `kailash-mcp/` | VARIANT | MCP integration patterns specific to Kailash's FastMCP server. Without Kailash, these patterns do not apply. |
| `git.md` | GLOBAL | Conventional commits, branch naming, no secrets in commits -- universal to all git repositories. |
| `python-environment.md` | VARIANT | Python venv setup specifically for Kailash project dependencies. Without Kailash, the environment rules differ. |
| `communication.md` | GLOBAL | Outcome-focused language, plain language for non-coders -- applies to any project communication. |
| `trust-plane-security.md` | VARIANT | Trust-plane crate patterns are Kailash-specific cryptographic constructs. Without Kailash, there is no trust plane. |

Final score: 6 GLOBAL, 4 VARIANT. The journal entry 0012 achieved a similar partition across the full artifact set: 19 CORE rules vs 10 PLUGIN rules, with a 42% token-budget saving from the classification.

## Scoring Criteria

1. **Primary test applied first** (pass/fail): For each artifact, the practitioner must write the counterfactual question before writing the classification. Practitioners who write "VARIANT -- it's a Kailash file" without the counterfactual are failing even if the classification is correct.
2. **Classification accuracy** (pass: 8/10 or better): The practitioner correctly classifies at least 8 of the 10 artifacts. The two most commonly missed are `python-environment.md` (feels universal but is Kailash-specific) and `testing-strategies/` (feels Kailash-specific but is universal).
3. **Self-correction quality** (pass/fail): For any artifact where the classification changed after reading the file, the practitioner names the specific reasoning error -- not just "I was wrong" but "I applied directory association before the primary test" or "I assumed Python rules are always variant."
4. **Answer-key comparison** (pass/fail): The practitioner produces a written comparison against the answer key, with at least one learning per disagreement.

## Common Mistakes

1. **Classifying by file location instead of content.** The practitioner sees a filename like `python-environment.md` and assumes "Python-specific means variant." But the real question is whether the rule's content applies without Kailash. Many Python rules are universal (e.g., "use virtual environments" is good practice everywhere). In this case `python-environment.md` IS a variant because its content prescribes Kailash-specific dependency setup, but the practitioner must arrive at that answer through the content test, not through the filename. The Gate 1 rs review (journal 0048) found that 15 out of 43 directories would have been misclassified by this shortcut.

2. **Applying subsidiary tests before the primary test.** The practitioner thinks "this file is large, so it must be important, so it should be global" or "this was written recently during a Kailash session, so it must be variant." File size, authorship, and sync convenience are irrelevant to the classification. The single-sentence test exists precisely to override these secondary considerations. Journal entry 0048 shows that applying the test post-hoc (after initial population) rather than at authoring time cost an entire codify session of reclassification work.

3. **Treating "universal within the Terrene ecosystem" as global.** The practitioner classifies `terrene-naming.md` or `independence.md` as variant because "only Terrene projects need these." But the test is "does this apply if the user is NOT using Kailash?" -- and Terrene naming rules apply even in non-Kailash Terrene projects. The distinction is Kailash-specificity, not Terrene-specificity.

## Extensions

1. **Ambiguous-case gauntlet.** Add 5 deliberately ambiguous artifacts where the classification is non-obvious: `e2e-god-mode.md` (Playwright patterns -- universal or Kailash-specific?), `cc-artifacts.md` (Claude Code quality -- universal to CC but not to non-CC projects), `cross-sdk-inspection.md` (cross-SDK alignment -- Terrene governance or Kailash-specific?). The practitioner must write a 2-sentence justification for each, and the drill leader reviews whether the reasoning follows the primary test or shortcuts to intuition.

2. **Post-hoc reclassification under pressure.** Give the practitioner the full 43-directory skill listing from journal entry 0048 (Gate 1 rs variant classification). They have 15 minutes to classify all 43 using the single-sentence test, simulating the real Gate 1 review pressure. Score on whether they maintain the primary-test-first discipline under time pressure or revert to shortcut heuristics. The answer key from the journal entry records that only 2 of 43 directories actually needed upstreaming as new rs variants.

3. **Misclassification blast-radius analysis.** Take one artifact that was misclassified (e.g., a global rule placed as variant-rs) and trace the consequences through the sync system. What happens when `/sync` runs? Which downstream repos get the wrong content? How many repos are affected? The practitioner must produce a blast-radius diagram showing the sync-manifest path from the misclassified file to every affected USE template and downstream project.
