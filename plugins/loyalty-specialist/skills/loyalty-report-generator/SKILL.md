---
name: loyalty-report-generator
description: >
  This skill should be used when the user asks to "generate a loyalty report",
  "create a program report", "analyze a JeriCommerce program", "run a loyalty
  performance report", or provides a program UUID, slug, or name expecting
  a JeriCommerce loyalty program analysis. Covers the full data collection,
  analysis, and PDF report generation workflow.
version: 2026.02.18-0
---

# JeriCommerce Loyalty Report Generator

Generate professional PDF loyalty program performance reports by querying the JeriCommerce production database via the Metabase MCP connector and analyzing the results.

## Required MCP Tool

**All SQL queries MUST be executed through the Metabase MCP server**, never directly against the database.

Use the MCP tool to run native queries against the JeriCommerce database:

```
Tool: mcp__metabase__run-native-query
Parameters:
  database_id: 3
  query: "<SQL query here>"
```

**IMPORTANT**: Do NOT attempt to connect to the database directly. Do NOT use `psql`, `pg`, or any other database client. Always use the `mcp__metabase__run-native-query` tool with `database_id: 3`.

If the Metabase MCP tool is not available, inform the user that the Metabase MCP server must be configured before generating reports.

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

1. **Identify the program** — Use `mcp__metabase__run-native-query` with the Step 1 query from `references/sql-queries.md` to get the program UUID and details.
2. **Collect all data** — Execute Steps 2–16 from `references/sql-queries.md` **in order** via `mcp__metabase__run-native-query`, using the program UUID. Run ALL queries before generating the report. Do NOT generate partial reports.
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
