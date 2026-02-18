---
name: loyalty-report-generator
description: >
  This skill should be used when the user asks to "generate a loyalty report",
  "create a program report", "analyze a JeriCommerce program", "run a loyalty
  performance report", or provides a program UUID, slug, or name expecting
  a JeriCommerce loyalty program analysis. Covers the full data collection,
  analysis, and PDF report generation workflow.
version: 2026.02.18-2
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
2. **Collect all data** — Execute Steps 2–16 from `references/sql-queries.md` **in order** via `Metabase:execute` (database_id: 3), using the program UUID. Run ALL queries before generating the report. Do NOT generate partial reports.
3. **Analyze data** — Apply the heuristics from `references/report-structure.md` (Section: Insights & Recommendations) to generate actionable recommendations.
4. **Generate PDF** — Read the PDF skill (`/mnt/.skills/skills/pdf/SKILL.md`) and create the report following the structure and branding in `references/report-structure.md`.
5. **File naming** — Save as `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`

## Key Data Notes

- **Pass status enums**: 0 = created/pending, 1 = installed/active, 2 = uninstalled
- **Pass type enums**: 0 = Apple Wallet, 1 = Google Wallet
- **Campaign type enums**: 0 = push notification, 1 = location, 2 = card update, 3 = anonymous campaign, 4 = proximity
- **Customer origin enums**: SHOPIFY, JERICOMMERCE, INITIAL_SYNC, UNKNOWN
- **Engagement flows**: INCENTIVIZE_PURCHASES, INSTALL_WALLET_PASS, MEMBER_GET_MEMBER, FOLLOW_US, COMPLETE_USER_PROFILE, CUSTOMER_SCANNED, VERIFY_ACCOUNT, WALLET_LINK_CLICK
- **Transactions amount**: stored in cents, always divide by 100
- **Coupon batches**: `discount_type` is 'fixed' (value in whole currency) or 'percentage' (value is percentage number)
- If a query returns empty results, note it in the report as "No data available" — do NOT skip the section
- If the program has an `integration` field set (e.g. LoyaltyLion), note that loyalty data may be managed externally

## Reference Files

- **`references/sql-queries.md`** — All 16 SQL queries for data collection (Steps 1–16)
- **`references/report-structure.md`** — PDF report structure, branding, and recommendation heuristics
