# JeriCommerce — Product Context

## What is JeriCommerce

JeriCommerce is a loyalty platform for Shopify merchants. It lets merchants create and manage loyalty programs where their customers earn points, unlock rewards, and receive wallet passes (Apple Wallet & Google Wallet) that update in real time.

## Applications & Components

### Admin (Embedded Shopify App)

The merchant-facing application, embedded inside the Shopify admin panel. Merchants use it to:

- Configure their loyalty program (branding, points rules, tiers)
- Manage customers and their balances
- Create and manage campaigns (push notifications, scheduled, triggered, link-based)
- Set up rewards and coupons
- View analytics and performance dashboards
- Manage locations and scanning devices
- Configure integrations (Klaviyo, Loyalty Lion, etc.)

**Tech**: React app embedded in Shopify Admin via Shopify App Bridge.

### Customer App

The end-customer facing web application. Customers of the merchant use it to:

- Register for the loyalty program
- View their points balance and tier status
- Browse and claim available rewards
- Download their wallet pass (Apple/Google)
- View their transaction history
- Manage their profile and referrals

**Tech**: Web application accessible via custom domain or pass link.

### Backend API

The core API that powers everything. It handles:

- Customer registration and authentication
- Points balance management (increase, decrease, transactions)
- Wallet pass generation and updates (Apple Wallet & Google Wallet)
- Campaign orchestration and push notification delivery
- Coupon and reward management
- Shopify webhook processing (orders, customers, refunds)
- Integration with third-party services (Klaviyo, Loyalty Lion, Convercus)
- Analytics aggregation

**Tech**: NestJS API (Node.js/TypeScript).

### Shopify Extensions

Extensions that run inside the merchant's Shopify storefront:

- **Theme App Extensions**: Widgets embedded in the storefront theme (points display, registration forms)
- **Checkout Extensions**: Components in the Shopify checkout flow
- **Script Tags**: JavaScript injected into the storefront for tracking

### Wallet Passes

Digital loyalty cards for customers:

- **Apple Wallet**: `.pkpass` files that install in the iOS Wallet app
- **Google Wallet**: Google Pay pass objects
- Both update dynamically when balance, tier, or campaign data changes
- Push notifications can be sent directly to installed passes

## Key Domain Concepts

| Concept | Description |
|---------|-------------|
| **Program** | A merchant's loyalty program. Identified by UUID or slug. Everything belongs to a program. |
| **Customer** | An end-user enrolled in a merchant's program. Has a balance, tier, passes, and transactions. |
| **Pass** | A digital wallet card (Apple or Google). Linked to a customer. Updates when data changes. |
| **Balance** | Points balance of a customer. Stored as integer. Modified via transactions. |
| **Transaction** | A record of a balance change (earning or spending points). Amount is in cents. |
| **Campaign** | A communication sent to customers (push notification). Types: Location, Scheduled, Links, Triggered, Anonymous. |
| **Reward** | Something a customer can redeem with their points (e.g., a discount coupon). |
| **Coupon** | A discount code that can be assigned to customers or claimed. Has batches of codes. |
| **Tier** | A level in the loyalty program (e.g., Bronze, Silver, Gold). Based on points or activity. |
| **Integration** | A connection between the program and an external service (Shopify, Klaviyo, etc.). |
| **Feature** | A capability that can be enabled per integration (e.g., loyalty, rewards, users, segments). |

## Environments

| Environment | API Base URL | Swagger Docs (JSON) |
|-------------|-------------|---------------------|
| Staging | `https://s.api.jericommerce.com` | `https://s.api.jericommerce.com/docs-json` |
| Production | `https://api.jericommerce.com` | `https://api.jericommerce.com/docs-json` |

**Default to staging** for all testing unless the user explicitly requests production.

## Authentication

Two methods are available for API calls:

| Method | Header | Typical usage |
|--------|--------|---------------|
| Bearer Token (JWT) | `Authorization: Bearer <token>` | Admin and authenticated endpoints |
| API Key | `api-key: <key>` | Program-level API access |

To find API keys for a program, query Metabase:
```sql
SELECT ak.key, ak.name, p.name AS program_name
FROM api_keys ak
JOIN programs p ON ak.program_id = p.id
WHERE p.slug = '{slug}' AND ak.deleted_at IS NULL
```
