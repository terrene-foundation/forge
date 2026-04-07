---
source: uiux-designer (AI interaction patterns lens)
date: 2026-04-07
session: forge-curriculum reframe (craft school)
destination: lyceum/programs/forge (curriculum spine — practitioner craft)
status: pre-rigor-bar — do not use as curriculum source
---

# CO/COC Practitioner Craft: Attentional and Interventional Pattern Catalog

## (a) Attentional Patterns — what to notice

| #   | Pattern                              | Symptom / real-session example                                                                                                                                                                    | Source                  |
| --- | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| A1  | **Contamination Drift**              | An agent inside a rule-loaded session "tries to act vanilla" and overcorrects, producing answers neither rule-bound nor truly baseline ("file a ticket and move on").                             | 0049 Phase 1            |
| A2  | **Tool-Confident Misuse**            | Model names the right tool but uses it wrong: `f"...{psycopg2.sql.Identifier(name)}..."`. Looks competent at a glance — fails on inspection.                                                      | 0049 (MiniMax)          |
| A3  | **Silent Mechanism Failure**         | Hook prints `[WORKSPACE] Session notes: 3h ago` to terminal but never injects into model context. The signal is visible to the human but invisible to the AI.                                     | 0017                    |
| A4  | **Convention Drift Across Edits**    | Skill numbering diverges between repos (rs `05=enterprise`, py `05=mcp`) because each /codify session assigned numbers locally with no global cross-check.                                        | 0020                    |
| A5  | **Context Bleed (Repo→Repo)**        | "BUILD repo" wording from kailash-py leaking into coc-claude-py USE repo. The wrong rule is firing in the wrong place.                                                                            | 0003 H4, 0018           |
| A6  | **Scope Leak (always-loaded rules)** | 1,300 lines/turn of pact-governance and trust-plane-security loading on every edit because rules use prose-scope, not `paths:` frontmatter.                                                       | 0003 H1                 |
| A7  | **False-Positive Learning**          | Layer-5 produces 4,579 "observations," 99% session-start/stop noise, and 8 identical "workflow_builder pattern" instincts at hardcoded confidence 0.9. Output that looks like learning but isn't. | 0004                    |
| A8  | **Plan/Estimate Compression**        | A deep analysis that itself estimates 7 sessions gets restated as "2-3 sessions" in a downstream plan with no justification.                                                                      | 0034 F1                 |
| A9  | **Integration-Seam Blindness**       | Each workspace's tests pass; nobody is exercising the seam where domains meet. The model is confident locally and ignorant globally.                                                              | 0034 F2                 |
| A10 | **Moving-Target Build**              | MCP introspection tools written against registries that are simultaneously being refactored — code that is correct at write-time and stale at run-time.                                           | 0034 F3                 |
| A11 | **Cargo-Cult Citation**              | Agent references a skill or file that no longer exists (RS agents pointing to deleted `10-governance`, `32-kaizen-agents`). The shape of an authority claim, no actual authority.                 | 0011 RT3-1              |
| A12 | **Caveat-as-Fix**                    | Finding logged with severity, then no fix. Acknowledgement substituting for action ("pre-existing, not in scope").                                                                                | zero-tolerance.md, 0011 |
| A13 | **Untrusted-Input Execution Path**   | `execSync` reading from `.claude/VERSION` — a model would happily generate this code because it looks like file IO, not an injection sink.                                                        | 0050                    |
| A14 | **Assumption Projection**            | "We assumed security.md was model-native" — turned out true for Claude, false for MiniMax (plaintext password compare). The assumption was about _the model_, not the work.                       | 0049 finding 4          |

## (b) Interventional Patterns — moves to make

| #   | Move                              | When to apply                                                                                                                                                                      | Scope        |
| --- | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| I1  | **Clean-Room Restart**            | When you suspect contamination drift (A1) — start a fresh `claude -p` in `/tmp/...` with no rules loaded, re-ask the question, compare.                                            | CO-general   |
| I2  | **Pre-Inject Context**            | When a mechanism is silent to the model (A3), move the signal from terminal/stderr into UserPromptSubmit or the next user turn.                                                    | COC-specific |
| I3  | **Narrowing (paths: scope)**      | When always-loaded rules are bleeding (A6), add `paths:` frontmatter so the rule only fires on the relevant directory.                                                             | COC-specific |
| I4  | **Spec-Citation Redirect**        | When the model drifts from intent, cite the exact spec clause (CARE Trust Plane, EATP posture, zero-tolerance Rule 1). The citation re-anchors the Mirror.                         | CO-general   |
| I5  | **Split the Turn (pre/post)**     | When a complex turn would interleave decisions you must approve, force a pre-turn (analysis only, no edits) and a post-turn (execute approved plan).                               | CO-general   |
| I6  | **Gate-Insertion**                | When you spot integration blindness (A9) or moving-target builds (A10), insert a gate workspace/phase that nothing else can pass through.                                          | CO-general   |
| I7  | **Estimate Re-derivation**        | When a downstream estimate compresses an upstream one (A8), force the model to recompute from line-counts and coupling points, not from the compressed claim.                      | CO-general   |
| I8  | **Empirical Ablation**            | When deciding whether a rule is "model-native," don't argue — run the rule-removed scenario in isolation. Decision driven by evidence, not intuition.                              | COC-specific |
| I9  | **Tool-Substitution**             | When you see tool-confident misuse (A2), don't correct the call site — replace `execSync` with `execFileSync`, replace raw SQL with DataFlow. Remove the foot-gun, don't patch it. | COC-specific |
| I10 | **Reference Sweep**               | When an artifact is removed/renamed, immediately grep for dangling references and update them in the same turn (A11 prevention).                                                   | COC-specific |
| I11 | **Fix-Now Owning**                | When you find a pre-existing failure, take ownership in this turn — diagnose, fix, regression-test, commit. No deferral language.                                                  | CO-general   |
| I12 | **Variant Demotion**              | When content is leaking across domains (A5), reclassify Global → Variant or Always-loaded → On-demand and re-sync.                                                                 | COC-specific |
| I13 | **Required-Tool Declaration**     | When silent degradation is possible, declare required dependencies upfront so absence is fail-fast, not silent-wrong.                                                              | CO-general   |
| I14 | **Ask-the-Human Before Removing** | Before deleting a half-implemented function, ask the user what it should do. If "remove it," delete cleanly. Never leave a stub.                                                   | CO-general   |

## (c) Prompt-Craft Progression

| Stage | Name                        | What the practitioner does                                                                                                                                                                | What they don't yet do                                                                    |
| ----- | --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| 1     | **Wishing**                 | Writes outcome-shaped requests ("make it work"). Treats the model as a search box. Surprised when output is wrong.                                                                        | Doesn't notice that the model doesn't share their context.                                |
| 2     | **Specifying**              | Lists requirements, file paths, expected behavior. Output quality goes up sharply. Still single-turn.                                                                                     | Doesn't yet split, gate, or restart. Still trusts tone over substance.                    |
| 3     | **Steering**                | Reads stream-of-thought, interrupts mid-action, redirects via citation, knows when to start over. Uses approval gates and pre/post splits (I5, I6, I11).                                  | Still treats each session as isolated; doesn't shape the always-loaded context.           |
| 4     | **Authoring Context**       | Edits CLAUDE.md, rules, hooks, skills, variants. Decides what loads always vs on-demand. Runs ablations. Fingerprints AI-slop.                                                            | May over-engineer the static layer; still calibrating empirical vs intuitive rule weight. |
| 5     | **Calibrating Empirically** | Distinguishes "I think this rule matters" from "ablation showed this rule matters." Picks the _weakest model in scope_ as the target. Treats their own assumptions as testable (A14, I8). | —                                                                                         |

## (d) CO-general vs COC-specific tagging

**CO-general (any AI domain):** A1, A2, A7, A8, A9, A11, A12, A14 / I1, I4, I5, I6, I7, I11, I13, I14, and prompt-craft stages 1–3 and 5.

**COC-specific (codegen-only):** A3, A4, A5, A6, A10, A13 / I2, I3, I8, I9, I10, I12, and the artifact-authoring portion of stage 4.

**Boundary cases:** A1 (contamination) is universal but ablation (I8) as the corrective is COC-shaped because it depends on the existence of a clean-room repo. A2 generalises to any tool-using model, but the specific symptoms (sql.Identifier, execSync) are COC. A4 and A5 are versions of "convention drift" that exist in any long-running CO domain — they merely show up as artifact drift in COC.

## Sources

- `/Users/esperie/repos/training/forge/.claude/skills/25-ai-interaction-patterns/SKILL.md`
- `/Users/esperie/repos/loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md`
- `/Users/esperie/repos/loom/journal/0050-DISCOVERY-session-start-command-injection.md`
- `/Users/esperie/repos/loom/journal/0017-DISCOVERY-session-notes-broken.md`
- `/Users/esperie/repos/loom/journal/0014-DISCOVERY-drift-audit.md`
- `/Users/esperie/repos/loom/journal/0011-RISK-redteam-r1-findings.md`
- `/Users/esperie/repos/loom/journal/0003-DISCOVERY-cc-artifact-audit.md`
- `/Users/esperie/repos/loom/journal/0020-DISCOVERY-rs-skill-numbering-divergence.md`
- `/Users/esperie/repos/loom/journal/0004-RISK-learning-system-broken.md`
- `/Users/esperie/repos/loom/journal/0034-RISK-stack-gaps-redteam-attack.md`
