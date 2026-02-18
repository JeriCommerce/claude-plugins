# JeriCommerce Report Generator

Generate professional PDF loyalty program performance reports for JeriCommerce clients.

## What it does

Given a program UUID, slug, or name, this plugin queries the JeriCommerce production database (via Metabase), collects 16 data points across membership, passes, revenue, engagement, rewards, campaigns, and web usage, then generates a branded PDF report with insights and actionable recommendations.

## Prerequisites

- **Metabase MCP server** must be connected and configured with access to `database_id: 3` (jericommerce-db)
- **PDF skill** available in the Cowork environment

## Usage

```
/generate-report old-jeffrey
/generate-report 91133c45-9401-4c6c-9f90-284ef7cf5b6f
/generate-report Old Jeffrey
```

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/generate-report` | Entry point — triggers the full report workflow |
| Skill | `loyalty-report-generator` | Domain knowledge: SQL queries, report structure, branding, recommendation heuristics |

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
