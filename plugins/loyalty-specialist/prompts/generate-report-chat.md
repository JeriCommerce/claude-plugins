# JeriCommerce Loyalty Report — Chat Prompt

> Copy and paste this prompt into Claude Desktop **Chat** mode (with Metabase connector enabled).
> Replace `{PROGRAM}` with the program UUID, slug, or name.

---

Generate a professional loyalty program performance report for **{PROGRAM}**.

## Instructions

You MUST use the **Metabase connector** to execute all SQL queries. Use the `Metabase:execute` tool with `database_id: 3` for every query. Do NOT attempt to connect to the database directly.

If a query returns empty results, note it in the report as "No data available" — do NOT skip the section.

## Step 1: Identify the program

Run this query to find the program (adapt the WHERE clause based on input type — UUID, slug, or name with ILIKE):

```sql
SELECT id, name, slug, contact_email, integration, created_at, currency,
       nfc_enabled, barcode_format, default_language,
       balance_notifications, tier_notifications, location_notifications, marketing_notifications,
       configurations->'wallet'->>'title' as wallet_title
FROM programs
WHERE slug = '{PROGRAM}'
```

## Step 2: Collect all data

Once you have the program UUID, execute ALL of the following queries in order using `Metabase:execute` with `database_id: 3`. Collect ALL results before generating the report.

### 2.1 Subscription Status
```sql
SELECT s.status, s.type, s.created_at, s.capped_amount
FROM subscriptions s
WHERE s.program_id = '{program_id}'::uuid
ORDER BY s.created_at DESC LIMIT 1
```

### 2.2 Membership Overview (KPIs)
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

### 2.3 Pass Installation Metrics
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

### 2.4 Pass Installation Trend (Monthly)
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

### 2.5 Customer Origin Breakdown
```sql
SELECT origin, COUNT(*) AS count
FROM customers
WHERE program_id = '{program_id}'::uuid
GROUP BY origin ORDER BY count DESC
```

### 2.6 Loyalty Balances Distribution
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

### 2.7 Tier Distribution
```sql
SELECT
  t.name AS tier_name, t.required_points, t.factor,
  COUNT(ct.id) AS member_count
FROM tiers t
LEFT JOIN customers_tiers ct ON ct.current_tier_id = t.id
WHERE t.program_id = '{program_id}'::uuid
GROUP BY t.id, t.name, t.required_points, t.factor
ORDER BY t.required_points ASC
```

### 2.8 Revenue & Transactions
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

### 2.9 Monthly Revenue Trend
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

### 2.10 Engagement Flows Performance
```sql
SELECT
  ef.name AS flow_name, pef.active,
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

### 2.11 Rewards Catalog & Redemption
```sql
SELECT
  r.title, r.redeem_cost, cb.discount_type,
  cb.value AS discount_value, cb.minimum_spend,
  COUNT(DISTINCT cc.id) AS codes_issued,
  COUNT(DISTINCT cc.id) FILTER (WHERE cc.claimed = true) AS codes_claimed,
  COUNT(DISTINCT cc.id) FILTER (WHERE cc.transaction_id IS NOT NULL) AS codes_used
FROM rewards r
JOIN coupons co ON r.coupon_id = co.id
JOIN coupon_batches cb ON cb.coupon_id = co.id
LEFT JOIN coupon_codes cc ON cc.coupon_batch_id = cb.id
WHERE co.program_id = '{program_id}'::uuid
  AND r.deleted_at IS NULL AND co.deleted_at IS NULL
GROUP BY r.title, r.redeem_cost, cb.discount_type, cb.value, cb.minimum_spend
ORDER BY codes_issued DESC
```

### 2.12 Push Campaign Performance
```sql
SELECT
  camp.name, camp.type, camp.started_at, camp.finished_at,
  COUNT(DISTINCT cc.customer_id) AS audience_size,
  COUNT(DISTINCT cl.customer_id) AS unique_clickers,
  ROUND(COUNT(DISTINCT cl.customer_id)::numeric * 100.0 / NULLIF(COUNT(DISTINCT cc.customer_id), 0), 1) AS ctr_pct
FROM campaigns camp
LEFT JOIN campaign_customers cc ON cc.campaign_id = camp.id
LEFT JOIN clicks cl ON cl.campaign_id = camp.id
WHERE camp.program_id = '{program_id}'::uuid
  AND camp.deleted_at IS NULL AND camp.started_at IS NOT NULL
GROUP BY camp.id, camp.name, camp.type, camp.started_at, camp.finished_at
ORDER BY camp.started_at DESC LIMIT 20
```

### 2.13 Event Activity Summary (Last 90 Days)
```sql
SELECT
  ce.type, COUNT(*) AS total_events,
  COUNT(DISTINCT ce.customer_id) AS unique_customers,
  MAX(ce.created_at) AS last_occurrence
FROM customer_events ce
JOIN customers c ON ce.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
  AND ce.created_at >= NOW() - INTERVAL '90 days'
GROUP BY ce.type ORDER BY total_events DESC
```

### 2.14 Web App Visits — Pages
```sql
SELECT
  CASE
    WHEN url LIKE '%/rewards%' THEN 'Rewards'
    WHEN url LIKE '%/profile%' THEN 'Profile'
    WHEN url LIKE '%/referral%' THEN 'Referral'
    WHEN url LIKE '%/history%' THEN 'History'
    ELSE 'Other'
  END AS page_section,
  COUNT(*) AS visits, COUNT(DISTINCT customer_id) AS unique_visitors
FROM visits
WHERE program_id = '{program_id}'::uuid AND created_at >= NOW() - INTERVAL '90 days'
GROUP BY 1 ORDER BY visits DESC
```

### 2.15 Web App Visits — Devices
```sql
SELECT os, COUNT(*) AS visits,
  ROUND(COUNT(*)::numeric * 100.0 / SUM(COUNT(*)) OVER(), 1) AS pct
FROM visits
WHERE program_id = '{program_id}'::uuid
  AND created_at >= NOW() - INTERVAL '90 days' AND os IS NOT NULL
GROUP BY os ORDER BY visits DESC
```

### 2.16 Referral Program Performance
```sql
SELECT
  COUNT(DISTINCT c.id) FILTER (WHERE c.referral IS NOT NULL AND c.referral != '') AS referred_customers,
  COUNT(DISTINCT c.referral) FILTER (WHERE c.referral IS NOT NULL AND c.referral != '') AS active_referrers,
  COUNT(DISTINCT cld.id) FILTER (WHERE cld.referral_link IS NOT NULL) AS members_with_referral_link
FROM customers c
LEFT JOIN customers_loyalty_data cld ON cld.customer_id = c.id
WHERE c.program_id = '{program_id}'::uuid
```

## Step 3: Analyze and generate report

After collecting ALL data, generate a professional PDF report using **ReportLab** with `BaseDocTemplate` and two `PageTemplate`s (cover + content).

### Report sections

1. **Cover Page** — Full-page purple (`#7000FF`) background, `#5600CC` stripe at top, program name (52pt white), subtitle, date, program details (slug, integration, currency, plan)
2. **Executive Summary** — 4–8 KPI cards in strips of 4 (Total Members, Active Passes, Install Rate, Total Revenue, AOV, Churn Rate, optionally Orders 30d, Revenue 30d) + 2–3 sentence narrative
3. **Membership & Growth** — Total customers, acquisition trend table, origin breakdown table, pass install rate
4. **Pass Engagement** — KPI strip, platform split table, installation trend table, churn rate
5. **Loyalty Economics** — KPI strip (avg/median/max balance), points distribution table, tier breakdown table
6. **Revenue Impact** — Total revenue, monthly trend table, AOV, revenue per member
7. **Engagement Flows** — Full table: flow name, active status, rates, points, executions, unique customers, last 30d
8. **Rewards & Redemption** — Catalog table, redemption funnel KPI strip (issued → claimed → used → conversion rate)
9. **Push Campaigns** — Recent campaigns table with audience, clicks, CTR
10. **Web App Usage** — Top pages table, device breakdown table, referral adoption
11. **Event Activity** — Full event summary table (last 90 days)
12. **Insights & Recommendations** — 5–8 actionable recommendations grouped by: Quick Wins, Strategic, Watch

### Recommendation heuristics

| Condition | Recommendation |
|-----------|---------------|
| Pass install rate < 30% of customers | Improve distribution (email flows, QR in store) |
| Churn rate > 20% | Launch reactivation campaigns |
| Zero balance > 50% of members | Earning flows may not be active or visible enough |
| CTR < 5% on campaigns | Review notification copy and timing |
| Low referral adoption | Promote referral link visibility |
| Only 1 tier | Suggest implementing tier system for gamification |
| No recent campaigns | Encourage regular push cadence (2–4/month) |
| Avg balance high but low redemptions | Rewards may be too expensive or not compelling |

### Visual design

#### Colors
- `#7000FF` primary | `#5600CC` cover stripe | `#1C1C1C` body text | `#666666` secondary
- `#999999` labels/footer | `#F3EEFF` KPI backgrounds | `#EDE5FF` separator lines
- `#FAFAFA` alternating rows | `#E5E5E5` table grid | `#FFFFFF` white

#### Components
- **KPI Cards**: `#F3EEFF` background, value 20pt bold purple, label 8pt grey, rounded corners
- **KPI Strips**: horizontal row of 3–4 cards, equal widths, stack multiple strips with 8pt spacing
- **Data Tables**: purple header with white text, alternating white/`#FAFAFA` rows, 0.4pt grid
- **Section Separators**: 1.2pt `#EDE5FF` line below each section title
- **Recommendation Bullets**: bullet prefix, bold first sentence as action title

#### Layout
- Page size: A4, margins: 22mm all sides
- Each major section starts on a new page (`PageBreak`)
- Content pages: 8pt purple accent bar at top, footer with timestamp + page number
- Section pattern: Title → Separator → Spacer → KPI strip(s) → Subtitle → Table → Narrative

#### Number formatting
- European format: dots for thousands (`1.633.928`), commas for decimals (`95,89 EUR`)

### Voice & tone

The report is written by a JeriCommerce analyst addressed to the client (the loyalty program manager). Trusted advisor sharing insights, not a robot dumping data.

- **Direct but not blunt** — get to the point, with warmth
- **Confident but not absolute** — use "This could suggest...", "It looks like...", "We'd recommend considering..." instead of "This means..." or "You must..."
- **Young and professional** — not a corporate consultancy, but always respectful
- **Conversational, not casual** — speak to a client you respect
- Always provide **your perspective** on what the data means, not just numbers
- Frame recommendations as **suggestions**: "We'd suggest...", "It might be worth..."
- When something is going well, **say so**. When concerning, be **honest but constructive** — pair the problem with a solution
- Use **"we"** (JeriCommerce) and **"your"** (the client's program)

### Writing style

- **Short paragraphs only** — max 2–3 sentences, then add a line break
- **Add blank lines** between every paragraph, bullet group, and table
- **Never write walls of text** — if a paragraph exceeds 3 lines, split it
- **Use bullet points** instead of inline comma-separated lists
- **One idea per paragraph** — separate concepts with line breaks
- The report should feel **airy and scannable**, not dense

### Data milestones

Consider these when analyzing results — mention them in the report when they impact a metric:

| Date / Fact | Impact |
|-------------|--------|
| 2026-02-17 | Start of coupon code → transaction linking. Before this date, `coupon_codes.transaction_id` is always NULL — do NOT interpret as "unused codes". |
| All transactions | Only non-anonymous (identified) customers are stored. Anonymous purchases are NOT in the database. Revenue metrics reflect identified members only, not total store revenue. |

### Data notes

- **Amounts**: `transactions.amount` is in **cents** — divide by 100
- **UUIDs**: always cast string inputs as `'value'::uuid`
- **Soft deletes**: `deleted_at` column on `campaigns`, `coupons`, `rewards` (NOT on `programs`, `customers`, `passes`)
- **Pass status**: 0 = pending, 1 = installed/active, 2 = uninstalled
- **Pass type**: 0 = Apple Wallet, 1 = Google Wallet
- **Campaign type**: 0 = push, 1 = location, 2 = card update, 3 = anonymous, 4 = proximity
- **Customer origin**: SHOPIFY, JERICOMMERCE, INITIAL_SYNC, UNKNOWN
- **Coupon batches**: `discount_type` is 'fixed' (value in whole currency) or 'percentage'
- **Transaction status**: filter by `status IN ('completed', 'processed')` — both are successful transactions

Save as `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`.
