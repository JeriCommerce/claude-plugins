# Customer Association Workflow

## Identifying the Customer(s)

Ask the user to identify the customer(s) making this request.

**Always ask:** "Do you know the customer's name or slug so I can look them up in Linear?"

## For Each Customer

1. **Search first** — Use `mcp__linear__list_customers` with the `query` parameter to search by name.
2. **If found** — Confirm with the user: "I found [Customer Name] in Linear. Is this the right one?"
3. **If not found** — Ask the user: "I didn't find [name] in Linear. Would you like me to create them as a new customer?" If yes, use `mcp__linear__save_customer` with the customer name.
4. **Multiple customers** — Ask: "Are there other customers requesting this as well?" Repeat the search/create flow for each one.

## Output

Collect all customer IDs for use when creating the issue and associating customer needs.
