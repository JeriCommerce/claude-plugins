---
name: program-autopsy
description: >
  Investigate and diagnose a JeriCommerce program's lifecycle, health, or churn.
  Use this skill whenever the user asks to analyze a program, investigate why a
  merchant uninstalled, check what a program did, audit a program's activity,
  debug a program's issues, review a merchant's onboarding journey, or understand
  what happened with a specific slug. Also trigger when the user mentions a program
  slug and wants to understand its behavior, errors, configuration state, or
  merchant actions in the admin. Trigger phrases include "analiza el programa",
  "que hizo este programa", "por que se desinstalo", "investiga el slug",
  "program health check", "churn analysis", "autopsia", "que paso con", or any
  reference to investigating a specific program by slug or shop domain.
version: 202604.08.0
---

# Program Autopsy

Systematic investigation of a JeriCommerce program's lifecycle using three data sources: Metabase (DB state), BetterStack (backend logs/errors), and PostHog (merchant admin behavior).

## Required Tools

- `Metabase:execute` — SQL queries against production DB (database_id: 3)
- `mcp__claude_ai_BetterStack__telemetry_query` — Backend logs and error queries
- `mcp__claude_ai_PostHog__query-run` — HogQL queries for merchant behavior
- `visualize:show_widget` — Render the final HTML autopsy report

If any required tool is unavailable, inform the user and specify which data source will be missing from the investigation.

## Input

The user provides one of:
- Program **slug** (e.g., `tbftbr-8i`, `new-polinesia`)
- **Shop domain** (e.g., `example-store.myshopify.com`)
- Program **UUID**

## Investigation Protocol

Execute all data source queries, then synthesize. Do NOT ask the user for clarification before running queries — start investigating immediately with whatever identifier is provided.

### Phase 1: BetterStack + Metabase (parallel)

Run these two data sources in parallel — neither depends on the other.

**BetterStack — Backend Logs & Errors**

Read `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/references/betterstack-queries.md` for query templates and log signal patterns. Run the listed queries and extract install/uninstall timestamps, shop domain, and errors.

**Metabase — Database State**

Read `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/references/metabase-queries.md` for all SQL queries. Run queries 1-6. If the program was deleted (uninstall deletes the row), queries may return empty — that's a signal in itself.

### Phase 2: PostHog — Merchant Admin Behavior

Read `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/references/posthog-queries.md` for query templates and event mappings.

Adjust `date_from` based on the timeline established from BetterStack in Phase 1. Query all events for the program slug.

### Phase 3: Synthesize & Render

1. Match collected data against the churn signal detection rules below — only include signals with actual evidence
2. Read `${CLAUDE_PLUGIN_ROOT}/skills/program-autopsy/references/widget-template.md` for the HTML template specs
3. Aggregate data into the required variables
4. Write a brief prose introduction (2-3 sentences in Spanish) summarizing the key finding
5. Render the widget using `visualize:show_widget`

## Churn Signal Detection Rules

| Signal | Trigger | Confidence |
|--------|---------|------------|
| Price sensitivity | Visited `/settings/subscription` in last 3 actions before uninstall | High |
| Broken first impression | First pageview landed on `/not-found` | High |
| Generic card appearance | Brand extraction failed in BetterStack | Medium |
| No real customer testing | 0-1 customers (self only) | Medium |
| Exploration without commitment | >20 min session, multiple saves, no subscription upgrade | Medium |
| Quick bounce | <10 min session, <3 saves | High |
| Integration gap | Visited `/settings/technical` or integrations page multiple times | Low |
| Backend errors during onboarding | Statement timeouts or sync failures in first 5 min | Medium |
| No program integrations | 0 rows in `program_integrations` for the program | High |

**How to apply:** Check each signal against data from all three sources. Only include signals with actual evidence — do not speculate. Rank by confidence (High first). Include 2-3 signals max in the widget. For each, write a specific evidence sentence referencing timestamps or counts.

## Important Notes

- Programs that uninstalled have their DB rows **deleted** — investigation relies on BetterStack + PostHog
- BetterStack cold storage (`s3Cluster`) is needed for anything older than ~7 minutes
- PostHog date ranges should match the program's lifecycle from Phase 1

## Reference Files

- `references/betterstack-queries.md` — BetterStack query templates and log signal patterns
- `references/metabase-queries.md` — Metabase SQL queries for all DB entities
- `references/posthog-queries.md` — PostHog HogQL queries and event mappings
- `references/widget-template.md` — HTML widget template and design system specs
