# BetterStack Queries

**Source ID**: `1612715`
**Table**: `t358529.jericommerce_servers`
**Cold storage**: `s3Cluster(primary, t358529_jericommerce_servers_s3)` — use for anything older than ~7 minutes
**Hot storage**: `remote(t358529_jericommerce_servers_logs)` — last few minutes only
**Row types**: `_row_type = 1` (logs), `_row_type = 3` (spans), `_row_type = 4` (errors)

## 1. Full activity timeline

```sql
SELECT
  dt,
  JSONExtract(raw, 'message', 'Nullable(String)') as message,
  JSONExtract(raw, 'attributes', 'jeri.program.slug', 'Nullable(String)') as slug
FROM s3Cluster(primary, t358529_jericommerce_servers_s3)
WHERE _row_type = 1
  AND raw ILIKE '%{{SLUG}}%'
ORDER BY dt ASC
LIMIT 100
```

For recent programs (< 7 min old), use hot storage:
```sql
SELECT ... FROM remote(t358529_jericommerce_servers_logs) WHERE ...
```

## 2. Errors only

```sql
SELECT
  dt,
  JSONExtract(raw, 'message', 'Nullable(String)') as message
FROM s3Cluster(primary, t358529_jericommerce_servers_s3)
WHERE _row_type = 1
  AND raw ILIKE '%{{SLUG}}%'
  AND (raw ILIKE '%error%' OR raw ILIKE '%fail%' OR raw ILIKE '%cancel%'
       OR raw ILIKE '%timeout%' OR raw ILIKE '%exception%')
ORDER BY dt DESC
LIMIT 30
```

## Key log signals

| Log message pattern | Meaning |
|---------------------|---------|
| `"App Installed"` / `"App Uninstalled"` | Install/uninstall timestamps (Google Chat notifications) |
| `"Brand extraction failed"` | Wallet card looks generic — poor first impression |
| `"Brand extraction returned no results"` | Same as above |
| `"canceling statement due to statement timeout"` | DB pool issues during onboarding |
| `"sync_program_feature_integrations"` errors | Integration sync failures |
| `"Calculating tier for customer"` | Customer activity — reveals emails |

## Important notes

- `severity_text` column does **NOT** exist in cold storage — never include it in `s3Cluster` queries
- Shop domain usually appears in brand extraction log messages
- If the slug contains special characters, ensure proper escaping in ILIKE patterns
