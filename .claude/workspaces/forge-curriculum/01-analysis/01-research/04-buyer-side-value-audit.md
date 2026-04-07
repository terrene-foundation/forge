---
source: value-auditor
date: 2026-04-07
session: forge-curriculum reframe (craft school)
destination: lyceum/programs/forge (positioning) — and any downstream client-course collateral
status: pre-rigor-bar — do not use as curriculum source
---

# Value Audit: FORGE Methodology vs Craft School Framing

**Auditor**: Skeptical Head of AI signing a six-figure CoE training engagement
**Evidence base**: Journal logs 0011, 0014, 0049, 0050

---

## 1. Day-1 economic value

Craft school wins decisively. A methodology graduate can recite CARE/EATP/CO/COC and map a workflow to "Dual Plane" — that is a $0 deliverable on day 1 of CoE work. A craft graduate, on day 1, can: stand up `.claude/` with the right rule tier for the model in use (Tier 1 vs Tier 2 vs MiniMax tier — see 0049's 4-tier table), spot a rule that is model-native and shouldn't be paid for in tokens, run a clean-room ablation in `/tmp` instead of trusting in-session agent self-reports, write a hook that uses `execFileSync` not `execSync` (0050), and run a drift audit across BUILD/USE repos (0014). Those are billable artifacts on day 1.

## 2. Methodology-only failure surface

Six things a methodology graduate cannot do:

1. Distinguish contaminated in-session ablation from clean-room ablation — they will trust agent self-reports and ship the wrong rule set.
2. Choose a tier. They have no instinct for the ~48k vs ~10k vs ~6.5k token tradeoff or which model needs which.
3. Catch `execSync` with attacker-controlled input in a hook — because no spec teaches you to grep your hooks for shell interpretation.
4. Detect 85% artifact drift between BUILD and USE repos (0014) — they have no audit reflex, only a vocabulary.
5. Know that MiniMax does plaintext password compares and that "model-native" is model-specific, not universal.
6. Know when to intervene in a session vs when to let the agent converge — the entire "human-on-the-loop" judgment is tacit.

## 3. Enterprise risk under methodology-only training

Concrete failure modes I would put in the procurement risk register:

- **Token bill blowout**: 5x overpayment on context (~37k unnecessary rule tokens × every session × every seat). At 200 staff × 50 sessions/week, that is real six-figure annual waste.
- **Shipped command injection** in hooks (exactly 0050). One malicious cloned repo with a crafted `.claude/VERSION` and you have RCE on every developer laptop. That is a board-reportable incident.
- **Silent artifact drift** (0014: 66% of skill files drifted, 31 unique skills in one variant) — the CoE thinks it has one standard, actually runs N divergent ones, and can't explain audit findings.
- **Wrong-model rule set**: deploy Claude-tuned rules to MiniMax, get plaintext password storage in production code that passed "review."
- **Runaway sessions**: no instinct for when convergence has stopped and the agent is just burning tokens.
- **False security posture**: the team can name PACT D/T/R but cannot actually verify a hook is safe.

## 4. Defensibility

Craft school is dramatically harder to clone. CARE/EATP/CO specs are CC BY 4.0 — a competitor can rebrand and resell the methodology in a quarter. The journal corpus (50+ DISCOVERY/RISK entries with reproductions, ablation data, root causes) is private, sequential, and only generable by actually running the system at scale for a year. A competitor would need to operate COC for 12+ months on real projects to produce equivalent material. That is the moat.

## 5. Narrative coherence with university buyers

The craft framing strengthens, not weakens, credibility — _if positioned correctly_. University-trained buyers respect "distilled from N reproducible incidents" more than "we teach a framework." It reads like a clinical residency or an apprenticeship in a trading firm, not a bootcamp.

**Risk**: if pitched as "we have war stories," it sounds anecdotal.
**Mitigation**: lead with the journal as a structured evidence base (typed entries, reproductions, classifications) and the ablation table from 0049 — that is empirical, not folklore.

**Honest concern**: dropping methodology entirely is wrong. The buyer wants to hear that craft is _organized by_ CARE/EATP/CO, not that those specs are absent. **Frame specs as the index, craft as the content.**

## 6. Premium price justification

Craft justifies premium; methodology does not. Methodology training is commoditized — every consulting firm offers a "responsible AI framework" course at $2-5k/seat. Premium pricing ($15-30k/seat or six-figure cohort) requires the buyer to believe the graduate avoids one shipped incident, one token blowout, or one drift audit failure — all of which are individually worth more than the entire engagement. The craft school's economic argument is **insurance + efficiency**: one prevented `execSync` RCE is worth the whole contract; one correct tier choice pays for the seat in tokens within a quarter. Methodology's economic argument is "your team will speak the same language" — true, but not worth a premium.

---

## Bottom line

Drop pure methodology. Adopt craft school, but keep the specs as the **organizing index** of the journal corpus so university buyers see structure, not anecdote. The defensibility, day-1 utility, and premium justification all collapse without the journal-derived craft layer.

**The single highest-impact framing decision**: build the curriculum directly from typed journal entries (DISCOVERY/RISK/lessons), with each lesson labeled by which spec concept it instantiates — **specs as taxonomy, craft as substance**.
