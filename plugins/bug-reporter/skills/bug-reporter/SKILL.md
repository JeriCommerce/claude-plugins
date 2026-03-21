---
name: Jeri - Bug Reporter
description: >
  Use when the user wants to "report a bug", "create a bug issue", "log a customer issue",
  "report an error", "something is broken", or describes a problem that needs to be tracked.
  Guides the user through structured bug reporting and creates a Linear issue.
version: 202602.19.0
---

# Bug Reporter

Guide users through structured bug reporting and create well-classified Linear issues in the JeriCommerce support project.

## Workflow

### Step 1: Check tool availability

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/tools.md` for required Linear tools and the target project.

If the Linear connector is unavailable, inform the user and stop.

### Step 2: Gather information

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/voice-tone.md` for communication style. Apply throughout the conversation.

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

### Step 3: Classify the issue

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/classification.md` for priority levels and area labels.

Determine priority and applicable labels based on the gathered information.

### Step 4: Confirm with the user

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/templates.md` for the confirmation summary format.

Show the summary and ask: "Does this look right? I'll create it in Linear once you confirm."

### Step 5: Create the issue in Linear

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/templates.md` for the issue description template and Linear parameters.

Create the issue using the tools from Step 1. After creation, share the Linear issue URL.

## Reference Files

- `references/tools.md` — Linear connector tools and target project
- `references/classification.md` — Priority levels and area labels
- `references/templates.md` — Confirmation summary and issue description templates
- `references/voice-tone.md` — Voice, tone, language rules, and product context
