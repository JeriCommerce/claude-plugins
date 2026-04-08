# Program Autopsy

Systematic investigation of a JeriCommerce program's lifecycle, health, or churn. Queries three data sources — Metabase, BetterStack, and PostHog — to build a complete picture of what a merchant did, what went wrong, and why they may have churned. Then helps you reach out to recover them.

## Prerequisites

- **Metabase connector** enabled in Settings > Extensions
- **BetterStack** MCP server connected
- **PostHog** MCP server connected
- **Visualize** extension for rendering the HTML report widget
- **Gmail** MCP server connected (optional, for drafting recovery emails)

## Skills

### program-autopsy

Investigates a program's full lifecycle across all three data sources.

**Automatic trigger:** Mention a program slug or ask about a program's health:

```
Analiza el programa tbftbr-8i
What happened with new-polinesia?
Por que se desinstalo example-store.myshopify.com?
```

**Explicit:** `/program-autopsy:investigate tbftbr-8i`

Produces a visual HTML widget with header cards, journey timeline, configuration summary, churn signals, and diagnostic verdict.

### churn-recovery

Drafts a personalized recovery message based on autopsy findings.

**Automatic trigger:** After an autopsy, ask to contact the merchant:

```
Contacta al cliente
Write a recovery email
Intenta recuperar al merchant
```

**Explicit:** `/program-autopsy:recover` (uses autopsy data already in conversation)

Produces: subject line options, email body, sending notes, and an alternative short version for Intercom/chat.

## Workflow

The intended flow is:

1. Run the autopsy: `/program-autopsy:investigate some-slug`
2. Review the findings
3. Draft recovery outreach: `/program-autopsy:recover`
4. Review the draft, personalize, and send
