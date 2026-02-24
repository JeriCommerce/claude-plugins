---
description: Capture a customer feature request and create a Linear issue as a user story
argument-hint: ["optional brief description of the request"]
---

Parse `$ARGUMENTS`:
- If provided, use as the initial feature description to start the conversation
- If empty, ask the user to describe what the customer is requesting

Follow these steps:

1. Read the feature-request skill at `${CLAUDE_PLUGIN_ROOT}/skills/feature-request/SKILL.md`.

2. If `$ARGUMENTS` contains a description, acknowledge it and begin asking follow-up questions to fully understand the request. If no arguments, ask "What feature is the customer requesting?"

3. Follow the skill workflow: understand the request deeply (keep asking until clear), identify and associate customer(s) in Linear, classify priority and labels, show summary for confirmation, then create the issue and customer needs in Linear.

4. After creating the issue, share the Linear issue URL with the user.
