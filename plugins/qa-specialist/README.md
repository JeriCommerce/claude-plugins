# QA Specialist Plugin

QA testing assistant for the JeriCommerce platform. Helps verify features, test bug fixes, and document findings.

## What it does

Assists with quality assurance testing by combining multiple tools into a unified testing workflow:

- **Reads Linear tickets** to understand what to test and posts QA findings back as comments
- **Queries Metabase** to find test data, verify database state, and get API credentials
- **Inspects BetterStack logs** to verify actions were processed and catch errors
- **Makes HTTP requests** to staging/production APIs to test endpoints directly
- **References Swagger docs** to understand exact request/response schemas

Not every session requires all tools — the plugin adapts to what's needed, from quick endpoint checks to full QA sessions.

## Prerequisites

- **Linear connector** — for reading/updating tickets (Settings → Extensions → Linear)
- **Metabase connector** — for querying test data and verifying results (Settings → Extensions → Metabase)
- **BetterStack connector** — for log inspection (Settings → Extensions → BetterStack)

All connectors are optional. The plugin works with whatever is available, informing you of any limitations.

## Usage

### Cowork mode (plugin installed)

```
/qa
/qa LIN-123
/qa "test the balance increase endpoint for old-jeffrey"
/qa "verify registration flow on staging"
```

### Chat mode

Describe your testing needs to Claude with the relevant connectors enabled. It will guide you through the process.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/qa` | Entry point — starts a QA testing session |
| Skill | `qa-tester` | Full QA workflow: understand scope, prepare data, test, verify, document |

## What it can do

1. **Understand scope** — Read Linear tickets or take descriptions to plan test scenarios
2. **Prepare test data** — Query Metabase for programs, customers, API keys, and current state
3. **Execute tests** — Make API calls to staging/production and verify responses
4. **Check logs** — Query BetterStack for errors, expected entries, and side effects
5. **Document findings** — Compile QA summary with PASS/FAIL results and post to Linear

## Environments

| Environment | API Base URL |
|-------------|-------------|
| Staging (default) | `https://s.api.jericommerce.com` |
| Production | `https://api.jericommerce.com` |

The plugin defaults to staging. Production write operations always require explicit confirmation.
