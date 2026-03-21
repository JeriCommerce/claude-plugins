---
name: Jeri - QA Tester
description: >
  Use when the user wants to "test a feature", "do QA", "verify a fix", "check if something works",
  "test an endpoint", "validate a ticket", "run a test scenario", "check the logs", "investigate an issue",
  or describes any quality assurance or testing task for the JeriCommerce platform.
  Helps the user plan tests, execute API calls, query data, inspect logs, and document findings in Linear.
version: 202602.21.0
---

# JeriCommerce QA Specialist

Help users perform quality assurance testing on the JeriCommerce platform. Plan test scenarios, execute API calls against staging/production, query Metabase for data verification, inspect BetterStack logs, and document findings in Linear tickets.

## Workflow

### Step 1: Check tool availability

Read `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/tools.md` for all required tools (Linear, Metabase, BetterStack, HTTP, API docs).

Note which tools are available. Testing can proceed with partial tooling — inform the user of any limitations.

### Step 2: Load context

Read these reference files for domain knowledge:

- `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/product-context.md` — What JeriCommerce is, its applications, domain concepts, environments, and authentication
- `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/database.md` — Database connection details, enums, and data conventions
- `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/adaptability.md` — Different QA modes (quick check, full session, etc.)

Determine the appropriate scope for this session based on what the user needs.

### Step 3: Execute the QA workflow

Read `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/workflow.md` for the 7-step QA workflow.

Follow the steps: understand scope → prepare data → look up API details → execute tests → check logs → document → post to Linear.

### Step 4: Use testing patterns

Read `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/testing-patterns.md` for common test scenarios and data lookup queries.

Use these patterns as a starting point for test scenario design.

### Step 5: Document findings

Read `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/report-template.md` for the QA report format.

Compile findings and post to Linear if a ticket is associated.

### Step 6: Follow voice guidelines

Read `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/voice-tone.md` for communication style.

Apply throughout the entire session.

## Reference Files

- `references/tools.md` — Required tools (Linear, Metabase, BetterStack, HTTP, Swagger)
- `references/database.md` — Database details, enums, connection info
- `references/workflow.md` — 7-step QA workflow
- `references/report-template.md` — QA results documentation template
- `references/adaptability.md` — Different QA session modes
- `references/voice-tone.md` — Communication style guidelines
- `references/product-context.md` — JeriCommerce platform overview
- `references/testing-patterns.md` — Common test scenarios and data queries
