---
title: COR Specification Concept Index
date: 2026-04-08
type: spec-index
source: terrene/foundation/docs/02-standards/co/applications/co-for-research.md
---

# COR Specification Concept Index

Exhaustive enumeration of every discrete concept in the COR spec, organized by CO layer. Each concept is tagged as COR-specific (new) or CO-inherited (instantiation of base CO concept).

## Application Identity (3 concepts)

| #   | Concept                                                                             | Type         | Notes                            |
| --- | ----------------------------------------------------------------------------------- | ------------ | -------------------------------- |
| C01 | COR target domain: academic research, thesis co-authorship, publication preparation | COR-specific | Defines the application boundary |
| C02 | COR target practitioners: researchers, authors, academics co-writing with AI        | COR-specific | Audience definition              |
| C03 | COR first deployment: CARE thesis v3.0                                              | COR-specific | Proof point                      |

## Three Research Failure Modes (15 concepts)

Parallel to COC's three failure modes (amnesia, convention drift, security blindness), COR defines research-specific instantiations.

| #   | Concept                                                                              | Type         | Notes                                     |
| --- | ------------------------------------------------------------------------------------ | ------------ | ----------------------------------------- |
| C04 | Amnesia in research — scope boundary forgetting across sessions                      | CO-inherited | CO failure mode 1, research instantiation |
| C05 | Cross-paper scope leakage (CARE paper drifts into EATP detail)                       | COR-specific | Multi-paper research problem              |
| C06 | Cross-paper citation suffix inconsistency (Hong 2026a vs 2026b)                      | COR-specific | Research-specific amnesia                 |
| C07 | Previous deliberation decisions forgotten and relitigated                            | CO-inherited | Anti-amnesia, research context            |
| C08 | Convention drift in research — generic vs venue-specific conventions                 | CO-inherited | CO failure mode 2, research instantiation |
| C09 | Venue-specific format requirements (AIES: AAAI 2-column; AI & Society: double-blind) | COR-specific | Research convention                       |
| C10 | Author voice drift toward generic academic prose                                     | COR-specific | Research-specific convention drift        |
| C11 | Citation density falling below venue minimums                                        | COR-specific | Research convention                       |
| C12 | Safety blindness in research — fabricated citations                                  | CO-inherited | CO failure mode 3, research instantiation |
| C13 | Misattribution — attributing claims to papers that don't make those claims           | COR-specific | Research integrity                        |
| C14 | Conjecture presented as established finding                                          | COR-specific | Research integrity                        |
| C15 | Unqualified superlatives — "first", "only", "novel" without qualification            | COR-specific | Overclaim prevention                      |
| C16 | Archival content risk — irretractable content in arXiv/SSRN                          | COR-specific | Publication safety                        |
| C17 | Rubber-stamping — AI approves without challenging weak arguments                     | COR-specific | Research quality                          |
| C18 | Institutional knowledge inventory for research (7 categories)                        | CO-inherited | L2 pattern, research instantiation        |

## Layer 1 — Intent: Agent Specializations (18 concepts)

| #   | Concept                                                                 | Type         | Notes                                          |
| --- | ----------------------------------------------------------------------- | ------------ | ---------------------------------------------- |
| C19 | literature-researcher agent — systematic discovery and deep reading     | COR-specific | No COC parallel                                |
| C20 | Academic search protocols as agent knowledge                            | COR-specific |                                                |
| C21 | Citation verification as agent capability                               | COR-specific |                                                |
| C22 | Source quality assessment as agent skill                                | COR-specific |                                                |
| C23 | field-expert agent — domain knowledge tutoring, configurable per domain | COR-specific | No COC parallel                                |
| C24 | Key papers, debates, schools of thought as expert knowledge             | COR-specific |                                                |
| C25 | Common miscitations as expert knowledge                                 | COR-specific |                                                |
| C26 | claims-verifier agent — factual and attributive claim verification      | COR-specific | Closest COC parallel: red-team agent           |
| C27 | Fabrication pattern detection as agent capability                       | COR-specific |                                                |
| C28 | Overclaim detection as agent capability                                 | COR-specific |                                                |
| C29 | argument-critic agent — adversarial reviewer simulation                 | COR-specific | Closest COC parallel: red-team agent           |
| C30 | Venue-specific reviewer expectations as agent knowledge                 | COR-specific |                                                |
| C31 | Logical fallacy detection as agent skill                                | COR-specific |                                                |
| C32 | writing-partner agent — paragraph-level co-writing with deliberation    | COR-specific | No COC parallel                                |
| C33 | Inline margin notes explaining every choice                             | COR-specific | Key COR innovation                             |
| C34 | Citation placement norms as agent knowledge                             | COR-specific |                                                |
| C35 | cross-reference-auditor agent — citation integrity checking             | COR-specific | Closest COC parallel: gold-standards-validator |
| C36 | Routing rules: 6 task-type → agent mappings                             | CO-inherited | L1 pattern, research instantiation             |

## Layer 2 — Context: Progressive Disclosure (16 concepts)

| #   | Concept                                                                               | Type         | Notes              |
| --- | ------------------------------------------------------------------------------------- | ------------ | ------------------ |
| C37 | Level 0 (Always Loaded): 6 rule files                                                 | CO-inherited | L2 pattern         |
| C38 | research-integrity.md — scope boundaries, verification, disclosure                    | COR-specific |                    |
| C39 | research-teaching.md — teaching obligations                                           | COR-specific | Key COR innovation |
| C40 | deliberation-records.md — decision recording requirements                             | COR-specific |                    |
| C41 | publication-claims.md — overclaim prevention                                          | COR-specific |                    |
| C42 | publication-quality.md — venue standards                                              | COR-specific |                    |
| C43 | academic-writing-style.md — Principle 8 voice preservation                            | COR-specific | Key COR innovation |
| C44 | Level 1 (Session Context): anti-amnesia injection                                     | CO-inherited | L2 pattern         |
| C45 | Paper scope as session context                                                        | COR-specific |                    |
| C46 | Current section being written as context                                              | COR-specific |                    |
| C47 | Previous deliberation decisions from decision records                                 | COR-specific |                    |
| C48 | Author's voice and argumentative preferences                                          | COR-specific |                    |
| C49 | Level 2 (On-Demand): standards reference skills, publication skill, arXiv skill       | CO-inherited | L2 pattern         |
| C50 | Level 3 (Deep Reference): thesis versions, spec documents                             | CO-inherited | L2 pattern         |
| C51 | Anti-amnesia injection at session start                                               | CO-inherited | L2 pattern         |
| C52 | Anti-amnesia content: scope reminder, verification requirement, decision-record check | COR-specific |                    |

## Layer 3 — Guardrails (13 concepts)

| #   | Concept                                                          | Type         | Notes                           |
| --- | ---------------------------------------------------------------- | ------------ | ------------------------------- |
| C53 | Hard rule: no unverified citations in final draft                | COR-specific | Hook-enforced                   |
| C54 | Hard rule: no scope violations (wrong paper detail leakage)      | COR-specific | Hook-enforced                   |
| C55 | Hard rule: no overclaims ("first", "only" without qualification) | COR-specific | Hook-enforced                   |
| C56 | Hard rule: no fabricated references — FATAL, blocks submission   | COR-specific | Hook-enforced, highest severity |
| C57 | Soft rule: every paragraph teaches                               | COR-specific | Key COR innovation              |
| C58 | Soft rule: alternatives presented for key phrases (2-3 options)  | COR-specific |                                 |
| C59 | Soft rule: deliberation decisions recorded                       | COR-specific |                                 |
| C60 | Soft rule: literature notes include connection to paper          | COR-specific |                                 |
| C61 | Soft rule: authentic voice preservation                          | COR-specific | Principle 8                     |
| C62 | Hook enforcement: validate-research-content.js                   | COR-specific |                                 |
| C63 | Hook enforcement: validate-publication-content.js                | COR-specific |                                 |
| C64 | Hook enforcement: validate-teaching-notes.js                     | COR-specific |                                 |
| C65 | Hard vs soft rule classification pattern                         | CO-inherited | L3 pattern                      |

## Layer 4 — Instructions: Two Writing Modes (30 concepts)

| #   | Concept                                                                                                                                | Type         | Notes                              |
| --- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------------------------------- |
| C66 | /craft mode — author writes, AI teaches and critiques                                                                                  | COR-specific | Key COR innovation                 |
| C67 | /craft use cases: strong-view sections, defense prep, originality coaching                                                             | COR-specific |                                    |
| C68 | /craft output: craft-notes/ (literature gaps, challenges, originality)                                                                 | COR-specific |                                    |
| C69 | /write-para mode — AI drafts, author approves                                                                                          | COR-specific | Key COR innovation                 |
| C70 | /write-para use cases: rapid iteration, technical descriptions, lit review synthesis                                                   | COR-specific |                                    |
| C71 | /write-para output: deliberation/ logs, versions/ snapshots                                                                            | COR-specific |                                    |
| C72 | COR command chain: 10 commands mapping to COC equivalents                                                                              | CO-inherited | L4 pattern, research instantiation |
| C73 | /literature command — systematic field search                                                                                          | COR-specific |                                    |
| C74 | /teach command — concept explanation with field context                                                                                | COR-specific |                                    |
| C75 | /deliberate command — structural decision recording                                                                                    | COR-specific |                                    |
| C76 | /challenge command — hostile reviewer simulation                                                                                       | COR-specific |                                    |
| C77 | /validate-claim command — claim verification against sources                                                                           | COR-specific |                                    |
| C78 | /check-refs command — cross-reference integrity                                                                                        | COR-specific |                                    |
| C79 | /preflight command — pre-submission deep validation                                                                                    | COR-specific |                                    |
| C80 | /publish command — venue-specific preparation                                                                                          | COR-specific |                                    |
| C81 | 5-phase research workflow: LEARN → DECIDE → WRITE → VERIFY → SUBMIT                                                                    | COR-specific |                                    |
| C82 | Phase 1 LEARN: /teach + /literature → literature notes                                                                                 | COR-specific |                                    |
| C83 | Phase 2 DECIDE: /deliberate → decision records                                                                                         | COR-specific |                                    |
| C84 | Phase 3 WRITE: /craft or /write-para → drafts                                                                                          | COR-specific |                                    |
| C85 | Phase 4 VERIFY: /validate-claim + /challenge + /check-refs → reviews                                                                   | COR-specific |                                    |
| C86 | Phase 5 SUBMIT: /preflight + /publish → submission                                                                                     | COR-specific |                                    |
| C87 | 6 approval gates (literature inclusion, structural decisions, every paragraph, craft assessment, FATAL findings, submission readiness) | COR-specific |                                    |
| C88 | Literature inclusion gate: author decides which papers enter working bibliography                                                      | COR-specific |                                    |
| C89 | Every-paragraph gate: explicit approval before moving on                                                                               | COR-specific |                                    |
| C90 | FATAL findings gate: must address before continuing section                                                                            | COR-specific |                                    |
| C91 | Submission readiness gate: final go/no-go                                                                                              | COR-specific |                                    |
| C92 | Session continuity via /wrapup and .session-notes                                                                                      | CO-inherited | L4 pattern                         |
| C93 | Session context: current section, unresolved deliberations, lit gaps, challenges                                                       | COR-specific |                                    |
| C94 | Teaching obligation — every agent interaction shifts from "execute" to "teach me why, then execute"                                    | COR-specific | Key COR innovation #1              |
| C95 | Dual writing modes as key COR innovation #2                                                                                            | COR-specific |                                    |

## Layer 5 — Learning (18 concepts)

| #    | Concept                                                                                                                                 | Type         | Notes              |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------ |
| C96  | Journal as primary knowledge trail                                                                                                      | CO-inherited | L5 pattern         |
| C97  | 8 journal entry types: TEACH, LITERATURE, DELIBERATION, MARGIN, DEFENSE, CLAIM, CONNECTION, GAP                                         | COR-specific |                    |
| C98  | TEACH entry type — concept explanations, field context, debates                                                                         | COR-specific |                    |
| C99  | LITERATURE entry type — paper assessments, relevance, connection                                                                        | COR-specific |                    |
| C100 | DELIBERATION entry type — structural decisions, rationale, alternatives                                                                 | COR-specific |                    |
| C101 | MARGIN entry type — inline annotations explaining writing choices                                                                       | COR-specific | Key COR innovation |
| C102 | DEFENSE entry type — argument challenges, author's defenses                                                                             | COR-specific |                    |
| C103 | CLAIM entry type — claim verification results (verified/overclaimed)                                                                    | COR-specific |                    |
| C104 | CONNECTION entry type — cross-paper or cross-concept links                                                                              | COR-specific |                    |
| C105 | GAP entry type — literature or argument gaps requiring attention                                                                        | COR-specific |                    |
| C106 | Journal entry naming: NNNN-TYPE-topic.md with frontmatter                                                                               | CO-inherited | L5 pattern         |
| C107 | Honest limitation: journal entries are AI-curated, not author's own words                                                               | COR-specific |                    |
| C108 | 5 observation categories: argument patterns, literature preferences, teaching effectiveness, reviewer predictions, scope drift patterns | COR-specific |                    |
| C109 | Confidence thresholds per category (70-90%)                                                                                             | COR-specific |                    |
| C110 | Reviewer prediction calibration: post-review feedback loop                                                                              | COR-specific |                    |
| C111 | Feedback loop: compare challenge predictions vs actual reviewer comments                                                                | COR-specific |                    |
| C112 | Update argument-critic calibration based on hit rate                                                                                    | COR-specific |                    |
| C113 | Record successful argument defenses for reuse across papers                                                                             | COR-specific |                    |

## Principle 8 — Authentic Voice Preservation (8 concepts)

| #    | Concept                                                                     | Type         | Notes                          |
| ---- | --------------------------------------------------------------------------- | ------------ | ------------------------------ |
| C114 | AI assistance disclosure per venue policy                                   | COR-specific | Key — COR is primary P8 domain |
| C115 | Human direction via two writing modes and approval gates                    | COR-specific |                                |
| C116 | Auditable trail: journal + deliberation records + decision logs + approvals | COR-specific |                                |
| C117 | Detection bias mitigation via academic-writing-style.md                     | COR-specific |                                |
| C118 | Vocabulary pattern constraints (Kobak et al. 2024)                          | COR-specific |                                |
| C119 | Sentence uniformity constraints (burstiness detectors)                      | COR-specific |                                |
| C120 | Stylistic pattern constraints (false positives for non-native speakers)     | COR-specific |                                |
| C121 | Distinction: detection mitigation ≠ hiding AI use                           | COR-specific |                                |

## EATP Integration (5 concepts)

| #    | Concept                                                            | Type         | Notes                           |
| ---- | ------------------------------------------------------------------ | ------------ | ------------------------------- |
| C122 | EATP as optional integrity layer for COR                           | CO-inherited | EATP integration pattern        |
| C123 | Decision records as lightweight audit anchors (hash-chained)       | COR-specific |                                 |
| C124 | Constraint envelope defining paper scope boundaries                | COR-specific |                                 |
| C125 | Verification gradient (QUICK/STANDARD/FULL) for research decisions | CO-inherited | EATP concept, COR instantiation |
| C126 | Trust posture progression: SUPERVISED → SHARED_PLANNING            | CO-inherited | EATP concept, COR instantiation |

## Honest Limitations (6 concepts)

| #    | Concept                                                                 | Type         | Notes |
| ---- | ----------------------------------------------------------------------- | ------------ | ----- |
| C127 | Citation verification depends on web access (paywall limitation)        | COR-specific |       |
| C128 | AI is not a domain expert (synthesizes, doesn't know)                   | COR-specific |       |
| C129 | Argument-critic calibrated against generic reviewers, not specific ones | COR-specific |       |
| C130 | Journal entries are AI-curated interpretations                          | COR-specific |       |
| C131 | Teaching quality degrades outside training distribution                 | COR-specific |       |
| C132 | L5 confidence thresholds are aspirational, not calibrated               | COR-specific |       |

## COC–COR Mapping (3 concepts)

| #    | Concept                                                                                                                                | Type         | Notes               |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------- |
| C133 | COC agents → development tasks; COR agents → research tasks                                                                            | COR-specific | Structural parallel |
| C134 | COR agents must teach as they work, not just execute                                                                                   | COR-specific | Key differentiation |
| C135 | COR innovation: Principle 8 originated here because academic co-authorship sits at AI assistance × intellectual integrity intersection | COR-specific | Origin story        |

## Totals

| Category                                       | Count   |
| ---------------------------------------------- | ------- |
| COR-specific concepts                          | 108     |
| CO-inherited concepts (research instantiation) | 27      |
| **Total enumerated concepts**                  | **135** |

Key COR innovations (concepts not paralleled in COC):

1. Teaching obligation (C94) — every interaction teaches
2. Dual writing modes (C66, C69, C95) — /craft and /write-para
3. Authentic voice preservation (C114-C121) — Principle 8 primary domain
4. 8 journal entry types (C97-C105) — richer than COC's journal
5. Margin notes as first-class artifact (C33, C101) — inline deliberation
6. Fabricated citation as FATAL (C56) — highest severity guardrail in any CO application
