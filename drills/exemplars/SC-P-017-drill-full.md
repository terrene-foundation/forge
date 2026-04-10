---
drill_id: DR-SC-P-017-FULL
source_atom: SC-P-017
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Detect False-Positive Learning Pipeline

## Setup

The practitioner is auditing the Layer 5 (Learning) system for a Kailash SDK project. The project lead says: "Our learning pipeline is working great — we've captured over 5,000 observations and generated instinct files automatically. The `/codify` command reads from the instincts to update our agents."

The practitioner receives three artifacts to audit:

**Artifact 1 — Observation log** (`learning/observations.jsonl`, 5,000 entries):

```jsonl
{"type":"session_start","timestamp":"2026-03-01T09:00:00Z","session_id":"s-001"}
{"type":"session_start","timestamp":"2026-03-01T09:01:12Z","session_id":"s-002"}
{"type":"session_stop","timestamp":"2026-03-01T09:45:00Z","session_id":"s-001","duration_s":2700}
{"type":"session_start","timestamp":"2026-03-01T10:00:00Z","session_id":"s-003"}
{"type":"workflow_pattern","timestamp":"2026-03-01T10:12:33Z","session_id":"s-003","pattern":"workflow_builder","confidence":0.85,"context":"User created ETL pipeline with 3 transform nodes"}
{"type":"session_stop","timestamp":"2026-03-01T10:30:00Z","session_id":"s-002","duration_s":5328}
{"type":"session_start","timestamp":"2026-03-01T11:00:00Z","session_id":"s-004"}
{"type":"error_occurrence","timestamp":"2026-03-01T11:05:00Z","session_id":"s-004","error":"ImportError: cannot import DataFlow from kailash","resolution":"fixed import path","context":"User had stale import from v0.8"}
{"type":"session_stop","timestamp":"2026-03-01T11:45:00Z","session_id":"s-003","duration_s":6300}
{"type":"session_stop","timestamp":"2026-03-01T12:00:00Z","session_id":"s-004","duration_s":3600}
... (4,990 more entries following the same distribution)
```

**Distribution summary** (provided as a comment at the end of the file):

```
# Event type distribution:
#   session_start:      2,480 (49.6%)
#   session_stop:       2,320 (46.4%)
#   workflow_pattern:     120 (2.4%)
#   error_occurrence:      55 (1.1%)
#   error_fix:             18 (0.36%)
#   connection_pattern:     7 (0.14%)
# Total:               5,000
# High-signal events:    200 (4.0%)
# Infrastructure noise: 4,800 (96.0%)
```

**Artifact 2 — Generated instinct files** (`learning/instincts/`, 6 files):

```yaml
# instincts/instinct-001.yaml
name: "Use workflow_builder pattern"
description: "When creating workflows, use the workflow_builder pattern for node composition"
confidence: 0.9
source_events: 120
category: "workflow"

# instincts/instinct-002.yaml
name: "Use workflow_builder for pipelines"
description: "When building data pipelines, use the workflow_builder pattern"
confidence: 0.9
source_events: 120
category: "workflow"

# instincts/instinct-003.yaml
name: "Apply workflow_builder pattern"
description: "When constructing workflows, apply the workflow_builder pattern for composition"
confidence: 0.9
source_events: 120
category: "workflow"

# instincts/instinct-004.yaml
name: "Workflow builder for node composition"
description: "Use workflow_builder pattern when composing nodes into workflows"
confidence: 0.9
source_events: 120
category: "workflow"

# instincts/instinct-005.yaml
name: "Use workflow_builder approach"
description: "When orchestrating nodes, use the workflow_builder approach"
confidence: 0.9
source_events: 115
category: "workflow"

# instincts/instinct-006.yaml
name: "Apply workflow_builder for ETL"
description: "When building ETL workflows, apply the workflow_builder pattern"
confidence: 0.9
source_events: 118
category: "workflow"
```

**Artifact 3 — Pipeline directories:**

```
learning/
├── observations.jsonl       (5,000 entries)
├── instincts/               (6 files, as shown above)
├── evolved/                 (directory exists, EMPTY)
└── processing/
    └── generate-instincts.js (120 lines)
```

The `generate-instincts.js` script contains the following core logic:

```javascript
// Core processing logic (lines 45-78)
const observations = readJsonl("observations.jsonl");
const patterns = observations.filter((o) => o.type === "workflow_pattern");
const grouped = groupBy(patterns, "pattern");

for (const [pattern, events] of Object.entries(grouped)) {
  for (let i = 0; i < NUM_INSTINCTS; i++) {
    writeInstinct({
      name: generateVariation(`Use ${pattern} pattern`, i),
      description: generateVariation(
        `When building workflows, use the ${pattern} pattern`,
        i,
      ),
      confidence: 0.9, // hardcoded
      source_events: events.length,
      category: "workflow",
    });
  }
}
```

The `/codify` command's relevant section:

```javascript
// From commands/codify.js (lines 88-102)
async function codify() {
  const instincts = loadInstincts("learning/instincts/");
  // ... agent update logic reads from instincts
  console.log(`Loaded ${instincts.length} instincts for codification.`);
  // NOTE: does not read from learning/evolved/
  // NOTE: does not validate instinct quality or deduplication
}
```

## Task

1. **Input stage audit:** Examine the observation log distribution. What percentage of events are high-signal (useful for learning)? What types of knowledge could be extracted from the high-signal events? What types of knowledge are lost because the pipeline does not capture them?

2. **Processing stage audit:** Examine the instinct files and the processing script. Identify every processing-quality problem. For each problem, explain what the script does wrong and what the output would look like if the problem were fixed.

3. **Promotion stage audit:** Examine the `evolved/` directory and the `/codify` command. What is the promotion pathway from observation to institutional knowledge? Where is it broken?

4. **End-to-end diagnosis:** Write a one-paragraph summary of the pipeline's failure mode. The summary must name all three stages (input, processing, promotion) and explain why fixing any single stage alone would not make the pipeline functional.

5. **Fix proposal:** Propose a concrete fix that addresses all three stages. For each stage, specify what changes and what the corrected output looks like.

## Model Answer

**1. Input stage audit:**

High-signal events represent 200 of 5,000 observations (4.0%). The remaining 96% are infrastructure noise — session start/stop events that record when work happened but not what decisions were made.

The 200 high-signal events break down as:

- 120 `workflow_pattern` events — record which patterns were used, with context
- 55 `error_occurrence` events — record what went wrong and how it was resolved
- 18 `error_fix` events — record explicit fix actions
- 7 `connection_pattern` events — record how nodes were wired together

**Knowledge extractable from high-signal events:**

- Workflow preferences: which patterns are used most, in what contexts (ETL vs API vs agent)
- Common errors: which import paths, version mismatches, or configuration mistakes recur
- Fix patterns: which resolutions are applied repeatedly (could become automated suggestions)
- Wiring patterns: which node connection topologies are common (linear, fan-out, cyclic)

**Knowledge lost because the pipeline does not capture it:**

- Decision points: when the agent chose between two approaches and why (no `decision_point` event type)
- Rule invocations: when a rule fired and whether it helped or hindered (no `rule_applied` event type)
- User corrections: when the user rejected an agent suggestion and provided a different direction (no `user_correction` event type)
- Quality signals: when output was accepted vs revised (no `output_quality` event type)

The input stage captures volume but not the events that carry the most learning value. Even the 200 high-signal events are limited — they record what happened but not why.

**2. Processing stage audit:**

Four processing-quality problems:

| Problem                         | What the script does                                                                                                                                                                                                          | What it should do                                                                                                                                                                                                                                      |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Hardcoded confidence**        | Every instinct gets `confidence: 0.9` regardless of evidence strength. 120 events and 7 events both produce 0.9 confidence.                                                                                                   | Confidence should scale with evidence: a pattern seen 120 times across 50 sessions has higher confidence than one seen 7 times in 3 sessions. Formula example: `confidence = min(0.95, 0.5 + 0.1 * log2(event_count))`.                                |
| **Duplicate generation**        | `NUM_INSTINCTS` loop produces multiple instinct files for the same pattern, differentiated only by `generateVariation()` rephrasing. All 6 instincts are the same insight: "use workflow_builder."                            | One pattern should produce one instinct. If the pattern has sub-variants (workflow_builder for ETL vs workflow_builder for API), those are genuinely different instincts — but the script does not detect sub-variants.                                |
| **Single event type processed** | The script filters for `workflow_pattern` only. The 55 `error_occurrence`, 18 `error_fix`, and 7 `connection_pattern` events are silently discarded.                                                                          | All four high-signal event types should be processed. Error patterns should produce "avoid this mistake" instincts. Fix patterns should produce "apply this resolution" instincts. Connection patterns should produce "wire nodes this way" instincts. |
| **No semantic analysis**        | `groupBy(patterns, 'pattern')` groups by the literal pattern name string. Two events with `pattern: "workflow_builder"` are grouped together even if one is "3-node linear ETL" and the other is "12-node cyclic agent loop." | Group by pattern name AND context. The `context` field distinguishes "ETL pipeline with 3 transform nodes" from "agent loop with retry logic." These are different use cases of the same pattern and should produce different instincts.               |

**3. Promotion stage audit:**

The promotion pathway is: observations → instincts → evolved → institutional knowledge (rules, skills, agent behaviour).

The pathway is broken at two points:

- **evolved/ is empty.** No instinct has ever been promoted from `instincts/` to `evolved/`. There is no script, command, or process that moves an instinct from the generated directory to the evolved directory. The directory exists as a structural placeholder with no functional connection.
- **/codify does not read from evolved/.** Even if instincts were promoted to `evolved/`, the `/codify` command reads from `instincts/` (the raw, unvalidated generated files), not from `evolved/` (the intended curated directory). The `/codify` command also performs no quality validation — it does not check for duplicates, does not verify confidence scores against evidence, and does not filter low-quality instincts.

The combined effect: the pipeline has no curation stage. Raw generated instincts (with all their duplicates and hardcoded confidence) flow directly to `/codify` as if they were validated institutional knowledge. The `evolved/` directory, which was designed to be the human-review checkpoint, is bypassed entirely.

**4. End-to-end diagnosis:**

The learning pipeline is structurally complete — it has an observation store, a processing script, an instinct directory, an evolved directory, and a `/codify` command — but functionally dead at every stage. The input stage captures 5,000 events of which 96% are infrastructure noise, producing a thin evidence base of 200 high-signal events. The processing stage takes those 200 events, discards 80 of them (errors, fixes, connections), and converts the remaining 120 into 6 duplicate instincts with hardcoded confidence, destroying the signal that survived the input filter. The promotion stage has no functioning pathway — the evolved directory is empty and `/codify` bypasses it entirely, reading raw instincts as if they were curated knowledge. Fixing input quality alone (capturing decision points instead of session starts) would feed better data into a processing script that still outputs duplicates at hardcoded confidence. Fixing the processing script alone (deduplication, scaled confidence, multi-event-type processing) would produce higher-quality instincts from a 96%-noise input and promote them through a curation stage that does not exist. Fixing the promotion pathway alone (connecting evolved/ to /codify with human review) would curate and promote the same 6 duplicate instincts. All three stages must be fixed together for the pipeline to produce knowledge.

**5. Fix proposal:**

**Input stage fix:**

- Add four new event types to the observation capture: `decision_point`, `rule_applied`, `user_correction`, `output_quality`.
- Add a filter to the session-end hook that suppresses `session_start` and `session_stop` events from the learning log (or routes them to a separate telemetry log).
- Target: high-signal events should be >50% of the log, not 4%.
- Corrected output: an observation log where the majority of entries describe decisions, errors, fixes, and patterns — not session bookkeeping.

**Processing stage fix:**

- Remove the `NUM_INSTINCTS` loop. One pattern produces one instinct.
- Replace hardcoded `confidence: 0.9` with evidence-scaled confidence: `min(0.95, 0.5 + 0.1 * log2(event_count))`. A pattern with 7 events gets ~0.78; a pattern with 120 events gets ~0.89.
- Process all four high-signal event types, not just `workflow_pattern`. Each event type produces a different instinct category: `workflow` for patterns, `avoidance` for errors, `resolution` for fixes, `wiring` for connections.
- Group by pattern name AND context field to distinguish sub-variants.
- Corrected output: instead of 6 near-identical "use workflow_builder" instincts, the pipeline produces ~15-20 distinct instincts across four categories with varying confidence scores.

**Promotion stage fix:**

- Add a `promote-instinct.js` script that moves an instinct from `instincts/` to `evolved/` after human review. The script requires a reviewer comment and a confirmed relevance tag.
- Modify `/codify` to read from `evolved/` (curated) instead of `instincts/` (raw). Add a validation step that rejects instincts with duplicate descriptions or confidence scores that do not match their evidence count.
- Corrected output: `evolved/` contains 5-8 distinct, human-reviewed instincts. `/codify` incorporates only curated knowledge into agent behaviour.

## Scoring

| Criterion                                                                                  | Points |
| ------------------------------------------------------------------------------------------ | ------ |
| Input noise ratio correctly identified (~96% noise, ~4% high-signal)                       | 1      |
| At least two processing problems identified (hardcoded confidence + duplicates at minimum) | 2      |
| Processing audit goes beyond "capture better observations" to critique the script itself   | 1      |
| Promotion pathway broken at both points (empty evolved/ AND /codify bypasses it)           | 2      |
| End-to-end diagnosis names all three stages and explains why single-stage fixes fail       | 2      |
| Fix proposal addresses all three stages with concrete changes (not just "improve quality") | 2      |
| **Total**                                                                                  | **10** |

## Extensions

- **Variant A (harder):** The project lead pushes back: "We fixed the processing script to deduplicate and scale confidence. The pipeline now produces 4 distinct instincts with varying confidence. The learning system is fixed." The evolved directory is still empty. The `/codify` command still reads from raw instincts. Is the pipeline fixed? Explain what "fixed" means for a learning pipeline vs what "fixed" means for a processing script.
- **Variant B (metrics trap):** The team adds a dashboard that shows: "5,000 observations captured, 15 instincts generated (up from 6), 4 categories covered (up from 1), average confidence 0.82 (down from 0.9)." Every metric has improved. But `evolved/` is still empty and `/codify` still reads from `instincts/`. Design a metric that would expose the promotion-stage failure that the current dashboard hides.
