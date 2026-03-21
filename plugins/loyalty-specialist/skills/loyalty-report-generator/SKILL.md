---
name: Jeri - Loyalty Report Generator
description: >
  This skill should be used when the user asks to "generate a loyalty report",
  "create a program report", "analyze a JeriCommerce program", "run a loyalty
  performance report", or provides a program UUID, slug, or name expecting
  a JeriCommerce loyalty program analysis. Covers the full data collection,
  analysis, and PDF report generation workflow.
version: 202603.10.1
---

# JeriCommerce Loyalty Report Generator

Generate professional PDF loyalty program performance reports by querying the JeriCommerce production database via the Metabase MCP connector and analyzing the results.

## Workflow

### Step 1: Check tool availability

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/tools.md` for required Metabase tools and how to execute queries.

If the Metabase connector is unavailable, inform the user and stop.

### Step 2: Load data model

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/data-model.md` for the integration data model, database details, enums, number formatting, and data milestones.

### Step 3: Identify the program

Use `Metabase:execute` with `database_id: 3` and the Step 1 query from `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/sql-queries.md` to get the program UUID and details.

### Step 4: Resolve feature IDs

Run the Step 1.5 query from `references/sql-queries.md` to get the UUIDs for `loyalty` and `rewards` features. Store as `{loyalty_feature_id}` and `{rewards_feature_id}`. Never hardcode feature UUIDs — they may vary across environments.

### Step 5: Detect integration provider

Run the Step 1.6 query from `references/sql-queries.md` using the resolved feature IDs to determine `has_jc_loyalty`, `has_jc_rewards`, and `active_integrations`. This decides which queries and report sections to include.

### Step 6: Collect data (conditional)

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/sql-queries.md` for all 16 SQL queries.

Execute queries via `Metabase:execute` (database_id: 3). Use this decision tree:

**Always run:** Steps 2–6, 13–16 (subscription, membership, passes, origin, campaigns, events, web visits, referrals)

**Only if `has_jc_loyalty = true`:** Steps 7–11 (balances, tiers, revenue, monthly trend, engagement flows)

**Only if `has_jc_rewards = true`:** Step 12 (rewards catalog & redemption)

| Condition | Skip report sections | Skip queries |
|-----------|---------------------|--------------|
| `has_jc_loyalty = false` | Loyalty Economics, Revenue Impact, Engagement Flows | Steps 7, 8, 9, 10, 11 |
| `has_jc_rewards = false` | Rewards & Redemption | Step 12 |

Run ALL applicable queries before generating the report. Do NOT generate partial reports.

### Step 7: Analyze data and generate report

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/report-structure.md` for the PDF report structure, branding, visual design, heuristics, and recommendation logic.

Apply the appropriate heuristic set based on `has_jc_loyalty`. Generate actionable recommendations. If `has_jc_loyalty = true`, also generate:
- **Section 12: Recommended Configuration** — Compare current settings against optimal values. Present as a parameter comparison table with rationale.
- **Section 13: Growth Projection** — Run a 12-month projection comparing Base vs Optimized scenarios using the formulas in `references/report-structure.md`.

### Step 8: Generate PDF

Read the PDF skill (`/mnt/.skills/skills/pdf/SKILL.md`) and create the report following the structure and branding in `references/report-structure.md`. Use the full or reduced report structure based on integration detection.

### Step 9: File naming

Save as `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`

## Reference Files

- `references/tools.md` — Metabase connector tools and query execution
- `references/data-model.md` — Integration data model, database details, enums, and data milestones
- `references/sql-queries.md` — All 16 SQL queries for data collection (Steps 1–16)
- `references/report-structure.md` — PDF report structure, branding, and recommendation heuristics
