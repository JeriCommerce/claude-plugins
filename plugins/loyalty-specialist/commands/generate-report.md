---
description: Generate a JeriCommerce loyalty program performance report
argument-hint: [program-id] ["optional focus instructions"]
---

Parse `$ARGUMENTS` to extract:
- **Program identifier** (`$1`): the first argument — a UUID, slug, or program name
- **Focus instructions** (optional): any remaining text after the program identifier, typically in quotes. This describes specific areas to emphasize, deep-dive into, or custom analysis requests.

Examples:
- `/generate-report old-jeffrey` → full standard report
- `/generate-report old-jeffrey "focus on churn and reactivation"` → full report with expanded analysis on churn
- `/generate-report 91133c45-9401-4c6c-9f90-284ef7cf5b6f "analyze why revenue dropped last month"` → full report with custom revenue deep-dive

Follow these steps exactly:

1. Read the loyalty-report-generator skill at `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/SKILL.md` for the workflow overview and data notes.

2. Read the SQL queries reference at `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/sql-queries.md`.

3. Read the report structure reference at `${CLAUDE_PLUGIN_ROOT}/skills/loyalty-report-generator/references/report-structure.md`.

4. Execute **Step 1** from the SQL queries to identify the program using `Metabase:execute` with `database_id: 3`. If the input looks like a UUID, query by `id`. If it contains only lowercase letters and hyphens, query by `slug`. Otherwise, search by name with `ILIKE`. If multiple results are found, ask the user to confirm.

5. Execute **Step 1.5** to detect the integration provider (`has_jc_loyalty`, `has_jc_rewards`, `active_integrations`). This determines which queries and report sections to include.

6. Execute the applicable data collection queries (Steps 2–16) using the confirmed program UUID. **All queries must be run through `Metabase:execute` with `database_id: 3`**. Skip Steps 7–11 if `has_jc_loyalty = false`. Skip Step 12 if `has_jc_rewards = false`. Collect ALL applicable results before proceeding.

7. Analyze the collected data applying the appropriate recommendation heuristics from the report structure reference (full or reduced based on integration detection).

8. **If focus instructions were provided**: After the standard analysis, perform a deeper analysis on the specified topic. This may include:
   - Writing additional SQL queries to drill down into the focus area (e.g., cohort analysis, time comparisons, segmentation)
   - Expanding the relevant report section with more granular data and context
   - Adding a dedicated "Deep Dive: {topic}" section before the Insights & Recommendations section
   - Tailoring the recommendations to prioritize actions related to the focus area

9. Read the PDF skill and generate the report as a professional PDF following the exact structure, branding, and formatting specified in the report structure reference. If focus instructions were provided, include the deep-dive section.

10. Save the PDF as `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`.
