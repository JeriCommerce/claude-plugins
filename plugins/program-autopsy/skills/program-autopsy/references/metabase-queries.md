# Metabase SQL Queries

All queries run against **database_id: 3** (production) via `Metabase:execute`.

## 1. Program basics

```sql
SELECT p.id, p.slug, p.name, p.created_at, p.updated_at, p.integration, p.partner,
       p.currency, p.nfc_enabled, p.default_language, p.barcode_format,
       s.id as sub_id, s.type as plan_type, s.status as sub_status,
       s.created_at as sub_created, s.capped_amount
FROM programs p
LEFT JOIN subscriptions s ON s.program_id = p.id
WHERE p.slug = '{{SLUG}}'
```

If empty: "Program deleted from DB (likely uninstalled)."

## 2. Customers and passes

```sql
SELECT count(*) as total_customers,
       count(CASE WHEN pa.status = '1' THEN 1 END) as installed_passes,
       count(CASE WHEN pa.kind = 'store-card' THEN 1 END) as store_cards
FROM customers c
LEFT JOIN passes pa ON pa.customer_id = c.id
WHERE c.program_id = '{{PROGRAM_UUID}}'
```

## 3. Rewards, earning flows, tiers

```sql
SELECT 'rewards' as entity, count(*) as total FROM rewards WHERE program_id = '{{PROGRAM_UUID}}'
UNION ALL
SELECT 'earning_flows', count(*) FROM earning_flows WHERE program_id = '{{PROGRAM_UUID}}'
UNION ALL
SELECT 'tiers', count(*) FROM tiers WHERE program_id = '{{PROGRAM_UUID}}'
UNION ALL
SELECT 'coupon_batches', count(*) FROM coupon_batches WHERE program_id = '{{PROGRAM_UUID}}'
UNION ALL
SELECT 'locations', count(*) FROM locations WHERE program_id = '{{PROGRAM_UUID}}'
UNION ALL
SELECT 'devices', count(*) FROM devices WHERE program_id = '{{PROGRAM_UUID}}'
```

## 4. Transactions

```sql
SELECT count(*) as total_transactions,
       count(CASE WHEN order_id IS NOT NULL AND status = 'processed' THEN 1 END) as purchases,
       sum(CASE WHEN amount > 0 THEN amount ELSE 0 END) / 100.0 as total_earned_eur,
       sum(CASE WHEN amount < 0 THEN abs(amount) ELSE 0 END) / 100.0 as total_redeemed_eur
FROM transactions
WHERE program_id = '{{PROGRAM_UUID}}'
```

## 5. Customer events

```sql
SELECT type::text, count(*) as cnt
FROM customer_events
WHERE program_id = '{{PROGRAM_UUID}}'
GROUP BY type::text
ORDER BY cnt DESC
```

## 6. Program integrations (critical churn signal)

```sql
SELECT id, type, status, created_at, updated_at
FROM program_integrations
WHERE program_id = '{{PROGRAM_UUID}}'
ORDER BY created_at ASC
```

## Column reference

- **programs**: snake_case columns (`created_at`, `default_language`, etc.)
- **subscriptions**: `program_id`, `type` (plan), `status`, `capped_amount`
- When unsure about column names, query `information_schema.columns`
