# JeriCommerce Claude Code Plugins

Official plugin marketplace for [Claude Code](https://code.claude.com) by JeriCommerce.

## Installation

Add this marketplace to your Claude Code:

```
/plugin marketplace add JeriCommerce/claude-plugins
```

Then install individual plugins:

```
/plugin install loyalty-specialist@jericommerce-plugins
```

## Available Plugins

| Plugin | Description | Version |
|--------|-------------|---------|
| [loyalty-specialist](./plugins/loyalty-specialist/) | Generate professional PDF loyalty program performance reports by querying Metabase and analyzing program data | 0.1.0 |

## Updating

To get the latest plugins after changes are pushed:

```
/plugin marketplace update
```

---

## Creating a New Plugin

### 1. Directory structure

Create a new folder under `plugins/` with this structure:

```
plugins/my-plugin/
├── .claude-plugin/
│   └── plugin.json        # Required: plugin metadata
├── skills/                 # Optional: auto-invoked by Claude when relevant
│   └── my-skill/
│       ├── SKILL.md        # Skill definition (frontmatter + instructions)
│       └── references/     # Optional: supporting files the skill can read
│           └── data.md
├── commands/               # Optional: user-invoked via /my-plugin:command-name
│   └── my-command.md
└── README.md               # Recommended: plugin documentation
```

All directories are optional except `.claude-plugin/`. Include only what your plugin needs.

### 2. Plugin manifest (`plugin.json`)

Create `.claude-plugin/plugin.json` with your plugin's metadata:

```json
{
  "name": "my-plugin",
  "description": "Short description of what the plugin does",
  "version": "1.0.0",
  "author": {
    "name": "JeriCommerce"
  },
  "keywords": ["relevant", "search", "terms"]
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Unique identifier in kebab-case. Becomes the namespace for commands (`/my-plugin:command`) |
| `description` | No | Shown in the plugin manager when browsing plugins |
| `version` | No | Semantic versioning (`MAJOR.MINOR.PATCH`). Bump this on every change or users won't see updates |
| `author` | No | Attribution. `{ "name": "...", "email": "..." }` |
| `keywords` | No | Tags for plugin discovery |

### 3. Skills (auto-invoked by Claude)

Skills are loaded automatically when Claude determines they're relevant to the current task. Create a folder inside `skills/` with a `SKILL.md` file.

**`skills/my-skill/SKILL.md`:**

```markdown
---
name: my-skill
description: >
  Describe WHEN Claude should use this skill. Include trigger phrases
  and scenarios. Example: "Use when the user asks about X, works on Y,
  or mentions Z."
---

# My Skill

Instructions for Claude when this skill is activated.

## Reference Files

You can point Claude to supporting files in the same directory:

Read `${CLAUDE_PLUGIN_ROOT}/skills/my-skill/references/data.md` for details.
```

**Frontmatter fields:**

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Skill identifier |
| `description` | Yes | Tells Claude when to activate the skill. Be specific with trigger phrases |
| `version` | No | Skill version |
| `disable-model-invocation` | No | Set to `true` if only the user should invoke it (not Claude automatically) |

**Tips:**
- Use `${CLAUDE_PLUGIN_ROOT}` to reference files within your plugin directory
- Put large reference data in `references/` subdirectory to keep `SKILL.md` focused
- The `description` field is critical — Claude reads it to decide when to use the skill

### 4. Commands (user-invoked via slash command)

Commands are triggered explicitly by the user typing `/plugin-name:command-name`. Create `.md` files inside `commands/`.

**`commands/my-command.md`:**

```markdown
---
description: Short description shown in /help
argument-hint: [required-arg] ["optional arg"]
---

Parse `$ARGUMENTS` to extract:
- **First argument** (`$1`): description
- **Second argument** (optional): description

## Steps

1. First, read the skill file at `${CLAUDE_PLUGIN_ROOT}/skills/my-skill/SKILL.md`
2. Do something with the arguments
3. Produce output
```

**Frontmatter fields:**

| Field | Required | Description |
|-------|----------|-------------|
| `description` | Yes | Shown when the user runs `/help` |
| `argument-hint` | No | Usage hint shown to the user |

**Variables available:**
- `$ARGUMENTS` — Full text after the command name
- `$1`, `$2`, ... — Positional arguments
- `${CLAUDE_PLUGIN_ROOT}` — Absolute path to the plugin directory

### 5. Register in the marketplace

Add your plugin entry to `.claude-plugin/marketplace.json` in the `plugins` array:

```json
{
  "name": "my-plugin",
  "source": "./plugins/my-plugin",
  "description": "Same or similar description as plugin.json",
  "version": "1.0.0",
  "author": {
    "name": "JeriCommerce"
  },
  "keywords": ["relevant", "terms"],
  "category": "development"
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Must match the `name` in `plugin.json` |
| `source` | Yes | Relative path to the plugin directory (always `./plugins/plugin-name`) |
| `description` | No | Shown when browsing the marketplace |
| `version` | No | Should match `plugin.json`. **Avoid setting it in both places** — `plugin.json` always wins |
| `category` | No | For organization (e.g., `development`, `analytics`, `devops`) |

### 6. Update the Available Plugins table

Add a row to the table in this README:

```markdown
| [my-plugin](./plugins/my-plugin/) | Short description | 1.0.0 |
```

### 7. Commit and push

```bash
git add -A
git commit -m "feat: add my-plugin"
git push origin main
```

Users will see the new plugin after running `/plugin marketplace update`.

---

## Quick Reference: Plugin Components

| Component | Location | Invoked by | Use case |
|-----------|----------|------------|----------|
| **Skills** | `skills/name/SKILL.md` | Claude (automatic) | Domain knowledge, context, workflows that Claude applies when relevant |
| **Commands** | `commands/name.md` | User (`/plugin:name`) | Explicit actions with arguments (generate reports, run analyses) |
| **Agents** | `agents/name.md` | Claude or user | Specialized subagents for complex tasks |
| **Hooks** | `hooks/hooks.json` | Events (automatic) | React to events like file saves, tool use, etc. |
| **MCP Servers** | `.mcp.json` | Automatic | Connect to external tools and APIs |

---

## Example: Minimal Plugin

The simplest possible plugin with one skill:

```
plugins/my-helper/
├── .claude-plugin/
│   └── plugin.json
└── skills/
    └── helper/
        └── SKILL.md
```

**`plugin.json`:**
```json
{
  "name": "my-helper",
  "description": "Provides context about X",
  "version": "1.0.0"
}
```

**`SKILL.md`:**
```markdown
---
name: helper
description: Use when the user asks about X or works on X-related code.
---

When helping with X, follow these rules:
1. Always do A
2. Never do B
3. Prefer C over D
```

**`marketplace.json` entry:**
```json
{
  "name": "my-helper",
  "source": "./plugins/my-helper",
  "description": "Provides context about X",
  "version": "1.0.0"
}
```

---

## License

Proprietary — JeriCommerce
