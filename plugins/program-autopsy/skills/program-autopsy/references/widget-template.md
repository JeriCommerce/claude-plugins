# Autopsy Widget Template

Generate the final visual summary using `visualize:show_widget` with `title: "program_autopsy_{{SLUG}}"`.

The widget must follow the Visualize design system: CSS variables for colors, no gradients/shadows, flat surfaces, 0.5px borders, transparent background. Load Chart.js from cdnjs if a timeline chart is needed.

## Data aggregation

Before rendering, aggregate raw data into these variables:

- `shopName`: from PostHog URLs or BetterStack brand extraction logs
- `sessionMinutes`: diff between install and uninstall timestamps
- `customerCount`: from Metabase, or "N/A" if deleted
- `planType`: from Metabase subscriptions, or "Free"
- `timelineSteps[]`: array of `{time, description, color}` from PostHog events
- `configuredItems[]`: array of `{name, status: configured|visited|untouched, detail}`
- `churnSignals[]`: array of `{name, evidence, confidence: high|medium|low}`
- `diagnosticText`: synthesized conclusion in Spanish

## Widget structure

### Section A — Header (metric cards)

`var(--color-background-secondary)` cards in CSS grid (3-4 columns, `gap: 12px`).

| Card | Value | Source |
|------|-------|--------|
| Shop | Domain name (without `.myshopify.com`) | BetterStack / PostHog URL params |
| Session | Duration in minutes (e.g. "31 min") | Install to uninstall timestamps |
| Customers | Count, or "DB deleted" if program gone | Metabase |
| Plan | Plan type or "Free" | Metabase |

Each card: 13px muted label on top, 20px/500 value below. If deleted, show "Deleted" in `color: var(--color-text-danger)`.

### Section B — Journey timeline (vertical stepper)

Vertical timeline of the merchant's journey. Each step:
- Timestamp (13px, muted, fixed-width ~70px)
- Colored dot (8px circle)
- Description (14px)

Dot colors:
- **Purple** (`#7000FF`): configuration saves
- **Green** (`--color-text-success`): positive signals (program created, earning flow configured)
- **Amber** (`--color-text-warning`): warning signals (subscription page, technical settings)
- **Red** (`--color-text-danger`): errors or churn signals
- **Gray** (`--color-text-secondary`): neutral navigation

Collapse consecutive `$web_vitals` and redundant `$set` events. Max ~15 steps. Use program's local timezone if detectable, otherwise UTC.

### Section C — Configuration summary (pills/badges)

Show what was configured vs untouched using small pills:
- **Configured** (green bg): items saved or modified
- **Visited** (amber bg): pages visited but not saved
- **Untouched** (gray bg): major features never visited

Categories: Wallet design, Webapp, Loyalty settings, Earning flows, Rewards, Tiers, Referrals, Push notifications, Integrations/Klaviyo, POS/NFC, Subscription.

### Section D — Churn signals (signal cards)

2-3 cards with significant findings:
- Left border accent (4px, colored by severity: red=high, amber=medium)
- No border-radius on card
- Bold signal name (14px, 500)
- Evidence text (13px, muted)
- Confidence badge: "Alta" / "Media" / "Baja"

### Section E — Diagnostic verdict

Single block with `border-left: 4px solid var(--color-border-info)`:
- "Diagnostico" header (16px/500)
- 2-3 sentence summary in Spanish
- Two `sendPrompt()` buttons: "Contactar al merchant" (triggers recovery skill) and "Explorar mejoras de onboarding"

## Loading messages

Use playful loading messages in Spanish:
```
["Interrogando a Metabase", "Rastreando huellas en BetterStack", "Leyendo el diario del merchant", "Redactando el veredicto"]
```
