---
source: analyst (failure-analysis lens)
date: 2026-04-07
session: forge-curriculum reframe (craft school)
destination: lyceum/programs/forge (curriculum) — also informs atelier/co-codegen anti-patterns
status: pre-rigor-bar — do not use as curriculum source
---

# Failure Modes of Methodology-Only FORGE Framing

## Executive Summary

A methodology-only graduate of FORGE-COC will fluently describe Five-Layer Architecture, CARE planes, and PACT D/T/R, yet ship demonstrably broken COC systems. The journal corpus shows that COC competence lives in micro-decisions invisible to a curriculum: when to distrust an agent's self-report, when stderr means "the user sees it" vs "the model sees it," when a hook is silently dead, when a "fix" is contamination. These are craft instincts learned from thousands of observed failures.

## Failure Modes

### 1. Context-channel confusion (the stderr-vs-Claude failure)

A methodology graduate building a session-notes hook will print to stderr and assume "Claude saw it." Journal `0017-DISCOVERY-session-notes-broken.md` shows this exact failure shipped: `[WORKSPACE] Session notes: 3h ago` went to the user's terminal while Claude's context received nothing. The anti-amnesia mechanism was structurally complete and functionally dead. Graduates lack the instinct that stderr, system messages, UserPromptSubmit injection, and tool output are _different channels with different audiences_.

**Cost**: silently broken continuity infrastructure that the buyer believes is working — the worst class of bug because it produces no error.

### 2. Trusting in-session agent self-reports as evidence

A methodology graduate evaluates rule necessity by asking agents "what would you do without this rule?" Journal `0049-DISCOVERY-coc-token-ablation-experiment.md` shows the contamination trap: in-session agents _overcorrected_ to look vanilla and gave the opposite answer from true clean-room baseline. Without the craft instinct that "agents lie about themselves under instruction pressure," graduates will make rule-pruning decisions on contaminated data and ship COC variants that omit rules the model actually needs.

**Cost**: token-budget tiers calibrated on false data — the employer pays for security holes (MiniMax plaintext password comparison) the graduate "proved" weren't needed.

### 3. Treating "structurally complete" as "working"

Journal `0004-RISK-learning-system-broken.md` documents Layer 5 with 4 commands, 4 lib files, 4,579 observations — and zero useful output (99% infrastructure noise, 8 identical instincts at hardcoded 0.9 confidence). A methodology graduate ships the architecture and declares victory. They lack the instinct to ask "what does the actual data stream contain?" — the discipline of inspecting captured observations for signal vs noise.

**Cost**: the learning loop the client paid for produces noise forever; institutional knowledge never compounds.

### 4. execSync where execFileSync was required

Journal `0050-DISCOVERY-session-start-command-injection.md`: a `version_url` from `.claude/VERSION` passed through `execSync` is arbitrary code execution on session start from any cloned repo. A methodology graduate writes hooks knowing "validate inputs" abstractly but lacks the muscle memory that _every shell-interpreting call is a vulnerability surface in a hook_.

**Cost**: supply-chain RCE shipped to every downstream repo on sync.

### 5. Drift blindness — assuming sync = aligned

Journal `0014-DISCOVERY-drift-audit.md`: 85% drift between py and rs templates, 66% of skill files diverged, with no mechanism to distinguish intentional from accidental drift. A methodology graduate teaching "BUILD → USE distribution" without the audit instinct will let templates rot for months before noticing.

**Cost**: every downstream repo inherits stale rules; the COC platform silently degrades.

### 6. Missing the "frontmatter scope" reflex

Journal `0003-DISCOVERY-cc-artifact-audit.md` shows ~1,300 lines/turn wasted because rules lacked `paths:` frontmatter — a 48% baseline reduction left on the table. A methodology graduate writes correct rules that load globally and burns the client's token budget every turn, every session, forever.

**Cost**: directly metered in API spend.

### 7. Compression without ablation evidence

Graduate cuts rules by intuition rather than running clean-room `claude -p` baselines (`0049`). Removes things the model needed; keeps things it didn't.

**Cost**: regression in compliance behavior with no diagnostic trail back to which removal caused it.

## Skill Atoms Implied (not modules — atoms)

Channel-routing literacy; agent-self-report skepticism; data-stream inspection; shell-call paranoia; drift auditing; frontmatter scoping reflex; clean-room ablation discipline.

## Source Files Referenced

- `/Users/esperie/repos/loom/journal/0049-DISCOVERY-coc-token-ablation-experiment.md`
- `/Users/esperie/repos/loom/journal/0050-DISCOVERY-session-start-command-injection.md`
- `/Users/esperie/repos/loom/journal/0017-DISCOVERY-session-notes-broken.md`
- `/Users/esperie/repos/loom/journal/0014-DISCOVERY-drift-audit.md`
- `/Users/esperie/repos/loom/journal/0011-RISK-redteam-r1-findings.md`
- `/Users/esperie/repos/loom/journal/0001-DECISION-co-refocusing.md`
- `/Users/esperie/repos/loom/journal/0004-RISK-learning-system-broken.md`
- `/Users/esperie/repos/loom/journal/0003-DISCOVERY-cc-artifact-audit.md`
- `/Users/esperie/repos/loom/journal/0034-RISK-stack-gaps-redteam-attack.md`
