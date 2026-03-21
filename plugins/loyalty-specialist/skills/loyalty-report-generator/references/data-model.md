# Data Model & Database Details

## Input

The user provides one of:
- A **program UUID** (e.g. `91133c45-9401-4c6c-9f90-284ef7cf5b6f`)
- A **program slug** (e.g. `old-jeffrey`)
- A **program name** (e.g. `Old Jeffrey`) — search with `ILIKE` if needed

If ambiguous, ask the user to confirm which program.

## Integration Data Model

Understanding how integrations work is critical for the integration detection step:

- **`integrations`** — Catalog of available integration providers (e.g., `jericommerce`, `shopify`, `shopify/loyalty-lion`, `convercus`)
- **`features`** — Catalog of capabilities (e.g., `loyalty`, `rewards`, `users`, `segments`). Features with `multi = false` can only be provided by ONE integration per program
- **`program_integrations`** — Links a program to its active integrations, with an `enabled_features` UUID array indicating which features each integration provides for that program

To determine if JeriCommerce manages loyalty for a program: check if there's a `program_integrations` record where the integration is `jericommerce` AND the `enabled_features` array contains the ID of the `loyalty` feature (resolved dynamically from the `features` table).

## Database Details

- **Metabase database_id: 3** (`jericommerce-db`, PostgreSQL 17.7, timezone `Europe/Madrid`)
- All monetary amounts in `transactions.amount` are in **cents** (divide by 100)
- All UUIDs are `uuid` type — always cast string inputs: `'value'::uuid`
- Soft deletes use `deleted_at` column (present on: `campaigns`, `coupons`, `rewards`; NOT on: `programs`, `customers`, `passes`)

## Key Data Notes

- **Pass status enums**: 0 = created/pending, 1 = installed/active, 2 = uninstalled
- **Pass type enums**: 0 = Apple Wallet, 1 = Google Wallet
- **Campaign type enums** (`campaigns.type` — `CampaignTypeEnum`): 0 = Location, 1 = Scheduled, 2 = Links, 3 = Triggered, 4 = Anonymous
- **Notification type enums** (`notifications.type` — `NotificationTypeEnum`): 0 = BalanceChanged, 1 = TierStatusChanged, 2 = Marketing, 3 = RewardAvailable
- **Location campaigns (type 0)** do not support CTR tracking — exclude them from CTR analysis
- **Customer origin enums**: SHOPIFY, JERICOMMERCE, INITIAL_SYNC, UNKNOWN
- **Engagement flows**: INCENTIVIZE_PURCHASES, INSTALL_WALLET_PASS, MEMBER_GET_MEMBER, FOLLOW_US, COMPLETE_USER_PROFILE, CUSTOMER_SCANNED, VERIFY_ACCOUNT, WALLET_LINK_CLICK
- **Transactions amount**: stored in cents, always divide by 100
- **Coupon batches**: `discount_type` is 'fixed' (value in whole currency) or 'percentage' (value is percentage number)
- **Transaction status**: filter by `status IN ('completed', 'processed')` — both represent successful transactions
- **Number formatting**: use European format — dots for thousands (`1.633.928`), commas for decimals (`95,89 EUR`)
- If a query returns empty results, note it in the report as "No data available" — do NOT skip the section
- If the program has an `integration` field set (e.g. LoyaltyLion), note that loyalty data may be managed externally

## Data Milestones

Important dates and facts that affect how data should be interpreted. Always consider these when analyzing results — mention them in the report when they impact a metric.

| Date / Fact | Impact |
|-------------|--------|
| 2026-02-17 | Start of coupon code → transaction linking. Before this date, `coupon_codes.transaction_id` is always NULL. Do NOT interpret pre-2026-02-17 NULL values as "unused codes" — tracking simply didn't exist yet. |
| All transactions | Only non-anonymous (identified) customers are stored. Anonymous purchases are NOT in the database. Revenue and transaction metrics reflect only identified loyalty members, not total store revenue. |

When generating the report:
- If analyzing coupon redemption rates, note that `codes_used` (linked to transactions) is only reliable from 2026-02-17 onwards
- If comparing revenue metrics, clarify that figures represent identified member revenue only, not total store revenue
- Add a footnote in the relevant sections when a milestone affects the data interpretation
