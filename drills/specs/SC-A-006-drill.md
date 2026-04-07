---
drill_id: DR-SC-A-006
source_atom: SC-A-006
name: "Negative tests for deny-by-default — drill"
craft_area: attend-how, intervene-how
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a minimal auth middleware implementation (15-20 lines) with a `deny_by_default` flag and a route-permission map. The implementation has the kailash-rs 0002 bug: both branches of the flag execute the same code. The drill: (a) write a test that exercises an unmapped route with `deny_by_default = true` and expects denial, (b) run the test and observe it fail, (c) fix the implementation, (d) run the test and observe it pass, (e) write a second negative test for an unknown role on a mapped route. Score on: (a) the first test is written before reading the implementation (test-first, not test-after), (b) the test asserts both the HTTP status (403) and the presence of a denial log entry, (c) the fix changes only the `deny_by_default = true` branch, not both branches.
