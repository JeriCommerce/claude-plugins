---
name: Jeri - Feature Request
description: >
  Use when the user wants to "create a feature request", "log a customer request", "submit a product request",
  "a customer wants...", "a client is asking for...", or describes a feature that a customer needs.
  Guides the user through detailed requirement gathering, associates Linear customers, and creates
  a well-structured user story issue in Linear.
version: 202603.24.0
---

# Feature Request

Guide users through capturing customer feature requests and creating well-structured Linear issues written as user stories. Associate one or more Linear customers to each request.

## Workflow

### Step 1: Check tool availability

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/tools.md` for required Linear MCP tools and the target project.

If the Linear MCP tools are unavailable, inform the user and stop.

### Step 2: Load communication guidelines

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/voice-tone.md` for communication style and language rules. Apply throughout the conversation.

### Step 3: Understand the request

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/discovery-questions.md` for the discovery question flow.

Interview the user following that guide until you can articulate a clear user story with **who**, **what**, and **why**.

### Step 4: Identify the customer(s)

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/customer-workflow.md` for the customer search/create flow.

Follow that workflow to find or create each customer in Linear and collect their IDs.

### Step 5: Classify the request

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/classification.md` for priority levels and area labels.

Determine priority and applicable labels based on the gathered information.

### Step 6: Confirm with the user

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/templates.md` for the confirmation summary format.

Show the summary and ask for confirmation. Allow changes before proceeding.

### Step 7: Create the issue and associate customers

Read `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/references/templates.md` for issue description template, Linear parameters, and customer need association.

1. Create the issue using the tools from Step 1
2. Associate each customer using `mcp__linear__save_customer_need`
3. Share the Linear issue URL

## Reference Files

- `references/tools.md` — Linear MCP tools and target project
- `references/classification.md` — Priority levels and area labels
- `references/templates.md` — Confirmation summary, issue description template, and customer need association
- `references/voice-tone.md` — Voice, tone, language rules, and product context
- `references/discovery-questions.md` — Discovery question flow for understanding the request
- `references/customer-workflow.md` — Customer search/create flow in Linear
