# JeriCommerce Report Generator

Generate professional PDF loyalty program performance reports for JeriCommerce clients.

## What it does

Given a program UUID, slug, or name, this plugin queries the JeriCommerce production database (via Metabase), collects 16 data points across membership, passes, revenue, engagement, rewards, campaigns, and web usage, then generates a branded PDF report with insights and actionable recommendations.

## Prerequisites

- **Metabase connector extension** must be enabled in the Desktop app (Settings → Extensions → Metabase). This plugin uses `Metabase:execute` to run SQL queries against `database_id: 3` (jericommerce-db, PostgreSQL 17.7). If the Metabase extension is not available, the plugin will not be able to collect data.
- **PDF skill** available in the Cowork environment

## Usage

### Cowork mode (plugin installed)

```
/generate-report old-jeffrey
/generate-report 91133c45-9401-4c6c-9f90-284ef7cf5b6f
/generate-report Old Jeffrey "focus on churn and reactivation"
```

### Chat mode (no plugin needed)

If the Metabase connector doesn't work in Cowork, use Chat mode instead:

1. Open the file `prompts/generate-report-chat.md` from this plugin
2. Copy the entire content
3. Replace `{PROGRAM}` with your program UUID, slug, or name
4. Paste it into a Claude Desktop **Chat** conversation (with Metabase connector enabled)

The Chat prompt is self-contained — it includes all SQL queries, report structure, and branding inline.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/generate-report` | Entry point — triggers the full report workflow (Cowork) |
| Skill | `loyalty-report-generator` | Domain knowledge: SQL queries, report structure, branding, recommendation heuristics (Cowork) |
| Prompt | `prompts/generate-report-chat.md` | Self-contained prompt with all queries inline (Chat) |

## Report Sections

1. Executive Summary (KPIs)
2. Membership & Growth
3. Pass Engagement
4. Loyalty Economics
5. Revenue Impact
6. Engagement Flows
7. Rewards & Redemption
8. Push Campaigns
9. Web App Usage
10. Insights & Recommendations
