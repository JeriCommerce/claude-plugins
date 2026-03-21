# QA Workflow

## Step 1: Understand what to test

Start by understanding the scope. Ask the user conversationally:

1. **What are we testing?** — A specific Linear ticket, a feature, a bug fix, an endpoint, or a general area?
2. **Which environment?** — Staging (default) or production?
3. **Which program?** — What program/merchant should we test against? (get slug or UUID)

If the user provides a Linear ticket ID or URL:
- Fetch the ticket with `Linear:get_issue`
- Read the description, acceptance criteria, and any linked PRs
- Use this to derive test scenarios

If the user describes a feature or area:
- Propose test scenarios based on the product context and testing patterns references
- Ask if they want to add or modify any

## Step 2: Prepare test data

Use Metabase to find or prepare the data needed for testing:

1. Look up the target program: `SELECT id, name, slug FROM programs WHERE slug = '{slug}'`
2. Find test customers or relevant data
3. Get API keys if HTTP calls are needed: query `api_keys` table
4. Note the current state of any data that will be modified (for before/after comparison)

If credentials are needed and not found in Metabase, ask the user to provide them.

## Step 3: Look up API details

When you need to make an API call:

1. Fetch the Swagger spec for the target environment using `WebFetch`
2. Extract the exact request schema (path params, query params, headers, body) for the endpoint you need
3. Use that information to construct the correct request

This ensures you always work with the current API contract, not stale documentation.

## Step 4: Execute tests

For each test scenario:

1. **Describe** what you're about to test and why
2. **Execute** the test action:
   - API call via `curl` or `WebFetch`
   - Data query via Metabase
   - Log inspection via BetterStack
   - Or guide the user through manual steps
3. **Verify** the result:
   - Check the API response (status code, body)
   - Verify database state changed as expected (Metabase query)
   - Check logs for errors or expected entries (BetterStack)
4. **Record** the result: PASS, FAIL, or BLOCKED

**Important guidelines:**
- Always explain what you're doing before doing it
- For write operations, show the request you're about to make and get user confirmation
- **Never make write operations against production** without explicit user approval
- If a test fails, investigate the root cause before moving on
- If you're blocked, note what's blocking and move to the next test

## Step 5: Check logs

After executing test actions, check BetterStack for:

- Errors or warnings related to the actions performed
- Expected log entries confirming the action was processed
- Unexpected side effects or cascading errors
- Performance issues (slow responses, timeouts)

Use the query instructions tool first if unfamiliar with the log query syntax.

## Step 6: Document findings

Compile a QA summary using the report template from `references/report-template.md`.

## Step 7: Post to Linear

If a Linear ticket is associated:

1. Post the QA summary as a comment on the ticket using `Linear:create_comment`
2. If all tests pass, suggest updating the ticket status (ask the user first)
3. If tests fail, suggest keeping the ticket in its current status and optionally creating a sub-issue for each bug found

If no ticket is associated but issues were found:
- Ask the user if they want to create a new ticket for any findings
- Offer to use the bug-reporter workflow for critical issues
