# FORGE Scope, PACT as Fourth Standard, Dual-Destination Routing

## Scope

FORGE-program rules. Loaded every analysis, todo, implementation, and codify session.

## 1. Four Standards — CARE / EATP / CO / PACT

The Terrene Foundation standards stack is a **quartet**, not a trinity. All four are peers. FORGE teaches all four as first principles.

| Standard | What it is                                                                                                                                            | Canonical location                                                                                                                                                                    |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CARE** | Philosophy — Dual Plane, Mirror Thesis, Human-on-the-Loop, Constraint Envelopes, Knowledge Ledger, Cross-Functional Bridges                           | `terrene/foundation/docs/02-standards/care/`                                                                                                                                          |
| **EATP** | Protocol — Trust Lineage Chain, posture/operations, interoperability, conformance                                                                     | `terrene/foundation/docs/02-standards/eatp/`                                                                                                                                          |
| **CO**   | Methodology — 8 first principles, 5-layer architecture (Intent / Context / Guardrails / Instructions / Learning), process model, application template | `terrene/foundation/docs/02-standards/co/`                                                                                                                                            |
| **PACT** | Governance — D/T/R accountability grammar, operating envelopes, knowledge clearance, verification gradient                                            | Thesis at `02-standards/publications/PACT-Core-Thesis.md`; workspace at `terrene/foundation/workspaces/pact/`; **treated as the fourth standard** by FORGE and by the programme owner |

**Authority note:** The programme owner declares what is a standard. PACT is the fourth standard. The filesystem is a snapshot of the graduation process, not the source of truth about peer status.

### MUST

- Treat all four standards as peers. Every skill atom's spec lineage can cite any of CARE/EATP/CO/PACT with equal authority
- When spec evidence needs to be cited, cite the foundation path if the concept is in `02-standards/`, or the thesis path if the concept is only in `publications/`, and note the source type explicitly
- When terrene-naming.md still says "trinity (CARE + EATP + CO)", that naming rule is **stale** with respect to PACT's elevation — flag the discrepancy but do not silently edit template-synced rules from this repo

```
DO:    "PACT § Operating Envelopes (thesis, §4) defines four envelope dimensions
        that constrain agent action across Financial, Operational, Temporal, and
        Data Access axes. CARE § Constraint Envelopes (02-architecture/02) supplies
        the philosophical grounding."

DO NOT: "PACT is being considered as a fourth standard."  ← stale framing; it IS the fourth.
DO NOT: "The CARE/EATP/CO trinity defines..."            ← wrong arity; it's a quartet.
```

### MUST NOT

- Describe PACT as "being graduated", "workspace phase", "proposed fourth standard", or "thesis-level" — it is the fourth standard
- Describe the Foundation standards stack as a "trinity" — it is a quartet
- Hedge PACT's authority with filesystem-level caveats — the filesystem is catching up, the authority isn't contingent on it

**Why:** Prior sessions deferred to filesystem state over programme-owner authority and produced framings that said "PACT is being proposed" — which, if published, would undermine the authority the programme owner has already exercised. The four-standard framing is load-bearing for every downstream artifact and cannot be hedged.

## 2. CO Applications — All In Scope, Usage-Weighted

CO is the domain-agnostic base methodology. It applies to multiple domains. **All formalised applications are in scope for FORGE.** There is no "v1 = COC only" gate.

| Application          | Acronym | Status               | Usage weight    | Source                                                                                                                                                                                                 |
| -------------------- | ------- | -------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **CO for Codegen**   | **COC** | In scope — primary   | Heaviest        | `02-standards/publications/COC-Core-Thesis.md` (documented as thesis because COC is the _first_ domain application, predating the `applications/` directory convention; treated as a full application) |
| **CO for Research**  | **COR** | In scope — secondary | Second-heaviest | `02-standards/co/applications/co-for-research.md`                                                                                                                                                      |
| **CO for Education** | **COE** | In scope — tertiary  | Active          | `02-standards/co/applications/co-for-education.md`                                                                                                                                                     |
| CO for Compliance    | —       | In scope             | Lower           | `02-standards/co/applications/co-for-compliance.md`                                                                                                                                                    |
| CO for Finance       | —       | In scope             | Lower           | `02-standards/co/applications/co-for-finance.md`                                                                                                                                                       |
| CO for Governance    | —       | In scope             | Lower           | `02-standards/co/applications/co-for-governance.md`                                                                                                                                                    |
| CO for Learners      | —       | In scope             | Lower           | `02-standards/co/applications/co-for-learners.md`                                                                                                                                                      |

**Usage weighting** dictates build order, not inclusion. FORGE builds the COC layer first (heaviest, most evidenced), then the COR layer, then COE, then the rest. Each layer is usable as it completes.

### MUST

- Treat all seven applications as in scope. None are "deferred catalog stubs"
- Build skill atoms application-by-application, in usage-weight order: COC → COR → COE → the rest
- Tag every skill atom with which application(s) it is evidence for (an atom can be evidence for more than one)
- When teaching the 5-layer architecture, always show the layer generically (CO) AND the COC-specific instantiation at minimum; COR and COE instantiations come in their respective passes
- When COE is taught _as a CO application_, teach it directly — it is subject matter, not just pedagogy meta

```
DO:    "CO § Layer 2 Context is the institutional-knowledge layer. In COC it is
        rendered as CLAUDE.md + rules/ + skills/. In COR it is rendered as the
        institutional literature corpus + prior-work ledger. In COE it is rendered
        as the course context + learner history + assessment state."

DO NOT: "COE is meta to FORGE, not subject matter."        ← wrong, it's a real application
DO NOT: "v1 covers COC only; defer COR/COE as stubs."      ← wrong, all in scope
DO NOT: "COComp, COL are deferred."                        ← these don't exist, don't cite them
```

### MUST NOT

- Cite "COComp" (complexity) or "COL" (law) as CO applications — these were fabricated in a prior framing and do not exist
- Treat COE as _only_ meta-pedagogy — it is a real application with its own craft (the fact that FORGE uses COE pedagogy to teach itself is a separate observation about FORGE's production methodology, not a claim about COE being out-of-scope)
- Build the full catalog in parallel across all seven applications — build in usage-weight order to keep the rigor bar consistent
- Invent new application slots — new applications require a foundation `02-standards/co/applications/co-for-*.md` file to be added first

**Why:** The previous framing fabricated two applications (COComp, COL), missed three real ones (compliance, finance, learners), and deferred the three most-used applications (COC, COR, COE) as stubs. The result was a scope that was both too small (artificial v1=COC gate) and too wrong (non-existent applications cited alongside real ones). Usage-weighted layering gives rigor per layer without scope creep.

## 3. PACT Has Three Layers — Always Qualify

PACT as a standard has three layers of instantiation. Every reference in a FORGE artifact must qualify which layer is meant. This is a clarification rule, not a "three things in tension" rule — the three layers are coherent parts of one standard.

| Layer                 | What it is                                                                                                                                                                                        | Where it lives                                                                     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **PACT (spec)**       | The normative specification — D/T/R accountability grammar, operating envelopes, knowledge clearance, verification gradient                                                                       | `02-standards/publications/PACT-Core-Thesis.md` and `workspaces/pact/01-analysis/` |
| **PACT (primitives)** | The Kailash SDK primitives that implement PACT-spec concepts in code — D/T/R types, envelope decorators, clearance APIs                                                                           | `kailash-pact` Python package; also `loom/kailash-py/packages/kailash-pact/`       |
| **PACT (platform)**   | The open-source reference platform built on the primitives — the canonical end-to-end implementation of the standard. Named open-source counterpart to any commercial implementation (e.g. Aegis) | `terrene/contrib/pact` and related                                                 |

### MUST

- Qualify PACT on first use in every artifact: **PACT (spec)**, **PACT (primitives)**, or **PACT (platform)**
- When teaching governance theory → PACT (spec)
- When teaching code patterns → PACT (primitives)
- When teaching deployment / reference architecture → PACT (platform)
- When the three layers interact, spell out which layer does what

```
DO:    "PACT (spec) § Operating Envelopes defines four envelope dimensions.
        PACT (primitives) implements three of them via the @envelope decorator;
        the fourth is enforced at PACT (platform) admission control."

DO NOT: "PACT defines envelopes. Use PACT to enforce them."  ← which layer does which?
```

### MUST NOT

- Use bare "PACT" in a sentence that mixes two of the layers
- Treat the SDK primitives package and the reference platform as the same thing — they have different release cycles and audiences
- Refer to any commercial implementation as "the PACT implementation" — there is one normative spec, many implementations, and one open-source _reference_ (PACT platform)

**Why:** The three layers have collapsed into each other in past sessions, producing artifacts that say "PACT enforces X" when only the platform does, or "import PACT" when the spec is meant. Qualification is cheap up front and impossible to backfill later.

## 4. Dual-Destination Routing — Tag Every Output

Every FORGE output is tagged with one of three destinations:

| Tag          | Destination                   | Content type                                                                                                                  |
| ------------ | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `forge`      | `lyceum/programs/forge/` only | Practitioner craft (Layer 1) — uniquely FORGE, tied to the programme's specific pedagogy                                      |
| `co-codegen` | `atelier/co-codegen/` only    | Domain-agnostic methodology (artifact-authoring craft, brokerage/governance craft) that any CO ecosystem consumer should have |
| `both`       | Both repos                    | Content that lives in `co-codegen` as canonical and is referenced (not duplicated) from `forge`                               |

**Distill once, route both ways.** When research surfaces a brokerage move or artifact pattern, the canonical write-up goes to `co-codegen` even if FORGE is the only current consumer — because the next CO ecosystem programme will need it too.

### MUST

- Add a destination tag (`destination: forge | co-codegen | both`) to the frontmatter of every analysis report, synthesis doc, skill atom, drill, and case
- When in doubt: would a non-FORGE CO ecosystem consumer (e.g. an internal team adopting CO/COC/COR/COE for their own staff) want this exact artifact? If yes → `co-codegen` or `both`. If no → `forge`.
- Practitioner-craft content (uniquely-FORGE pedagogy moves, dual-plane classification as taught, mirror-thesis interventions as practised) → almost always `forge`
- Artifact-authoring craft (agent/skill/rule/hook/command authoring patterns) → almost always `co-codegen` or `both`
- Brokerage / governance / sync craft (authority chain enforcement, variant classification, sync moves) → almost always `co-codegen` or `both`

```
DO:    ---
       destination: both
       application: COC
       spec_lineage: [CO § Layer 3 Guardrails, PACT § Operating Envelopes]
       ---
       # Skill: Authoring a path-scoped rule with DO/DO NOT examples

DO NOT: # Skill: Authoring a rule
       (no destination, no application, no spec lineage — invisible to routing)
```

### MUST NOT

- Write content directly into `atelier/co-codegen/` from a FORGE session — produce it here with the tag, then route via the next sync pass
- Duplicate the same artifact in both repos as parallel copies — choose one canonical location and reference from the other
- Tag uniquely-FORGE pedagogy content as `co-codegen` (it would pollute the agnostic methodology repo with FORGE-specific framing)

**Why:** The CO ecosystem has at least three planned consumers of artifact-authoring and brokerage craft (FORGE, future internal team adopters, future external open-source contributors). Without dual-destination tagging, every artifact gets written specifically for FORGE and has to be re-extracted later. Tagging up front lets the same write-up serve all three consumers from day one.

## 5. Rigor Bar — "Worthy of a World-Class Standard"

FORGE artifacts are programme-owner-declared material that will sit alongside CARE / EATP / CO / PACT in the foundation's public presentation. The rigor bar is accordingly high.

### MUST

- Enumerate spec concepts concretely (not summarise). "CARE defines dual plane" is not enumeration. "CARE § 02-architecture/01-dual-plane states that the Trust Plane governs policy and the Execution Plane governs action, with cross-plane bridges defined in §02-architecture/03" is enumeration.
- Cite every craft claim with a verifiable path. Journal evidence = full repo-relative path to the entry. Spec lineage = full path to the spec file and the section.
- Read entries before citing them. No synthesising "from memory of what the corpus probably contains."
- Name gaps as gaps. If a spec concept has no journal evidence, write "no evidence in corpus as of YYYY-MM-DD" — do not pad with theory.
- Correct errors in the framing the moment they surface. Do not rationalise.

**Why:** Two sessions of prior work produced reports that were synthesised from the agent's model of the corpus rather than from actual reads, and the framing accreted fabrications (COComp, COL, "atelier as journal source") that went unchecked. That work is below the bar and is now debt. The rigor bar above is the guard against repeating it.
