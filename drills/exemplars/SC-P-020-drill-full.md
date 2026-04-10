---
drill_id: DR-SC-P-020-FULL
source_atom: SC-P-020
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Second Drift Occurrence as Structural Signal

## Setup

The practitioner receives a project journal from a multi-repo SDK ecosystem. The project has a template repository (`coc-template/`) that syncs artifacts to three downstream repositories (`sdk-py/`, `sdk-rs/`, `cli-rs/`). The sync mechanism copies `.claude/` artifacts from the template to each downstream repo on demand.

The journal contains four entries, written across separate sessions over two weeks. The practitioner is told: "These entries came from different sessions. The authors did not compare notes across entries."

**Entry 1** (Session 7):

```
Title: DISCOVERY — Stale skill references in sdk-py agents
Found: 3 agent definitions in sdk-py/.claude/agents/ reference skill directories
that were renamed in the template 2 sessions ago. The agents route to
08-deployment and 12-testing, but those directories were renumbered to
05-deployment and 09-testing in the template. The sdk-py agents still
use the old numbers.
Action: Updated the 3 agent files to use the new skill directory numbers.
Time: 20 minutes.
```

**Entry 2** (Session 11):

```
Title: DISCOVERY — Orphan variant rules in sdk-rs
Found: 4 rule files exist in sdk-rs/.claude/rules/ that have no counterpart
in the template. They were created during a local /codify session and never
upstreamed. The template has since added 2 new rules that cover the same
ground but with different names and content. sdk-rs now has 6 rule files
where the template has 2 — 4 orphans + 2 synced.
Action: Deleted the 4 orphan rules. Verified the 2 template rules cover
the same intent.
Time: 35 minutes.
```

**Entry 3** (Session 14):

```
Title: DISCOVERY — cli-rs skill numbering divergence
Found: cli-rs/.claude/skills/ uses a completely different numbering scheme
from the template. The template numbers skills 01-30; cli-rs numbers
them 01-15 with different assignments. Skill 05 in the template is
"kailash-mcp" but skill 05 in cli-rs is "enterprise-patterns." 8 skill
numbers have different meanings across the two repos.
Action: Rebuilt the cli-rs skill directory to match the template numbering.
Reassigned all 15 skills to their canonical template numbers. Updated
4 agent files that referenced the old local numbers.
Time: 90 minutes.
```

**Entry 4** (Session 16):

```
Title: RISK — Manual sync missed 2 new template skills
Found: Template added skills 31 and 32 in Session 15. The manual sync to
sdk-py in Session 16 copied them, but sdk-rs and cli-rs were not synced.
sdk-rs and cli-rs are now 2 skills behind the template.
Action: Synced skills 31-32 to sdk-rs and cli-rs. Added a reminder to the
session notes to sync all 3 repos after template changes.
Time: 15 minutes.
```

## Task

1. Read all four entries. Classify each by the **class** of failure, not by the specific artifact type or repo. Two or more entries may share the same failure class even though they involve different repos, different artifact types, and different surface symptoms.

2. Identify the entry at which the practitioner should have stopped fixing individual instances and started building a structural prevention mechanism. Explain why that entry -- not any later one -- was the intervention point. Your answer must reference the class of failure, not the specific artifacts.

3. Propose a mechanism-level fix that would have prevented every subsequent instance in that failure class. The fix must be structural (automated, enforced, visible in the workflow) rather than procedural (a reminder, a checklist, a verbal agreement). Explain what the mechanism checks, when it runs, and what it blocks.

4. For the entries that occurred after your identified intervention point, estimate the cumulative cost that would have been avoided if the mechanism had been built at the correct moment. Express cost as time and as downstream risk, not just as "minutes saved."

## Model Answer

**Step 1 -- Failure class analysis:**

All four entries belong to the same failure class: **artifact divergence between a canonical source and downstream copies, where no enforcement mechanism validates parity.** The surface symptoms differ -- stale references (Entry 1), orphan artifacts (Entry 2), numbering scheme divergence (Entry 3), missed sync (Entry 4) -- but the structural cause is identical: the template is the intended single source of truth, downstream repos are intended to mirror it, and nothing in the system enforces or validates that mirror relationship. Each entry is a different manifestation of unvalidated parity.

A tempting but incorrect classification would separate them into three or four classes: "stale references" (Entry 1), "orphan artifacts" (Entry 2), "numbering divergence" (Entry 3), and "sync omission" (Entry 4). This artifact-level classification misses the structural pattern. The test: if you fixed all four specific instances but built no mechanism, would a fifth instance of the same class occur? Yes -- because the production mechanism (unvalidated sync) is unchanged.

**Step 2 -- The intervention point is Entry 2:**

Entry 1 is the first occurrence. The practitioner finds stale skill references in sdk-py. This is an incident: fix it, note the class ("artifact divergence without enforcement"), and move on. One occurrence does not justify building infrastructure.

Entry 2 is the second occurrence of the same class. The surface is different (orphan rules in sdk-rs vs. stale references in sdk-py), but the class is identical: artifacts in a downstream repo have diverged from the template, and nothing detected the divergence until a human stumbled on it. At Entry 2, the practitioner has evidence that the system _produces this class of failure structurally_. Fixing Entry 2's specific instance without addressing the mechanism guarantees a third occurrence.

Entry 3 is NOT the intervention point, even though it is larger (90 minutes) and more dramatic (8 conflicting skill numbers). By Entry 3, one preventable instance has already occurred. The practitioner who intervenes at Entry 3 recognized the pattern one occurrence too late.

Entry 4 is the worst possible intervention point. By Entry 4, the practitioner has let two preventable instances through and is now adding procedural fixes (a reminder in session notes) instead of structural enforcement -- which is itself a form of convention drift.

**Step 3 -- Mechanism-level fix:**

A **pre-sync parity validation** that runs automatically before any downstream work begins in a target repo. The mechanism:

- **What it checks:** For every file in `template/.claude/`, verify that the corresponding file in `target/.claude/` exists, has the same content hash (for global artifacts) or is listed in the sync manifest as an intentional variant. For every file in `target/.claude/` that does NOT exist in the template, verify it is listed as a local-only artifact in the manifest. Flag any file that fails both checks as an unclassified divergence.
- **When it runs:** At session start (as a session-start hook) in every downstream repo, AND as a pre-sync validation step before the `/sync` command writes anything. The session-start check catches drift that accumulated between sessions. The pre-sync check catches drift that would be masked by a partial sync.
- **What it blocks:** If the parity check finds unclassified divergences, the hook emits a warning into the model's context (stdout, not stderr -- see SC-P-019) listing the divergent files. If divergence count exceeds a threshold (e.g., 3 files), the hook blocks `/sync` until the divergences are resolved. This prevents the "sync over the top of unknown state" failure mode.

This mechanism would have prevented Entries 2, 3, and 4 entirely: Entry 2's orphan rules would have been flagged at session start, Entry 3's numbering divergence would have been caught before it grew to 8 conflicting numbers, and Entry 4's missed sync would have been detected by the session-start check in the un-synced repos.

**Step 4 -- Cumulative avoided cost:**

| Entry   | Time spent | Downstream risk avoided                                                                                                                                                                                                                                                                                                                                                                 |
| ------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Entry 2 | 35 min     | 4 orphan rules could have been consumed by new agent definitions written between Session 11 and discovery, creating a second layer of references to non-canonical rules                                                                                                                                                                                                                 |
| Entry 3 | 90 min     | 8 skill numbers with different meanings across repos means any cross-repo documentation, training material, or agent configuration written between Session 11 and Session 14 could reference the wrong skill. The 4 agent files updated in Entry 3 are the known references; unknown consumers (session notes, journal entries, verbal communication) may still carry the wrong numbers |
| Entry 4 | 15 min     | Low direct cost, but the procedural fix (a reminder in session notes) is itself unreliable -- it depends on the next practitioner reading and following the reminder, which is probabilistic compliance, not deterministic enforcement                                                                                                                                                  |

**Total time avoided:** 140 minutes (Entries 2 + 3 + 4). But the time cost understates the real saving. The downstream risk is that each unfixed instance creates secondary artifacts (agent configs, documentation, journal entries) that reference the divergent state, compounding the correction cost when the divergence is eventually discovered. Entry 3's 90-minute fix was not just "rebuild 15 skill directories" -- it was also "find and update every reference to the old numbering," which grows combinatorially with the time between divergence and detection.

## Scoring

| Criterion                                                                                                                                                                | Points |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------ |
| All four entries classified under the same failure class (artifact divergence without enforcement), not by surface symptom (stale refs, orphans, numbering, missed sync) | 2      |
| Entry 2 identified as the intervention point with explicit reasoning: second occurrence of the same class, not the most expensive or most dramatic instance              | 2      |
| Mechanism is structural (automated, runs without human initiation) not procedural (checklist, reminder, verbal agreement)                                                | 2      |
| Mechanism specifies what it checks (content hash or manifest classification), when it runs (session start + pre-sync), and what it blocks (sync or session continuation) | 2      |
| Cost analysis includes downstream risk (secondary artifacts referencing divergent state), not just direct time                                                           | 1      |
| Practitioner correctly distinguishes deterministic enforcement from probabilistic compliance when evaluating Entry 4's "reminder" fix                                    | 1      |
| **Total**                                                                                                                                                                | **10** |

## Extensions

1. **Cross-class discrimination.** Add a fifth journal entry that describes a DIFFERENT failure class: "Found that the DataFlow migration script in sdk-py generates SQL that is incompatible with the Rust SDK's expected schema." The practitioner must explain why this is NOT the same class as Entries 1-4 (it is a semantic compatibility failure, not an artifact parity failure) and why the parity-check mechanism from Step 3 would not catch it. This tests whether the practitioner can distinguish structural classes rather than lumping all "things that went wrong across repos" into one bucket.

2. **Retroactive class cataloging.** Give the practitioner a project journal with 12 entries and ask them to assign each to a failure class, producing a class catalog with occurrence counts. Three classes should have 2+ occurrences (triggering the structural-signal rule). The practitioner must propose a mechanism for each multi-occurrence class and explain the priority order for building them (highest-frequency class first, highest-cost class first, or highest-risk class first -- and defend the choice).
