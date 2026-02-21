---
description: Start a QA testing session — test features, verify fixes, inspect logs, and document findings
argument-hint: ["optional ticket ID, feature description, or endpoint to test"]
---

Parse `$ARGUMENTS`:
- If a **Linear ticket ID** (e.g., `LIN-123`, `JER-456`, or a Linear URL), fetch the ticket to understand what to test
- If a **description** (e.g., "balance increase endpoint", "registration flow"), use as the testing focus
- If **empty**, ask the user what they want to test

Follow these steps:

1. Read the qa-tester skill at `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/SKILL.md`.

2. Read the product context at `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/product-context.md`.

3. Read the testing patterns at `${CLAUDE_PLUGIN_ROOT}/skills/qa-tester/references/testing-patterns.md`.

4. If `$ARGUMENTS` contains a Linear ticket ID or URL, fetch the ticket details to understand what needs testing. If it contains a description, acknowledge it and begin planning tests. If empty, ask "What would you like to test today?"

5. Follow the skill workflow: understand scope, prepare data, look up API details from Swagger when needed, execute tests, check logs, document findings, and post to Linear if applicable.
