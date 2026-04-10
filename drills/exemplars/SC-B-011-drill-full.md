---
drill_id: DR-SC-B-011-FULL
source_atom: SC-B-011
difficulty: intermediate
time_box_minutes: 30
modality: brokerage-rep
application: [COC]
---

# Version Tracking Cascade Across the Repo Chain

## Setup

The practitioner manages a three-repo distribution chain used by the Kailash Python SDK ecosystem:

- **Source S** — the BUILD repo (`kailash-py`), where SDK code and COC artifacts are authored
- **Template T** — the USE template (`kailash-coc-claude-py`), which downstream projects scaffold from
- **Downstream D** — a project repo (`my-project`) that was scaffolded from T and receives sync updates

Each repo has two version-tracking mechanisms:

1. **`.claude/VERSION`** — a JSON file tracking COC artifact currency and upstream version
2. **`pyproject.toml`** — the SDK's release version (S only) or dependency pins (T and D)

**Source S state:**

```
# pyproject.toml
[project]
name = "kailash"
version = "2.3.0"

# .claude/VERSION
{
  "version": "1.5",
  "upstream": null,
  "sdk_version": "2.1.0",
  "updated": "2026-03-15"
}
```

Note: S's `pyproject.toml` says the SDK is at `2.3.0`, but the `.claude/VERSION` file's `sdk_version` field still says `2.1.0` — it was last stamped during a `/codify` run when the SDK was at `2.1.0` and has not been re-stamped since.

**Template T state:**

```
# pyproject.toml (dependency pins)
[project]
dependencies = [
  "kailash>=2.0.0",
]

# .claude/VERSION
{
  "version": "1.4",
  "upstream": "1.5",
  "sdk_version": "2.0.0",
  "updated": "2026-02-20"
}
```

T's VERSION knows that upstream (S) is at version `1.5` (it fetched this on last session start). T's own version is `1.4` — it has not synced yet. T's dependency pin says `kailash>=2.0.0`, which is stale relative to S's actual SDK version of `2.3.0`.

**Downstream D state:**

```
# pyproject.toml (dependency pins)
[project]
dependencies = [
  "kailash>=2.0.0",
]

# .claude/VERSION
{
  "version": "1.3",
  "upstream": "1.4",
  "sdk_version": "2.0.0",
  "updated": "2026-01-10"
}
```

D is one version behind T and two versions behind S. D's dependency pin matches T's (both `>=2.0.0`), but both are stale relative to S's current SDK version.

## Task

1. Enumerate all version mismatches in the chain. For each mismatch, state which two values disagree, which repo holds each value, and why the mismatch matters.

2. Walk through **trigger point 1 — session start on D**. What should the session-start check surface? What does the practitioner see, and what action should it prompt?

3. Walk through **trigger point 2 — `/codify` on S** after implementing a feature that uses a new API introduced in SDK `2.3.0`. What should the `/codify` command stamp into the proposal? What is the risk if it stamps the VERSION file's current `sdk_version` (`2.1.0`) instead of the actual SDK version (`2.3.0`)?

4. Walk through **trigger point 3 — `/sync` from S through T to D**. What should Gate 1 check? What should Gate 2 update in T's `pyproject.toml`? What does D receive at the end of the chain?

5. The developer on D says: "I'm still on SDK 2.0.0 and my tests pass — why should I care about the version cascade?" Explain what will break and when.

## Model Answer

**1. Version mismatches (six total):**

| #   | Mismatch                                    | Repo A value         | Repo B value            | Why it matters                                                                                                                                                                         |
| --- | ------------------------------------------- | -------------------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | S: VERSION.sdk_version vs pyproject.toml    | sdk_version: 2.1.0   | pyproject.toml: 2.3.0   | S's artifacts were stamped at 2.1.0 but the SDK has since added APIs in 2.2.x and 2.3.0; any /codify run now produces artifacts that may reference 2.3.0 APIs but are stamped as 2.1.0 |
| 2   | S: VERSION.version vs T: VERSION.upstream   | S version: 1.5       | T upstream: 1.5 (match) | This one actually matches — T knows S is at 1.5. No mismatch.                                                                                                                          |
| 3   | T: VERSION.version vs T: VERSION.upstream   | T version: 1.4       | T upstream: 1.5         | T has not synced to S's latest artifacts — T is one version behind                                                                                                                     |
| 4   | T: dependency pin vs S: pyproject.toml      | T pin: >=2.0.0       | S actual: 2.3.0         | T's pin allows SDK 2.0.0, but artifacts being synced from S may use 2.3.0 APIs                                                                                                         |
| 5   | D: VERSION.version vs D: VERSION.upstream   | D version: 1.3       | D upstream: 1.4         | D has not synced to T's latest artifacts — D is one version behind T                                                                                                                   |
| 6   | D: VERSION.sdk_version vs S: pyproject.toml | D sdk_version: 2.0.0 | S actual: 2.3.0         | D's artifacts were authored against SDK 2.0.0; the current SDK is 2.3.0 with breaking API changes                                                                                      |

**2. Trigger point 1 — session start on D:**

The session-start check fetches T's `.claude/VERSION` (via GitHub raw URL with a 3-second timeout) and compares:

- **D.version (1.3) < D.upstream (1.4):** D has not synced to the version it already knows about. The check should surface: _"Your COC artifacts are at version 1.3 but your upstream template is at 1.4. Run /sync to update."_
- **D.upstream (1.4) < T.version.actual (1.4):** These match — D's record of upstream is current with T's actual version. (But T itself is behind S, which D cannot see directly.)
- **D.sdk_version (2.0.0) vs installed SDK:** If D has `kailash==2.0.0` installed, this is consistent. If D has upgraded to `kailash==2.2.0` locally, the session start should flag: _"Your COC artifacts were authored for SDK 2.0.0 but you have SDK 2.2.0 installed. Some artifacts may reference deprecated patterns."_

The session-start check surfaces the D-level mismatches but cannot see through to S. That is by design — D's broker is T, not S.

**3. Trigger point 2 — `/codify` on S:**

After implementing a feature using a `2.3.0` API, `/codify` creates a proposal manifest (`.claude/.proposals/latest.yaml`). The proposal must stamp:

```yaml
sdk_version: "2.3.0" # from pyproject.toml, NOT from .claude/VERSION
coc_version: "1.6" # incremented from current 1.5
authored_against: "kailash==2.3.0"
```

**The risk of stamping 2.1.0:** If `/codify` reads the VERSION file's `sdk_version` field (2.1.0) instead of reading `pyproject.toml` directly, the proposal claims compatibility with SDK 2.1.0. When `/sync` propagates this to T and then to D, Gate 1 sees `sdk_version: 2.1.0` and compares it against T's pin of `>=2.0.0` — no conflict detected. But the artifacts actually use 2.3.0 APIs (e.g., a new node type, a new parameter on `DataFlow.__init__`). D, still on SDK 2.0.0, receives artifacts that reference APIs it does not have. The failure is silent until D's agent tries to use a skill that invokes the 2.3.0 API, at which point it gets an `ImportError` or `AttributeError` with no indication that the version cascade is the root cause.

The fix is: `/codify` must always read `pyproject.toml` (or `Cargo.toml`) for the SDK version, not the cached `sdk_version` in VERSION. Then it must also update VERSION's `sdk_version` field to match.

**4. Trigger point 3 — `/sync` from S through T:**

**Gate 1 (S → T):** The sync reads S's proposal and checks:

- Proposal `sdk_version` (should be 2.3.0 if stamped correctly) against T's current `sdk_version` (2.0.0). The gap of 2.0.0 → 2.3.0 is flagged for review: _"These artifacts were authored against SDK 2.3.0 but the template currently targets SDK 2.0.0. Verify backward compatibility or update the template's minimum pin."_
- Proposal `coc_version` (1.6) against T's `version` (1.4). Two-version jump is informational.

**Gate 2 (update T):** After merging artifacts, Gate 2 must:

1. Update T's `.claude/VERSION`: `version: 1.6`, `sdk_version: 2.3.0`
2. Read S's `pyproject.toml` to get all Kailash package versions
3. Update T's `pyproject.toml` dependency pins: `kailash>=2.3.0` (from `>=2.0.0`)
4. Update any workspace-member dependency pins if applicable

**End of chain — what D receives:** When D next runs `/sync` from T, it receives:

- Artifacts authored against SDK 2.3.0
- T's VERSION showing `version: 1.6`, `sdk_version: 2.3.0`
- A Gate 1 flag: _"These artifacts require SDK 2.3.0 but your project pins kailash>=2.0.0."_
- D must update its own `pyproject.toml` to `kailash>=2.3.0` and run `pip install --upgrade kailash` before the new artifacts are safe to use.

**5. Why the developer on D should care:**

"Your tests pass today because your current COC artifacts were authored against SDK 2.0.0 — they use only 2.0.0 APIs. The moment you sync to the latest artifacts, those artifacts will reference SDK 2.3.0 APIs that do not exist in your installed SDK. Your agents will fail at runtime with import errors or attribute errors, and the error messages will not mention the version mismatch — they will say 'module has no attribute X' and you will debug the wrong thing. The version cascade exists to surface this mismatch at sync time (a controlled checkpoint) rather than at runtime (an uncontrolled failure). Upgrade the SDK before you sync, or pin your sync to T's version 1.4 until you are ready to upgrade."

## Scoring

| Criterion                                                                         | Points |
| --------------------------------------------------------------------------------- | ------ |
| All six mismatches enumerated with correct values and repos identified            | 2      |
| Session-start check correctly scoped to D's visible horizon (T, not S)            | 1      |
| /codify stamp reads pyproject.toml not VERSION; risk of wrong stamp explained     | 2      |
| Gate 1 and Gate 2 actions are specific (not generic "check versions")             | 2      |
| End-of-chain consequence for D is concrete (pin update + SDK upgrade required)    | 1      |
| Developer explanation names the specific failure mode (runtime, not compile-time) | 1      |
| Three version tracks (COC artifact, SDK release, dependency pin) explicitly named | 1      |
| **Total**                                                                         | **10** |

## Extensions

- **Variant A (harder):** S has two workspace members: `kailash` at `2.3.0` and `kailash-dataflow` at `1.8.0`. T's `pyproject.toml` pins both. A `/codify` run on S produces artifacts that use both packages. Walk through how Gate 2 handles multiple dependency pins and what happens if only one is updated.
- **Variant B (Rust):** Replace the Python chain with a Rust chain (S = `kailash-rs`, T = `kailash-coc-claude-rs`, D = a Rust project). The version source is `Cargo.toml` instead of `pyproject.toml`, and dependency pins use `kailash = ">=2.3.0"` syntax. Identify any differences in how the cascade operates with Cargo's version resolution vs pip's.
