---
atom_id: SC-P-010
name: Red team CI, auth, and release processes for timing-dependent failures
craft_layer: practitioner
destination: forge
craft_areas: [attend-how]
applications: [COC]
spec_lineage:
  - CO §3.3 — Safety Blindness
  - CO §5 — Deterministic Enforcement Over Probabilistic Compliance
journal_evidence:
  - path: loom/kailash-py/workspaces/kailash/journal/0010-DISCOVERY-pypi-tag-push-race-condition.md
    polarity: NEGATIVE
    quote: "Session 5 claimed 4 packages were released to PyPI, but only kailash-kaizen 2.3.2 actually published. Three packages had tags created and pushed to GitHub but the `publish-pypi.yml` workflow never triggered for them."
  - path: loom/kailash-rs/workspaces/v3.8-issues/journal/0003-RISK-timing-sidechannel-apikey.md
    polarity: NEGATIVE
    quote: "`ApiKeyConfig::validate_key()` used `.any()` on the stored key hashes, which short-circuits on the first match. While each individual comparison used `subtle::ConstantTimeEq`, the iterator terminated early when a match was found at index 0 vs index N."
  - path: loom/kailash-rs/workspaces/sync-express/journal/0003-RISK-sync-transaction-drop.md
    polarity: NEGATIVE
    quote: "When wrapped in a sync context without an active tokio runtime, the Drop path silently fails -- the transaction is neither committed nor rolled back, potentially leaving the connection in a dirty state."
practice_modality: drill
status: draft
---

## The move

During red team, explicitly probe for timing-dependent failures across three categories: (1) CI/release race conditions -- what happens when two operations that are assumed to be sequential actually run concurrently? (2) Authentication timing side-channels -- does the time to validate a credential leak information about the credential's correctness? (3) Transaction and resource lifecycle drops -- when an operation is interrupted or its runtime context disappears, does cleanup happen correctly or does the system enter a dirty state? For each category, construct a concrete scenario where the timing assumption fails, then verify whether the system handles it or breaks silently.

## When it fires

Three trigger conditions:

1. A release or publish pipeline processes multiple artifacts in a single operation (multiple tags pushed, multiple packages published, multiple deploys triggered). The assumption that "each triggers independently" is the timing assumption to test.
2. An authentication or authorization path uses short-circuit evaluation (iterators with `.any()`, `.find()`, early `return`). The assumption that "constant-time comparison is sufficient" is wrong if the iteration itself is variable-time.
3. A resource with RAII/Drop semantics (transactions, connections, file handles) is used in a context where the runtime may not be available at Drop time (sync wrappers around async code, cross-thread transfers, signal handlers).

## What it serves

CO §3.3 (Safety Blindness) states that "the AI optimizes for the most direct path to task completion. Safety measures are never the most direct path." Timing analysis is a safety measure that is never on the direct path: the code works in testing (where operations are sequential, auth is fast, and runtimes are always available), and the timing failure only manifests in production conditions. CO §5 (Deterministic Enforcement Over Probabilistic Compliance) requires that critical rules be enforced by mechanisms operating outside the AI's context window. Timing-dependent failures are the class of bug that probabilistic compliance never catches -- the code looks correct on every read, and the failure only manifests under specific concurrent conditions.

## Evidence walkthrough

- **0010-DISCOVERY-pypi-tag-push-race-condition** (NEGATIVE) -- A session claimed 4 packages were released to PyPI. Only 1 actually published. The root cause: all 4 tags pointed to the same merge commit and were pushed in a single `git push --tags` operation. GitHub Actions triggered the publish workflow for only one of the concurrent tag pushes. The failure was silent -- no error, no notification, just 3 packages that appeared to be released but were not on PyPI. The prevention is procedural: push tags one at a time with a delay, or use `workflow_dispatch` as the primary mechanism. This is a CI race condition where the assumption "each tag push triggers an independent workflow" was wrong.
- **0003-RISK-timing-sidechannel-apikey** (NEGATIVE) -- API key validation used `subtle::ConstantTimeEq` for each individual comparison (correct), but wrapped it in `.any()` which short-circuits on the first match (incorrect). With multiple API keys configured, an attacker could determine which key index matched based on response timing, revealing which service or user the key belongs to. The fix replaced `.any()` with a manual loop that always iterates ALL stored hashes, accumulating the result with bitwise OR. This is an authentication timing side-channel where the constant-time primitive was used correctly but the iteration strategy negated its protection.
- **0003-RISK-sync-transaction-drop** (NEGATIVE) -- `DataFlowTransaction` implements RAII Drop semantics that call `Handle::try_current()` to roll back uncommitted transactions. In a sync wrapper context without an active tokio runtime, the Drop path silently fails: the transaction is neither committed nor rolled back, leaving the connection in a dirty state. The fix requires `DataFlowTransactionSync` to own an `Arc<Runtime>` so that Drop can use the owned runtime for rollback. This is a resource lifecycle failure where the cleanup mechanism depends on a runtime context that may not exist when cleanup fires.

## How to drill it

Give the practitioner a system description with three components: (1) a CI publish pipeline that processes 3 artifacts triggered by git tags, (2) an auth middleware that validates bearer tokens against a list of valid tokens using an iterator, and (3) a database transaction wrapper that uses Drop for rollback. Each component has a timing bug embedded:

1. The CI pipeline pushes all tags simultaneously (race condition).
2. The auth middleware uses `.find()` which short-circuits (timing side-channel).
3. The transaction Drop calls an async rollback from a sync context (runtime absence).

The practitioner must:

1. Identify the timing assumption in each component.
2. Construct a concrete scenario where the assumption fails (e.g., "attacker sends 1000 requests with key at index 0 vs index 5, measures median response time").
3. Propose a fix for each that eliminates the timing dependency (sequential tag push, constant-time iteration, owned runtime in Drop).
4. Describe how to test the fix (how do you verify that the race condition is resolved, that the iteration is constant-time, that Drop works without an ambient runtime?).

Score on whether all three timing bugs are found, whether the attack scenarios are concrete enough to be reproducible, and whether the fixes address the root cause (timing dependency) rather than the symptom (specific failure mode).

## Common failure mode

The practitioner reviews the code for logical correctness and concludes it is correct -- because it IS logically correct. The `.any()` call returns the right boolean. The Drop implementation calls the right rollback function. The tag push creates the right tags. The timing dimension is invisible to logical review. The API key entry is the sharpest example: every individual comparison was constant-time (the practitioner used the right primitive), but the iteration strategy negated the protection (the practitioner did not consider the timing behavior of the iterator itself).

## Related atoms

- SC-B-006 (multi-round red team to numeric convergence) -- timing bugs are the class of finding that often requires multiple red team rounds to surface, because the first round focuses on logical correctness.
- SC-A-004 (Layer 3 hook security audit) -- timing side-channels in auth are a security concern that hooks and guardrails should detect, not just red team passes.
- SC-P-003 (cross-feature interaction red team) -- timing failures often emerge from the interaction of two correct subsystems (e.g., correct constant-time comparison + correct iterator = incorrect timing behavior).
