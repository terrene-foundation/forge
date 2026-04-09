---
case_id: CASE-15
source_path: loom/journal/0040-DECISION-python-venv-enforcement.md
title: "Rule Plus Hook"
atoms_served: [rule-plus-hook-combination, soft-to-hard-enforcement-escalation]
spec_concepts: [CO §5, CO §5.1, CO §5.2, CO §19]
craft_layer: artifact
recommended_use: teaching
destination: both
application: COC
---

# CASE-15: Rule Plus Hook

## 1. The Situation

Python projects across the Kailash ecosystem were using the machine's global Python interpreter for testing and development. Developers ran `pytest` or `python` directly, relying on whatever packages happened to be installed globally. Tests that passed locally with global Python could fail in CI, which used a clean environment with only the declared dependencies. The mismatch was invisible during development -- everything appeared to work -- and only surfaced as CI failures that were difficult to diagnose because the developer could not reproduce them locally.

No enforcement mechanism existed. There was an informal convention that projects should use virtual environments, but nothing prevented a developer (or an agent) from running global Python. The convention was known but not enforced.

## 2. The Trigger

The practitioner identified that the convention-to-enforcement gap was the root cause of recurring CI failures. The informal "use a venv" convention had been communicated but was routinely bypassed -- not out of malice but because the global Python path was the path of least resistance. The trigger was recognizing that a convention without enforcement is a suggestion, and suggestions do not prevent failures.

The attentional pattern is **enforcement-gap detection**: the practitioner looked at the gap between the stated policy ("use venv") and the actual behavior ("use whatever Python is on PATH") and asked what artifact combination would close the gap.

## 3. The Move

The move is **rule-plus-hook-combination**: the practitioner created two artifacts that work together to enforce the constraint at different points in the workflow.

First, a rule file (`loom/.claude/rules/python-environment.md`) was created as a global rule, path-scoped to `*.py`, `pyproject.toml`, and `tests/`. The rule states that all Python projects must use `.venv` at the project root managed by `uv`, that global Python is blocked, and that all Python operations must use `uv run`. The rule is the declarative component -- it tells agents what the constraint is and why it exists.

Second, a session-start hook was added to `loom/scripts/hooks/session-start.js` with a venv check block. The hook detects whether a `.venv` directory exists at the project root and warns when it is missing or stale. The hook is the runtime component -- it checks the constraint at the start of every session rather than relying on the agent to remember the rule.

The skill is in recognizing that neither artifact alone is sufficient. A rule without a hook relies on the agent reading and remembering the rule across all sessions. A hook without a rule enforces behavior without explaining the rationale, making it impossible for agents (or humans) to reason about exceptions. Together, the rule provides the "why" and the hook provides the "when."

## 4. The Outcome

The two artifacts together closed the enforcement gap. The rule ensures that any agent working with Python files knows the constraint exists and understands the rationale (CI reproducibility). The session-start hook ensures that the constraint is checked at the beginning of every session, catching stale or missing virtual environments before any Python code runs. The journal notes that the hook currently warns rather than blocks (exit code 0 rather than exit code 2), leaving the open question of whether it should escalate to a hard block.

The decision was documented as a DECISION journal entry, capturing the rationale and the artifacts created. The journal's discussion section raises the escalation question explicitly: should the venv check block the session (exit code 2) instead of warning? This is an example of the **soft-to-hard-enforcement-escalation** pattern -- start with a warning to validate the check does not produce false positives, then escalate to a block once confidence is established.

## 5. The Spec Connection

**CO §5 (Guardrails)** states that constraints must be enforced automatically rather than relying on manual compliance. The global Python convention was a constraint without a guardrail -- it relied on developers and agents remembering to activate a virtual environment. The rule-plus-hook combination converts the convention into an enforced guardrail: the rule declares the constraint and the hook checks it automatically at session start.

**CO §5.1 (Declarative Guardrails)** distinguishes between imperative enforcement (code that blocks a specific action) and declarative enforcement (a rule that states a constraint for reasoning agents to follow). The rule file is a declarative guardrail -- it does not block global Python at the filesystem level but tells agents that global Python is not permitted and explains why. This works because COC agents read rules as part of their context loading.

**CO §5.2 (Runtime Guardrails)** covers enforcement that runs at specific lifecycle points. The session-start hook is a runtime guardrail -- it executes at the beginning of every session and checks whether the environment conforms to the declared constraint. Runtime guardrails catch violations that declarative guardrails miss (an agent that did not load the rule, or a new session that inherited a stale environment).

**CO §19 (Convention Codification)** addresses the process of turning informal conventions into formal artifacts. The move from "we should use venvs" (informal convention) to a rule file plus a session-start hook (formal artifacts) is a direct instance of convention codification. The principle says that conventions left informal will drift; this case shows the specific mechanism (rule plus hook) that prevents the drift.

## 6. Discussion Questions

1. The hook currently warns (exit code 0) rather than blocks (exit code 2) when `.venv` is missing. What criteria would you use to decide when to escalate from warning to blocking? Is there a principled threshold (e.g., "after N sessions with zero false positives"), or should the escalation be a human decision?

2. The rule is path-scoped to `*.py`, `pyproject.toml`, and `tests/`. What happens if a Python file exists outside these paths (e.g., a script in `scripts/` or a notebook in `notebooks/`)? Is the path scope too narrow, or does it correctly capture the enforcement boundary?

3. If you had only one artifact slot -- either a rule or a hook, but not both -- which would you choose for this constraint? What class of violations would each miss on its own?

4. The rule specifies `uv` as the package manager. If a project legitimately needs a different environment manager (e.g., `conda` for a data science project with native dependencies), how should the rule handle the exception? Should the exception live in the rule, in the hook, or in the project's own configuration?
