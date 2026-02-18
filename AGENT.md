# Agent Guidelines — JeriCommerce Claude Plugins

> Internal development guide for contributors working on this marketplace.
> Claude should read this file before making any changes to the repo.

---

## Repository Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json       # Plugin catalog (update on every new plugin)
├── AGENT.md                   # This file — development conventions
├── README.md                  # Public-facing docs & plugin creation guide
├── .gitignore
└── plugins/
    └── {plugin-name}/         # One directory per plugin
        ├── .claude-plugin/
        │   └── plugin.json    # Plugin manifest (required)
        ├── README.md          # Plugin documentation (required)
        ├── skills/            # Auto-invoked by Claude
        ├── commands/          # User-invoked via /plugin:command
        ├── agents/            # Specialized subagents
        └── references/        # Supporting data files
```

---

## Versioning

All versions in this repo use **date-based semver** format:

```
YYYYMM.DD.N
```

- `YYYYMM` = year + month (MAJOR) — e.g., `202602`
- `DD` = day of month (MINOR) — e.g., `18`
- `N` = release number of the day (PATCH) — starts at `0`
- Example: `202602.18.0`, `202602.18.1`, `202602.19.0`

This format is **semver-compliant** (`MAJOR.MINOR.PATCH`) which is required by Claude Code's plugin system.

**Where versions live** (keep in sync):
1. `plugins/{name}/.claude-plugin/plugin.json` → `version`
2. `.claude-plugin/marketplace.json` → plugin entry `version`
3. `.claude-plugin/marketplace.json` → `metadata.version` (marketplace-level)
4. `plugins/{name}/skills/*/SKILL.md` → frontmatter `version`
5. `README.md` → Available Plugins table

When bumping a version, update **all five locations**.

---

## Creating a New Plugin — Checklist

1. [ ] Create `plugins/{plugin-name}/` directory
2. [ ] Create `.claude-plugin/plugin.json` with `name`, `description`, `version`, `author`, `keywords`
3. [ ] Create `README.md` explaining what the plugin does and its prerequisites
4. [ ] Add at least one of: `skills/`, `commands/`, or `agents/`
5. [ ] Add entry to root `.claude-plugin/marketplace.json` → `plugins` array
6. [ ] Add row to root `README.md` → Available Plugins table
7. [ ] Bump `metadata.version` in `marketplace.json`
8. [ ] Commit with `feat: add {plugin-name}`

---

## Naming Conventions

| Element | Convention | Example |
|---------|-----------|---------|
| Plugin directory | `kebab-case` | `loyalty-specialist` |
| Plugin name (in manifests) | `kebab-case` | `loyalty-specialist` |
| Skill directory | `kebab-case` | `loyalty-report-generator` |
| Command file | `kebab-case.md` | `generate-report.md` |
| Agent file | `kebab-case.md` | `data-collector.md` |
| Reference files | `kebab-case.md` | `sql-queries.md` |

---

## Writing Skills

Skills are the primary way to give Claude domain knowledge. They activate automatically when Claude determines they're relevant.

### Rules

- The `description` field in frontmatter is the most important part — Claude reads it to decide when to activate the skill
- Include specific **trigger phrases** in the description (e.g., "generate a loyalty report", "analyze a program")
- Keep `SKILL.md` focused on **workflow and instructions** — put large data in `references/`
- Use `${CLAUDE_PLUGIN_ROOT}` to reference files within the plugin directory
- Always specify **which MCP tools** the skill requires (exact tool name, parameters)

### Template

```markdown
---
name: {skill-name}
description: >
  Use when the user asks to "{action 1}", "{action 2}", or "{action 3}".
  Also use when {scenario}.
version: YYYY.MM.DD-N
---

# {Skill Title}

Brief description of what this skill does.

## Required MCP Tools

List every external tool the skill depends on:

- `mcp__{server}__{tool}` with parameter X = Y

If a required tool is unavailable, inform the user.

## Workflow

1. Step one
2. Step two
3. Step three

## Reference Files

- `references/file.md` — Description of what it contains
```

---

## Writing Commands

Commands are explicit entry points triggered by the user via `/plugin-name:command-name`.

### Rules

- Always include `description` in frontmatter (shown in `/help`)
- Include `argument-hint` to show expected input format
- Parse `$ARGUMENTS` at the top of the command
- Reference skills and references via `${CLAUDE_PLUGIN_ROOT}`
- Commands should orchestrate (read skill → execute steps), not duplicate skill content

### Template

```markdown
---
description: One-line description of the command
argument-hint: [required-arg] ["optional arg"]
---

Parse `$ARGUMENTS` to extract:
- **Argument 1** (`$1`): description
- **Argument 2** (optional): description

Follow these steps:

1. Read the skill at `${CLAUDE_PLUGIN_ROOT}/skills/{skill}/SKILL.md`
2. Read references at `${CLAUDE_PLUGIN_ROOT}/skills/{skill}/references/{file}.md`
3. Execute the workflow from the skill
4. Produce output
```

---

## Writing Agents

Agents run in isolated context windows. Use them for tasks that produce large output, need restricted tools, or benefit from parallel execution.

### Rules

- Always restrict `tools` to the minimum needed
- Write a clear `description` — Claude uses it to decide when to delegate
- Include "use proactively" in description if the agent should auto-trigger
- Preload skills via `skills` field instead of reading them at runtime
- Reference MCP tools by full name (e.g., `mcp__metabase__run-native-query`)

### Template

```markdown
---
name: {agent-name}
description: >
  {When Claude should delegate to this agent}.
  Use proactively when {scenario}.
tools: Read, Grep, Glob, Bash
model: sonnet
skills:
  - {skill-to-preload}
---

You are a {role} specialist.

When invoked:
1. Step one
2. Step two
3. Return a summary of findings
```

---

## External Tool References

When a plugin depends on an external tool (connector extension, MCP server, etc.), **always** document:

1. The exact tool name (e.g., `Metabase:execute`, `mcp__{server}__{tool}`)
2. Required parameters with example values
3. What happens if the tool is unavailable
4. Where to enable it (Settings → Extensions, MCP config, etc.)

### Connector extensions (Desktop app)

Tools from connector extensions use the format `{Connector}:{tool}`:

```markdown
## Required Tools — Metabase Connector

All queries MUST be executed through the Metabase connector:

Tool: `Metabase:execute`
Parameters:
  database_id: 3
  query: "SELECT ..."

If unavailable, inform the user to enable it in Settings → Extensions.
```

### MCP servers

Tools from MCP servers use the format `mcp__{server}__{tool}`:

```markdown
Tool: `mcp__github__search_repositories`
Parameters:
  query: "org:JeriCommerce"
```

---

## Commit Conventions

Follow [Conventional Commits](https://www.conventionalcommits.org/):

| Prefix | Use |
|--------|-----|
| `feat:` | New plugin or major new component |
| `fix:` | Bug fix or correction in existing plugin |
| `docs:` | Documentation-only changes |
| `chore:` | Maintenance (version bumps, gitignore, etc.) |
| `refactor:` | Restructuring without behavior change |

Scope is optional: `feat(loyalty-specialist): add tier analysis query`

---

## Pull Request Checklist

Before merging:

- [ ] Plugin has `.claude-plugin/plugin.json` with all required fields
- [ ] Plugin has `README.md` with prerequisites and usage
- [ ] All MCP dependencies are documented with exact tool names
- [ ] Version is set in date format (`YYYY.MM.DD-N`) across all locations
- [ ] Entry added to `marketplace.json`
- [ ] Row added to root `README.md` table
- [ ] Marketplace `metadata.version` bumped
- [ ] No hardcoded paths — uses `${CLAUDE_PLUGIN_ROOT}` for all internal references
- [ ] No direct database access — all queries go through MCP tools
