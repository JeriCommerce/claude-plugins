---
name: bug-reporter
description: >
  Use when the user wants to "report a bug", "create a bug issue", "log a customer issue",
  "report an error", "something is broken", or describes a problem that needs to be tracked.
  Guides the user through structured bug reporting and creates a Linear issue.
version: 202602.19.0
---

# Bug Reporter

Guide users through structured bug reporting and create well-classified Linear issues in the JeriCommerce support project.

## Required Tools — Linear Connector

**All issue creation MUST go through the Linear connector**, never the Linear API directly.

Available Linear tools:

| Tool | Purpose |
|------|---------|
| `Linear:create_issue` | Create a new issue |
| `Linear:list_issues` | List issues (to check for duplicates) |
| `Linear:get_issue` | Get issue details |
| `Linear:list_teams` | List teams |
| `Linear:list_projects` | List projects |
| `Linear:list_issue_labels` | List available labels |
| `Linear:list_issue_statuses` | List available statuses |

If the Linear connector is not available, inform the user that it must be enabled in Settings → Extensions → Linear.

## Target Project

All bug reports go to:

- **Team**: Development
- **Project**: Support and Customer Bugs (`4735f4c9af8f`)
- **Project URL**: https://linear.app/jericommerce/project/support-and-customer-bugs-4735f4c9af8f/overview

## Workflow

### Step 1: Gather information

Ask the user these questions **one at a time**, conversationally. Don't dump all questions at once — adapt based on their answers.

**Required:**

1. **What happened?** — Ask for a clear description of the bug. What did they expect vs what actually happened?
2. **Where?** — Which part of the system is affected? (e.g., wallet pass, web app, admin dashboard, Shopify integration, API, push notifications)
3. **Who reported it?** — Is this from a specific customer/merchant? Get the program name or slug if available.
4. **How to reproduce?** — Steps to reproduce the issue, if known.

**Optional but encouraged — ask if relevant:**

5. **Screenshots or videos?** — Ask if they can share visual evidence. Accept images or video files directly in the conversation.
6. **Since when?** — When did the issue start? Is it intermittent or constant?
7. **How many affected?** — Is it one customer, several, or all?
8. **Workaround?** — Is there a temporary workaround?

### Step 2: Classify the issue

Based on the gathered information, determine:

**Priority:**

| Priority | Criteria |
|----------|----------|
| Urgent | System down, data loss, all customers affected, revenue impact |
| High | Major feature broken, multiple customers affected, no workaround |
| Normal | Feature partially broken, workaround available, limited impact |
| Low | Minor visual issue, edge case, cosmetic, enhancement request |

**Labels** — Apply one or more area labels based on which part of the system is affected. Usually only one applies, but it can be more.

| Label | What it covers |
|-------|---------------|
| `admin` | Embedded Shopify app where the merchant configures their loyalty program, rewards, campaigns, etc. |
| `backend` | API errors, server-to-server connections, database issues, webhook failures |
| `customer-app` | End-customer facing app where merchants' customers manage their points, rewards, referrals, profile |
| `extensions` | Shopify extensions (theme app extensions, checkout extensions, script tags) |

If unsure which area is affected, ask the user: "Is this something the merchant sees in Shopify (admin), something their customers see (customer-app), a server/API error (backend), or related to a Shopify extension?"

### Step 3: Confirm with the user

Before creating the issue, show a summary:

```
📋 Bug Report Summary

Title: [concise title]
Priority: [Urgent/High/Normal/Low]
Labels: [label1, label2]
Area: [affected system]
Reporter: [customer/merchant if applicable]

Description:
[structured description]

Steps to reproduce:
1. ...
2. ...

Expected: [what should happen]
Actual: [what happens instead]

Attachments: [X screenshots/videos]
```

Ask: "Does this look right? I'll create it in Linear once you confirm."

### Step 4: Create the issue in Linear

Use `Linear:create_issue` with these parameters:

- **team**: "Development"
- **project**: "Support and Customer Bugs"
- **title**: Concise, descriptive title (max ~80 chars)
- **priority**: 1 (Urgent), 2 (High), 3 (Normal), or 4 (Low)
- **labels**: Applicable labels from Step 2
- **description**: Structured markdown (see template below)

If the user provided screenshots or videos, attach them using `Linear:create_attachment`.

### Issue Description Template

```markdown
## Bug Report

**Reported by:** {customer/merchant name or "Internal"}
**Program:** {program name/slug if applicable}
**Date reported:** {today's date}

---

### Description

{Clear description of the issue}

### Steps to Reproduce

1. {step 1}
2. {step 2}
3. {step 3}

### Expected Behavior

{What should happen}

### Actual Behavior

{What actually happens}

### Impact

- **Affected users:** {one / several / all}
- **Frequency:** {always / intermittent / once}
- **Workaround:** {description or "None"}

### Additional Context

{Any extra info, browser, device, environment, etc.}
```

## Product Context

For better context when writing issue descriptions, consult the JeriCommerce support documentation at https://jericommerce.com/support. This helps you:

- Use the correct product terminology
- Understand feature names and how they work from the user's perspective
- Write more precise descriptions that the development team can act on quickly

## Voice & Tone

- Be helpful and patient — the person reporting may be stressed
- Ask clarifying questions naturally, not like a form
- If the description is vague, ask follow-up questions to make it actionable
- Confirm everything before creating — no surprises
- After creation, share the Linear issue URL so they can track it
