---
name: Jeri - Loyalty Report Generator
description: >
  This skill should be used when the user asks to "generate a loyalty report",
  "create a program report", "analyze a JeriCommerce program", "run a loyalty
  performance report", or provides a program UUID, slug, or name expecting
  a JeriCommerce loyalty program analysis. Covers the full data collection,
  analysis, and PDF report generation workflow.
version: 202603.24.0
---

# JeriCommerce Loyalty Report Generator

Generate professional PDF loyalty program performance reports by querying the JeriCommerce production database via the Metabase MCP connector and analyzing the results.

## Workflow

### Step 1: Check tool availability

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/tools.md` for required Metabase tools and how to execute queries.

If the Metabase connector is unavailable, inform the user and stop.

### Step 2: Load data model

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/data-model.md` for the integration data model, database details, enums, number formatting, and data milestones.

### Step 3: Identify the program and detect integrations

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/sql-queries.md` for all queries and the execution decision tree.

Run Steps 1, 1.5, and 1.6 to identify the program, resolve feature IDs, and detect `has_jc_loyalty` / `has_jc_rewards`.

### Step 4: Collect data (conditional)

Follow the execution decision tree in `references/sql-queries.md` to run all applicable queries via `Metabase:execute` (database_id: 3).

Run ALL applicable queries before generating the report. Do NOT generate partial reports.

### Step 5: Analyze data and generate report

Read `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/report-structure.md` for the PDF report structure, branding, visual design, heuristics, and recommendation logic.

Apply the appropriate heuristic set based on `has_jc_loyalty`. Generate actionable recommendations. Use the full or reduced report structure based on integration detection.

### Step 6: Generate PDF

Read the PDF skill (`/mnt/.skills/skills/pdf/SKILL.md`) and create the report following the structure and branding in `references/report-structure.md`.

### Step 7: File naming

Save as `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`

## Reference Files

- `references/tools.md` — Metabase connector tools and query execution
- `references/data-model.md` — Integration data model, database details, enums, and data milestones
- `references/sql-queries.md` — All 16 SQL queries, execution decision tree, and conditional logic
- `references/report-structure.md` — PDF report structure, branding, and recommendation heuristics
