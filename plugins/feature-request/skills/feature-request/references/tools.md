# Required Tools — Linear MCP

**All operations MUST go through the Linear MCP tools**, never the Linear API directly.

Available Linear tools:

| Tool | Purpose |
|------|---------|
| `mcp__linear__create_issue` | Create a new feature request issue |
| `mcp__linear__list_issues` | Check for duplicate or related requests |
| `mcp__linear__list_teams` | List teams |
| `mcp__linear__list_projects` | List projects |
| `mcp__linear__list_issue_labels` | List available labels |
| `mcp__linear__list_customers` | Search for existing customers |
| `mcp__linear__save_customer` | Create a new customer |
| `mcp__linear__save_customer_need` | Associate a customer need to an issue |

If the Linear MCP tools are not available, inform the user that the Linear MCP server must be configured.

## Target Project

All feature requests go to:

- **Team**: Development
- **Project**: Feature Requests
- **Label**: `feature-request`

> If the "Feature Requests" project does not exist, create the issue in team "Development" without a project and inform the user.
