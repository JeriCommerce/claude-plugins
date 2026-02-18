# Report Structure & Branding

## Output Format

Generate the report as a **PDF file** using the PDF skill (read `/mnt/.skills/skills/pdf/SKILL.md` first).

File name: `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`

## Branding

- **Primary purple**: `#7000FF`
- **Dark**: `#1C1C1C`
- **Grey**: `#666666`
- **Light purple**: `#F3EEFF`
- Clean, professional formatting with clear section headers
- Data tables with alternating row shading
- Footer on every page: generation timestamp + "Confidential — Prepared by JeriCommerce"

## Writing Style & Readability

- **Use short paragraphs** — max 2–3 sentences per paragraph, then add a line break
- **Add blank lines between every paragraph, bullet group, and table** to create visual breathing room
- **Never write walls of text** — if a paragraph exceeds 3 lines, break it into two
- **Use bullet points** instead of inline lists (never "A, B, and C" — use a bulleted list instead)
- **One idea per paragraph** — separate different concepts with line breaks
- **Add a line break before and after every table and data block**
- **Section introductions should be 1–2 sentences max**, then go straight to the data
- The report should feel **airy and scannable**, not dense and academic

## Report Sections

### Cover Page
- JeriCommerce logo area (purple `#7000FF` branding)
- Program name (large)
- "Loyalty Program Performance Report"
- Report date range and generation date
- Confidential notice

### Executive Summary (1 page)
- 4–6 key KPIs in large format: Total Members, Active Passes, Install Rate, Total Revenue, Avg Order Value, Churn Rate
- 2–3 sentence narrative summary of program health

### 1. Membership & Growth
- Total customers and growth trajectory
- New member acquisition trend (monthly chart data as table)
- Customer origin breakdown (SHOPIFY vs JERICOMMERCE vs INITIAL_SYNC)
- Pass installation rate and Apple vs Google split

### 2. Pass Engagement
- Installation trend over time
- Churn rate analysis (uninstalled / total created)
- Platform distribution (iOS vs Android)
- Active pass penetration (active passes / total customers)

### 3. Loyalty Economics
- Points balance distribution
- Tier breakdown with member counts
- Average balance and spending power analysis
- Tier progression recommendations

### 4. Revenue Impact
- Total attributed revenue
- Monthly revenue trend
- Average order value
- Revenue per loyalty member
- Transactions in last 30 days vs prior period

### 5. Engagement Flows
- Active earning flows and their configuration
- Execution counts per flow
- Top-performing flows by unique customer engagement
- Inactive flows that could be activated

### 6. Rewards & Redemption
- Reward catalog overview
- Redemption funnel (issued → claimed → used)
- Claim rate and usage rate per reward
- Recommendations on reward optimization

### 7. Push Campaigns
- Recent campaigns with CTR
- Top-performing campaigns
- Average CTR benchmark
- Campaign frequency analysis

### 8. Web App Usage
- Top visited sections
- Device/OS breakdown
- Referral program adoption

### 9. Insights & Recommendations

Provide **5–8 actionable recommendations** grouped by priority:

- **Quick Wins** (can implement this week)
- **Strategic** (require planning)
- **Watch** (monitor these metrics)

#### Recommendation Heuristics

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
