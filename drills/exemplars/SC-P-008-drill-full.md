---
drill_id: DR-SC-P-008-FULL
source_atom: SC-P-008
difficulty: intermediate
time_box_minutes: 30
modality: case
application: [COC]
---

# Trace Deep Dependency Chains to Find Where a Single Broken Link Breaks the Install Path

## Setup

The practitioner receives a `pyproject.toml` for a hypothetical package called `kailash-learn` and a dependency tree showing the full transitive closure. Three fragility points are embedded in the tree.

**pyproject.toml** (relevant excerpt):

```toml
[project]
name = "kailash-learn"
version = "0.4.0"
requires-python = ">=3.10"
dependencies = [
    "polars>=0.38",
    "onnxruntime>=1.14",
    "tokenizers>=0.13",
]
```

**Dependency tree** (resolved by `pip install --dry-run` on x86_64 Linux):

```
kailash-learn 0.4.0
├── polars >=0.38
│   ├── polars-core 0.53.1 (binary wheel, all platforms)
│   └── polars-io 0.53.1 (binary wheel, all platforms)
├── onnxruntime >=1.14
│   ├── numpy >=1.24 (binary wheel, all platforms)
│   ├── protobuf >=3.20 (binary wheel, all platforms)
│   ├── sympy >=1.1 (pure Python)
│   └── flatbuffers >=23.1 (pure Python)
├── tokenizers >=0.13
│   ├── huggingface-hub >=0.16 (pure Python)
│   │   ├── requests >=2.28 (pure Python)
│   │   ├── filelock >=3.0 (pure Python)
│   │   ├── fsspec >=2023.5 (pure Python)
│   │   └── safetensors >=0.4 (binary wheel, all platforms)
│   └── rustworkx >=0.13 (requires Rust compiler for source build)
│       └── numpy >=1.24 (binary wheel, all platforms)
```

**Additional context** (provided to the practitioner as "package notes"):

- `tokenizers` v0.13.0 through v0.13.3 expose `tokenizers.Tokenizer.from_pretrained()` but do NOT expose `tokenizers.processors.TemplateProcessing`, which was added in v0.14.0. The `kailash-learn` codebase imports `TemplateProcessing` in its tokenization pipeline.
- `rustworkx` publishes binary wheels for x86_64 Linux, x86_64 macOS, and Windows. ARM Linux (aarch64-unknown-linux-gnu) and ARM macOS (aarch64-apple-darwin) require building from source, which needs a Rust toolchain installed.
- `rustworkx` is maintained by a single core contributor. The last 4 releases shipped at 2-month, 5-month, 8-month, and 11-month intervals respectively — an accelerating gap.

## Task

1. Trace the full dependency tree. List all 12 packages in the transitive closure. For each of the 3 direct dependencies (`polars`, `onnxruntime`, `tokenizers`), count the depth of its longest transitive chain.
2. Identify the three fragility points. For each, answer the three fragility questions:
   - (a) Is this dependency maintained by more than one person?
   - (b) Does it publish binary wheels for all target platforms?
   - (c) Has its API broken between the minimum pinned version and the current version?
3. Propose tightened version pins for the 3 direct dependencies. For each, state the new floor, the new ceiling, and the rationale for both bounds.
4. Describe a CI version-matrix test for this dependency set. Which combinations would you test, and why?

## Model Answer

**1. Transitive closure and chain depths:**

12 packages total: kailash-learn, polars, polars-core, polars-io, onnxruntime, numpy, protobuf, sympy, flatbuffers, tokenizers, huggingface-hub, requests, filelock, fsspec, safetensors, rustworkx. (16 unique packages including kailash-learn itself; 15 dependencies.)

Longest chain depths:

- `polars` chain: depth 2 (polars -> polars-core)
- `onnxruntime` chain: depth 2 (onnxruntime -> numpy)
- `tokenizers` chain: depth 3 (tokenizers -> huggingface-hub -> safetensors) and depth 3 (tokenizers -> rustworkx -> numpy)

The `tokenizers` subtree has the deepest chains and the most transitive dependencies (7 packages), making it the highest-risk branch for fragility.

**2. Three fragility points:**

| #   | Package                    | Fragility Question Failed                       | Risk                                                                                                                                                                                                                                                                                                            |
| --- | -------------------------- | ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | `tokenizers` (version pin) | (c) API broken between minimum and current      | `>=0.13` allows installing 0.13.0–0.13.3, which lack `TemplateProcessing`. The `kailash-learn` codebase imports `TemplateProcessing`, so any resolver that picks 0.13.x will produce an `ImportError` at runtime. This is the TRL pattern: the pin floor admits versions where the required API does not exist. |
| 2   | `rustworkx` (platform)     | (b) No binary wheels for ARM platforms          | ARM Linux and ARM macOS users must have a Rust toolchain installed. `pip install kailash-learn` will fail with a compilation error on these platforms unless the user has previously installed Rust. There is no error message explaining why — the user sees a wall of Rust compiler output.                   |
| 3   | `rustworkx` (maintenance)  | (a) Single maintainer with decelerating cadence | Last 4 release intervals: 2, 5, 8, 11 months. If a security vulnerability or API break is discovered in rustworkx, the expected response time is now measured in months, not weeks. A critical numpy incompatibility could block kailash-learn's numpy upgrade path for half a year.                            |

Note that `rustworkx` fails ALL three questions — single maintainer, missing ARM wheels, and the decelerating release cadence makes API stability unknowable. It is the highest-fragility node in the entire tree.

**3. Tightened version pins:**

| Dependency    | Current  | Proposed      | Rationale                                                                                                                                                                                                                                                                   |
| ------------- | -------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `polars`      | `>=0.38` | `>=0.50,<1.0` | Floor at 0.50 because kailash-learn uses the polars 0.50+ expression API (`pl.col().over()`). Ceiling at <1.0 because polars pre-1.0 releases have frequent breaking changes but 1.0 promises stability. 0.38 is 15 minor versions behind current; no reason to support it. |
| `onnxruntime` | `>=1.14` | `>=1.14,<2.0` | Floor at 1.14 is acceptable — the InferenceSession API has been stable since 1.14. Ceiling at <2.0 because a major version bump could change the session API. The current pin is nearly correct; only the ceiling is missing.                                               |
| `tokenizers`  | `>=0.13` | `>=0.14,<1.0` | Floor MUST be 0.14.0, not 0.13, because `TemplateProcessing` was introduced in 0.14.0. Any pin that admits 0.13.x will produce an ImportError. Ceiling at <1.0 for the same major-version stability reason.                                                                 |

**4. CI version-matrix test:**

```yaml
strategy:
  matrix:
    python-version: ["3.10", "3.12"]
    os: [ubuntu-latest, macos-latest, macos-14] # macos-14 = ARM
    deps:
      - name: "floor"
        polars: "==0.50"
        onnxruntime: "==1.14"
        tokenizers: "==0.14.0"
      - name: "ceiling"
        polars: "==0.53"
        onnxruntime: "==1.19"
        tokenizers: "==0.21"
      - name: "mixed"
        polars: "==0.50"
        onnxruntime: "==1.19"
        tokenizers: "==0.14.0"
```

Three dependency combinations:

- **Floor**: all dependencies at the minimum pinned version. Catches API-not-present failures (the tokenizers fragility).
- **Ceiling**: all dependencies at the latest version within the pin range. Catches deprecation and breaking-change failures.
- **Mixed**: floor of the tightest pins with ceiling of the loosest. Catches cross-dependency incompatibilities (e.g., old tokenizers with new numpy).

The ARM macOS runner (`macos-14`) is critical because it tests the `rustworkx` compilation path. If CI does not test ARM, the fragility is invisible until an ARM user reports it.

## Scoring

| Criterion                                                                                 | Points |
| ----------------------------------------------------------------------------------------- | ------ |
| All 12+ transitive dependencies enumerated with chain depths                              | 1      |
| Fragility 1 (tokenizers API gap) identified with specific version boundary (0.13 vs 0.14) | 2      |
| Fragility 2 (rustworkx ARM compilation) identified with specific platforms named          | 2      |
| Fragility 3 (rustworkx single maintainer / decelerating cadence) identified               | 1      |
| Version pins have both a justified floor AND a justified ceiling for all 3 dependencies   | 2      |
| CI matrix covers floor, ceiling, and at least one ARM platform                            | 2      |
| **Total**                                                                                 | **10** |

## Extensions

1. **Silent failure variant.** Replace the `TemplateProcessing` import with a function that exists in tokenizers 0.13 but changed its return type in 0.14 (returns `str` in 0.13, returns `Encoding` object in 0.14). The code calls `.ids` on the return value, which works in 0.14 but produces `AttributeError: 'str' object has no attribute 'ids'` in 0.13. The practitioner must trace a runtime failure (not an import failure) to a version-dependent API change — a harder diagnostic than a missing import.

2. **Vendor lock-in fragility.** Add a fourth direct dependency on a package that is maintained by a single company and has no drop-in alternative (e.g., a proprietary tokenizer backend). The fragility is not technical but strategic: if the company abandons the package, there is no migration path. The practitioner must propose a mitigation that differs from version pinning — either an abstraction layer, a vendored fork, or a compatibility shim.

3. **Cascading pin conflict.** After tightening the pins, the practitioner discovers that `tokenizers>=0.14` requires `huggingface-hub>=0.19`, but `safetensors>=0.4` (required by huggingface-hub) conflicts with the version of safetensors that `onnxruntime` vendors internally. The practitioner must resolve the conflict without loosening the tokenizers floor below 0.14.
