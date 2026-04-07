---
case_id: CASE-05
source_path: loom/journal/0050-DISCOVERY-session-start-command-injection.md
title: "The Enforcers Must Obey Their Own Rules"
atoms_served: [layer3-hook-security-audit, defense-in-depth-self-audit]
spec_concepts: [CO §3.3, CO §19, CO §5, CARE §44]
craft_layer: artifact
recommended_use: teaching
destination: both
application: COC
---

# CASE-05: The Enforcers Must Obey Their Own Rules

## 1. The Situation

The COC template included session-start hooks -- JavaScript files that run automatically when a Claude Code session begins. These hooks enforce project invariants: checking SDK versions, validating environment state, verifying sync currency. They are Layer 3 guardrails: deterministic enforcement mechanisms that constrain agent behavior regardless of the agent's instructions or intentions.

The hooks were part of the security infrastructure. They ran with full shell access on the developer's machine. They were trusted implicitly because they were authored by the COC system itself.

## 2. The Trigger

A red team security review examined the hook files themselves -- not the code the hooks were designed to protect, but the enforcement code. Two command injection vulnerabilities were found:

**CRITICAL**: `scripts/hooks/lib/version-utils.js` at line 44 used `execSync` with a URL read from the `.claude/VERSION` file. A crafted `version_url` containing `$(...)` shell syntax would execute arbitrary commands when the hook ran on session start. Any repository with a malicious `.claude/VERSION` file -- for example, a cloned repository from an untrusted source -- could execute arbitrary code on the developer's machine the moment they opened a Claude Code session.

**HIGH**: `scripts/hooks/session-start.js` used `execSync` with a constructed Python path in a new `checkSdkPinFreshness()` function. Shell metacharacters in directory names could inject commands.

Both vulnerabilities existed in `execSync`, which passes its argument through a shell interpreter. The fix was converting both to `execFileSync`, which executes the command directly without shell interpretation, plus adding `isFinite()` validation on sync marker timestamps to prevent NaN bypass.

## 3. The Move

The move is **layer3-hook-security-audit** combined with **defense-in-depth-self-audit**. The key skill is applying the same security scrutiny to the enforcement infrastructure that the enforcement infrastructure applies to user code. The hooks contained rules about input validation and injection prevention. The hooks themselves violated those rules.

The practitioner treated the hook files as attack surface rather than trusted infrastructure. This required a perspective shift: hooks are not "our code that enforces safety" but "code that runs with elevated privilege and therefore requires elevated scrutiny."

## 4. The Outcome

Both vulnerabilities were fixed by replacing `execSync` (shell interpretation) with `execFileSync` (no shell). The timestamp validation was tightened with `isFinite()`. The fixes shipped in v1.5.0.

The deeper outcome is a principle: enforcement code must be held to a stricter standard than the code it enforces. A security rule that says "never use `exec` with unsanitized input" must itself not use `exec` with unsanitized input. If it does, the attack surface is worse than having no enforcement at all -- because the enforcement code runs automatically, on session start, with the developer's full permissions, and is never manually reviewed because it is "trusted."

## 5. The Spec Connection

**CO §3.3 (Safety Blindness)** is the central concept. The hooks were written to prevent safety failures in user code. The authors focused on what the hooks enforced and did not scrutinize the hooks' own implementation. §3.3 names this exact pattern: the most direct path to "enforcement exists" did not include "enforcement is itself safe." The safety blindness was meta-level -- blindness about the safety of the safety system.

**CO §19 (Layer 3 -- Guardrails)** defines the architectural layer where hooks live. Layer 3 is deterministic enforcement: hooks, rules, validation scripts. This case reveals that Layer 3 components are themselves subject to the same vulnerabilities they guard against. §19 establishes guardrails as a layer; this case establishes that the layer must be self-referentially secure.

**CO §5 (Deterministic Enforcement)** requires that critical behaviors be enforced by mechanisms that cannot be bypassed by agent intention. The command injection vulnerability bypassed deterministic enforcement not by subverting the agent but by subverting the enforcement mechanism itself. §5's guarantee holds only if the enforcement mechanism is itself sound.

**CARE §44 (Failure Modes)** catalogs the ways systems can fail. The relevant failure mode here is "trusted component becomes attack vector." The `.claude/VERSION` file is a configuration file that the hook trusts implicitly. CARE §44 says: enumerate failure modes including failures of trust assumptions. The VERSION file being a vector for command injection is a trust-assumption failure.

## 6. Discussion Questions

1. The command injection was exploitable via a malicious `.claude/VERSION` file in a cloned repository. In a world where developers routinely clone open-source repositories and run Claude Code sessions in them, how should the hook system handle configuration files that may have been authored by untrusted parties? Should all hook inputs be treated as adversarial by default?

2. If the red team had not examined the hook files themselves, the vulnerability would have persisted indefinitely -- because hooks are "infrastructure" and infrastructure is rarely re-audited. What triggers should cause enforcement infrastructure to be re-examined? Is there a way to make "audit the auditor" a routine part of the COC process rather than a one-off insight?

3. The fix was converting `execSync` to `execFileSync` -- a one-line change per call site. The vulnerability existed because the original author chose the convenient API (`execSync`) over the safe API (`execFileSync`). This is the same class of choice that the security rules warn user code about. Should COC hook authoring have its own, stricter set of rules? Or is the existing security rule sufficient if it is actually applied to hook code during review?
