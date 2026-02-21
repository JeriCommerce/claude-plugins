# QA Testing Patterns

## Common Test Scenarios

### 1. Customer Registration Flow

**What to test:**
1. Check if a customer already exists in the program
2. Register a new customer
3. Verify email validation flow (if applicable)
4. Verify pass creation after registration

**What to verify in DB:**
- Customer record exists in `customers` table with correct data
- Pass was generated in `passes` table for the new customer
- Welcome campaign was triggered (if configured) — check `campaign_sends`
- Integration webhooks fired — check BetterStack logs

**Always look up the registration endpoints in the Swagger spec** before making calls — the request body schema may have changed.

### 2. Balance Operations

**What to test:**
1. Record the current balance before the test
2. Increase balance and verify the change
3. Decrease balance and verify the change
4. Check edge cases: decrease below zero, large amounts

**What to verify in DB:**
- Balance field updated on the `customers` record
- Transaction record created with correct `amount`, `type`, and `status`
- Pass updated with new balance (check BetterStack for pass update logs)

### 3. Campaign Testing

**What to test:**
1. List existing campaigns for the program
2. Create a test campaign (if needed)
3. Verify push notification delivery
4. Verify campaign analytics update

**What to verify in DB:**
- Campaign exists with correct configuration
- Campaign sends logged for eligible customers
- Check BetterStack for push notification delivery status

### 4. Coupon & Reward Flow

**What to test:**
1. Create a coupon for the program
2. Generate a batch of codes
3. Assign a coupon to a customer
4. Claim/redeem a coupon code
5. Verify the redemption status

**What to verify in DB:**
- Coupon codes generated in `coupon_codes` table
- Assignment reflected in customer's rewards
- Claimed code marked as used
- Balance adjustment if the coupon has a point value

### 5. Webhook & Integration Testing

**What to test:**
1. Identify configured integrations for the program
2. Trigger an action that should fire a webhook (e.g., order through Shopify)
3. Verify the webhook was received and processed

**What to verify:**
- BetterStack logs show webhook received with correct payload
- Expected side effect occurred (balance change, pass update, etc.)
- No errors in processing

### 6. Wallet Pass Operations

**What to test:**
1. Request an Apple Wallet or Google Wallet pass
2. Verify pass content reflects current customer state
3. Modify customer data (balance, tier) and verify pass updates

**What to verify:**
- Pass file/object generated correctly
- Pass content matches DB state (balance, tier, branding)
- BetterStack logs show pass update events after data changes

## Data Lookup Queries

### Find a program

```sql
SELECT id, name, slug, created_at
FROM programs
WHERE slug = '{slug}'
-- or
SELECT id, name, slug, created_at
FROM programs
WHERE name ILIKE '%{name}%'
```

### Find a customer

```sql
SELECT c.id, c.email, c.balance, c.created_at, p.name AS program_name
FROM customers c
JOIN programs p ON c.program_id = p.id
WHERE c.email = '{email}' AND p.slug = '{slug}'
```

### Find API keys for a program

```sql
SELECT ak.id, ak.key, ak.name, ak.created_at
FROM api_keys ak
JOIN programs p ON ak.program_id = p.id
WHERE p.slug = '{slug}'
AND ak.deleted_at IS NULL
```

### Recent transactions for a customer

```sql
SELECT t.id, t.amount, t.type, t.status, t.created_at, t.metadata
FROM transactions t
WHERE t.customer_id = '{customerId}'::uuid
ORDER BY t.created_at DESC
LIMIT 20
```

### Customer passes

```sql
SELECT p.id, p.type, p.status, p.created_at, p.updated_at
FROM passes p
WHERE p.customer_id = '{customerId}'::uuid
ORDER BY p.created_at DESC
```

### Program integrations

```sql
SELECT i.name AS integration, pi.enabled_features, pi.created_at
FROM program_integrations pi
JOIN integrations i ON pi.integration_id = i.id
WHERE pi.program_id = '{programId}'::uuid
```

### Active campaigns for a program

```sql
SELECT id, name, type, status, created_at
FROM campaigns
WHERE program_id = '{programId}'::uuid
AND deleted_at IS NULL
ORDER BY created_at DESC
```

## Test Data Conventions

- **Test emails**: Use `qa+{test-name}@jericommerce.com` format for test customers
- **Test programs**: Prefer staging programs; ask user for the target program
- **Cleanup**: Note any test data created so it can be cleaned up later
- **Never test destructive operations in production** without explicit user confirmation
