---
drill_id: DR-SC-P-023
source_atom: SC-P-023
name: "Insert a gating workspace at integration seams rather than adding assertions to existing tests — drill"
craft_area: intervene-how
practice_modality: drill
difficulty: beginner
time_box_minutes: 15
---

## How to drill it

Present the practitioner with 4 workspaces: WS-A produces a data model, WS-B produces an event system, WS-C produces an API layer, and WS-D (downstream) builds a client against all three. Each of WS-A, WS-B, WS-C has passing tests. The practitioner is asked: "Is the plan ready for WS-D?" The correct answer is no: there is no integration gate that verifies A's data model is compatible with B's event payloads, B's events are routable through C's API, and C's API returns data in the shape D expects. The practitioner must: (1) identify the missing integration seams, (2) propose a gating workspace (not assertions within A, B, or C), (3) define what the gate tests (a reference app or integration test suite exercising all seams), and (4) specify that WS-D is blocked until the gate passes. Score on whether the practitioner adds a structural gate rather than sprinkling assertions into existing workspaces.
