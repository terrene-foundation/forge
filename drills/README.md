# FORGE Drill Library

**57 drill specs** extracted from the skill atom catalog. Each drill spec captures the "How to drill it" section from its source atom as a structured, reusable practice unit.

## Directory structure

```
drills/
  specs/          # 57 drill spec files (frontmatter + drill text)
  exemplars/      # 15 fully expanded exemplar drills
  README.md       # This index
```

## Index by craft area

### attend-how (25 drills)

| Drill ID    | Name                                                              | Modality      | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ------------- | ------------ | ---- |
| DR-SC-A-002 | Frontend mock data detection                                      | build         | advanced     | 45m  |
| DR-SC-A-003 | Defense-in-depth codification across multiple artifact locations  | drill         | intermediate | 30m  |
| DR-SC-A-004 | Layer 3 hook security audit                                       | case          | intermediate | 30m  |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs              | case          | intermediate | 30m  |
| DR-SC-A-006 | Negative tests for deny-by-default                                | drill         | intermediate | 30m  |
| DR-SC-A-007 | Zero-config security audit                                        | drill         | intermediate | 30m  |
| DR-SC-A-011 | Framework-first check before building new functionality           | drill         | intermediate | 30m  |
| DR-SC-B-008 | Convergence-as-validation across independent reds                 | observation   | intermediate | 30m  |
| DR-SC-B-010 | Cross-repo divergence audit with numbered drift-gap list          | brokerage-rep | intermediate | 30m  |
| DR-SC-B-013 | Map cross-workspace dependency surface before parallelizing       | brokerage-rep | intermediate | 30m  |
| DR-SC-B-016 | Audit cross-language parity bidirectionally — the other SDK may b | brokerage-rep | intermediate | 30m  |
| DR-SC-P-001 | Identify adjacent-but-disconnected modules as spec conformance ga | case          | intermediate | 30m  |
| DR-SC-P-002 | Run empirical model-vs-rule ablation in a clean-room environment  | build         | advanced     | 45m  |
| DR-SC-P-003 | Write cross-feature interaction tests before individual feature t | drill         | beginner     | 15m  |
| DR-SC-P-004 | Label every file with a disposition before starting implementatio | drill         | beginner     | 15m  |
| DR-SC-P-005 | Diagnose friction-gradient inversions where the best API has the  | case          | intermediate | 30m  |
| DR-SC-P-007 | Define session completion state before starting, not after conver | drill         | intermediate | 30m  |
| DR-SC-P-008 | Trace deep dependency chains to find where a single broken link b | case          | intermediate | 30m  |
| DR-SC-P-009 | Treat architecture documents as testable claims against the actua | drill         | beginner     | 15m  |
| DR-SC-P-010 | Red team CI, auth, and release processes for timing-dependent fai | drill         | beginner     | 15m  |
| DR-SC-P-011 | Detect when an audit categorization method is structurally blind  | case          | intermediate | 30m  |
| DR-SC-P-013 | Treat internal estimate inconsistency as a high-confidence signal | drill         | beginner     | 15m  |
| DR-SC-P-016 | Detect overcorrection in both directions as a single contaminatio | case          | intermediate | 30m  |
| DR-SC-P-017 | Detect false-positive learning pipelines that report healthy metr | case          | intermediate | 30m  |
| DR-SC-P-022 | Author rules for the weakest model in the deployment envelope, no | case          | intermediate | 30m  |

### attend-when (7 drills)

| Drill ID    | Name                                                              | Modality      | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ------------- | ------------ | ---- |
| DR-SC-B-005 | Spec-to-code conformance audit                                    | brokerage-rep | intermediate | 30m  |
| DR-SC-B-006 | Multi-round red team to numeric convergence                       | brokerage-rep | intermediate | 30m  |
| DR-SC-B-008 | Convergence-as-validation across independent reds                 | observation   | intermediate | 30m  |
| DR-SC-P-017 | Detect false-positive learning pipelines that report healthy metr | case          | intermediate | 30m  |
| DR-SC-P-019 | Verify session-start continuity reaches the model, not just the t | drill         | intermediate | 30m  |
| DR-SC-P-020 | Treat the second occurrence of a drift class as a structural sign | case          | intermediate | 30m  |
| DR-SC-P-024 | Intervene when a temporary workaround accumulates its second cons | case          | intermediate | 30m  |

### behaviour (2 drills)

| Drill ID    | Name                                                              | Modality | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | -------- | ------------ | ---- |
| DR-SC-P-016 | Detect overcorrection in both directions as a single contaminatio | case     | intermediate | 30m  |
| DR-SC-P-022 | Author rules for the weakest model in the deployment envelope, no | case     | intermediate | 30m  |

### institutional-memory (21 drills)

| Drill ID    | Name                                                              | Modality      | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ------------- | ------------ | ---- |
| DR-SC-A-001 | Codify-with-explicit-NOT-codified                                 | drill         | beginner     | 15m  |
| DR-SC-A-003 | Defense-in-depth codification across multiple artifact locations  | drill         | intermediate | 30m  |
| DR-SC-A-009 | Progressive disclosure architecture for CC artifacts              | drill         | beginner     | 15m  |
| DR-SC-A-010 | Token budget — baked-in vs external boundary decision             | case          | intermediate | 30m  |
| DR-SC-A-012 | Author CLAUDE.md as the Layer 2 master directive                  | build         | advanced     | 45m  |
| DR-SC-A-013 | Design agent specialisation routing as explicit Layer 1 Intent    | build         | advanced     | 45m  |
| DR-SC-A-014 | Decompose milestones with hard-blocking approval gates            | drill         | intermediate | 30m  |
| DR-SC-A-015 | Structure the Layer 2 institutional knowledge corpus by artifact  | build         | advanced     | 45m  |
| DR-SC-B-001 | Read-then-merge sync semantics                                    | brokerage-rep | intermediate | 30m  |
| DR-SC-B-002 | Authority-chain escalation when local fix gets reverted           | brokerage-rep | intermediate | 30m  |
| DR-SC-B-003 | Variant classification — single-sentence test rule                | drill         | beginner     | 15m  |
| DR-SC-B-004 | Specialist delegation as required first move (framework work)     | brokerage-rep | intermediate | 30m  |
| DR-SC-B-005 | Spec-to-code conformance audit                                    | brokerage-rep | intermediate | 30m  |
| DR-SC-B-007 | Worktree-per-agent for safe parallel execution                    | build         | advanced     | 45m  |
| DR-SC-B-009 | Cross-SDK upstream brokerage                                      | brokerage-rep | intermediate | 30m  |
| DR-SC-B-010 | Cross-repo divergence audit with numbered drift-gap list          | brokerage-rep | intermediate | 30m  |
| DR-SC-B-011 | Version tracking cascade across the repo chain                    | brokerage-rep | intermediate | 30m  |
| DR-SC-B-012 | Automate manifest-to-filesystem parity check and halt on stalenes | drill         | beginner     | 15m  |
| DR-SC-B-014 | Journal decision reversals with explicit supersession references  | brokerage-rep | intermediate | 30m  |
| DR-SC-B-017 | Assign verification-gradient zone explicitly before delegating to | brokerage-rep | intermediate | 30m  |
| DR-SC-P-019 | Verify session-start continuity reaches the model, not just the t | drill         | intermediate | 30m  |

### intervene-how (13 drills)

| Drill ID    | Name                                                              | Modality      | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ------------- | ------------ | ---- |
| DR-SC-A-002 | Frontend mock data detection                                      | build         | advanced     | 45m  |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs              | case          | intermediate | 30m  |
| DR-SC-A-006 | Negative tests for deny-by-default                                | drill         | intermediate | 30m  |
| DR-SC-A-007 | Zero-config security audit                                        | drill         | intermediate | 30m  |
| DR-SC-A-008 | Rule classification — critical vs advisory                        | drill         | beginner     | 15m  |
| DR-SC-A-013 | Design agent specialisation routing as explicit Layer 1 Intent    | build         | advanced     | 45m  |
| DR-SC-A-014 | Decompose milestones with hard-blocking approval gates            | drill         | intermediate | 30m  |
| DR-SC-B-007 | Worktree-per-agent for safe parallel execution                    | build         | advanced     | 45m  |
| DR-SC-B-009 | Cross-SDK upstream brokerage                                      | brokerage-rep | intermediate | 30m  |
| DR-SC-B-018 | Separate the policy decision point from the policy enforcement po | brokerage-rep | intermediate | 30m  |
| DR-SC-P-018 | Split a multi-session blocker into fast-unblock and parallel-comp | drill         | beginner     | 15m  |
| DR-SC-P-021 | Grep the entire repo for dangling references immediately after ev | drill         | beginner     | 15m  |
| DR-SC-P-023 | Insert a gating workspace at integration seams rather than adding | drill         | beginner     | 15m  |

### intervene-when (13 drills)

| Drill ID    | Name                                                              | Modality      | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ------------- | ------------ | ---- |
| DR-SC-A-004 | Layer 3 hook security audit                                       | case          | intermediate | 30m  |
| DR-SC-A-011 | Framework-first check before building new functionality           | drill         | intermediate | 30m  |
| DR-SC-B-002 | Authority-chain escalation when local fix gets reverted           | brokerage-rep | intermediate | 30m  |
| DR-SC-B-004 | Specialist delegation as required first move (framework work)     | brokerage-rep | intermediate | 30m  |
| DR-SC-B-006 | Multi-round red team to numeric convergence                       | brokerage-rep | intermediate | 30m  |
| DR-SC-B-015 | Reinterpret MUST rules from "human performs" to "human approves"  | brokerage-rep | intermediate | 30m  |
| DR-SC-B-017 | Assign verification-gradient zone explicitly before delegating to | brokerage-rep | intermediate | 30m  |
| DR-SC-P-005 | Diagnose friction-gradient inversions where the best API has the  | case          | intermediate | 30m  |
| DR-SC-P-006 | Decide whether new functionality is a module inside an existing p | case          | intermediate | 30m  |
| DR-SC-P-007 | Define session completion state before starting, not after conver | drill         | intermediate | 30m  |
| DR-SC-P-012 | Decide whether to demote or strengthen a rule when ablation shows | case          | intermediate | 30m  |
| DR-SC-P-014 | Distinguish pre-existing failures from pre-existing gaps when app | case          | intermediate | 30m  |
| DR-SC-P-024 | Intervene when a temporary workaround accumulates its second cons | case          | intermediate | 30m  |

### prompt (1 drills)

| Drill ID    | Name                                                              | Modality | Difficulty | Time |
| ----------- | ----------------------------------------------------------------- | -------- | ---------- | ---- |
| DR-SC-P-015 | End complex prompts with "what would invalidate this?" rather tha | drill    | beginner   | 15m  |

## Index by practice modality

### brokerage-rep (14 drills)

| Drill ID    | Name                                                              | Craft area                           | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ------------------------------------ | ------------ | ---- |
| DR-SC-B-001 | Read-then-merge sync semantics                                    | institutional-memory                 | intermediate | 30m  |
| DR-SC-B-002 | Authority-chain escalation when local fix gets reverted           | institutional-memory, intervene-when | intermediate | 30m  |
| DR-SC-B-004 | Specialist delegation as required first move (framework work)     | intervene-when, institutional-memory | intermediate | 30m  |
| DR-SC-B-005 | Spec-to-code conformance audit                                    | attend-when, institutional-memory    | intermediate | 30m  |
| DR-SC-B-006 | Multi-round red team to numeric convergence                       | attend-when, intervene-when          | intermediate | 30m  |
| DR-SC-B-009 | Cross-SDK upstream brokerage                                      | intervene-how, institutional-memory  | intermediate | 30m  |
| DR-SC-B-010 | Cross-repo divergence audit with numbered drift-gap list          | attend-how, institutional-memory     | intermediate | 30m  |
| DR-SC-B-011 | Version tracking cascade across the repo chain                    | institutional-memory                 | intermediate | 30m  |
| DR-SC-B-013 | Map cross-workspace dependency surface before parallelizing       | attend-how                           | intermediate | 30m  |
| DR-SC-B-014 | Journal decision reversals with explicit supersession references  | institutional-memory                 | intermediate | 30m  |
| DR-SC-B-015 | Reinterpret MUST rules from "human performs" to "human approves"  | intervene-when                       | intermediate | 30m  |
| DR-SC-B-016 | Audit cross-language parity bidirectionally — the other SDK may b | attend-how                           | intermediate | 30m  |
| DR-SC-B-017 | Assign verification-gradient zone explicitly before delegating to | intervene-when, institutional-memory | intermediate | 30m  |
| DR-SC-B-018 | Separate the policy decision point from the policy enforcement po | intervene-how                        | intermediate | 30m  |

### build (6 drills)

| Drill ID    | Name                                                             | Craft area                          | Difficulty | Time |
| ----------- | ---------------------------------------------------------------- | ----------------------------------- | ---------- | ---- |
| DR-SC-A-002 | Frontend mock data detection                                     | attend-how, intervene-how           | advanced   | 45m  |
| DR-SC-A-012 | Author CLAUDE.md as the Layer 2 master directive                 | institutional-memory                | advanced   | 45m  |
| DR-SC-A-013 | Design agent specialisation routing as explicit Layer 1 Intent   | intervene-how, institutional-memory | advanced   | 45m  |
| DR-SC-A-015 | Structure the Layer 2 institutional knowledge corpus by artifact | institutional-memory                | advanced   | 45m  |
| DR-SC-B-007 | Worktree-per-agent for safe parallel execution                   | institutional-memory, intervene-how | advanced   | 45m  |
| DR-SC-P-002 | Run empirical model-vs-rule ablation in a clean-room environment | attend-how                          | advanced   | 45m  |

### case (15 drills)

| Drill ID    | Name                                                              | Craft area                  | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | --------------------------- | ------------ | ---- |
| DR-SC-A-004 | Layer 3 hook security audit                                       | attend-how, intervene-when  | intermediate | 30m  |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs              | attend-how, intervene-how   | intermediate | 30m  |
| DR-SC-A-010 | Token budget — baked-in vs external boundary decision             | institutional-memory        | intermediate | 30m  |
| DR-SC-P-001 | Identify adjacent-but-disconnected modules as spec conformance ga | attend-how                  | intermediate | 30m  |
| DR-SC-P-005 | Diagnose friction-gradient inversions where the best API has the  | attend-how, intervene-when  | intermediate | 30m  |
| DR-SC-P-006 | Decide whether new functionality is a module inside an existing p | intervene-when              | intermediate | 30m  |
| DR-SC-P-008 | Trace deep dependency chains to find where a single broken link b | attend-how                  | intermediate | 30m  |
| DR-SC-P-011 | Detect when an audit categorization method is structurally blind  | attend-how                  | intermediate | 30m  |
| DR-SC-P-012 | Decide whether to demote or strengthen a rule when ablation shows | intervene-when              | intermediate | 30m  |
| DR-SC-P-014 | Distinguish pre-existing failures from pre-existing gaps when app | intervene-when              | intermediate | 30m  |
| DR-SC-P-016 | Detect overcorrection in both directions as a single contaminatio | behaviour, attend-how       | intermediate | 30m  |
| DR-SC-P-017 | Detect false-positive learning pipelines that report healthy metr | attend-how, attend-when     | intermediate | 30m  |
| DR-SC-P-020 | Treat the second occurrence of a drift class as a structural sign | attend-when                 | intermediate | 30m  |
| DR-SC-P-022 | Author rules for the weakest model in the deployment envelope, no | behaviour, attend-how       | intermediate | 30m  |
| DR-SC-P-024 | Intervene when a temporary workaround accumulates its second cons | attend-when, intervene-when | intermediate | 30m  |

### drill (21 drills)

| Drill ID    | Name                                                              | Craft area                          | Difficulty   | Time |
| ----------- | ----------------------------------------------------------------- | ----------------------------------- | ------------ | ---- |
| DR-SC-A-001 | Codify-with-explicit-NOT-codified                                 | institutional-memory                | beginner     | 15m  |
| DR-SC-A-003 | Defense-in-depth codification across multiple artifact locations  | institutional-memory, attend-how    | intermediate | 30m  |
| DR-SC-A-006 | Negative tests for deny-by-default                                | attend-how, intervene-how           | intermediate | 30m  |
| DR-SC-A-007 | Zero-config security audit                                        | attend-how, intervene-how           | intermediate | 30m  |
| DR-SC-A-008 | Rule classification — critical vs advisory                        | intervene-how                       | beginner     | 15m  |
| DR-SC-A-009 | Progressive disclosure architecture for CC artifacts              | institutional-memory                | beginner     | 15m  |
| DR-SC-A-011 | Framework-first check before building new functionality           | intervene-when, attend-how          | intermediate | 30m  |
| DR-SC-A-014 | Decompose milestones with hard-blocking approval gates            | intervene-how, institutional-memory | intermediate | 30m  |
| DR-SC-B-003 | Variant classification — single-sentence test rule                | institutional-memory                | beginner     | 15m  |
| DR-SC-B-012 | Automate manifest-to-filesystem parity check and halt on stalenes | institutional-memory                | beginner     | 15m  |
| DR-SC-P-003 | Write cross-feature interaction tests before individual feature t | attend-how                          | beginner     | 15m  |
| DR-SC-P-004 | Label every file with a disposition before starting implementatio | attend-how                          | beginner     | 15m  |
| DR-SC-P-007 | Define session completion state before starting, not after conver | attend-how, intervene-when          | intermediate | 30m  |
| DR-SC-P-009 | Treat architecture documents as testable claims against the actua | attend-how                          | beginner     | 15m  |
| DR-SC-P-010 | Red team CI, auth, and release processes for timing-dependent fai | attend-how                          | beginner     | 15m  |
| DR-SC-P-013 | Treat internal estimate inconsistency as a high-confidence signal | attend-how                          | beginner     | 15m  |
| DR-SC-P-015 | End complex prompts with "what would invalidate this?" rather tha | prompt                              | beginner     | 15m  |
| DR-SC-P-018 | Split a multi-session blocker into fast-unblock and parallel-comp | intervene-how                       | beginner     | 15m  |
| DR-SC-P-019 | Verify session-start continuity reaches the model, not just the t | attend-when, institutional-memory   | intermediate | 30m  |
| DR-SC-P-021 | Grep the entire repo for dangling references immediately after ev | intervene-how                       | beginner     | 15m  |
| DR-SC-P-023 | Insert a gating workspace at integration seams rather than adding | intervene-how                       | beginner     | 15m  |

### observation (1 drills)

| Drill ID    | Name                                              | Craft area              | Difficulty   | Time |
| ----------- | ------------------------------------------------- | ----------------------- | ------------ | ---- |
| DR-SC-B-008 | Convergence-as-validation across independent reds | attend-how, attend-when | intermediate | 30m  |

## Index by difficulty

### beginner (14 drills)

| Drill ID    | Name                                                              | Modality | Craft area           | Time |
| ----------- | ----------------------------------------------------------------- | -------- | -------------------- | ---- |
| DR-SC-A-001 | Codify-with-explicit-NOT-codified                                 | drill    | institutional-memory | 15m  |
| DR-SC-A-008 | Rule classification — critical vs advisory                        | drill    | intervene-how        | 15m  |
| DR-SC-A-009 | Progressive disclosure architecture for CC artifacts              | drill    | institutional-memory | 15m  |
| DR-SC-B-003 | Variant classification — single-sentence test rule                | drill    | institutional-memory | 15m  |
| DR-SC-B-012 | Automate manifest-to-filesystem parity check and halt on stalenes | drill    | institutional-memory | 15m  |
| DR-SC-P-003 | Write cross-feature interaction tests before individual feature t | drill    | attend-how           | 15m  |
| DR-SC-P-004 | Label every file with a disposition before starting implementatio | drill    | attend-how           | 15m  |
| DR-SC-P-009 | Treat architecture documents as testable claims against the actua | drill    | attend-how           | 15m  |
| DR-SC-P-010 | Red team CI, auth, and release processes for timing-dependent fai | drill    | attend-how           | 15m  |
| DR-SC-P-013 | Treat internal estimate inconsistency as a high-confidence signal | drill    | attend-how           | 15m  |
| DR-SC-P-015 | End complex prompts with "what would invalidate this?" rather tha | drill    | prompt               | 15m  |
| DR-SC-P-018 | Split a multi-session blocker into fast-unblock and parallel-comp | drill    | intervene-how        | 15m  |
| DR-SC-P-021 | Grep the entire repo for dangling references immediately after ev | drill    | intervene-how        | 15m  |
| DR-SC-P-023 | Insert a gating workspace at integration seams rather than adding | drill    | intervene-how        | 15m  |

### intermediate (37 drills)

| Drill ID    | Name                                                              | Modality      | Craft area                           | Time |
| ----------- | ----------------------------------------------------------------- | ------------- | ------------------------------------ | ---- |
| DR-SC-A-003 | Defense-in-depth codification across multiple artifact locations  | drill         | institutional-memory, attend-how     | 30m  |
| DR-SC-A-004 | Layer 3 hook security audit                                       | case          | attend-how, intervene-when           | 30m  |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs              | case          | attend-how, intervene-how            | 30m  |
| DR-SC-A-006 | Negative tests for deny-by-default                                | drill         | attend-how, intervene-how            | 30m  |
| DR-SC-A-007 | Zero-config security audit                                        | drill         | attend-how, intervene-how            | 30m  |
| DR-SC-A-010 | Token budget — baked-in vs external boundary decision             | case          | institutional-memory                 | 30m  |
| DR-SC-A-011 | Framework-first check before building new functionality           | drill         | intervene-when, attend-how           | 30m  |
| DR-SC-A-014 | Decompose milestones with hard-blocking approval gates            | drill         | intervene-how, institutional-memory  | 30m  |
| DR-SC-B-001 | Read-then-merge sync semantics                                    | brokerage-rep | institutional-memory                 | 30m  |
| DR-SC-B-002 | Authority-chain escalation when local fix gets reverted           | brokerage-rep | institutional-memory, intervene-when | 30m  |
| DR-SC-B-004 | Specialist delegation as required first move (framework work)     | brokerage-rep | intervene-when, institutional-memory | 30m  |
| DR-SC-B-005 | Spec-to-code conformance audit                                    | brokerage-rep | attend-when, institutional-memory    | 30m  |
| DR-SC-B-006 | Multi-round red team to numeric convergence                       | brokerage-rep | attend-when, intervene-when          | 30m  |
| DR-SC-B-008 | Convergence-as-validation across independent reds                 | observation   | attend-how, attend-when              | 30m  |
| DR-SC-B-009 | Cross-SDK upstream brokerage                                      | brokerage-rep | intervene-how, institutional-memory  | 30m  |
| DR-SC-B-010 | Cross-repo divergence audit with numbered drift-gap list          | brokerage-rep | attend-how, institutional-memory     | 30m  |
| DR-SC-B-011 | Version tracking cascade across the repo chain                    | brokerage-rep | institutional-memory                 | 30m  |
| DR-SC-B-013 | Map cross-workspace dependency surface before parallelizing       | brokerage-rep | attend-how                           | 30m  |
| DR-SC-B-014 | Journal decision reversals with explicit supersession references  | brokerage-rep | institutional-memory                 | 30m  |
| DR-SC-B-015 | Reinterpret MUST rules from "human performs" to "human approves"  | brokerage-rep | intervene-when                       | 30m  |
| DR-SC-B-016 | Audit cross-language parity bidirectionally — the other SDK may b | brokerage-rep | attend-how                           | 30m  |
| DR-SC-B-017 | Assign verification-gradient zone explicitly before delegating to | brokerage-rep | intervene-when, institutional-memory | 30m  |
| DR-SC-B-018 | Separate the policy decision point from the policy enforcement po | brokerage-rep | intervene-how                        | 30m  |
| DR-SC-P-001 | Identify adjacent-but-disconnected modules as spec conformance ga | case          | attend-how                           | 30m  |
| DR-SC-P-005 | Diagnose friction-gradient inversions where the best API has the  | case          | attend-how, intervene-when           | 30m  |
| DR-SC-P-006 | Decide whether new functionality is a module inside an existing p | case          | intervene-when                       | 30m  |
| DR-SC-P-007 | Define session completion state before starting, not after conver | drill         | attend-how, intervene-when           | 30m  |
| DR-SC-P-008 | Trace deep dependency chains to find where a single broken link b | case          | attend-how                           | 30m  |
| DR-SC-P-011 | Detect when an audit categorization method is structurally blind  | case          | attend-how                           | 30m  |
| DR-SC-P-012 | Decide whether to demote or strengthen a rule when ablation shows | case          | intervene-when                       | 30m  |
| DR-SC-P-014 | Distinguish pre-existing failures from pre-existing gaps when app | case          | intervene-when                       | 30m  |
| DR-SC-P-016 | Detect overcorrection in both directions as a single contaminatio | case          | behaviour, attend-how                | 30m  |
| DR-SC-P-017 | Detect false-positive learning pipelines that report healthy metr | case          | attend-how, attend-when              | 30m  |
| DR-SC-P-019 | Verify session-start continuity reaches the model, not just the t | drill         | attend-when, institutional-memory    | 30m  |
| DR-SC-P-020 | Treat the second occurrence of a drift class as a structural sign | case          | attend-when                          | 30m  |
| DR-SC-P-022 | Author rules for the weakest model in the deployment envelope, no | case          | behaviour, attend-how                | 30m  |
| DR-SC-P-024 | Intervene when a temporary workaround accumulates its second cons | case          | attend-when, intervene-when          | 30m  |

### advanced (6 drills)

| Drill ID    | Name                                                             | Modality | Craft area                          | Time |
| ----------- | ---------------------------------------------------------------- | -------- | ----------------------------------- | ---- |
| DR-SC-A-002 | Frontend mock data detection                                     | build    | attend-how, intervene-how           | 45m  |
| DR-SC-A-012 | Author CLAUDE.md as the Layer 2 master directive                 | build    | institutional-memory                | 45m  |
| DR-SC-A-013 | Design agent specialisation routing as explicit Layer 1 Intent   | build    | intervene-how, institutional-memory | 45m  |
| DR-SC-A-015 | Structure the Layer 2 institutional knowledge corpus by artifact | build    | institutional-memory                | 45m  |
| DR-SC-B-007 | Worktree-per-agent for safe parallel execution                   | build    | institutional-memory, intervene-how | 45m  |
| DR-SC-P-002 | Run empirical model-vs-rule ablation in a clean-room environment | build    | attend-how                          | 45m  |

## Index by spec lineage (primary standard)

### CO (56 drills)

| Drill ID    | Name                                                    | Spec lineage                                                                     | Difficulty   | Time |
| ----------- | ------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------ | ---- |
| DR-SC-A-001 | Codify-with-explicit-NOT-codified                       | CO §39 — Layer 5 — Learning; CO §47 — Knowledge Curator; CO §17 — Single Source  | beginner     | 15m  |
| DR-SC-A-002 | Frontend mock data detection                            | CO §3.3 — Safety Blindness; CO §29 — Evidence-Based Completion; CO §19 — Layer 3 | advanced     | 45m  |
| DR-SC-A-003 | Defense-in-depth codification across multiple artifact  | CO §5.4 — Defense in Depth; CO §19 — Layer 3 — Guardrails                        | intermediate | 30m  |
| DR-SC-A-004 | Layer 3 hook security audit                             | CO §3.3 — Safety Blindness; CO §19 — Layer 3 — Guardrails                        | intermediate | 30m  |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs    | CO §5 — Deterministic Enforcement Over Probabilistic Compliance                  | intermediate | 30m  |
| DR-SC-A-006 | Negative tests for deny-by-default                      | CO §49 — Quality Criteria — Enforcement Reliability                              | intermediate | 30m  |
| DR-SC-A-007 | Zero-config security audit                              | CO §3.3 — Safety Blindness; CO §19 — Layer 3 — Guardrails                        | intermediate | 30m  |
| DR-SC-A-008 | Rule classification — critical vs advisory              | CO §19 — Layer 3 — Guardrails; CO §5 — Deterministic Enforcement Over Probabilis | beginner     | 15m  |
| DR-SC-A-009 | Progressive disclosure architecture for CC artifacts    | CO §15 — Progressive Disclosure; CO §13 — Layer 2 — Context; CO §14 — Master Dir | beginner     | 15m  |
| DR-SC-A-010 | Token budget — baked-in vs external boundary decision   | CO §13 — Layer 2 — Context; CO §14 — Master Directive; CO §15 — Progressive Disc | intermediate | 30m  |
| DR-SC-A-011 | Framework-first check before building new functionality | CO §16 — Framework-First; CO §17 — Single Source of Truth                        | intermediate | 30m  |
| DR-SC-A-012 | Author CLAUDE.md as the Layer 2 master directive        | CO §13 — Layer 2 — Context; CO §14 — Master Directive; CO §15 — Progressive Disc | advanced     | 45m  |
| DR-SC-A-013 | Design agent specialisation routing as explicit Layer 1 | CO §9 — Layer 1 — Intent; CO §11 — Routing Rules                                 | advanced     | 45m  |
| DR-SC-A-014 | Decompose milestones with hard-blocking approval gates  | CO §26 — Layer 4 — Instructions; CO §27 — Structured Workflow; CO §28 — Approval | intermediate | 30m  |
| DR-SC-A-015 | Structure the Layer 2 institutional knowledge corpus by | CO §13 — Layer 2 — Context; CO §14 — Master Directive; CO §39 — Layer 5 — Learni | advanced     | 45m  |
| DR-SC-B-001 | Read-then-merge sync semantics                          | CO §17 — Single Source of Truth; CO §28 — Approval Gate                          | intermediate | 30m  |
| DR-SC-B-002 | Authority-chain escalation when local fix gets reverted | CO §17 — Single Source of Truth                                                  | intermediate | 30m  |
| DR-SC-B-003 | Variant classification — single-sentence test rule      | CO §17 — Single Source of Truth; CO §16 — Framework-First                        | beginner     | 15m  |
| DR-SC-B-004 | Specialist delegation as required first move (framework | CO §9 — Layer 1 — Intent; CO §11 — Routing Rules                                 | intermediate | 30m  |
| DR-SC-B-005 | Spec-to-code conformance audit                          | CO §49 — Quality Criteria — Enforcement Reliability                              | intermediate | 30m  |
| DR-SC-B-006 | Multi-round red team to numeric convergence             | CO §28 — Approval Gate; CO §49 — Quality Criteria — Enforcement Reliability      | intermediate | 30m  |
| DR-SC-B-007 | Worktree-per-agent for safe parallel execution          | CO §71 — COC Observation-Instinct-Evolution Pipeline                             | advanced     | 45m  |
| DR-SC-B-008 | Convergence-as-validation across independent reds       | CO §5.4 — Defense in Depth; CO §49 — Quality Criteria — Enforcement Reliability  | intermediate | 30m  |
| DR-SC-B-009 | Cross-SDK upstream brokerage                            | CO §17 — Single Source of Truth                                                  | intermediate | 30m  |
| DR-SC-B-010 | Cross-repo divergence audit with numbered drift-gap lis | CO §17 — Single Source of Truth; CO §49 — Quality Criteria — Enforcement Reliabi | intermediate | 30m  |
| DR-SC-B-011 | Version tracking cascade across the repo chain          | CO §17 — Single Source of Truth; CO §13 — Layer 2 — Context                      | intermediate | 30m  |
| DR-SC-B-012 | Automate manifest-to-filesystem parity check and halt o | CO §49 — Quality Criteria — Enforcement Reliability; CO §17 — Single Source of T | beginner     | 15m  |
| DR-SC-B-013 | Map cross-workspace dependency surface before paralleli | CO §27 — Structured Workflow; CO §17 — Single Source of Truth                    | intermediate | 30m  |
| DR-SC-B-014 | Journal decision reversals with explicit supersession r | CO §28 — Approval Gate; CO §17 — Single Source of Truth                          | intermediate | 30m  |
| DR-SC-B-015 | Reinterpret MUST rules from "human performs" to "human  | CO §28 — Approval Gate; CO §4 — Human-on-the-Loop                                | intermediate | 30m  |
| DR-SC-B-016 | Audit cross-language parity bidirectionally — the other | CO §17 — Single Source of Truth; CO §29 — Evidence-Based Completion              | intermediate | 30m  |
| DR-SC-B-018 | Separate the policy decision point from the policy enfo | CO §5 — Deterministic Enforcement Over Probabilistic Compliance                  | intermediate | 30m  |
| DR-SC-P-001 | Identify adjacent-but-disconnected modules as spec conf | CO §17 — Single Source of Truth                                                  | intermediate | 30m  |
| DR-SC-P-002 | Run empirical model-vs-rule ablation in a clean-room en | CO §1 — Institutional Knowledge Thesis; CO §22 — Advisory Rules                  | advanced     | 45m  |
| DR-SC-P-003 | Write cross-feature interaction tests before individual | CO §3.3 — Safety Blindness (Failure Mode)                                        | beginner     | 15m  |
| DR-SC-P-004 | Label every file with a disposition before starting imp | CO §33 — Phase — Analyze; CO §29 — Evidence-Based Completion                     | beginner     | 15m  |
| DR-SC-P-005 | Diagnose friction-gradient inversions where the best AP | CO §1 — Institutional Knowledge Thesis; CO §3.2 — Convention Drift (Failure Mode | intermediate | 30m  |
| DR-SC-P-006 | Decide whether new functionality is a module inside an  | CO §16 — Framework-First; CO §17 — Single Source of Truth                        | intermediate | 30m  |
| DR-SC-P-007 | Define session completion state before starting, not af | CO §28 — Approval Gate; CO §29 — Evidence-Based Completion                       | intermediate | 30m  |
| DR-SC-P-008 | Trace deep dependency chains to find where a single bro | CO §3.3 — Safety Blindness                                                       | intermediate | 30m  |
| DR-SC-P-009 | Treat architecture documents as testable claims against | CO §29 — Evidence-Based Completion; CO §16 — Framework-First                     | beginner     | 15m  |
| DR-SC-P-010 | Red team CI, auth, and release processes for timing-dep | CO §3.3 — Safety Blindness; CO §5 — Deterministic Enforcement Over Probabilistic | beginner     | 15m  |
| DR-SC-P-011 | Detect when an audit categorization method is structura | CO §15 — Progressive Disclosure; CO §49 — Quality Criteria — Enforcement Reliabi | intermediate | 30m  |
| DR-SC-P-012 | Decide whether to demote or strengthen a rule when abla | CO §1 — Institutional Knowledge Thesis; CO §39 — Layer 5 — Learning; CO §22 — Ad | intermediate | 30m  |
| DR-SC-P-013 | Treat internal estimate inconsistency as a high-confide | CO §28 — Approval Gate                                                           | beginner     | 15m  |
| DR-SC-P-014 | Distinguish pre-existing failures from pre-existing gap | CO §29 — Evidence-Based Completion; CO §21 — Critical Rules                      | intermediate | 30m  |
| DR-SC-P-015 | End complex prompts with "what would invalidate this?"  | CO §28 — Approval Gate                                                           | beginner     | 15m  |
| DR-SC-P-016 | Detect overcorrection in both directions as a single co | CO §3.3 — Safety Blindness (Failure Mode); CO §1 — Institutional Knowledge Thesi | intermediate | 30m  |
| DR-SC-P-017 | Detect false-positive learning pipelines that report he | CO §39 — Layer 5 (Learning); CO §3.3 — Safety Blindness (Failure Mode)           | intermediate | 30m  |
| DR-SC-P-018 | Split a multi-session blocker into fast-unblock and par | CO §27 — Structured Workflow; CO §28 — Approval Gate                             | beginner     | 15m  |
| DR-SC-P-019 | Verify session-start continuity reaches the model, not  | CO §39 — Layer 5 (Learning); CO §9 — Layer 1 (Intent)                            | intermediate | 30m  |
| DR-SC-P-020 | Treat the second occurrence of a drift class as a struc | CO §3.2 — Convention Drift (Failure Mode); CO §17 — Single Source of Truth       | intermediate | 30m  |
| DR-SC-P-021 | Grep the entire repo for dangling references immediatel | CO §17 — Single Source of Truth; CO §15 — Progressive Disclosure                 | beginner     | 15m  |
| DR-SC-P-022 | Author rules for the weakest model in the deployment en | CO §1 — Institutional Knowledge Thesis; CO §22 — Advisory Rules                  | intermediate | 30m  |
| DR-SC-P-023 | Insert a gating workspace at integration seams rather t | CO §27 — Structured Workflow; CO §28 — Approval Gate                             | beginner     | 15m  |
| DR-SC-P-024 | Intervene when a temporary workaround accumulates its s | CO §3.2 — Convention Drift (Failure Mode); CO §5 — Deterministic Enforcement Ove | intermediate | 30m  |

### PACT (9 drills)

| Drill ID    | Name                                                    | Spec lineage                                                                     | Difficulty   | Time |
| ----------- | ------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------ | ---- |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs    | PACT §10 — Monotonic Tightening Invariant                                        | intermediate | 30m  |
| DR-SC-A-006 | Negative tests for deny-by-default                      | PACT §23 — Blocked Zone                                                          | intermediate | 30m  |
| DR-SC-B-002 | Authority-chain escalation when local fix gets reverted | PACT §10 — Monotonic Tightening; PACT §11 — Role Envelope                        | intermediate | 30m  |
| DR-SC-B-004 | Specialist delegation as required first move (framework | PACT §11 — Role Envelope                                                         | intermediate | 30m  |
| DR-SC-B-005 | Spec-to-code conformance audit                          | PACT §40 — EATP Integration (Governance-Protocol Mapping); PACT §10 — Monotonic  | intermediate | 30m  |
| DR-SC-B-009 | Cross-SDK upstream brokerage                            | PACT §10 — Monotonic Tightening Invariant                                        | intermediate | 30m  |
| DR-SC-B-017 | Assign verification-gradient zone explicitly before del | PACT §19 — Verification Gradient (4-Zone); PACT §11 — Role Envelope (Standing)   | intermediate | 30m  |
| DR-SC-P-001 | Identify adjacent-but-disconnected modules as spec conf | PACT §40 — EATP Integration (Governance-Protocol Mapping)                        | intermediate | 30m  |
| DR-SC-P-003 | Write cross-feature interaction tests before individual | PACT §51 — Edge Case — Vacant Roles; PACT §35 — Bridges (Standing Cross-function | beginner     | 15m  |

### EATP (2 drills)

| Drill ID    | Name                                                    | Spec lineage                                      | Difficulty   | Time |
| ----------- | ------------------------------------------------------- | ------------------------------------------------- | ------------ | ---- |
| DR-SC-A-005 | Encapsulation as security boundary in signed structs    | EATP §16 — Element 2 — Delegation Record          | intermediate | 30m  |
| DR-SC-B-018 | Separate the policy decision point from the policy enfo | EATP §40 — Enforcement Model (PDP/PEP Separation) | intermediate | 30m  |

### CARE (2 drills)

| Drill ID    | Name                                                   | Spec lineage                                  | Difficulty   | Time |
| ----------- | ------------------------------------------------------ | --------------------------------------------- | ------------ | ---- |
| DR-SC-B-001 | Read-then-merge sync semantics                         | CARE §6 — Trust Verification Bridge           | intermediate | 30m  |
| DR-SC-B-015 | Reinterpret MUST rules from "human performs" to "human | CARE §4 — Human-on-the-Loop (Framework Model) | intermediate | 30m  |
