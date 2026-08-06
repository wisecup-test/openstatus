# Validate Search Parameters Through Centralized Cache Parser: Search Parameter Schemas

These rules are ALWAYS ACTIVE for all server-side page components that accept searchParams props, async page functions that perform data prefetching based on URL parameters, and components that implement conditional routing or redirects based on query parameters.

### Rules

- **R-SEARCH-001** MUST: Define search parameter schemas in dedicated schema files co-located with page components.
- **R-SEARCH-002** MUST: Invoke the cache parser immediately after the async searchParams prop is received, before any conditional logic or data fetching operations.
- **R-SEARCH-003** MUST: Ensure parsing operations complete before evaluating redirect conditions to prevent routing decisions based on unvalidated input.
- **R-SEARCH-004** SHOULD: Implement standard error handling patterns for validation failures, such as redirecting to default page state or showing error boundaries.
- **R-SEARCH-005** SHOULD: Maintain clear ownership and facilitate schema discovery by co-locating schema definitions with their corresponding page components.

### Verify

```bash
# Discover the project's static analysis configuration and execute the type-checking workflow
# to verify search parameter schemas match usage
echo "Running type-checking workflow..."

# Locate the project's test suite and execute tests covering search parameter validation
echo "Running search parameter validation tests..."

# Identify the project's linting configuration and run the linter to detect direct
# searchParams access without cache parser invocation
echo "Running linter to detect unvalidated searchParams access..."
```

**Accept when:**
- All server-side page components that accept searchParams props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to searchParams properties occurs without prior cache parser invocation
- Search parameter schemas are defined in dedicated files co-located with page components
- Error handling for invalid search parameters is implemented at the page level

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for compliance with the centralized search parameter validation pattern. Type checking failures, linting violations, or code review findings indicating unvalidated searchParams access block deployment.
</enforcement>