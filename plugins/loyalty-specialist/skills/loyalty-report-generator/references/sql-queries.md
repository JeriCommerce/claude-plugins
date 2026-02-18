# SQL Queries — Data Collection Sequence

Execute these queries **in order**, using the program UUID obtained in Step 1. Run all queries before generating the report.

## Step 1: Program Identity

```sql
SELECT id, name, slug, contact_email, integration, created_at, currency,
       nfc_enabled, barcode_format, default_language,
       balance_notifications, tier_notifications, location_notifications, marketing_notifications,
       configurations->'wallet'->>'title' as wallet_title
FROM programs
WHERE id = '{program_id}'::uuid
-- OR: WHERE slug = '{slug}'
-- OR: WHERE lower(name) ILIKE '%{name}%'
```

## Step 2: Subscription Status

```sql
SELECT s.status, s.type, s.created_at, s.capped_amount
FROM subscriptions s
WHERE s.program_id = '{program_id}'::uuid
ORDER BY s.created_at DESC LIMIT 1
```

## Step 3: Membership Overview (KPIs)

```sql
SELECT
  COUNT(DISTINCT c.id) AS total_customers,
  COUNT(DISTINCT c.id) FILTER (WHERE EXISTS (
    SELECT 1 FROM passes p WHERE p.customer_id = c.id AND p.status = '1' AND p.kind = 'store-card'
  )) AS customers_with_active_pass,
  COUNT(DISTINCT c.id) FILTER (WHERE c.created_at >= NOW() - INTERVAL '30 days') AS new_customers_30d,
  COUNT(DISTINCT c.id) FILTER (WHERE c.created_at >= NOW() - INTERVAL '7 days') AS new_customers_7d
FROM customers c
WHERE c.program_id = '{program_id}'::uuid
```

## Step 4: Pass Installation Metrics

```sql
SELECT
  COUNT(*) AS total_passes_created,
  COUNT(*) FILTER (WHERE status = '1') AS currently_installed,
  COUNT(*) FILTER (WHERE status = '2') AS uninstalled,
  COUNT(*) FILTER (WHERE type = '0') AS apple_passes,
  COUNT(*) FILTER (WHERE type = '1') AS google_passes,
  COUNT(*) FILTER (WHERE kind = 'store-card') AS loyalty_passes,
  COUNT(*) FILTER (WHERE kind = 'coupon') AS coupon_passes,
  ROUND(COUNT(*) FILTER (WHERE status = '2')::numeric * 100.0 / NULLIF(COUNT(*), 0), 1) AS churn_rate_pct
FROM passes
WHERE program_id = '{program_id}'::uuid
```

## Step 5: Pass Installation Trend (Monthly)

```sql
SELECT
  DATE_TRUNC('month', installed_at) AS month,
  COUNT(*) AS installs,
  COUNT(*) FILTER (WHERE type = '0') AS apple,
  COUNT(*) FILTER (WHERE type = '1') AS google
FROM passes
WHERE program_id = '{program_id}'::uuid
  AND installed_at IS NOT NULL
GROUP BY 1 ORDER BY 1
```

## Step 6: Customer Origin Breakdown

```sql
SELECT origin, COUNT(*) AS count
FROM customers
WHERE program_id = '{program_id}'::uuid
GROUP BY origin ORDER BY count DESC
```

## Step 7: Loyalty Balances Distribution

```sql
SELECT
  COUNT(*) AS total_members,
  ROUND(AVG(balance), 0) AS avg_balance,
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY balance) AS median_balance,
  MAX(balance) AS max_balance,
  COUNT(*) FILTER (WHERE balance = 0) AS zero_balance,
  COUNT(*) FILTER (WHERE balance > 0 AND balance <= 100) AS balance_1_100,
  COUNT(*) FILTER (WHERE balance > 100 AND balance <= 500) AS balance_101_500,
  COUNT(*) FILTER (WHERE balance > 500 AND balance <= 1000) AS balance_501_1000,
  COUNT(*) FILTER (WHERE balance > 1000) AS balance_over_1000
FROM customers_loyalty_data cld
JOIN customers c ON cld.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
```

## Step 8: Tier Distribution

```sql
SELECT
  t.name AS tier_name,
  t.required_points,
  t.factor,
  COUNT(ct.id) AS member_count
FROM tiers t
LEFT JOIN customers_tiers ct ON ct.current_tier_id = t.id
WHERE t.program_id = '{program_id}'::uuid
GROUP BY t.id, t.name, t.required_points, t.factor
ORDER BY t.required_points ASC
```

## Step 9: Revenue & Transactions

```sql
SELECT
  COUNT(*) AS total_transactions,
  SUM(amount) / 100.0 AS total_revenue,
  ROUND(AVG(amount) / 100.0, 2) AS avg_order_value,
  COUNT(DISTINCT customer_id) AS unique_purchasers,
  ROUND(SUM(amount) / 100.0 / NULLIF(COUNT(DISTINCT customer_id), 0), 2) AS revenue_per_customer,
  COUNT(*) FILTER (WHERE created_at >= NOW() - INTERVAL '30 days') AS transactions_30d,
  SUM(amount) FILTER (WHERE created_at >= NOW() - INTERVAL '30 days') / 100.0 AS revenue_30d
FROM transactions t
JOIN customers c ON t.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
  AND t.status IN ('completed', 'processed')
```

## Step 10: Monthly Revenue Trend

```sql
SELECT
  DATE_TRUNC('month', t.created_at) AS month,
  COUNT(*) AS orders,
  SUM(t.amount) / 100.0 AS revenue,
  COUNT(DISTINCT t.customer_id) AS unique_buyers
FROM transactions t
JOIN customers c ON t.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
  AND t.status IN ('completed', 'processed')
GROUP BY 1 ORDER BY 1
```

## Step 11: Engagement Flows Performance

```sql
SELECT
  ef.name AS flow_name,
  pef.active,
  pef.properties->>'creditRate' AS credit_rate,
  pef.properties->>'points' AS points,
  COUNT(DISTINCT efr.id) AS total_executions,
  COUNT(DISTINCT efr.customer_id) AS unique_customers,
  COUNT(DISTINCT efr.id) FILTER (WHERE efr.created_at >= NOW() - INTERVAL '30 days') AS executions_30d
FROM program_engagement_flows pef
JOIN engagement_flows ef ON pef.engagement_flow_id = ef.id
LEFT JOIN engagement_flow_runs efr ON efr.program_engagement_flow_id = pef.id
WHERE pef.program_id = '{program_id}'::uuid
GROUP BY ef.name, pef.active, pef.properties->>'creditRate', pef.properties->>'points'
ORDER BY total_executions DESC
```

## Step 12: Rewards Catalog & Redemption

```sql
SELECT
  r.title,
  r.redeem_cost,
  cb.discount_type,
  cb.value AS discount_value,
  cb.minimum_spend,
  COUNT(DISTINCT cc.id) AS codes_issued,
  COUNT(DISTINCT cc.id) FILTER (WHERE cc.claimed = true) AS codes_claimed,
  COUNT(DISTINCT cc.id) FILTER (WHERE cc.transaction_id IS NOT NULL) AS codes_used
FROM rewards r
JOIN coupons co ON r.coupon_id = co.id
JOIN coupon_batches cb ON cb.coupon_id = co.id
LEFT JOIN coupon_codes cc ON cc.coupon_batch_id = cb.id
WHERE co.program_id = '{program_id}'::uuid
  AND r.deleted_at IS NULL
  AND co.deleted_at IS NULL
GROUP BY r.title, r.redeem_cost, cb.discount_type, cb.value, cb.minimum_spend
ORDER BY codes_issued DESC
```

## Step 13: Push Campaign Performance

```sql
SELECT
  camp.name,
  camp.type,
  camp.started_at,
  camp.finished_at,
  COUNT(DISTINCT cc.customer_id) AS audience_size,
  COUNT(DISTINCT cl.customer_id) AS unique_clickers,
  ROUND(COUNT(DISTINCT cl.customer_id)::numeric * 100.0 / NULLIF(COUNT(DISTINCT cc.customer_id), 0), 1) AS ctr_pct
FROM campaigns camp
LEFT JOIN campaign_customers cc ON cc.campaign_id = camp.id
LEFT JOIN clicks cl ON cl.campaign_id = camp.id
WHERE camp.program_id = '{program_id}'::uuid
  AND camp.deleted_at IS NULL
  AND camp.started_at IS NOT NULL
GROUP BY camp.id, camp.name, camp.type, camp.started_at, camp.finished_at
ORDER BY camp.started_at DESC
LIMIT 20
```

## Step 14: Event Activity Summary (Last 90 Days)

```sql
SELECT
  ce.type,
  COUNT(*) AS total_events,
  COUNT(DISTINCT ce.customer_id) AS unique_customers,
  MAX(ce.created_at) AS last_occurrence
FROM customer_events ce
JOIN customers c ON ce.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
  AND ce.created_at >= NOW() - INTERVAL '90 days'
GROUP BY ce.type
ORDER BY total_events DESC
```

## Step 15: Web App Visits (Top Pages & Devices)

### Pages

```sql
SELECT
  CASE
    WHEN url LIKE '%/rewards%' THEN 'Rewards'
    WHEN url LIKE '%/profile%' THEN 'Profile'
    WHEN url LIKE '%/referral%' THEN 'Referral'
    WHEN url LIKE '%/history%' THEN 'History'
    ELSE 'Other'
  END AS page_section,
  COUNT(*) AS visits,
  COUNT(DISTINCT customer_id) AS unique_visitors
FROM visits
WHERE program_id = '{program_id}'::uuid
  AND created_at >= NOW() - INTERVAL '90 days'
GROUP BY 1
ORDER BY visits DESC
```

### Devices

```sql
SELECT os, COUNT(*) AS visits,
  ROUND(COUNT(*)::numeric * 100.0 / SUM(COUNT(*)) OVER(), 1) AS pct
FROM visits
WHERE program_id = '{program_id}'::uuid
  AND created_at >= NOW() - INTERVAL '90 days'
  AND os IS NOT NULL
GROUP BY os ORDER BY visits DESC
```

## Step 16: Referral Program Performance

```sql
SELECT
  COUNT(DISTINCT c.id) FILTER (WHERE c.referral IS NOT NULL AND c.referral != '') AS referred_customers,
  COUNT(DISTINCT c.referral) FILTER (WHERE c.referral IS NOT NULL AND c.referral != '') AS active_referrers,
  COUNT(DISTINCT cld.id) FILTER (WHERE cld.referral_link IS NOT NULL) AS members_with_referral_link
FROM customers c
LEFT JOIN customers_loyalty_data cld ON cld.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
```
