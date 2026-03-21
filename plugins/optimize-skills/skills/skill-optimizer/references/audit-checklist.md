# Skill Audit Checklist

## What to Look For

When auditing a SKILL.md file, identify every distinct section that could be its own file. Common section types:

### Instruction Sections
- **Workflow / Process** — Step-by-step instructions for completing the task
- **Rules / Constraints** — Hard rules that must be followed (e.g., "always write in English", "never skip validation")
- **Adaptability / Modes** — Different execution modes or shortcuts (e.g., "quick check" vs "full audit")

### Reference Sections
- **Tool Requirements** — External tools needed (MCP servers, connectors, APIs) with connection details and parameters
- **Data Model / Schema** — Database details, enums, field definitions, data relationships
- **Classification / Taxonomy** — Priority levels, label definitions, categories, status mappings
- **Configuration / Settings** — Environment-specific values, default parameters, thresholds

### Template Sections
- **Output Templates** — Structured formats for the skill's output (e.g., issue description template, report format)
- **Input Templates** — Expected input format or schema (e.g., JSON structure, CSV columns)
- **Confirmation Templates** — Summary templates shown to the user before taking action

### Tone & Style Sections
- **Voice & Tone** — How to communicate (formal vs casual, empathetic vs direct)
- **Language Rules** — Translation requirements, formatting conventions, terminology
- **Examples** — Good/bad examples of output, annotated with why they work or don't

### Data Sections
- **SQL Queries** — Database queries used by the skill
- **API Endpoints** — Endpoint details, request/response schemas
- **Quality Checks** — Validation rules, verification criteria
- **Heuristics** — Decision trees, threshold tables, recommendation logic

## Audit Output Format

Present the audit as a numbered list:

```
## Audit: [Skill Name]

**File**: [path to SKILL.md]
**Total lines**: [N]

### Sections Found

1. **[Section Name]** (lines X-Y)
   - Type: [instruction / reference / template / tone / data]
   - Content: [1-sentence description]
   - Suggested file: `references/[filename].md`

2. **[Section Name]** (lines X-Y)
   - Type: [instruction / reference / template / tone / data]
   - Content: [1-sentence description]
   - Suggested file: `references/[filename].md`

...

### Recommended Structure

```
skills/[skill-name]/
├── SKILL.md                    (orchestrator only)
└── references/
    ├── [file-1].md
    ├── [file-2].md
    └── ...
```

### Complexity Assessment

- **Current**: [monolithic / partially split / well-structured]
- **Sections to extract**: [N]
- **Expected improvement**: [description of what gets better]
```
