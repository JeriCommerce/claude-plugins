---
name: loyalty-report-generator
description: >
  This skill should be used when the user asks to "generate a loyalty report",
  "create a program report", "analyze a JeriCommerce program", "run a loyalty
  performance report", or provides a program UUID, slug, or name expecting
  a JeriCommerce loyalty program analysis. Covers the full data collection,
  analysis, and PDF report generation workflow.
version: 202602.18.1
---

# JeriCommerce Loyalty Report Generator

Generate professional PDF loyalty program performance reports by querying the JeriCommerce production database via the Metabase MCP connector and analyzing the results.

## Required Tools — Metabase Connector

**All SQL queries MUST be executed through the Metabase connector extension**, never directly against the database.

### Available Metabase tools

| Tool | Purpose | Limit |
|------|---------|-------|
| `Metabase:execute` | Run SQL queries or saved cards | Max 500 rows |
| `Metabase:export` | Export query results as CSV/JSON/XLSX | Max 1M rows |
| `Metabase:search` | Search across cards, dashboards, tables | — |
| `Metabase:retrieve` | Get details of a specific item by ID | — |
| `Metabase:list` | List all items of a type | — |

### How to execute SQL queries

Use `Metabase:execute` to run native SQL queries against the JeriCommerce database:

```
Tool: Metabase:execute
Parameters:
  database_id: 3
  query: "SELECT id, name FROM programs LIMIT 5"
```

For large result sets (>500 rows), use `Metabase:export` instead:

```
Tool: Metabase:export
Parameters:
  database_id: 3
  query: "SELECT * FROM customers WHERE program_id = '...'"
  format: "json"
```

**IMPORTANT**:
- Do NOT attempt to connect to the database directly
- Do NOT use `psql`, `pg`, or any other database client
- Always use `Metabase:execute` with `database_id: 3` for all queries
- If the Metabase connector is not available, inform the user that the Metabase extension must be enabled in Settings → Extensions before generating reports

## Input

The user provides one of:
- A **program UUID** (e.g. `91133c45-9401-4c6c-9f90-284ef7cf5b6f`)
- A **program slug** (e.g. `old-jeffrey`)
- A **program name** (e.g. `Old Jeffrey`) — search with `ILIKE` if needed

If ambiguous, ask the user to confirm which program.

## Database Details

- **Metabase database_id: 3** (`jericommerce-db`, PostgreSQL 17.7, timezone `Europe/Madrid`)
- All monetary amounts in `transactions.amount` are in **cents** (divide by 100)
- All UUIDs are `uuid` type — always cast string inputs: `'value'::uuid`
- Soft deletes use `deleted_at` column (present on: `campaigns`, `coupons`, `rewards`; NOT on: `programs`, `customers`, `passes`)

## Workflow

1. **Identify the program** — Use `Metabase:execute` with `database_id: 3` and the Step 1 query from `references/sql-queries.md` to get the program UUID and details.

2. **Detect integration provider** — Run the Step 1.5 query from `references/sql-queries.md` to determine `has_jc_loyalty`, `has_jc_rewards`, and `active_integrations`. This decides which queries and report sections to include.

3. **Collect data (conditional)** — Execute queries from `references/sql-queries.md` via `Metabase:execute` (database_id: 3). Use this decision tree:

   **Always run:** Steps 2–6, 13–16 (subscription, membership, passes, origin, campaigns, events, web visits, referrals)

   **Only if `has_jc_loyalty = true`:** Steps 7–11 (balances, tiers, revenue, monthly trend, engagement flows)

   **Only if `has_jc_rewards = true`:** Step 12 (rewards catalog & redemption)

   | Condition | Skip report sections | Skip queries |
   |-----------|---------------------|--------------|
   | `has_jc_loyalty = false` | Loyalty Economics, Revenue Impact, Engagement Flows | Steps 7, 8, 9, 10, 11 |
   | `has_jc_rewards = false` | Rewards & Redemption | Step 12 |

   Run ALL applicable queries before generating the report. Do NOT generate partial reports.

4. **Analyze data** — Apply the heuristics from `references/report-structure.md` (use the appropriate heuristic set based on `has_jc_loyalty`). Generate actionable recommendations.

5. **Generate PDF** — Read the PDF skill (`/mnt/.skills/skills/pdf/SKILL.md`) and create the report following the structure and branding in `references/report-structure.md`. Use the full or reduced report structure based on integration detection.

6. **File naming** — Save as `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`

## Key Data Notes

- **Pass status enums**: 0 = created/pending, 1 = installed/active, 2 = uninstalled
- **Pass type enums**: 0 = Apple Wallet, 1 = Google Wallet
- **Campaign type enums**: 0 = push notification, 1 = location, 2 = card update, 3 = anonymous campaign, 4 = proximity
- **Customer origin enums**: SHOPIFY, JERICOMMERCE, INITIAL_SYNC, UNKNOWN
- **Engagement flows**: INCENTIVIZE_PURCHASES, INSTALL_WALLET_PASS, MEMBER_GET_MEMBER, FOLLOW_US, COMPLETE_USER_PROFILE, CUSTOMER_SCANNED, VERIFY_ACCOUNT, WALLET_LINK_CLICK
- **Transactions amount**: stored in cents, always divide by 100
- **Coupon batches**: `discount_type` is 'fixed' (value in whole currency) or 'percentage' (value is percentage number)
- **Transaction status**: filter by `status IN ('completed', 'processed')` — both represent successful transactions
- **Number formatting**: use European format — dots for thousands (`1.633.928`), commas for decimals (`95,89 EUR`)
- If a query returns empty results, note it in the report as "No data available" — do NOT skip the section
- If the program has an `integration` field set (e.g. LoyaltyLion), note that loyalty data may be managed externally

## Data Milestones

Important dates and facts that affect how data should be interpreted. Always consider these when analyzing results — mention them in the report when they impact a metric.

| Date / Fact | Impact |
|-------------|--------|
| 2026-02-17 | Start of coupon code → transaction linking. Before this date, `coupon_codes.transaction_id` is always NULL. Do NOT interpret pre-2026-02-17 NULL values as "unused codes" — tracking simply didn't exist yet. |
| All transactions | Only non-anonymous (identified) customers are stored. Anonymous purchases are NOT in the database. Revenue and transaction metrics reflect only identified loyalty members, not total store revenue. |

When generating the report:
- If analyzing coupon redemption rates, note that `codes_used` (linked to transactions) is only reliable from 2026-02-17 onwards
- If comparing revenue metrics, clarify that figures represent identified member revenue only, not total store revenue
- Add a footnote in the relevant sections when a milestone affects the data interpretation

## Reference Files

- **`references/sql-queries.md`** — All 16 SQL queries for data collection (Steps 1–16)
- **`references/report-structure.md`** — PDF report structure, branding, and recommendation heuristics
