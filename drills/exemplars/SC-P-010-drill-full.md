---
drill_id: DR-SC-P-010-FULL
source_atom: SC-P-010
difficulty: beginner
time_box_minutes: 15
modality: drill
application: [COC]
---

# Red Team CI, Auth, and Release Processes for Timing-Dependent Failures

## Setup

The practitioner receives descriptions of three system components. Each description includes a code sketch and a one-line summary of the component's purpose. Each component contains a timing bug that is invisible to logical review — the code produces correct outputs but has a timing-dependent failure mode.

**Component 1 — Multi-Package Release Pipeline:**

```yaml
# .github/workflows/publish.yml
name: Publish to PyPI
on:
  push:
    tags: ["v*"]

jobs:
  publish:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        package: [core, dataflow, kaizen]
    steps:
      - uses: actions/checkout@v4
      - run: cd packages/${{ matrix.package }} && python -m build
      - run: twine upload dist/*
```

Release process (documented in RELEASING.md):

```bash
# Tag all three packages and push simultaneously
git tag -a core-v2.3.0 -m "Release core 2.3.0"
git tag -a dataflow-v1.3.0 -m "Release dataflow 1.3.0"
git tag -a kaizen-v0.6.0 -m "Release kaizen 0.6.0"
git push --tags  # pushes all three tags in one operation
```

**Component 2 — API Key Validation Middleware:**

```rust
pub struct ApiKeyConfig {
    valid_keys: Vec<HashedKey>,
}

impl ApiKeyConfig {
    pub fn validate(&self, candidate: &str) -> bool {
        let candidate_hash = hash_key(candidate);
        self.valid_keys
            .iter()
            .any(|stored| constant_time_eq(&candidate_hash, &stored.hash))
    }
}
```

Summary: "Each comparison uses constant-time equality. The middleware is safe against timing attacks."

**Component 3 — Sync Transaction Wrapper:**

```rust
pub struct SyncTransaction {
    inner: AsyncTransaction,
}

impl Drop for SyncTransaction {
    fn drop(&mut self) {
        if !self.inner.is_committed() {
            // Roll back uncommitted transaction on drop
            if let Ok(handle) = tokio::runtime::Handle::try_current() {
                handle.block_on(self.inner.rollback());
            }
        }
    }
}
```

Summary: "RAII pattern ensures uncommitted transactions are rolled back when the wrapper goes out of scope."

## Task

1. For each component, identify the **timing assumption** — the implicit assumption about temporal ordering or duration that the code depends on for correctness.
2. For each component, construct a **concrete failure scenario** — a specific sequence of events or attacker behaviour that violates the timing assumption and produces an observable failure.
3. For each component, propose a **fix** that eliminates the timing dependency rather than mitigating the symptom.
4. For each fix, describe **how to verify** that the fix works — what test or observation would confirm the timing dependency is eliminated?

## Model Answer

**Component 1 — Multi-Package Release Pipeline:**

**Timing assumption:** Each tag push triggers an independent GitHub Actions workflow run. Three tags pushed simultaneously will produce three independent publish jobs.

**Failure scenario:** `git push --tags` sends all three tags to GitHub in a single network operation. GitHub Actions receives three `push` events pointing to the same commit SHA within milliseconds. The Actions scheduler may coalesce the events, trigger only one workflow run (for whichever tag event it processes first), or trigger three runs that race for the same checkout. In the journal evidence (loom-meta 0010), only 1 of 4 packages actually published. The other 3 had tags on GitHub but were never uploaded to PyPI. No error was raised — the release appeared complete from the git history.

**Fix:** Push tags sequentially with a delay, or use `workflow_dispatch` as the primary publish mechanism:

```bash
# Sequential push with verification
for pkg in core dataflow kaizen; do
    git push origin ${pkg}-v${VERSION}
    sleep 10  # wait for workflow trigger
    gh run watch  # verify workflow started
done
```

**Verification:** After each tag push, poll the GitHub Actions API (`gh run list --workflow=publish.yml`) and confirm that a run was created for the specific tag. The release is not complete until all three runs reach the "completed/success" state. Automate this as a post-release check script that compares the set of tags pushed against the set of successful PyPI uploads.

---

**Component 2 — API Key Validation Middleware:**

**Timing assumption:** `constant_time_eq` makes the validation timing-safe. The assumption is that constant-time comparison is sufficient for timing-attack prevention.

**Failure scenario:** `.any()` short-circuits on the first `true` result. If `valid_keys` contains 5 keys and the candidate matches key at index 0, the iterator runs 1 comparison. If it matches key at index 4, the iterator runs 5 comparisons. Each comparison is constant-time, but the NUMBER of comparisons varies. An attacker with network access sends 10,000 requests and measures the median response time. If one key consistently returns ~1 unit faster than another, the attacker knows the match is at a lower index — revealing which service account or user the key belongs to, even without learning the key itself.

**Fix:** Replace `.any()` with a manual loop that always iterates ALL stored keys, accumulating the result with bitwise OR:

```rust
pub fn validate(&self, candidate: &str) -> bool {
    let candidate_hash = hash_key(candidate);
    let mut matched: u8 = 0;
    for stored in &self.valid_keys {
        matched |= constant_time_eq(&candidate_hash, &stored.hash) as u8;
    }
    matched != 0
}
```

**Verification:** Write a benchmark that measures validation time with a key matching at index 0 vs index N-1. Run 10,000 iterations of each. The standard deviation of the timing difference between the two positions should be within noise (< 1 microsecond). With the old `.any()` code, the difference will be proportional to the number of keys skipped.

---

**Component 3 — Sync Transaction Wrapper:**

**Timing assumption:** A tokio runtime will be available when `Drop` fires, because the `SyncTransaction` was created inside a tokio context and `Drop` runs in the same context.

**Failure scenario:** The `SyncTransaction` is created inside a `tokio::runtime::Runtime::block_on()` call. The owning function returns, and the `SyncTransaction` is moved into a struct that outlives the runtime block. When the struct is dropped — in a non-async context, during program shutdown, or on a different thread — `Handle::try_current()` returns `Err` because no tokio runtime is active. The `if let Ok(handle)` silently swallows the error. The transaction is neither committed nor rolled back. The database connection is returned to the pool in a dirty state. The next user of that connection inherits uncommitted writes or locks.

**Fix:** `SyncTransaction` must own an `Arc<Runtime>` so that Drop can use the owned runtime regardless of ambient context:

```rust
pub struct SyncTransaction {
    inner: AsyncTransaction,
    runtime: Arc<tokio::runtime::Runtime>,
}

impl Drop for SyncTransaction {
    fn drop(&mut self) {
        if !self.inner.is_committed() {
            self.runtime.block_on(self.inner.rollback());
        }
    }
}
```

**Verification:** Create a `SyncTransaction` inside a runtime block, then move it to a plain `std::thread::spawn` thread with no tokio runtime. Drop it there. Verify that the rollback executes (check the database for the absence of the uncommitted write). With the old code, the uncommitted write persists in the database after Drop.

## Scoring

| Criterion                                                                                                 | Points |
| --------------------------------------------------------------------------------------------------------- | ------ |
| Component 1: timing assumption identifies tag coalescing / concurrent trigger, not a logical bug          | 1      |
| Component 1: failure scenario is specific (N tags pushed, M workflows triggered, silent partial publish)  | 1      |
| Component 2: timing assumption identifies iterator short-circuit, not the comparison primitive            | 2      |
| Component 2: attack scenario includes measurable observable (response time difference per key index)      | 1      |
| Component 3: timing assumption identifies runtime absence at Drop time                                    | 1      |
| Component 3: failure scenario traces the consequence to dirty connection state, not just "rollback fails" | 1      |
| All three fixes eliminate the timing dependency at root cause, not symptom                                | 2      |
| At least 2 of 3 verification methods are concrete and testable (not "add more logging")                   | 1      |
| **Total**                                                                                                 | **10** |

## Extensions

1. **Fourth component — cache stampede.** Add a caching layer where the cache TTL and the background refresh interval are configured independently. The timing bug: if the refresh takes longer than the TTL, the cache expires before the refresh completes, and all concurrent requests hit the backing store simultaneously. The practitioner must identify the timing assumption (refresh completes before expiry), construct a load scenario that triggers the stampede, and propose a fix (probabilistic early expiry or a lock-based refresh).

2. **Composition variant.** Combine components 1 and 3: a release pipeline that publishes a package with a database migration. The tag push triggers a publish job AND a migration job. The migration job runs a `SyncTransaction` inside a GitHub Actions runner (where the tokio runtime availability depends on the job's execution environment). The practitioner must trace the interaction of both timing bugs — the migration may or may not run depending on tag coalescing, and if it runs, the transaction cleanup may fail due to runtime absence.
