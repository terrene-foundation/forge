---
case_id: CASE-25
source_path: loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md
title: "Timing Side-Channel in API Key Comparison"
atoms_served: [timing-sidechannel-detection, security-primitive-audit]
spec_concepts: [CO §49, CO §5, EATP §36, CO §19]
craft_layer: artifact
recommended_use: assessment
destination: both
application: COC
---

# CASE-25: Timing Side-Channel in API Key Comparison

## 1. The Situation

The Kailash Rust SDK's Nexus subsystem provides multi-channel deployment (API + CLI + MCP). The API channel supports API key authentication via `ApiKeyConfig`, which stores a set of valid key hashes and validates incoming requests against them. The implementation used `subtle::ConstantTimeEq` for each individual hash comparison -- the correct cryptographic primitive for preventing timing attacks on string comparison. The code appeared secure on inspection: constant-time comparison was used, keys were hashed, and the authentication pathway was standard.

The v3.8 release cycle included a post-implementation red team pass by a security-reviewer agent. The Nexus authentication code was one of many areas under review.

## 2. The Trigger

The security reviewer examined not just the comparison primitive but the iteration pattern around it. `ApiKeyConfig::validate_key()` used Rust's `.any()` iterator method over the stored key hashes. `.any()` is a short-circuiting iterator: it returns `true` as soon as any element satisfies the predicate and stops iterating. Each individual comparison was constant-time (via `subtle::ConstantTimeEq`), but the loop was not: when the matching key was at index 0, the function returned after one comparison; when it was at index N, it returned after N+1 comparisons.

The attentional skill is **timing-sidechannel-detection**: recognizing that constant-time primitives do not guarantee constant-time behavior at the call-site level. The security of the comparison depends on the entire execution path, not just the primitive. An attacker with multiple API keys registered in the system could measure response times to determine which key index matched, revealing which service or user identity the key belonged to. The finding was classified HIGH.

## 3. The Move

The move is **security-primitive-audit** -- verifying that a cryptographic primitive is used correctly in its surrounding context, not just that the correct primitive was chosen. The fix replaced `.any()` with a manual loop that always iterates ALL stored hashes, accumulating the result with bitwise OR. Regardless of whether the matching key is at index 0 or index N, the function always performs the same number of comparisons and the same number of bitwise operations. The timing profile becomes constant with respect to key position.

This is non-obvious because the code review surface is deceptive. A reviewer checking "is constant-time comparison used?" would see `subtle::ConstantTimeEq` and mark the check as passed. The vulnerability is not in the comparison but in the iteration. The `.any()` call is idiomatic Rust -- it is the natural, readable way to ask "does any element match?" The security flaw hides behind correct idiom and correct primitive selection. Finding it requires thinking about the timing behavior of the entire function, not just the comparison operation.

## 4. The Outcome

The fix was a small code change -- replacing a one-line `.any()` call with a multi-line loop using bitwise accumulation. The security impact was confined to multi-key configurations; single-key setups were not affected because the loop always had exactly one iteration regardless. The finding was caught during red team Round 1 and fixed before release. Had it shipped, the timing difference between index-0 and index-N matches would have been measurable in environments with multiple registered API keys, allowing an attacker to fingerprint which key (and thus which service identity) was making a request.

## 5. The Spec Connection

**CO §49 (Enforcement Reliability)** requires that enforcement mechanisms actually enforce what they claim to enforce. The `validate_key()` function claimed to perform secure key validation. The constant-time primitive enforced secure comparison at the byte level. But the short-circuiting iterator broke the enforcement at the function level -- the function's timing varied with the position of the matching key. §49 requires that enforcement be verified end-to-end, not just at the primitive level. A function that uses a secure primitive insecurely is an unreliable enforcer.

**CO §5 (Deterministic Enforcement)** requires that enforcement mechanisms produce consistent, predictable results. The timing side-channel made the function's behavior non-deterministic from a security perspective: the same correct key produced different timing profiles depending on its storage position. Deterministic enforcement in security contexts means constant observable behavior for all valid inputs, not just correct boolean outputs. The function returned the correct answer every time but leaked information through the time it took to compute that answer.

**EATP §36 (Graceful Degradation)** addresses how systems should behave when security properties are partially compromised. The timing side-channel did not compromise authentication itself -- no invalid key could pass validation. It compromised a secondary property: the confidentiality of which key matched. §36 asks whether the system degrades gracefully when one security layer is weakened. In this case, the answer was no: the information leaked (key identity) could enable targeted attacks against specific service accounts, escalating a minor timing leak into an identity-disclosure vulnerability.

**CO §19 (Layer 3 -- Guardrails)** defines the enforcement boundary that constrains agent and system behavior. API key validation sits squarely in Layer 3 -- it is a guardrail that determines who may access the system. §19 requires that guardrails be robust against adversarial probing. A timing side-channel is exactly the kind of adversarial probe that a guardrail must resist. The case shows that guardrail robustness requires auditing not just the logic (does it accept/reject correctly?) but the observable behavior (does it leak information through timing, error messages, or other channels?).

## 6. Discussion Questions

1. If the security reviewer had checked only "is constant-time comparison used?" and found `subtle::ConstantTimeEq`, the review would have passed. What audit checklist item would catch the iterator-level timing leak? Is "verify constant-time behavior at the function level, not just the primitive level" specific enough to be actionable, or does it require specialized security knowledge that a general-purpose red team may not have?

2. The `.any()` pattern is idiomatic Rust and appears in thousands of codebases. A linter rule that flags `.any()` on security-sensitive comparisons would produce false positives in non-security contexts. How should a COC artifact suite handle security patterns that conflict with language idiom? Should there be a security-specific rule that overrides the "use idiomatic code" guidance, or should the guidance be "use idiomatic code except in these enumerated security contexts"?

3. Single-key configurations were not vulnerable because the loop always had exactly one iteration. If the system had been designed with a "one key per service" policy, the timing side-channel would have been unexploitable. Should security architecture constrain configuration (e.g., "maximum one API key per endpoint") to reduce the attack surface, or does that sacrifice operational flexibility for a threat that was caught and fixed? Where is the boundary between "fix the code" and "constrain the configuration"?
