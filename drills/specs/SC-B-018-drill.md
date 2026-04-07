---
drill_id: DR-SC-B-018
source_atom: SC-B-018
name: "Separate the policy decision point from the policy enforcement point in access control — drill"
craft_area: intervene-how
practice_modality: brokerage-rep
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Present the practitioner with a governance module that has a single `authorize_and_execute(action, role)` function. The function checks the role's envelope, checks temporal constraints, and if all pass, executes the action. Ask the practitioner to (1) identify the PDP and PEP boundaries within the merged function, (2) refactor into two modules — a `PolicyDecider.evaluate(action, role) -> Verdict` and an `Enforcer.enforce(verdict, action)`, and (3) write a test for the PDP that verifies a blocked verdict is returned for an out-of-envelope action without any side effects from execution. Score: does the practitioner produce a PDP that returns a data structure (not a raised exception) and a PEP that is the only call site for the actual action? Does the refactored PEP handle all four verdict types (auto-approved, flagged, held, blocked)?
