---
name: Jeri - Feature Request
description: >
  Use when the user wants to "create a feature request", "log a customer request", "submit a product request",
  "a customer wants...", "a client is asking for...", or describes a feature that a customer needs.
  Guides the user through detailed requirement gathering, associates Linear customers, and creates
  a well-structured user story issue in Linear.
version: 202602.24.0
---

# Feature Request

Guide users through capturing customer feature requests and creating well-structured Linear issues written as user stories. Associate one or more Linear customers to each request.

## Required Tools — Linear MCP

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

## Workflow

### Step 1: Understand the request

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

**Important:** Keep probing until the request is specific and actionable. Vague requests like "improve the dashboard" need to be narrowed down to something concrete. Ask "Can you give me an example?" or "What specifically would you want to see?" to get clarity.

### Step 2: Identify the customer(s)

After understanding the request, ask the user to identify the customer(s) making this request.

**Always ask:** "Do you know the customer's name or slug so I can look them up in Linear?"

For **each** customer:

1. **Search first** — Use `mcp__linear__list_customers` with the `query` parameter to search by name.
2. **If found** — Confirm with the user: "I found [Customer Name] in Linear. Is this the right one?"
3. **If not found** — Ask the user: "I didn't find [name] in Linear. Would you like me to create them as a new customer?" If yes, use `mcp__linear__save_customer` with the customer name.
4. **Multiple customers** — Ask: "Are there other customers requesting this as well?" Repeat the search/create flow for each one.

Collect all customer IDs for Step 5.

### Step 3: Classify the request

Based on the gathered information, determine:

**Priority:**

| Priority | Criteria |
|----------|----------|
| Urgent | Critical for customer retention, contractual obligation, blocking a major deal |
| High | Highly requested by multiple customers, significant business impact |
| Normal | Valuable improvement, requested by one or a few customers |
| Low | Nice-to-have, minor enhancement, no immediate business pressure |

**Labels** — Apply `feature-request` plus one or more area labels:

| Label | What it covers |
|-------|---------------|
| `feature-request` | Always applied |
| `admin` | Embedded Shopify app where the merchant configures their loyalty program, rewards, campaigns, etc. |
| `backend` | API, server, data model, webhooks, integrations |
| `customer-app` | End-customer facing app where merchants' customers manage their points, rewards, referrals, profile |
| `extensions` | Shopify extensions (theme app extensions, checkout extensions, script tags) |

If unsure which area, ask: "Would this be something in the merchant's Shopify admin (admin), the customer-facing app (customer-app), the server/API (backend), or a Shopify extension?"

### Step 4: Confirm with the user

Before creating the issue, show a summary:

```
📋 Feature Request Summary

Title: [concise title]
Priority: [Urgent/High/Normal/Low]
Labels: [feature-request, area-label]
Customer(s): [Customer 1, Customer 2, ...]

User Story:
As a [type of user],
I want [what they want],
so that [why — the benefit/goal].

Acceptance Criteria:
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] ...

Additional Context:
[any extra details, examples, constraints]
```

Ask: "Does this look right? I'll create it in Linear once you confirm."

Allow the user to request changes before proceeding.

### Step 5: Create the issue and associate customers

#### 5a. Create the issue

Use `mcp__linear__create_issue` with these parameters:

- **team**: "Development"
- **project**: "Feature Requests"
- **title**: Concise, descriptive title (max ~80 chars)
- **priority**: 1 (Urgent), 2 (High), 3 (Normal), or 4 (Low)
- **labels**: `["feature-request", ...area labels]`
- **description**: Structured markdown in user story format (see template below)

#### 5b. Associate customer needs

For **each** customer identified in Step 2, use `mcp__linear__save_customer_need` with:

- **body**: A brief summary of what this specific customer needs (can be tailored per customer if their needs differ slightly)
- **customer**: The customer ID or name
- **issue**: The issue ID returned from Step 5a
- **priority**: 1 if the customer marked it as important, 0 otherwise

### Issue Description Template

```markdown
## Feature Request

**Requested by:** {customer name(s)}
**Date:** {today's date}

---

### User Story

**As a** {type of user — e.g., "merchant using the loyalty program", "end customer of a rewards program"},
**I want** {clear description of the desired feature or change},
**so that** {the benefit or goal this achieves}.

### Context & Motivation

{Why is this being requested? What problem does it solve? What's the current pain point or limitation?}

### Acceptance Criteria

- [ ] {Specific, testable criterion 1}
- [ ] {Specific, testable criterion 2}
- [ ] {Specific, testable criterion 3}

### Examples / References

{Any examples the customer provided, screenshots, links to similar features in other products, or mockup descriptions}

### Additional Notes

{Constraints, deadlines, dependencies, or anything else relevant}
```

## Product Context

For better context when writing feature request descriptions, consult the JeriCommerce support documentation at https://jericommerce.com/support. This helps you:

- Use the correct product terminology
- Understand existing features and how they work
- Write user stories that align with the product's domain language

## Language

All content written to Linear (title, description, customer needs) **must be in English**, regardless of the language the user is speaking in the conversation. The conversation with the user can be in any language, but everything that goes into Linear is always in English.

## Voice & Tone

- Be curious and thorough — the goal is to capture the request so well that anyone reading it understands exactly what the customer wants
- Ask clarifying questions naturally, like a product manager would in a discovery call
- Don't assume the technical solution — focus on the **what** and **why**, not the **how**
- If the user gives a vague request, don't accept it as-is — probe deeper
- Be patient with follow-ups — a well-written request saves time later
- After creation, share the Linear issue URL so they can track it
