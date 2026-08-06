# Validate Search Parameters Through Type-Safe Cache Before Processing: Consumer Discover Project

These rules are ALWAYS ACTIVE for all server-side page components that receive search parameters as promises from URL query strings, and for any data prefetching operations that depend on URL query parameters.

### Rules

- **R-PARAM-001** MUST: Discover the project's dependency lock artifact and resolve the exact installed version of all validation and parameter parsing libraries before implementing validation logic.
- **R-PARAM-002** MUST: Validate search parameters through a type-safe cache mechanism before consuming them in server-side rendering logic or data fetching operations.
- **R-PARAM-003** MUST: Ensure validation cache parsing occurs after awaiting the search parameters promise but before any data prefetching operations that depend on parameter values.
- **R-PARAM-004** MUST: Define validation schemas in a co-located module alongside page components to maintain clear ownership and reduce the risk of schema drift.
- **R-PARAM-005** MUST: For parameters that control routing decisions, validate them first and handle missing or invalid values with explicit redirects rather than allowing the page to render with undefined behavior.
- **R-PARAM-006** SHOULD: Establish code review checklist items for search parameter validation to prevent developers from forgetting to apply validation to new page components.
- **R-PARAM-007** SHOULD: Implement static analysis or linting rules to detect unvalidated search parameter usage in server-side page components.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering search parameter validation logic
# (Consumer must identify test runner from project configuration)

# Discover the project's static analysis or linting configuration and run checks that detect unvalidated search parameter usage in server-side page components
# (Consumer must identify linting tool from project configuration)

# Discover the project's code search capabilities and verify that all page components receiving search parameters include validation cache parsing before data operations
# (Consumer must use project's code search tool to audit all page components)
```

**Accept when:**
- All server-side page components that receive search parameters demonstrate validation cache parsing before any data fetching or business logic execution.
- Test coverage includes validation of malformed, missing, and malicious search parameter inputs.
- Static analysis or code review confirms no direct consumption of unvalidated search parameters in server-side rendering logic.
- Validation schemas are co-located with their corresponding page components.
- All parameters controlling routing decisions are validated with explicit redirect handling for invalid values.

<enforcement>
Claude Code MUST NOT skip or defer verification. All applicable page components MUST demonstrate validation cache parsing before data operations. Code review MUST block merge of components that consume search parameters without validation. Static analysis failures MUST trigger build failures. Security testing failures MUST be treated as high-priority defects.
</enforcement>