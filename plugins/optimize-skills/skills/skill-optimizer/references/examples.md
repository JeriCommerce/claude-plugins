# Restructuring Examples

## Before vs After

### Example: Bug Reporter Skill

**Before** — Single SKILL.md (184 lines):
```
SKILL.md
├── Frontmatter
├── Description
├── Required Tools — Linear Connector (tool table)
├── Target Project (team, project info)
├── Workflow
│   ├── Step 1: Gather information (questions)
│   ├── Step 2: Classify the issue (priority table, labels table)
│   ├── Step 3: Confirm with user (summary template)
│   └── Step 4: Create in Linear (parameters, description template)
├── Product Context (reference to docs)
├── Language (English rule)
└── Voice & Tone (guidelines)
```

**After** — Orchestrator + 4 reference files:
```
skills/bug-reporter/
├── SKILL.md                      (orchestrator — ~40 lines)
└── references/
    ├── tools.md                  (Linear tools table, target project)
    ├── classification.md         (priority levels, area labels)
    ├── templates.md              (confirmation summary, issue description)
    └── voice-tone.md             (tone, language rules, product context)
```

**Orchestrator SKILL.md:**
```markdown
---
name: Bug Reporter
description: >
  Use when the user wants to report a bug...
version: 202602.19.0
---

# Bug Reporter

Guide users through structured bug reporting and create Linear issues.

## Workflow

### Step 1: Load tools and target

Read `references/tools.md` for the required Linear tools and target project.

If the Linear connector is unavailable, inform the user.

### Step 2: Gather information

Ask the user about the bug conversationally, one question at a time.

**Required:** What happened? Where? Who reported? How to reproduce?
**Optional:** Screenshots? Since when? How many affected? Workaround?

### Step 3: Classify the issue

Read `references/classification.md` for priority levels and area labels.

Determine priority and applicable labels based on the gathered info.

### Step 4: Confirm with the user

Read `references/templates.md` for the confirmation summary format.

Show the summary and ask for confirmation before creating.

### Step 5: Create the issue

Read `references/templates.md` for the issue description template.

Use the Linear tools from Step 1 to create the issue.

### Step 6: Follow voice guidelines

Read `references/voice-tone.md` for communication style.

Apply throughout the entire conversation.

## Reference Files

- `references/tools.md` — Linear connector tools and target project
- `references/classification.md` — Priority levels and area labels
- `references/templates.md` — Confirmation and issue description templates
- `references/voice-tone.md` — Voice, tone, and language rules
```

## Common Patterns

### Pattern: Tools + Workflow + Output

Most skills follow this pattern:
1. `tools.md` — What tools are needed
2. `workflow.md` or inline steps — What to do (stays in SKILL.md as orchestration)
3. `templates.md` — What the output looks like

### Pattern: Classification + Templates

Skills that create structured items (issues, reports, requests):
1. `classification.md` — How to categorize the item
2. `templates.md` — The structured format for the item

### Pattern: Data + Process + Quality

Data-heavy skills (research, analysis, reporting):
1. `data-model.md` or `input-requirements.md` — What data to work with
2. `process.md` — How to analyze/transform the data
3. `output-format.md` — Expected output structure
4. `quality-checks.md` — Validation rules before delivering
