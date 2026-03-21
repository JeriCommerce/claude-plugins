# Feature Request Templates

## Confirmation Summary

Show this to the user before creating the issue:

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

**Important:** The summary shown to the user MUST already be in English (title, user story, acceptance criteria, context). If the user has been speaking in another language, translate everything now — do not wait until the Linear API call. This lets the user review the English content before it's created.

Allow the user to request changes before proceeding.

## Linear Issue Parameters

Use `mcp__linear__create_issue` with these parameters:

- **team**: "Development"
- **project**: "Feature Requests"
- **title**: Concise, descriptive title (max ~80 chars)
- **priority**: 1 (Urgent), 2 (High), 3 (Normal), or 4 (Low)
- **labels**: `["feature-request", ...area labels]`
- **description**: Structured markdown in user story format (see template below)

## Customer Need Association

For **each** customer identified, use `mcp__linear__save_customer_need` with:

- **body**: A brief summary of what this specific customer needs (can be tailored per customer if their needs differ slightly)
- **customer**: The customer ID or name
- **issue**: The issue ID returned from issue creation
- **priority**: 1 if the customer marked it as important, 0 otherwise

## Issue Description Template

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
