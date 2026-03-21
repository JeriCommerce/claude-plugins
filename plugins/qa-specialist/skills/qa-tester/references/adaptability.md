# Adaptability

Not every QA session requires all tools or all steps:

- **Quick endpoint check**: Skip data prep, just look up the Swagger spec, make the API call, and verify response
- **Data verification only**: Skip API calls, just query Metabase to verify data integrity
- **Log investigation**: Focus on BetterStack to trace an issue
- **Manual testing guidance**: Just help the user understand what to test and how, without automated execution
- **Ticket review**: Read the ticket, understand the feature, suggest test scenarios, but let the user execute

Adapt to what the user actually needs. The full workflow is for comprehensive QA sessions; many requests will only need a subset.
