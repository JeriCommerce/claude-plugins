# Bug Reporter Plugin

Report bugs and customer issues to Linear through a guided conversational flow.

## What it does

Guides users through structured bug reporting — asking the right questions to classify priority, area, and impact — then creates a well-formatted issue in the JeriCommerce Linear project with all context, screenshots, and videos attached.

## Prerequisites

- **Linear connector extension** must be enabled in the Desktop app (Settings → Extensions → Linear). This plugin uses Linear tools (`create_issue`, `create_attachment`, etc.) to manage issues. If the Linear extension is not available, the plugin will not work.

## Usage

### Cowork mode (plugin installed)

```
/report-bug
/report-bug "Wallet pass not updating points for merchant old-jeffrey"
```

### Chat mode

Describe the issue to Claude with the Linear connector enabled. It will guide you through the reporting process.

## Components

| Component | Name | Purpose |
|-----------|------|---------|
| Command | `/report-bug` | Entry point — starts the bug reporting flow |
| Skill | `bug-reporter` | Guided reporting workflow: gather info, classify, confirm, create issue |

## Where issues are created

All issues go to:
- **Team**: Development
- **Project**: [Support and Customer Bugs](https://linear.app/jericommerce/project/support-and-customer-bugs-4735f4c9af8f/overview)

## What it asks

1. What happened? (description)
2. Where? (affected area)
3. Who reported it? (customer/merchant)
4. How to reproduce?
5. Screenshots or videos? (accepts files)
6. Since when? (timeline)
7. How many affected?
8. Workaround available?

Questions are asked conversationally, not as a form. The plugin adapts based on your answers.
