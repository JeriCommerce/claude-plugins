# Template Engine Reference

## Data Attributes

| Attribute | Purpose |
|-----------|---------|
| `id="jeri-loyalty-page-root"` | Required root element ID |
| `data-shop="{{ shop.permanent_domain }}"` | Shop identifier (Liquid variable) |
| `data-locale="{{ request.locale.iso_code }}"` | Locale for translations (Liquid variable) |
| `class="jeri-loyalty-page--loading"` | Loading state class — JS removes it automatically |
| `data-jeri="section.earnings"` | Earnings section wrapper — auto-hides if no earnings |
| `data-jeri="section.tiers"` | Tiers section wrapper — auto-hides if no tiers |
| `data-jeri="earning.type"` | Earning type element for conditional icon display |
| `data-jeri-show-if="<type>"` | Show element only for a specific earning type |
| `data-jeri-html="tier.content"` | Inject tier HTML content (tiers may contain HTML) |

## Variable Interpolation

Use `{variable}` syntax in **text nodes only** (never in HTML attributes):

### Earning fields

| Variable | Description |
|----------|-------------|
| `{earning.title}` | Earning rule title |
| `{earning.description}` | Earning rule description |
| `{earning.points}` | Points awarded |

### Tier fields

| Variable | Description |
|----------|-------------|
| `{tier.name}` | Tier name |
| `{tier.content}` | Tier description (may contain HTML — use `data-jeri-html`) |
| `{tier.pointsRequired}` | Points needed to reach tier |

## Earning Types

Each earning type gets a unique icon. Use `data-jeri="earning.type" data-jeri-show-if="<type>"` with a hardcoded `src` per image.

| Type | Description | Notes |
|------|-------------|-------|
| `purchase` | Points per purchase | Most common |
| `social` | Follow on social media | ALL social networks share this type |
| `referral` | Refer a friend | |
| `profile` | Complete user profile | |
| `verify` | Verify email/account | |
| `wallet` | Install wallet pass | |
| `scan` | Scan QR in store | |
| `link` | Click wallet link | |

### Icon mapping pattern

```html
<!-- One img per earning type, only the matching one is shown -->
<img data-jeri="earning.type" data-jeri-show-if="purchase" src="https://example.com/purchase-icon.svg" alt="">
<img data-jeri="earning.type" data-jeri-show-if="social" src="https://example.com/social-icon.svg" alt="">
<img data-jeri="earning.type" data-jeri-show-if="referral" src="https://example.com/referral-icon.svg" alt="">
<!-- ...etc -->
```

## Tier Images

The JS engine does NOT inject tier images. Use CSS `:nth-child()` selectors with `content: url()` to assign images by position:

```css
.prefix__tier-badge:nth-child(1)::before { content: url('https://example.com/bronze.png'); }
.prefix__tier-badge:nth-child(2)::before { content: url('https://example.com/silver.png'); }
.prefix__tier-badge:nth-child(3)::before { content: url('https://example.com/gold.png'); }
```

## CTA Widget Routes

Use these `#jeri=` hash fragment routes for CTA buttons. For guests, redirect to the login page instead.

| Route | Action |
|-------|--------|
| `#jeri=loyalty/rewards` | Open rewards catalog |
| `#jeri=referrals` | Open referral program |
| `#jeri=loyalty` | Open loyalty landing |
| `#jeri=loyalty/earning-flows` | Open earning flows |

### CTA pattern

```html
<!-- For logged-in customers -->
<a href="#jeri=loyalty/rewards">View Rewards</a>

<!-- For guests — use schema setting for redirect URL -->
<a href="{{ section.settings.guest_cta_url | default: '/account/login' }}">Sign In</a>
```

## Flatten Earnings Layout

Use `display: contents` on group/container wrappers to achieve a unified grid layout for earnings:

```css
.prefix__earnings-group { display: contents; }
```
