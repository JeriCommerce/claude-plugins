# Database Details

- **Metabase database_id: 3** (`jericommerce-db`, PostgreSQL 17.7, timezone `Europe/Madrid`)
- All monetary amounts in `transactions.amount` are in **cents** (divide by 100)
- All UUIDs are `uuid` type — always cast string inputs: `'value'::uuid`
- Soft deletes use `deleted_at` column on: campaigns, coupons, rewards (NOT programs, customers, passes)

## Enums

- **Pass status enums**: 0 = created/pending, 1 = installed/active, 2 = uninstalled
- **Pass type enums**: 0 = Apple Wallet, 1 = Google Wallet
- **Campaign type enums**: 0 = Location, 1 = Scheduled, 2 = Links, 3 = Triggered, 4 = Anonymous
- **Transaction status**: filter by `status IN ('completed', 'processed')`
