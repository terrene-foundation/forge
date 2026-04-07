---
drill_id: DR-SC-A-007
source_atom: SC-A-007
name: "Zero-config security audit — drill"
craft_area: attend-how, intervene-how
practice_modality: drill
difficulty: intermediate
time_box_minutes: 30
---

## How to drill it

Give the practitioner a framework module that offers a zero-config `start()` function. The drill: (a) list every network-accessible resource the default creates, (b) classify each as read-only or read-write, (c) classify each as localhost-only or network-reachable, (d) identify which security parameters were defaulted to permissive values, and (e) write the one-line change that makes the default deny-by-default without breaking the zero-config promise. Score on completeness of the exposure inventory and whether the proposed fix preserves the convenience API's one-line usability. The data-fabric-engine 0008 resolution (localhost + writes-disabled + production-requires-auth) is the answer key pattern.
