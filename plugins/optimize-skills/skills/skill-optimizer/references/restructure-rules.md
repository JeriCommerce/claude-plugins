# Restructuring Rules

## The Orchestrator Pattern

After restructuring, SKILL.md becomes a pure **orchestrator**. It contains:

1. **Frontmatter** — `name`, `description`, `version` (unchanged)
2. **One-line summary** — What the skill does
3. **Step-by-step workflow** — Each step tells Claude which reference file to read and what to do with it
4. **Reference file index** — A list of all reference files with one-line descriptions

SKILL.md should contain **zero** rules, instructions, templates, examples, or data. All of that lives in the reference files.

## How to Write the Orchestrator

### Workflow Steps

Each step follows this pattern:

```markdown
### Step N: [Action Name]

Read `${SKILL_ROOT}/references/[file].md` for [what it contains].

[1-2 sentences describing what to do with the information from that file.]
```

Use `${SKILL_ROOT}` as a shorthand. If the plugin system uses `${CLAUDE_PLUGIN_ROOT}`, map it to the full path: `${CLAUDE_PLUGIN_ROOT}/skills/[skill-name]/references/[file].md`.

### Conditional Steps

If a step only applies in certain situations:

```markdown
### Step N: [Action Name] (only if [condition])

Read `${SKILL_ROOT}/references/[file].md` for [what it contains].
```

### Reference File Index

End SKILL.md with a complete index:

```markdown
## Reference Files

- `references/[file-1].md` — [one-line description]
- `references/[file-2].md` — [one-line description]
- ...
```

## How to Split Content into Files

### Naming Conventions

| Content type | Suggested filename |
|-------------|-------------------|
| Tool requirements | `tools.md` |
| Step-by-step process | `workflow.md` or `process.md` |
| Priority/label definitions | `classification.md` |
| Output format/template | `templates.md` or `output-format.md` |
| Voice and tone rules | `voice-tone.md` |
| Database details | `database.md` or `data-model.md` |
| Quality checks | `quality-checks.md` |
| Examples (good/bad) | `examples.md` |
| API details | `api-reference.md` |
| SQL queries | `sql-queries.md` |
| Heuristics/decision logic | `heuristics.md` |

### File Structure

Each reference file should:

1. Start with a `# Title` heading
2. Be self-contained — readable without context from other files
3. Use markdown tables, code blocks, and lists for scannability
4. Not reference other reference files (avoid circular dependencies)

### What NOT to Split

Keep things together when they are tightly coupled:

- A classification table and its explanation (both go in `classification.md`)
- A template and its field descriptions (both go in `templates.md`)
- Input requirements and validation rules for the same input

### Preserving Content

When extracting sections:

- Copy content **exactly** — do not rewrite, summarize, or improve it
- Preserve all tables, code blocks, examples, and formatting
- Keep section headings as-is (they may become the file's top-level heading)
- If a section references another section, add a note pointing to the correct file

## Validation After Restructuring

After restructuring, verify:

1. **No content lost** — Every line from the original SKILL.md exists in exactly one file
2. **No duplication** — No content appears in more than one file
3. **Orchestrator is clean** — SKILL.md contains only workflow steps and file references
4. **Files are self-contained** — Each reference file can be understood on its own
5. **Paths are correct** — All `${CLAUDE_PLUGIN_ROOT}` paths point to real files
