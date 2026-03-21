---
name: Jeri - Feature Request
description: >
  Use when the user wants to "create a feature request", "log a customer request", "submit a product request",
  "a customer wants...", "a client is asking for...", or describes a feature that a customer needs.
  Guides the user through detailed requirement gathering, associates Linear customers, and creates
  a well-structured user story issue in Linear.
version: 202603.11.0
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

Ask the user to describe what the customer wants. Then ask **follow-up questions one at a time** to fully understand the request. Adapt your questions based on their answers — don't follow a rigid script.

**Goal:** Understand the request well enough to write a clear user story. Keep asking until you can articulate the **who**, **what**, and **why**.

Key questions to explore (not necessarily in this order):

1. **What does the customer want?** — Get a clear description of the desired feature or change. Ask for specifics: what should it do? How should it behave?
2. **Why do they want it?** — What problem does it solve? What's the pain point or business goal?
3. **Who is it for?** — Is this for the merchant, their end customers, or both? What type of user would benefit?
4. **Where in the product?** — Which area of the system does this relate to? (admin dashboard, customer app, API, Shopify extensions, etc.)
5. **How do they envision it working?** — Do they have a specific idea of the UX or behavior? Any examples from other products?
6. **What's the priority for the customer?** — Is this critical for their business, a nice-to-have, or somewhere in between?
7. **Any constraints or context?** — Deadlines, dependencies on other features, regulatory requirements, etc.

**Important:** Keep probing until the request is specific and actionable. Vague requests like "improve the dashboard" need to be narrowed down to something concrete.

### Step 4: Identify the customer(s)

Ask the user to identify the customer(s) making this request.

**Always ask:** "Do you know the customer's name or slug so I can look them up in Linear?"

For **each** customer:

1. **Search first** — Use `mcp__linear__list_customers` with the `query` parameter to search by name.
2. **If found** — Confirm with the user: "I found [Customer Name] in Linear. Is this the right one?"
3. **If not found** — Ask the user: "I didn't find [name] in Linear. Would you like me to create them as a new customer?" If yes, use `mcp__linear__save_customer` with the customer name.
4. **Multiple customers** — Ask: "Are there other customers requesting this as well?" Repeat the search/create flow for each one.

Collect all customer IDs for Step 7.

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
