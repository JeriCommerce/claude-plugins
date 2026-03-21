# Issue Classification

## Priority

| Priority | Criteria |
|----------|----------|
| Urgent | System down, data loss, all customers affected, revenue impact |
| High | Major feature broken, multiple customers affected, no workaround |
| Normal | Feature partially broken, workaround available, limited impact |
| Low | Minor visual issue, edge case, cosmetic, enhancement request |

## Labels

Apply one or more area labels based on which part of the system is affected. Usually only one applies, but it can be more.

| Label | What it covers |
|-------|---------------|
| `admin` | Embedded Shopify app where the merchant configures their loyalty program, rewards, campaigns, etc. |
| `backend` | API errors, server-to-server connections, database issues, webhook failures |
| `customer-app` | End-customer facing app where merchants' customers manage their points, rewards, referrals, profile |
| `extensions` | Shopify extensions (theme app extensions, checkout extensions, script tags) |

If unsure which area is affected, ask the user: "Is this something the merchant sees in Shopify (admin), something their customers see (customer-app), a server/API error (backend), or related to a Shopify extension?"
