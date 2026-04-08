---
title: COR Reverse Index — Spec Concepts to Corpus Evidence
date: 2026-04-08
type: reverse-index
---

# COR Reverse Index

Maps COR spec concepts to corpus evidence. 135 COR spec concepts enumerated; evidence concentrated in `terrene/foundation/workspaces/care-thesis/journal/` (58 entries) and `terrene/journal/` (1 key entry).

## Evidence Coverage Summary

| COR Skill Area                           | Spec Concepts                     | Corpus Entries                                                   | Coverage            |
| ---------------------------------------- | --------------------------------- | ---------------------------------------------------------------- | ------------------- |
| Literature search & gap analysis         | C19-C22, C73, C82, C88            | 0009, 0013, 0036, 0037, 0052, 0053, 0057, 0058                   | Strong              |
| Claims verification & citation integrity | C12-C14, C26-C28, C53, C56, C78   | 0004, terrene/0006                                               | Moderate            |
| Argument construction & defense          | C29-C31, C66-C68, C76, C85, C90   | 0018, 0019, 0020, 0028, 0029, 0038, 0039, 0044, 0047, 0049, 0054 | Strong              |
| Multi-perspective synthesis              | C21 (four-critics), C36 (routing) | 0021, 0040, 0042                                                 | Moderate            |
| Publication strategy & venue             | C08-C11, C42, C80, C86, C91       | 0010, 0045, 0048, 0050, 0055                                     | Strong              |
| Academic voice & register                | C10, C43, C48, C61, C114-C121     | 0001, 0024, 0025, 0026                                           | Moderate            |
| Writing craft & deliberation             | C32-C34, C57-C60, C69-C71, C101   | 0001, 0003, 0005, 0006, 0034                                     | Moderate            |
| Reflexivity & methodology critique       | C128, C129, C132                  | 0054                                                             | Thin (single entry) |
| Scope boundary enforcement               | C04-C07, C44-C48, C54             | Spec-level only                                                  | Thin                |
| Teaching obligation                      | C39, C57, C94                     | 0004, 0007, 0013-TEACH, 0031, 0035                               | Moderate            |

## Atom Candidates (Mined from Heavy-Evidence Intersections)

10 atoms with strong evidence. Each has spec mooring + at least 1 read-and-verified journal entry.

| #   | Candidate                                                     | Spec Concepts                  | Key Evidence                 | Craft Area      |
| --- | ------------------------------------------------------------- | ------------------------------ | ---------------------------- | --------------- |
| 1   | Tier-ranked literature search with verification               | C19-C22, C73, C88              | 0009-LITERATURE              | attend-how      |
| 2   | Citation integrity audit — detect misquotation and spec drift | C12-C14, C53, C56              | 0004-TEACH, terrene/0006     | judge           |
| 3   | Hostile reviewer simulation as preemptive defense             | C29-C31, C76, C90              | 0018-DEFENSE, 0054-DEFENSE   | judge           |
| 4   | Multi-perspective synthesis into minimal publishable model    | C21 synthesis, C36 routing     | 0021-DELIBERATION            | decide          |
| 5   | Post-publication gap check as literature maintenance          | C73, C82, C105                 | 0052-LITERATURE              | attend-how      |
| 6   | Academic register calibration with detection-bias awareness   | C10, C43, C117-C120            | 0024-MARGIN                  | attend-how      |
| 7   | Venue strategy as constraint envelope                         | C08-C09, C42, C80              | 0010-DELIBERATION            | decide          |
| 8   | Margin note as deliberation artifact                          | C33, C57, C101                 | 0001-MARGIN                  | act-communicate |
| 9   | Reflexivity diagnosis in self-referential research            | C128, C135, honest-limitations | 0054-DEFENSE                 | judge           |
| 10  | Overclaim prevention — qualify every superlative              | C15, C28, C55                  | 0052-LITERATURE, 0026-MARGIN | judge           |

## Cross-Tagged Atom Foundation

The 18 atoms already tagged `[COC, COR, COE]` provide the governance and methodology foundation. New COR atoms build on top of them:

- SC-B-006 (multi-round convergence) → COR atom #3 (hostile reviewer) extends this to research peer review
- SC-B-005 (spec-to-code audit) → COR atom #2 (citation integrity) is the research instantiation
- SC-P-015 (ask for contradictions) → COR atom #3 and #9 use falsification prompting in research context
- SC-P-002 (ablation) → COR atom #4 uses the ablation mindset for theoretical model reduction
- SC-P-007 (completion state) → COR atom #7 (venue strategy) defines the publication completion state
