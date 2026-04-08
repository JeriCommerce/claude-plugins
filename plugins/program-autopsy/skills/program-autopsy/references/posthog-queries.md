# PostHog Queries

Query HogQL against PostHog to see what pages the merchant visited and what actions they took in the admin panel.

## 1. All events for the program

```json
{
  "kind": "DataVisualizationNode",
  "source": {
    "kind": "HogQLQuery",
    "query": "SELECT timestamp, event, properties.$current_url as url, properties.slug as slug FROM events WHERE properties.slug = '{{SLUG}}' OR properties.$current_url LIKE '%{{SLUG}}%' ORDER BY timestamp ASC LIMIT 200",
    "filters": {
      "dateRange": {"date_from": "-7d"}
    }
  }
}
```

Adjust `date_from` based on the program's creation date from BetterStack logs.

## Key PostHog events

| Event | Meaning |
|-------|---------|
| `$pageview` on `/not-found` | First-load bug — bad first impression |
| `@program/createProgramSuccess` | Program created |
| `@program/persistProgramSuccess` | Saved configuration changes |
| `@program/updateProgramIntegrationSuccess` | Changed integration/earning flow settings |
| `$set` on `/settings/subscription` | **Checked pricing** — key churn signal |
| `$set` on `/settings/loyalty` | Configured loyalty settings |
| `$set` on `/settings/technical` | Explored technical settings |
| `$pageleave` | Left a page (last one before uninstall = exit page) |

## URL patterns

| URL contains | Meaning |
|-------------|---------|
| `/customize/wallets` | Designing wallet card |
| `/customize/webapp` | Customizing web app |
| `/loyalty/engagement/` | Configuring earning flows |
| `link_source=search` | Re-entered via Shopify app search |

## Merchant identification

The `distinct_id` from PostHog events identifies the merchant user. Two different IDs may appear if the embedded Shopify session creates one identity and the admin panel another.
