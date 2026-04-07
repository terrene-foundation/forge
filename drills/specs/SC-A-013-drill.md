---
drill_id: DR-SC-A-013
source_atom: SC-A-013
name: "Design agent specialisation routing as explicit Layer 1 Intent — drill"
craft_area: intervene-how, institutional-memory
practice_modality: build
difficulty: advanced
time_box_minutes: 45
---

## How to drill it

Give the practitioner a system with 4 agents (e.g., code-agent, test-agent, review-agent, deploy-agent) and 10 request types (e.g., "write a function," "run the test suite," "review this PR," "deploy to staging," "add a dependency," "fix a failing test," "refactor this module," "update documentation," "triage this issue," "create a release"). The drill: (a) write a routing table that maps each request type to exactly one agent, (b) identify any request types that do not cleanly map to one agent and propose either splitting the request or adding a specialist, (c) write the routing table as an authored artifact (agent description frontmatter or a dedicated routing rule file). Score on completeness (all 10 types covered), uniqueness (no type mapped to two agents), and the quality of the practitioner's handling of ambiguous cases (e.g., "fix a failing test" could route to code-agent or test-agent — the practitioner must decide and justify).
