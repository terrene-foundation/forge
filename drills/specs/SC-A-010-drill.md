---
drill_id: DR-SC-A-010
source_atom: SC-A-010
name: "Token budget — baked-in vs external boundary decision — drill"
craft_area: institutional-memory
practice_modality: case
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with the kaizen-cli-rs 0011 data as a case study. The drill: (a) given the full list of 20+ external rules from the entry, classify each as "bake" (universal, personality-level, cacheable) or "keep external" (project-specific, user-editable, domain-scoped), (b) estimate the resulting token budget for each category, (c) identify which rules are in the cacheable prefix vs dynamic suffix under the current architecture, and (d) calculate the per-session cost difference between the current all-external model and the proposed hybrid. The entry's own classification (zero-tolerance, communication, agents rules as bake candidates; project CLAUDE.md and agent/skill definitions as external) is the answer key. Score on whether the practitioner's boundary matches the three-axis test (project-variance, cache-position, editability-need).
