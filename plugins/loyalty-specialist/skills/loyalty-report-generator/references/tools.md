# Required Tools — Metabase Connector

**All SQL queries MUST be executed through the Metabase connector extension**, never directly against the database.

## Available Metabase tools

| Tool | Purpose | Limit |
|------|---------|-------|
| `Metabase:execute` | Run SQL queries or saved cards | Max 500 rows |
| `Metabase:export` | Export query results as CSV/JSON/XLSX | Max 1M rows |
| `Metabase:search` | Search across cards, dashboards, tables | — |
| `Metabase:retrieve` | Get details of a specific item by ID | — |
| `Metabase:list` | List all items of a type | — |

## How to execute SQL queries

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
