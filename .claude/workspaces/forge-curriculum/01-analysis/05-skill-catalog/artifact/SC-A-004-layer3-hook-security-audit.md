---
atom_id: SC-A-004
name: Layer 3 hook security audit
craft_layer: artifact
destination: both
craft_areas: [attend-how, intervene-when]
applications: [COC]
spec_lineage:
  - CO §3.3 — Safety Blindness
  - CO §19 — Layer 3 — Guardrails
journal_evidence:
  - path: loom/journal/0050-DISCOVERY-session-start-command-injection.md
    polarity: NEGATIVE
    quote: "CRITICAL: scripts/hooks/lib/version-utils.js:44 — execSync with URL from .claude/VERSION file. A crafted version_url with $(...) would execute arbitrary commands."
  - path: loom/journal/0007-GAP-co-completeness-audit.md
    polarity: NEGATIVE
    quote: "Many rules are global (noise dilutes signal). cc-artifacts own rules are violated (globs: vs paths:, agents over 400 lines). The enforcers don't follow their own rules."
practice_modality: case
status: draft
---

## The move

Audit Layer 3 hook code for the same classes of vulnerability those hooks are designed to prevent. Hooks run with elevated privilege (they execute outside the AI's context window, deterministically, on every session start or pre-commit). A command injection in a hook is worse than a command injection in application code because the hook runs unconditionally — it cannot be skipped by the AI and cannot be caught by the hook itself. Treat every hook as an attack surface, not just an enforcement surface.

## When it fires

Every red team pass that touches Layer 3 artifacts (hooks, scripts in `scripts/hooks/`). The trigger is not "the hook has changed" — it is "the hook exists." Hooks that have not been security-audited since their creation are the highest-risk artifacts in the system because they run unconditionally and with shell access.

## What it serves

CO §3.3 (Safety Blindness) defines the failure mode: "The AI optimizes for the most direct path to task completion. Safety measures are never the most direct path. The AI deprioritizes them unless explicitly enforced." Hooks are the explicit enforcement — but if the hooks themselves contain the vulnerabilities they are meant to prevent, the enforcement is self-defeating. CO §19 (Layer 3 — Guardrails) defines Layer 3 as operating "outside the AI's context window, independent of whether the AI 'remembers' the rule." This means Layer 3 code has no AI oversight at runtime. The only oversight is the human or agent who wrote and reviewed the hook — making pre-deployment security audit the sole defense.

## Evidence walkthrough

- **loom-meta 0050** (NEGATIVE) — A red team security review found a critical command injection in `scripts/hooks/lib/version-utils.js` at line 44: `execSync` called with a URL read from the `.claude/VERSION` file. A crafted `version_url` containing `$(...)` shell substitution would execute arbitrary commands on every session start. The fix was converting from `execSync` (shell interpretation) to `execFileSync` (no shell). This is the canonical case: a session-start hook — the highest-frequency, highest-privilege enforcement point — contained a command injection vulnerability.
- **loom-meta 0007** (NEGATIVE) — The CO completeness audit found that "cc-artifacts own rules are violated (globs: vs paths:, agents over 400 lines). The enforcers don't follow their own rules." This is the same pattern at the rule level: rules that enforce cc-artifacts quality standards were themselves violating those standards. When combined with the hook-level finding in 0050, it establishes a pattern — Layer 3 artifacts are the least-audited artifacts in the system because practitioners assume enforcement code is correct by construction.

## How to drill it

Hand the practitioner a real hook file (e.g., `session-start.js` or `validate-workflow.js` from any kailash repo) and a checklist of three vulnerability classes: (1) shell injection via `execSync` with untrusted input, (2) path traversal via unsanitized file paths, (3) denial of service via unbounded reads or missing timeouts. The drill: identify all instances where the hook reads external input (files, environment variables, git state) and trace each input to its consumption point. Score on: (a) every external input identified, (b) each consumption point classified as safe or unsafe with a one-sentence rationale, (c) at least one false-positive correctly dismissed (not all external inputs are dangerous). The loom-meta 0050 entry provides an answer key for the `version-utils.js` case.

## Common failure mode

Practitioner treats hooks as infrastructure, not application code. The hook was written once, works, and is never revisited. Red team passes focus on the code the hooks protect (application logic, API endpoints) and skip the hooks themselves. The result is that the highest-privilege code in the system receives the least security scrutiny. The cure is a standing red team checklist item: "Have the Layer 3 hooks been audited for the same vulnerability classes they enforce against?"

## Related atoms

- SC-A-003 (defense-in-depth codification) — defense-in-depth requires multiple enforcement surfaces; this atom ensures each enforcement surface is itself secure, not just present.
- SC-A-006 (negative tests for deny-by-default) — both atoms address the same meta-problem: enforcement mechanisms that are assumed to work but have never been tested for the failure case they are meant to prevent.
