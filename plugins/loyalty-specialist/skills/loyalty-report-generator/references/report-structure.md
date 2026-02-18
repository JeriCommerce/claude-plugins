# Report Structure & Branding

## Output Format

Generate the report as a **PDF file** using **ReportLab** with `BaseDocTemplate` and two `PageTemplate`s (cover + content).

File name: `{program_name}_loyalty_report_{YYYY-MM-DD}.pdf`

## Visual Design System

### Colors

```python
PRIMARY    = "#7000FF"   # Main brand purple
PRIMARY_D  = "#5600CC"   # Darker purple (cover stripe)
DARK       = "#1C1C1C"   # Body text
GREY       = "#666666"   # Secondary text
GREY_L     = "#999999"   # Labels, footer
LIGHT_P    = "#F3EEFF"   # KPI card backgrounds
LIGHT_P2   = "#EDE5FF"   # Section separator lines
BG_LIGHT   = "#FAFAFA"   # Alternating table rows
BORDER     = "#E5E5E5"   # Table grid lines
WHITE      = "#FFFFFF"
```

### Page Templates

**Cover Page** (`onPage` canvas drawing, no platypus content):
- Full-page `PRIMARY` background fill
- Darker `PRIMARY_D` stripe at top (120pt)
- Subtle white semi-transparent horizontal rules above and below center content
- Program name: Helvetica-Bold 52pt, white, centered
- Subtitle: Helvetica 18pt, white, centered
- Date + program details (slug, integration, currency, plan): Helvetica 11–13pt, `#CCBBFF`, centered
- Footer: Helvetica 8pt, `#AA88FF`, centered — "Confidential -- Prepared by JeriCommerce"

**Content Pages** (`onPage` canvas drawing):
- **White background** — content pages must have a clean white background, no purple fill
- Top accent bar: thin `PRIMARY` line (2pt), full width at page top — subtle, not dominant
- Footer line: 0.5pt `BORDER` colored line at y=28
- Footer left: generation timestamp + confidentiality (Helvetica 7pt, `GREY_L`)
- Footer right: "Page N" (page number minus 1, since cover is page 0)

### Typography (ParagraphStyles)

| Style | Size | Font | Color | Usage |
|-------|------|------|-------|-------|
| SecTitle | 17pt | Helvetica-Bold | `PRIMARY` | Section headers (spaceBefore=14, spaceAfter=6) |
| SubTitle | 12pt | Helvetica-Bold | `DARK` | Sub-section headers (spaceBefore=10, spaceAfter=5) |
| Body | 10pt | Helvetica | `DARK` | Narrative paragraphs (leading=15) |
| KPIVal | 20pt | Helvetica-Bold | `PRIMARY` | KPI card value (centered) |
| KPILbl | 8pt | Helvetica | `GREY_L` | KPI card label (centered) |
| TH | 8.5pt | Helvetica-Bold | `WHITE` | Table header cells |
| TD | 8.5pt | Helvetica | `DARK` | Table body cells |
| CatTitle | 11pt | Helvetica-Bold | `PRIMARY` | Recommendation category headers |
| BulletItem | 9.5pt | Helvetica | `DARK` | Recommendation bullets (leftIndent=14, leading=14) |

### Components

**KPI Cards** — Table with single column, `LIGHT_P` background, 8px rounded corners:
- Top cell: value in `KPIVal` style (topPadding=12)
- Spacer cell: 2pt
- Bottom cell: label in `KPILbl` style (bottomPadding=10)

**KPI Strips** — Horizontal Table of N KPI cards, equal column widths (`CONTENT_W / N`), center-aligned. Use 3–4 cards per strip, stack multiple strips with 8pt spacing.

**Data Tables** — `PRIMARY` header row background with white text. Body rows alternate `WHITE` / `BG_LIGHT`. Grid: 0.4pt `BORDER`. Cell padding: 7pt header, 5pt body, 8pt left/right. `repeatRows=1`.

**Section Separators** — 1.2pt `LIGHT_P2` colored line below each section title, implemented as a single-cell table with `LINEABOVE`.

**Recommendation Bullets** — Use `&#8226;&nbsp;` HTML entity prefix, bold the first sentence as the action title, followed by explanation text.

### Layout

- Page size: A4
- Margins: 22mm all sides
- Content width: `PAGE_W - 2 * MARGIN` (~454pt)
- Each major section starts on a new page (`PageBreak`)
- Section pattern: Title → Separator → Spacer(6) → KPI strip(s) → Subtitle → Table → Narrative paragraph

### Number Formatting

Use **European format** throughout the report:
- Dots for thousands separator: `1.633.928`
- Commas for decimals: `95,89`
- Currency: `1.633.928,00 EUR`

## Voice & Tone

The report is written by a **JeriCommerce analyst** addressed to the **client** (the loyalty program manager). It should feel like a trusted advisor sharing insights — not a robot dumping data.

### Who is writing
- A JeriCommerce loyalty specialist — knowledgeable, approachable, helpful
- You are on the client's side, helping them understand their program

### Who is reading
- The client who manages the loyalty program (store owner, marketing manager, etc.)
- They may not be deeply technical — keep it accessible

### Tone guidelines
- **Direct but not blunt** — get to the point, but with warmth
- **Confident but not absolute** — share a point of view, don't dictate. Use "This could suggest...", "It looks like...", "We'd recommend considering..." instead of "This means..." or "You must..."
- **Young and professional** — we're not a corporate consultancy. No stiff language. But always respectful
- **Conversational, not casual** — write like you'd speak to a client you respect, not like a Slack message
- **Analytical, not academic** — explain what the data shows and why it matters, skip the jargon

### Language patterns

**Do:**
- "Your install rate sits at 42%, which is solid. There might be room to push it further with in-store QR codes."
- "We're seeing a dip in redemptions this month — it could be worth revisiting the reward catalog."
- "The data suggests that your Gold tier members are your most engaged segment."

**Don't:**
- "The install rate is 42%." (too dry, no perspective)
- "You need to add QR codes immediately." (too directive)
- "As per the analysis, the redemption metrics indicate a downward trajectory." (too corporate)
- "Your rewards suck." (too casual)

### Key rules
- Always provide **your perspective** on what the data means, not just the numbers
- Frame recommendations as **suggestions**, not orders: "We'd suggest...", "It might be worth...", "One option could be..."
- When something is going well, **say so** — don't only highlight problems
- When something is concerning, be **honest but constructive** — always pair the problem with a potential solution
- Use **"we"** (JeriCommerce) and **"your"** (the client's program) naturally

## Writing Style & Readability

- **Use short paragraphs** — max 2–3 sentences per paragraph, then add a line break
- **Add blank lines between every paragraph, bullet group, and table** to create visual breathing room
- **Never write walls of text** — if a paragraph exceeds 3 lines, break it into two
- **Use bullet points** instead of inline lists (never "A, B, and C" — use a bulleted list instead)
- **One idea per paragraph** — separate different concepts with line breaks
- **Add a line break before and after every table and data block**
- **Section introductions should be 1–2 sentences max**, then go straight to the data
- The report should feel **airy and scannable**, not dense and academic

## Report Sections — Full Report (`has_jc_loyalty = true`)

### Cover Page
- Full-page `PRIMARY` background with `PRIMARY_D` stripe
- Program name (large, white, centered)
- "Loyalty Program Performance Report"
- Program details: slug, integration, currency, plan
- Report date and generation date
- Confidential notice

### Executive Summary (1 page)
- 4–8 KPI cards in strips of 4: Total Members, Active Passes, Install Rate, Total Revenue, Avg Order Value, Churn Rate (optionally: Orders 30d, Revenue 30d)
- 2–3 sentence narrative summary of program health

### 1. Membership & Growth
- Total customers and growth trajectory
- New member acquisition trend (monthly table)
- Customer origin breakdown table (SHOPIFY vs JERICOMMERCE vs INITIAL_SYNC)
- Pass installation rate and Apple vs Google split

### 2. Pass Engagement
- KPI strip: total passes, installed, uninstalled, churn rate
- Platform split table (Apple vs Google)
- Installation trend table (monthly)
- Active pass penetration (active passes / total customers)

### 3. Loyalty Economics
- KPI strip: avg balance, median balance, max balance
- Points balance distribution table
- Tier breakdown table with member counts
- Tier progression recommendations

### 4. Revenue Impact
- Total attributed revenue
- Monthly revenue trend table
- Average order value
- Revenue per loyalty member
- Transactions in last 30 days vs prior period

### 5. Engagement Flows
- Full table: flow name, active status, rates, points, total executions, unique customers, last 30d
- Inactive flows that could be activated

### 6. Rewards & Redemption (only if `has_jc_rewards = true`)
- Reward catalog table
- Redemption funnel KPI strip (issued → claimed → used → conversion rate)
- Claim rate and usage rate per reward
- Recommendations on reward optimization

### 7. Push Campaigns
- Recent campaigns table with audience, clicks, CTR
- Top-performing campaigns
- Average CTR benchmark
- Campaign frequency analysis

### 8. Web App Usage
- Top visited pages table
- Device/OS breakdown table
- Referral program adoption status

### 9. Event Activity (Last 90 Days)
- Full event summary table: type, total events, unique customers, last occurrence

### 10. What's Working Well (only if ≥ 2 thresholds met)

**Critical rule:** Only include this section if at least 2 of the thresholds below are met. If fewer than 2 qualify, skip this section entirely. Do NOT fabricate positive narratives from weak data.

| Metric | Threshold | Highlight framing |
|--------|-----------|-------------------|
| Pass churn rate | < 5% | Exceptional retention |
| Pass install MoM growth | > 50% for 2+ consecutive months | Accelerating adoption |
| Apple/Google split | Neither < 20% | Cross-platform reach |
| Campaign CTR | Any campaign > 10% | High-impact campaigns |
| Reward conversion (issued-to-used) | > 30% | Compelling rewards |
| Repeat purchasers | > 30% of members | Strong purchase engagement |
| Tier progression | > 1% in non-base tiers | Active tier progression |
| Revenue per customer | > 2x AOV | High lifetime value |
| Engagement flow adoption | Any non-signup flow > 1,000 exec/30d | Active engagement loops |
| Web app visits (90d) | > 2,000 | Digital hub traction |
| New members (30d) | > 10% of total base | Strong acquisition momentum |
| Referral program | > 50 referred customers | Organic growth engine |

Each qualifying metric: titled paragraph with the specific number and why it matters.

### 11. Insights & Recommendations

Provide **5–8 actionable recommendations** grouped by priority:

- **Quick Wins** (can implement this week)
- **Strategic** (require planning)
- **Watch** (monitor these metrics)

#### Recommendation Heuristics (full report)

| Condition | Recommendation |
|-----------|---------------|
| Pass install rate < 30% of customers | Improve distribution (email flows, QR in store) |
| Churn rate > 20% | Launch reactivation campaigns |
| Zero balance > 50% of members | Earning flows may not be active or visible enough |
| CTR < 5% on Scheduled/Links campaigns | Review notification copy, timing, and ensure all campaigns include a trackable CTA link |
| Low referral adoption | Promote referral link visibility |
| Only 1 tier | Suggest implementing tier system for gamification |
| No recent campaigns | Encourage regular push cadence (2–4/month) |
| Avg balance high but low redemptions | Rewards may be too expensive or not compelling |
| All tier factors = 1x | Differentiate multipliers to incentivize progression |
| Cheapest reward requires > 500 EUR spend to earn | Add lower-cost rewards (200–500 pts) |
| Tier welcome rewards unused | Improve visibility or auto-apply on tier upgrade |
| MEMBER_GET_MEMBER active but 0 executions | Promote referral program across touchpoints |
| CUSTOMER_SCANNED disabled | Enable to reward in-store engagement |

---

## Report Sections — Reduced Report (`has_jc_loyalty = false`)

When the program uses an external loyalty provider, generate a reduced report. Add a note in the Executive Summary stating which external provider manages loyalty (from `active_integrations`), and that points, balances, tiers, transactions and redemptions are managed externally and not reflected in the report.

### Cover Page
- Same as full report

### Executive Summary (limited KPIs)
- KPI cards: Total Members, Active Passes, Install Rate, Churn Rate (no revenue/AOV)
- Note: "Loyalty data (points, tiers, transactions, redemptions) is managed by {external provider} and is not included in this report."
- 2–3 sentence narrative summary

### 1. Membership & Growth
- Same as full report

### 2. Pass Engagement
- Same as full report

### 3. Push Campaigns
- Same as full report §7

### 4. Web App Usage
- Same as full report §8

### 5. Event Activity (Last 90 Days)
- Same as full report §9

### 6. What's Working Well (only if ≥ 2 thresholds met)

When `has_jc_loyalty = false`, only evaluate these metrics:
- Pass churn rate (< 5%)
- Pass install MoM growth (> 50% for 2+ months)
- Apple/Google split (neither < 20%)
- Campaign CTR (any > 10%)
- Web app visits 90d (> 2,000)
- New members 30d (> 10% of total base)

### 7. Insights & Recommendations (adapted)

#### Recommendation Heuristics (reduced report)

| Condition | Recommendation |
|-----------|---------------|
| Pass install rate < 30% | Improve distribution |
| Churn > 20% | Investigate pass value proposition |
| No recent campaigns | Encourage push cadence |
| CTR < 5% on Scheduled/Links campaigns | Review notification copy, timing, and ensure all campaigns include a trackable CTA link |
| Campaign audiences < 100 | Focus on growing pass adoption first |
| Web app visits < 500 (90d) | Drive traffic via wallet pass and emails |
| Rewards page < 10% of total visits | Improve discoverability |
| External loyalty provider limits data visibility | Consider evaluating migration to JeriCommerce loyalty for full analytics |
