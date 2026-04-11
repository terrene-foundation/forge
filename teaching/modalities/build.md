---
title: Build modality
date: 2026-04-10
destination: forge
modality: build
---

# Build

A build is a construction exercise. The practitioner produces a real, inspectable artifact end-to-end — a rule file, a hook script, an agent definition, a clean-room environment, a working CLAUDE.md section. The output is the evidence of skill; there is no "model answer" because the artifact itself is what gets evaluated.

## What a build is

A build has four elements:

1. **Brief** — what to construct. Specific enough to scope the work; open enough that there is no single correct solution.
2. **Constraints** — non-negotiable requirements the artifact must satisfy (length budgets, required sections, must-work-with-X compatibility).
3. **Construction** — the practitioner does the actual work. Not a prompt to the model, not pseudocode, not a plan. The artifact.
4. **Evaluation** — inspection of the finished artifact against the constraints, plus qualitative judgment on craft.

Unlike a drill, there is no single scoring rubric that applies to every build. Each build atom defines its own evaluation criteria in the "How to drill it" section.

## When to pick a build

Choose the build modality for an atom when:

- The atom teaches _how to author_ something — a rule, an agent, a hook, an architecture document.
- The skill cannot be demonstrated by answering questions; the practitioner must ship the thing.
- The output is valuable in itself — even a beginner's attempt is a real artifact someone could use.
- Multiple correct solutions exist, and the space of good craft is wide rather than single-path.

Do not pick a build when:

- There is a single correct answer (use `drill`).
- The value comes from interpreting a narrative (use `case`).
- The move must happen inside an active workflow (use `brokerage-rep`).

## How to run a build

**Brief phase:**

1. State the brief concretely. "Write a hook that blocks frontend mock data in React components, using the defense-in-depth pattern." Not "learn about hooks."
2. State the constraints. The hook must run in pre-commit, must not block production data, must produce a clear error message, must have at least two detection patterns.
3. State what tools the practitioner may use. Kailash framework yes. Manual shell scripting yes. Copying an existing hook verbatim no.

**Construction phase:** 4. The practitioner works. This is real authoring time, not a timed exercise. Budgets are long: 30 minutes to 2 hours for a typical build. 5. The facilitator does not walk through the solution. If the practitioner is stuck, the facilitator asks questions ("what does the detection pattern need to catch?"), not provides answers. 6. Reference material is allowed — existing atoms, existing hooks, the catalog. The facilitator should encourage reading reference material before construction, not discourage it.

**Evaluation phase:** 7. The finished artifact is inspected against the constraints. Each constraint is a hard check. 8. Beyond the constraints, craft is assessed: is the code clear? Are the detection patterns sharp? Does the error message guide the practitioner toward the fix? 9. The facilitator names one specific thing the practitioner did well and one specific thing to improve. Generic feedback ("good job") is not evaluation.

**Time box:** Build atoms typically need 45 minutes to a full session. Stacking multiple builds in one session is usually a mistake — build stamina is real and the second build suffers.

## What counts as complete

A build is complete when:

- A real artifact exists (not a plan to create the artifact).
- The artifact satisfies all stated constraints.
- The practitioner can explain their design choices.

A build is not complete when the practitioner has written pseudocode or a design document. The point of a build is producing the thing.

## Common failure modes

1. **Walking through the solution instead of letting the practitioner work.** Turns the build into a guided tour. The practitioner ends up understanding the facilitator's artifact, not authoring their own. Fix: set a long time box and leave the room.

2. **Starting from a blank page with no reference material.** Reference material is not cheating — good authoring builds on known patterns. The goal is for the practitioner to produce something _like_ the exemplar atoms, not to reinvent them. Fix: explicitly point to reference atoms at the start.

3. **Evaluating only the artifact, not the design choices.** Two practitioners may produce valid artifacts via different reasoning. The reasoning matters — a practitioner who chose their detection patterns deliberately has learned more than one who copied a pattern that happened to work. Fix: ask "why this?" for every significant choice.

4. **Stacking multiple builds in one session.** Build fatigue is real. The second build in a session is 30-50% lower quality than the first. Fix: one build per session; use drills and cases to fill the rest of the time.

5. **Accepting "I would do X" instead of actual work.** The defining feature of the build modality is that the artifact exists. Verbal descriptions of what the artifact would look like are not builds. Fix: no sign-off until the file exists.

## Assessment compatibility

Builds work extremely well for assessment. The artifact is inspectable, the constraints are verifiable, and the craft is judgable by anyone who knows the domain. A well-constructed build assessment should:

- Give a novel brief (not the one used for practice).
- Allow access to reference material but not to completed solutions.
- Be evaluated by someone other than the practitioner.
- Produce a written critique that names specific choices.

## Atoms using this modality

Currently ~6 atoms use the build modality, concentrated in the artifact layer:

- SC-A-002 Frontend mock data detection
- SC-A-012 CLAUDE.md as Layer 2 context
- SC-A-013 Agent specialization routing
- SC-A-015 Layer 2 context structuring
- SC-B-007 Worktree-per-agent
- SC-P-002 Model-vs-rule ablation (build a clean-room environment)

See `catalog/README.md` Index 4 (Practice Modality) for confirmation.
