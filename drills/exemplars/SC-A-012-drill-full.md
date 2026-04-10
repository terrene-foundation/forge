---
drill_id: DR-SC-A-012-FULL
source_atom: SC-A-012
difficulty: advanced
time_box_minutes: 45
modality: build
application: [COC]
---

# Author CLAUDE.md as Layer 2 Master Directive

## Setup

The practitioner receives a project directory containing a bloated `CLAUDE.md` (412 lines) and a `.claude/` tree with 14 subordinate artifacts. The project is a data pipeline platform called "Conduit" that processes financial transaction feeds.

**File: `CLAUDE.md` (412 lines, truncated to show structural problems)**

```markdown
# Conduit — Financial Transaction Pipeline Platform

Conduit is a high-performance data pipeline platform built on the Kailash SDK that processes
financial transaction feeds from multiple banking partners. It validates, enriches, and routes
transactions to downstream systems.

## About This Project

Conduit was started in Q3 2025 as a replacement for the legacy batch processing system. It uses
Kailash DataFlow for database operations, Kailash Nexus for API deployment, and Kailash Core SDK
for workflow orchestration. The team consists of 3 engineers and 1 data analyst. The previous system
processed ~50K transactions/day; Conduit targets 500K/day with sub-second latency.

(... 40 more lines of project history and motivation ...)

## Code Style

All Python code MUST:

- Use absolute imports (e.g., `from conduit.models import Transaction`)
- Follow PEP 8 with 120-character line limit
- Use type hints on all function signatures
- Use `ruff` for formatting and linting

All commits MUST follow conventional commit format:

- `feat(scope): description` for new features
- `fix(scope): description` for bug fixes
- `docs(scope): description` for documentation

(... 25 more lines restating git.md content ...)

## Security Requirements

- All API keys MUST use environment variables, never hardcoded
- All database queries MUST use parameterised queries via DataFlow
- All user input MUST be validated before processing
- No `eval()` on any input
- No secrets in log output
- .env files in .gitignore

(... 20 more lines restating security.md content ...)

## Testing

We use 3-tier testing:

- Tier 1 (Unit): Mocking allowed, fast, <1s per test
- Tier 2 (Integration): Real database, no mocking
- Tier 3 (E2E): Real everything, state persistence verification

Coverage: 80% general, 100% for financial calculations.

(... 30 more lines restating testing.md content ...)

## DataFlow Models

Conduit uses the following DataFlow models:

- `Transaction` — core transaction record
- `Partner` — banking partner configuration
- `Feed` — inbound feed registration
- `ValidationRule` — configurable validation rules
- `EnrichmentSource` — external data sources for enrichment

(... 35 more lines of model descriptions ...)

## API Endpoints

- `POST /api/feeds` — Register a new transaction feed
- `POST /api/feeds/{id}/ingest` — Submit transactions for processing
- `GET /api/transactions` — Query processed transactions
- `GET /api/health` — System health check

(... 20 more lines of endpoint documentation ...)

## Architecture

Conduit has three pipeline stages:

1. Ingestion — accepts raw transaction feeds via Nexus API
2. Processing — validates and enriches via Core SDK workflows
3. Routing — dispatches to downstream systems via configurable rules

(... 60 more lines of architecture narrative ...)

## Deployment

Deployed via Docker Compose. PostgreSQL for storage. Redis for caching.

(... 40 more lines of deployment instructions ...)

## Team Conventions

When reporting progress, describe outcomes not implementation:
✅ "Transactions from Partner X now process in under 200ms"
❌ "Refactored the validation pipeline to use async batch processing"

(... 15 more lines restating communication.md content ...)

## Known Issues

1. Feed registration occasionally times out under heavy load
2. Enrichment sources with >10s latency are not retried
3. The partner onboarding workflow has a race condition on concurrent registrations

## Changelog

### v0.4.0 (2026-03-15)

- Added enrichment pipeline
- Fixed partner timeout issue
  ...
```

**`.claude/` directory listing:**

```
.claude/
  agents/
    pipeline-specialist.md       — specialist for DataFlow pipeline patterns
    validation-specialist.md     — specialist for transaction validation rules
  rules/
    security.md                  — security constraints (parameterised queries, no secrets)
    git.md                       — conventional commits, branch naming
    testing.md                   — 3-tier testing policy, coverage minimums
    financial-accuracy.md        — rounding rules, decimal precision, reconciliation
    communication.md             — outcome-focused reporting
    zero-tolerance.md            — no stubs, no placeholders
  skills/
    dataflow-patterns/SKILL.md   — DataFlow connection pool, model registration patterns
    nexus-deployment/SKILL.md    — Nexus multi-channel deployment patterns
    feed-processing/SKILL.md     — domain skill for transaction feed processing
  commands/
    ingest.md                    — /ingest command for running feed ingestion pipeline
    reconcile.md                 — /reconcile command for end-of-day reconciliation
```

## Task

1. **Identify restatements.** Read the existing CLAUDE.md and the subordinate artifacts. For every section in CLAUDE.md that restates content already present in a subordinate artifact, record: the section name, the subordinate artifact it duplicates, and the line count wasted on the restatement.

2. **Identify missing pointers.** List every subordinate artifact (agent, rule, skill, command) that exists in `.claude/` but is NOT referenced or indexed anywhere in the current CLAUDE.md. These are invisible artifacts — an agent reading only CLAUDE.md would never discover them.

3. **Rewrite CLAUDE.md as a master directive.** The rewritten file must:
   - Be under 200 lines
   - Name the project's identity and purpose in 3 sentences or fewer
   - List absolute directives (non-negotiable constraints specific to this project)
   - Index every subordinate artifact by purpose (not restate its content)
   - Include a rules classification table showing which rules are critical vs advisory
   - Include a commands table with purpose and invocation
   - Contain zero restated rules — every constraint must appear in exactly one place

4. **Verify completeness.** After rewriting, perform a completeness check: read only your new CLAUDE.md and answer these three questions without looking at any other file: (a) What agents exist and what do they specialise in? (b) What rules constrain this project and which are critical? (c) What commands are available and when should each be used? If any question cannot be answered, the directive is incomplete.

## Model Answer

**1. Restatement inventory:**

| CLAUDE.md Section        | Subordinate Artifact     | Lines Wasted |
| ------------------------ | ------------------------ | ------------ |
| Code Style (git portion) | `rules/git.md`           | ~25          |
| Security Requirements    | `rules/security.md`      | ~20          |
| Testing                  | `rules/testing.md`       | ~30          |
| Team Conventions         | `rules/communication.md` | ~15          |

Total lines wasted on restatements: ~90 lines (22% of the 412-line file). These 90 lines consume always-in-context tokens every turn while adding zero information the agent does not already receive from the rules system.

**2. Missing pointers:**

| Artifact                          | Type    | Missing From CLAUDE.md                                 |
| --------------------------------- | ------- | ------------------------------------------------------ |
| `agents/pipeline-specialist.md`   | Agent   | Not mentioned — agent is invisible                     |
| `agents/validation-specialist.md` | Agent   | Not mentioned — agent is invisible                     |
| `rules/financial-accuracy.md`     | Rule    | Not mentioned — rule loads silently but is not indexed |
| `rules/zero-tolerance.md`         | Rule    | Not mentioned — rule loads silently but is not indexed |
| `skills/dataflow-patterns/`       | Skill   | Not mentioned — skill is orphaned                      |
| `skills/nexus-deployment/`        | Skill   | Not mentioned — skill is orphaned                      |
| `skills/feed-processing/`         | Skill   | Not mentioned — skill is orphaned                      |
| `commands/ingest.md`              | Command | Not mentioned — user cannot discover it                |
| `commands/reconcile.md`           | Command | Not mentioned — user cannot discover it                |

9 of 14 subordinate artifacts are invisible. An agent reading only CLAUDE.md would know about code style, security, testing, and communication — but would not know that specialist agents exist, that domain skills are available, or that workflow commands can be invoked.

**3. Rewritten CLAUDE.md (148 lines):**

```markdown
# Conduit — Financial Transaction Pipeline Platform

Data pipeline platform built on Kailash SDK. Processes financial transaction
feeds from banking partners: validates, enriches, and routes to downstream
systems. PostgreSQL storage, Redis caching, Docker Compose deployment.

## Absolute Directives

### 1. Financial Accuracy is Non-Negotiable

All monetary calculations use `decimal.Decimal` with explicit rounding.
No floating-point arithmetic on currency values. Every transaction must
reconcile to zero residual. See `rules/financial-accuracy.md` for the
full precision and rounding specification.

### 2. Framework-First

All database operations through Kailash DataFlow. All API endpoints through
Kailash Nexus. All orchestration through Kailash Core SDK workflows.
No raw SQL, no custom HTTP servers, no manual orchestration.

### 3. Feed Processing Pipeline Order

Ingestion → Validation → Enrichment → Routing. No stage may be skipped.
Validation failures reject the transaction; enrichment failures degrade
gracefully (transaction proceeds with partial enrichment, flagged for
manual review).

## Agents

| Agent                   | Specialty                                                      | When to Delegate                                            |
| ----------------------- | -------------------------------------------------------------- | ----------------------------------------------------------- |
| `pipeline-specialist`   | DataFlow pipeline patterns, connection pools, batch processing | Any work touching the ingestion or routing pipeline         |
| `validation-specialist` | Transaction validation rules, partner-specific constraints     | Any work on validation logic or adding new validation rules |

## Rules

| Rule                    | Concern                                             | Classification                       |
| ----------------------- | --------------------------------------------------- | ------------------------------------ |
| `financial-accuracy.md` | Decimal precision, rounding, reconciliation         | **Critical** — financial correctness |
| `security.md`           | Parameterised queries, no secrets, input validation | **Critical** — data protection       |
| `zero-tolerance.md`     | No stubs, no placeholders, fix what you find        | **Critical** — code quality          |
| `testing.md`            | 3-tier testing, 100% coverage on financial paths    | **Critical** — financial correctness |
| `git.md`                | Conventional commits, branch naming                 | Required                             |
| `communication.md`      | Outcome-focused reporting                           | Advisory                             |

## Skills

| Skill                | Domain                                        | Load Trigger                                    |
| -------------------- | --------------------------------------------- | ----------------------------------------------- |
| `dataflow-patterns/` | DataFlow connection pools, model registration | On-demand when working with database layer      |
| `nexus-deployment/`  | Multi-channel API deployment patterns         | On-demand when deploying or modifying endpoints |
| `feed-processing/`   | Transaction feed ingestion, partner formats   | On-demand when working with feed pipeline       |

## Commands

| Command      | Purpose                         | When to Use                                        |
| ------------ | ------------------------------- | -------------------------------------------------- |
| `/ingest`    | Run the feed ingestion pipeline | Processing new transaction feeds from partners     |
| `/reconcile` | End-of-day reconciliation check | After daily processing completes, before reporting |

## DataFlow Models

| Model              | Purpose                              |
| ------------------ | ------------------------------------ |
| `Transaction`      | Core transaction record              |
| `Partner`          | Banking partner configuration        |
| `Feed`             | Inbound feed registration            |
| `ValidationRule`   | Configurable validation rules        |
| `EnrichmentSource` | External data sources for enrichment |

## API Surface

| Endpoint                 | Method | Purpose                            |
| ------------------------ | ------ | ---------------------------------- |
| `/api/feeds`             | POST   | Register a new transaction feed    |
| `/api/feeds/{id}/ingest` | POST   | Submit transactions for processing |
| `/api/transactions`      | GET    | Query processed transactions       |
| `/api/health`            | GET    | System health check                |

## Known Issues

1. Feed registration occasionally times out under heavy load
2. Enrichment sources with >10s latency are not retried
3. Partner onboarding workflow has a race condition on concurrent registrations
```

**4. Completeness verification:**

- (a) _What agents exist?_ Pipeline-specialist (DataFlow pipeline patterns) and validation-specialist (transaction validation rules). Both have clear delegation triggers. **PASS.**
- (b) _What rules constrain this project?_ Six rules, four critical (financial-accuracy, security, zero-tolerance, testing), one required (git), one advisory (communication). **PASS.**
- (c) _What commands are available?_ `/ingest` for feed processing, `/reconcile` for end-of-day checks. Both have clear "when to use" guidance. **PASS.**

The rewritten CLAUDE.md is 148 lines (vs 412 original), contains zero restated rules, and indexes all 14 subordinate artifacts. The token saving is ~264 lines of always-in-context budget freed for conversation.

## Scoring

| Criterion                                                                        | Points |
| -------------------------------------------------------------------------------- | ------ |
| All restatements identified with subordinate artifact and line count             | 2      |
| All missing artifact pointers identified (at least 8 of 9)                       | 1      |
| Rewritten CLAUDE.md is under 200 lines                                           | 1      |
| Identity statement is 3 sentences or fewer and names the repo's actual function  | 1      |
| Absolute directives are project-specific constraints, not restated generic rules | 1      |
| Every subordinate artifact (agent, rule, skill, command) is indexed with purpose | 2      |
| Rules classification table distinguishes critical from advisory                  | 1      |
| Completeness check passes — all three questions answerable from CLAUDE.md alone  | 1      |
| **Total**                                                                        | **10** |

## Extensions

- **Variant A (token budget audit):** After rewriting, estimate the token cost of the original vs rewritten CLAUDE.md. Use a rough heuristic of 1 token per 4 characters. Calculate: (a) how many tokens the restatements wasted per conversation turn, (b) over a 20-turn conversation, how many total tokens were consumed by redundant content, (c) what conversation length increase the savings enable. This connects to the 0049 ablation experiment that measured the full COC artifact set at ~48K always-loaded tokens. The practitioner should arrive at the insight that CLAUDE.md size is a direct trade-off against conversation depth.
- **Variant B (multi-repo master directive):** The practitioner receives a second project ("Conduit-Admin" — an internal dashboard that consumes Conduit's API) with its own bloated CLAUDE.md. Both projects share the same rules (security, git, testing, communication, zero-tolerance) but have different agents, skills, and commands. The practitioner must write master directives for both projects that index the shared rules without duplicating their content, and explain how the shared rules are maintained (synced from a template, not copy-pasted). This tests whether the practitioner understands that the master directive is not just a single-repo concern but a node in a sync topology.
