# Required Tools

## Linear Connector

For reading tickets and documenting findings:

| Tool | Purpose |
|------|---------|
| `Linear:get_issue` | Read ticket details to understand what to test |
| `Linear:list_issues` | Find related tickets |
| `Linear:update_issue` | Update ticket status based on test results |
| `Linear:create_comment` | Add QA findings as comments on tickets |
| `Linear:list_issue_statuses` | List available statuses |
| `Linear:list_issue_labels` | List available labels |
| `Linear:list_teams` | List teams |
| `Linear:list_projects` | List projects |
| `Linear:create_attachment` | Attach evidence to tickets |

If the Linear connector is not available, inform the user that it must be enabled in Settings → Extensions → Linear. Testing can still proceed but findings won't be posted to Linear.

## Metabase Connector

For querying test data and verifying database state:

| Tool | Purpose | Limit |
|------|---------|-------|
| `Metabase:execute` | Run SQL queries to find test data and verify results | Max 500 rows |
| `Metabase:export` | Export larger result sets | Max 1M rows |
| `Metabase:search` | Search for saved queries and dashboards | — |

**Database**: `database_id: 3` (jericommerce-db, PostgreSQL 17.7, timezone Europe/Madrid)

If the Metabase connector is not available, inform the user that it must be enabled in Settings → Extensions → Metabase. The user will need to provide test data manually.

## BetterStack

For inspecting logs related to test actions:

| Tool | Purpose |
|------|---------|
| `BetterStack:telemetry_query` | Query logs to verify actions and find errors |
| `BetterStack:telemetry_chart` | Visualize log patterns |
| `BetterStack:telemetry_get_source_fields` | Discover available log fields |
| `BetterStack:telemetry_list_sources` | List available log sources |
| `BetterStack:telemetry_get_query_instructions` | Get query syntax help |

If BetterStack is not available, inform the user. Log inspection will not be possible but testing can continue.

## HTTP Requests (WebFetch / Bash curl)

For making API calls to staging or production:

- Use `WebFetch` for simple GET requests
- Use `Bash` with `curl` for POST/PUT/DELETE requests that require specific headers and bodies
- **Default to staging** (`https://s.api.jericommerce.com`) unless the user explicitly requests production
- **Always confirm with the user** before making any write operation (POST/PUT/DELETE) against production

## API Documentation — Dynamic Swagger Lookup

**Do NOT rely on hardcoded endpoint lists.** The API evolves constantly. Instead, fetch the OpenAPI spec dynamically when you need to know how an endpoint works:

- **Staging**: `https://s.api.jericommerce.com/docs-json`
- **Production**: `https://api.jericommerce.com/docs-json`

Use `WebFetch` to fetch the spec and extract the information you need for the specific endpoint(s) you're testing:

```
WebFetch URL: https://s.api.jericommerce.com/docs-json
Prompt: "Extract the full schema for [METHOD] [PATH] — request body, query params, headers, response schema, and status codes"
```

Do this **every time** you need endpoint details. Don't assume you know the schema — look it up.
