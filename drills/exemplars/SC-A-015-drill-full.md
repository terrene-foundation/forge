---
drill_id: DR-SC-A-015-FULL
source_atom: SC-A-015
difficulty: advanced
time_box_minutes: 45
modality: build
application: [COC]
---

# Structure Layer 2 Institutional Knowledge Corpus

## Setup

The practitioner has just completed a `/codify` session on a Kailash-based inventory management platform ("Atlas"). The session produced 12 pieces of new institutional knowledge that need to be placed into the `.claude/` tree. The practitioner receives the 12 knowledge items below, plus the current `.claude/` directory listing showing what already exists.

**Current `.claude/` tree:**

```
.claude/
  agents/
    inventory-specialist.md
    reporting-specialist.md
  rules/
    security.md
    git.md
    zero-tolerance.md
  skills/
    01-core-sdk/SKILL.md
    02-dataflow/SKILL.md
  commands/
    ws.md
    wrapup.md
```

**The 12 knowledge items from the `/codify` session:**

| #   | Knowledge Item                                                                                                                                                                                                                     | Raw Description                                                                                                                    |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| K1  | "All inventory mutations must go through the `InventoryLedger` workflow — no direct database writes to the `stock_levels` table"                                                                                                   | Discovered when a direct SQL UPDATE caused a phantom stock discrepancy                                                             |
| K2  | "The DataFlow bulk upsert pattern for inventory syncs: use `BulkUpsertNode` with `conflict_key=['sku', 'warehouse_id']` and `update_columns=['quantity', 'last_synced']`"                                                          | Optimised pattern that reduced sync time from 45s to 3s for 10K SKU batches                                                        |
| K3  | "The reporting-specialist agent should also handle export jobs (CSV, Excel) because export formatting shares the same query infrastructure as reports"                                                                             | Discovered during an export feature where we built a separate export pipeline before realising reports already had the query layer |
| K4  | "The `/reconcile` command runs the end-of-day inventory reconciliation workflow with a convergence gate — it compares physical counts against ledger state and flags discrepancies above threshold"                                | New workflow command needed for daily operations                                                                                   |
| K5  | "Kailash ML's anomaly detection engine can identify unusual stock movements (e.g., sudden drops not explained by sales orders)"                                                                                                    | Reference knowledge about an available Kailash capability, not a project-specific constraint                                       |
| K6  | "Never trust external supplier quantity feeds without validation — supplier feed data must be validated against purchase order expectations before updating inventory"                                                             | Discovered when a supplier sent a feed with quantities 100x larger than ordered, nearly corrupting the ledger                      |
| K7  | "The `AuditTrailNode` must be wired into every inventory mutation workflow to capture who changed what, when, and why — this is a compliance requirement"                                                                          | Required for SOX compliance; the audit trail is non-negotiable                                                                     |
| K8  | "When the inventory-specialist encounters a stock discrepancy, it should run the reconciliation workflow before suggesting manual corrections — automated reconciliation resolves 80% of discrepancies without human intervention" | Behavioural guidance for the inventory-specialist agent's decision-making                                                          |
| K9  | "The warehouse location hierarchy follows a 4-level structure: Region > Warehouse > Zone > Bin. All inventory queries must specify at least the warehouse level to avoid full-table scans"                                         | Performance constraint discovered during load testing                                                                              |
| K10 | "Run the full test suite after any change to the `InventoryLedger` workflow — this workflow has cross-cutting effects on stock levels, audit trails, and reporting"                                                                | Testing guidance specific to one critical workflow                                                                                 |
| K11 | "The `/sync-suppliers` command pulls the latest supplier catalog and pricing data from all configured supplier APIs, validates against purchase order history, and updates the supplier reference tables"                          | New workflow command for supplier data synchronisation                                                                             |
| K12 | "Kailash Nexus supports webhook delivery for inventory events — use `NexusWebhookNode` with `event_type='inventory.mutation'` to notify downstream systems of stock changes in real time"                                          | Reference knowledge about a Kailash capability applicable to this project                                                          |

## Task

1. **Classify each item.** For each of the 12 knowledge items, assign it to exactly one artifact type: **rule**, **skill**, **agent** (update to existing agent description), or **command**. For each classification, write: (a) the artifact type, (b) the load trigger (always / on-demand / delegation / slash-invocation), (c) a one-sentence justification citing the load semantics that determine the placement.

2. **Identify misplacements.** Three of the 12 items have a "tempting wrong answer" — a plausible artifact type that would be incorrect based on load semantics. For at least two items, name the tempting wrong type and explain why it is wrong in one sentence.

3. **Write the placement plan.** For each item, specify the exact file path where it should land. For items that update existing files, name the file and describe what to add. For items that create new files, provide the filename and a 2-3 sentence description of the file's content.

4. **Build three of the artifacts.** Choose one rule, one skill addition, and one command from your classification. Write each artifact in full — not a summary or outline, but the actual file content that would go into `.claude/`. The rule must use MUST/MUST NOT structure with DO/DO NOT examples. The skill must include a practical code example. The command must include frontmatter describing its purpose and invocation.

5. **Verify load semantics.** After completing the placement plan, answer: if all 12 items were placed as rules (the "dump everything in rules" anti-pattern), how many always-in-context tokens would be wasted on knowledge that only needs to be loaded on demand? Estimate using a rough heuristic of 100 tokens per knowledge item placed as a rule versus 0 always-in-context tokens for the same item placed as a skill or command.

## Model Answer

**1. Classification table:**

| #   | Item                                      | Type               | Load Trigger     | Justification                                                                                                                                                                         |
| --- | ----------------------------------------- | ------------------ | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| K1  | InventoryLedger-only mutations            | **Rule**           | Always           | Behavioural constraint that must fire on every turn — any code change that touches `stock_levels` must be caught immediately, not when a skill happens to be loaded                   |
| K2  | DataFlow bulk upsert pattern              | **Skill**          | On-demand        | Reference pattern for a specific operation (inventory sync). Only needed when the practitioner is working on sync logic, not on every turn                                            |
| K3  | Reporting-specialist owns exports         | **Agent** (update) | Delegation       | Modifies the reporting-specialist's scope — the agent description must reflect that exports route to this agent, not a separate pipeline                                              |
| K4  | `/reconcile` command                      | **Command**        | Slash-invocation | Workflow phase with explicit invocation — the user runs `/reconcile` at end of day, not something that fires automatically                                                            |
| K5  | Kailash ML anomaly detection              | **Skill**          | On-demand        | Domain reference about an available capability. Not a constraint (rule) and not a workflow phase (command). Loaded when the practitioner considers anomaly detection                  |
| K6  | Validate supplier feeds                   | **Rule**           | Always           | Behavioural constraint that must fire whenever supplier feed code is touched — the consequence of violation (ledger corruption) is severe enough to warrant always-loaded enforcement |
| K7  | AuditTrailNode compliance                 | **Rule**           | Always           | Non-negotiable compliance constraint. Every inventory mutation workflow must include audit trail wiring. This must be visible on every turn because any new workflow could violate it |
| K8  | Inventory-specialist reconciliation-first | **Agent** (update) | Delegation       | Behavioural guidance for a specific agent — how the inventory-specialist should respond to discrepancies. Only relevant when that agent is active                                     |
| K9  | Warehouse hierarchy query constraint      | **Rule**           | Always           | Performance constraint that applies to all inventory queries. Must be visible whenever query code is written to prevent full-table scans                                              |
| K10 | Test suite after InventoryLedger changes  | **Skill**          | On-demand        | Testing guidance specific to one workflow. Not a universal constraint (would be a rule if it applied to all workflows). Loaded when the practitioner works on the ledger              |
| K11 | `/sync-suppliers` command                 | **Command**        | Slash-invocation | Workflow phase with explicit invocation — the user runs `/sync-suppliers` to pull supplier data                                                                                       |
| K12 | Nexus webhook for inventory events        | **Skill**          | On-demand        | Reference knowledge about a Kailash capability. Only needed when the practitioner is implementing event notification, not on every turn                                               |

**2. Tempting misplacements:**

- **K2 (bulk upsert pattern) tempting as a rule:** The practitioner might think "we should ALWAYS use BulkUpsertNode for syncs" and encode it as a rule. But the pattern is a how-to reference, not a constraint. A rule would say "MUST use batch operations for syncs > 100 rows." The BulkUpsertNode configuration details (conflict keys, update columns) are reference material loaded on demand, not a constraint enforced every turn. Placing it in a rule wastes always-in-context tokens on implementation details.

- **K10 (test after ledger changes) tempting as a rule:** The practitioner might encode this as a rule since it sounds like a constraint ("you MUST run tests after changing X"). But it is specific to one workflow, not universal. The existing `rules/zero-tolerance.md` and testing rules already mandate testing. K10 is supplementary guidance about which test suite is most important for a specific component — that is reference knowledge (skill), not a new behavioural constraint (rule). As a rule, it would fire on every turn even when the practitioner is nowhere near the ledger.

- **K8 (inventory-specialist reconciliation-first) tempting as a rule:** It sounds like a constraint ("always reconcile before suggesting manual corrections"). But the scope is a single agent's behaviour. Encoding it as a rule makes it visible to all agents on every turn, wasting context. Encoding it in the agent description makes it visible only when that agent is delegated to — which is the only time the knowledge is relevant.

**3. Placement plan:**

| #   | Type    | File Path                                       | Action                                                                                                                                                               |
| --- | ------- | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| K1  | Rule    | `.claude/rules/inventory-integrity.md`          | **Create.** Rule enforcing InventoryLedger-only mutations. MUST/MUST NOT with DO/DO NOT examples showing the forbidden direct SQL path vs the correct workflow path. |
| K2  | Skill   | `.claude/skills/02-dataflow/SKILL.md`           | **Append.** Add a "Bulk Upsert for Inventory Sync" section with the BulkUpsertNode configuration pattern and the performance comparison.                             |
| K3  | Agent   | `.claude/agents/reporting-specialist.md`        | **Update.** Add export jobs (CSV, Excel) to the agent's description and routes. Note that exports share query infrastructure with reports.                           |
| K4  | Command | `.claude/commands/reconcile.md`                 | **Create.** Command definition for `/reconcile` with frontmatter, purpose, and workflow steps.                                                                       |
| K5  | Skill   | `.claude/skills/34-kailash-ml/SKILL.md`         | **Create.** Skill covering Kailash ML anomaly detection for inventory, with example configuration.                                                                   |
| K6  | Rule    | `.claude/rules/inventory-integrity.md`          | **Append to K1 file.** Add a section on supplier feed validation as a second constraint in the same rule file.                                                       |
| K7  | Rule    | `.claude/rules/inventory-integrity.md`          | **Append to K1 file.** Add a section on mandatory AuditTrailNode wiring for compliance.                                                                              |
| K8  | Agent   | `.claude/agents/inventory-specialist.md`        | **Update.** Add behavioural guidance: run reconciliation workflow before suggesting manual corrections for discrepancies.                                            |
| K9  | Rule    | `.claude/rules/inventory-integrity.md`          | **Append to K1 file.** Add warehouse hierarchy query constraint (must specify at least warehouse level).                                                             |
| K10 | Skill   | `.claude/skills/12-testing-strategies/SKILL.md` | **Create or append.** Add guidance on InventoryLedger cross-cutting test requirements.                                                                               |
| K11 | Command | `.claude/commands/sync-suppliers.md`            | **Create.** Command definition for `/sync-suppliers` with frontmatter, purpose, and workflow steps.                                                                  |
| K12 | Skill   | `.claude/skills/03-nexus/SKILL.md`              | **Create or append.** Add Nexus webhook pattern for inventory mutation events with NexusWebhookNode configuration.                                                   |

**4. Three built artifacts:**

**Rule — `.claude/rules/inventory-integrity.md`:**

````markdown
# Inventory Integrity Rules

## 1. Ledger-Only Mutations

All inventory stock level changes MUST go through the `InventoryLedger`
workflow. No direct database writes to the `stock_levels` table.

```
DO: runtime.execute(inventory_ledger_workflow.build(
    sku="SKU-1234", warehouse_id="WH-01",
    quantity_delta=-5, reason="sales_order_1001"))

DO NOT: dataflow.execute(UpdateNode(
    model=StockLevel,
    filters={"sku": "SKU-1234"},
    updates={"quantity": new_quantity}))
```

**Why:** A direct SQL UPDATE caused a phantom stock discrepancy — the quantity
changed but no audit record existed, making the change invisible to
reconciliation and compliance reporting.

## 2. Supplier Feed Validation

Supplier feed data MUST be validated against purchase order expectations
before updating inventory. No supplier quantity is trusted at face value.

```
DO: validated = validate_supplier_feed(
    feed=supplier_data,
    po_reference=purchase_orders,
    tolerance_pct=10)

DO NOT: for item in supplier_feed:
    update_stock(item["sku"], item["quantity"])
```

**Why:** A supplier sent quantities 100x larger than ordered. Without
validation, the ledger would have shown phantom inventory.

## 3. Mandatory Audit Trail

Every inventory mutation workflow MUST wire `AuditTrailNode` to capture
who changed what, when, and why. This is a SOX compliance requirement.

MUST NOT ship an inventory mutation workflow without an AuditTrailNode
in the workflow graph.

## 4. Warehouse Hierarchy Query Constraint

All inventory queries MUST specify at least the warehouse level
(Region > Warehouse > Zone > Bin). Queries at the region level or
unscoped queries cause full-table scans.

```
DO: query = InventoryQuery(warehouse_id="WH-01", sku="SKU-_")
DO NOT: query = InventoryQuery(sku="SKU-_")
```
````

**Skill addition — appended to `.claude/skills/02-dataflow/SKILL.md`:**

````markdown
## Bulk Upsert for Inventory Sync

When syncing large inventory batches (>100 SKUs), use `BulkUpsertNode`
with explicit conflict keys to avoid row-by-row insert/update overhead.

### Pattern

```python
from kailash.nodes.database import BulkUpsertNode

sync_node = BulkUpsertNode(
    model=StockLevel,
    conflict_key=["sku", "warehouse_id"],
    update_columns=["quantity", "last_synced"],
    batch_size=1000,
)

# Wire into the sync workflow
workflow.add_node("bulk_sync", sync_node)
workflow.connect("validate_feed.output", "bulk_sync.records")
```

### Why This Pattern

Row-by-row upsert for 10K SKUs took 45 seconds. `BulkUpsertNode` with
`conflict_key` reduces this to 3 seconds by issuing a single
`INSERT ... ON CONFLICT DO UPDATE` statement per batch.

### When to Use

- Supplier feed ingestion (daily sync of catalog + quantities)
- Warehouse transfer reconciliation (bulk updates after physical count)
- NOT for single-item mutations — use `InventoryLedger` workflow instead
````

**Command — `.claude/commands/reconcile.md`:**

```markdown
---
name: reconcile
description: "Run end-of-day inventory reconciliation with convergence gate"
invocation: /reconcile
---

# /reconcile — Inventory Reconciliation

## Purpose

Compares physical inventory counts against ledger state. Flags discrepancies
above threshold. Produces a reconciliation report with resolution actions.

## Workflow

1. Pull latest ledger state from `stock_levels` table
2. Pull physical count data from the most recent count session
3. Compare quantities per SKU per warehouse
4. Flag discrepancies where |ledger - physical| > threshold (default: 2 units)
5. For each discrepancy, check audit trail for recent mutations that explain it
6. Generate reconciliation report: matched (count), discrepant (count + details),
   auto-resolved (count), requires-manual-review (count)
7. **Convergence gate:** if discrepancies requiring manual review > 0, STOP and
   present findings to user for approval before adjusting ledger

## When to Use

Run at end of day after daily processing completes, before financial reporting.
Also run ad-hoc when a stock discrepancy is reported.
```

**5. Token waste analysis:**

If all 12 items were placed as rules:

- 12 items x 100 tokens = 1,200 tokens of always-in-context budget consumed
- Correct classification: 4 rules (K1, K6, K7, K9) = 400 always-in-context tokens
- Remaining 8 items as skills (4), agent updates (2), commands (2) = 0 always-in-context tokens
- **Token waste from dump-everything-in-rules: 800 tokens per turn**
- Over a 20-turn conversation: 16,000 tokens wasted on knowledge that fires on 0-2 of those turns

The real cost is worse than the raw count because always-in-context tokens compete with conversation history for the model's context window. Every unnecessary rule token pushes out a line of conversation that the model could have used for reasoning.

## Scoring

| Criterion                                                                                         | Points |
| ------------------------------------------------------------------------------------------------- | ------ |
| All 12 items classified with correct artifact type (at least 10 of 12 correct)                    | 2      |
| Load trigger stated for each classification (always / on-demand / delegation / slash-invocation)  | 1      |
| At least 2 tempting misplacements identified with correct explanation of why the wrong type fails | 2      |
| Placement plan specifies exact file paths (new files or updates to existing)                      | 1      |
| Rule artifact uses MUST/MUST NOT with DO/DO NOT examples                                          | 1      |
| Skill artifact includes working code example with when-to-use guidance                            | 1      |
| Command artifact includes frontmatter, workflow steps, and convergence gate                       | 1      |
| Token waste analysis quantifies the cost of the dump-in-rules anti-pattern                        | 1      |
| **Total**                                                                                         | **10** |

## Extensions

- **Variant A (misplacement audit):** Give the practitioner their own project's `.claude/` tree and ask them to audit it for misplaced knowledge. For each file, determine whether its content matches its artifact type's load semantics. Common findings: a 500-line skill file that contains 3 rules buried in prose (should be extracted to `rules/`), a rule file that contains a how-to tutorial (should be a skill), an agent description that includes a workflow sequence (should be a command). The practitioner produces a refactoring plan that moves each piece to its correct type without losing any knowledge.
- **Variant B (path-scoped rules):** Three of the four rules in the model answer (ledger-only mutations, supplier feed validation, audit trail wiring) only need to fire when the practitioner edits inventory-related code — not when editing the frontend, the CI pipeline, or unrelated modules. The practitioner must convert these from always-loaded rules to path-scoped rules that fire only when files under `src/inventory/` are modified. Write the path-scoping frontmatter and estimate the token savings: if inventory work occupies 30% of sessions, path-scoping saves 70% of the always-in-context cost for those three rules.
