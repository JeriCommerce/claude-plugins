---
name: Jeri - Bug Reporter
description: >
  Use when the user wants to "report a bug", "create a bug issue", "log a customer issue",
  "report an error", "something is broken", or describes a problem that needs to be tracked.
  Guides the user through structured bug reporting and creates a Linear issue.
version: 202603.24.0
---

# Bug Reporter

Guide users through structured bug reporting and create well-classified Linear issues in the JeriCommerce support project.

## Workflow

### Step 1: Check tool availability

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/tools.md` for required Linear tools and the target project.

If the Linear connector is unavailable, inform the user and stop.

### Step 2: Load communication guidelines

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/voice-tone.md` for communication style. Apply throughout the conversation.

### Step 3: Gather information

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/interview-questions.md` for the required and optional questions.

Interview the user following the question flow in that file.

### Step 4: Classify the issue

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/classification.md` for priority levels and area labels.

Determine priority and applicable labels based on the gathered information.

### Step 5: Confirm with the user

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/templates.md` for the confirmation summary format.

Show the summary and ask: "Does this look right? I'll create it in Linear once you confirm."

### Step 6: Create the issue in Linear

Read `${CLAUDE_PLUGIN_ROOT}/skills/bug-reporter/references/templates.md` for the issue description template and Linear parameters.

Create the issue using the tools from Step 1. After creation, share the Linear issue URL.

## Reference Files

- `references/tools.md` — Linear connector tools and target project
- `references/classification.md` — Priority levels and area labels
- `references/templates.md` — Confirmation summary and issue description templates
- `references/voice-tone.md` — Voice, tone, language rules, and product context
- `references/interview-questions.md` — Required and optional questions for bug gathering
