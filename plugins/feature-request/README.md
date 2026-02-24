# Feature Request Plugin

Capture customer feature requests and create well-structured Linear issues written as user stories.

## What it does

Guides users through a detailed discovery conversation to fully understand what a customer is requesting and why. Creates a Linear issue formatted as a user story with acceptance criteria, and associates one or more Linear customers to the request via customer needs.

Key capabilities:

- **Deep requirement gathering** — Keeps asking follow-up questions until the request is specific and actionable
- **User story format** — Issues are written from the customer's perspective (As a... I want... So that...)
- **Customer association** — Searches for existing Linear customers or creates new ones, then links them to the issue via customer needs
- **Multiple customers** — Supports associating several customers to the same request

## Prerequisites

- **Linear MCP server** must be configured. This plugin uses Linear MCP tools (`mcp__linear__create_issue`, `mcp__linear__list_customers`, `mcp__linear__save_customer`, `mcp__linear__save_customer_need`, etc.) to manage issues and customers. If the Linear MCP server is not available, the plugin will not work.

## Usage

### Cowork mode (plugin installed)

```
/request-feature
/request-feature "Customer wants to export loyalty points as CSV"
```

### Chat mode

Describe the customer request to Claude with the Linear MCP tools available. It will guide you through the full discovery and creation process.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/request-feature` | Entry point — starts the feature request capture flow |
| Skill | `feature-request` | Full workflow: discover, identify customers, classify, confirm, create issue + customer needs |

## Where issues are created

All feature requests go to:
- **Team**: Development
- **Project**: Feature Requests
- **Label**: `feature-request`

## What it asks

1. What does the customer want? (detailed description)
2. Why do they want it? (pain point / business goal)
3. Who is it for? (merchant, end customer, both)
4. Where in the product? (admin, customer app, API, extensions)
5. How do they envision it working? (UX, behavior, examples)
6. What's the priority for the customer?
7. Any constraints or context? (deadlines, dependencies)
8. Customer name or slug? (for Linear customer association)
9. Any other customers requesting this?

Questions are asked conversationally, adapting based on your answers. The goal is to capture the request so well that anyone reading the issue understands exactly what the customer wants.
