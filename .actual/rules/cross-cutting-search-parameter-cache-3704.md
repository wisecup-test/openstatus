# Validate Search Parameters Through Centralized Cache Parser: Search Parameter Cache

These rules are ALWAYS ACTIVE for all server-side page components that accept searchParams props, async page functions that perform data prefetching based on URL parameters, and components that implement conditional routing or redirects based on query parameters.

### Rules

- **R-SEARCH-001** MUST: Invoke the search parameter cache parser on the Promise-wrapped searchParams object before extracting individual parameter values.
- **R-SEARCH-002** MUST: Implement the cache parser invocation immediately after the async searchParams prop is received, before any conditional logic or data fetching operations.
- **R-SEARCH-003** MUST: For pages with conditional redirects, ensure the parsing operation completes before evaluating redirect conditions to prevent routing decisions based on unvalidated input.
- **R-SEARCH-004** MUST: Define search parameter schemas in dedicated files co-located with page components to maintain clear ownership and facilitate schema discovery.
- **R-SEARCH-005** SHOULD: Establish standard error handling patterns for validation failures, such as redirecting to default page state or showing error boundaries.

### Verify

```bash
# Discover the project's static analysis configuration and execute the type-checking workflow
# to verify search parameter schemas match usage
echo "Running type-checking workflow..."

# Locate the project's test suite and execute tests covering search parameter validation
echo "Running search parameter validation tests..."

# Identify the project's linting configuration and run the linter to detect direct searchParams
# access without cache parser invocation
echo "Running linter to detect unvalidated searchParams access..."
```

**Accept when:**
- All server-side page components that accept searchParams props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to searchParams properties occurs without prior cache parser invocation
- Search parameter schemas are defined in dedicated files co-located with page components
- Error handling for invalid search parameters is implemented at the page level

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking in the continuous integration pipeline, code review process, and automated linting rules are mandatory enforcement mechanisms. Violations block deployment.
</enforcement>