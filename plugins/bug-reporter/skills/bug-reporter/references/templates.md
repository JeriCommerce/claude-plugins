# Bug Report Templates

## Confirmation Summary

Show this to the user before creating the issue:

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

## Linear Issue Parameters

Use `Linear:create_issue` with these parameters:

- **team**: "Development"
- **project**: "Support and Customer Bugs"
- **title**: Concise, descriptive title (max ~80 chars)
- **priority**: 1 (Urgent), 2 (High), 3 (Normal), or 4 (Low)
- **labels**: Applicable labels from classification
- **description**: Structured markdown (see template below)

If the user provided screenshots or videos, attach them using `Linear:create_attachment`.

## Issue Description Template

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
