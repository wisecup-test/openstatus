# Validate Search Parameters Through Centralized Cache Parser: Page Components That

These rules are ALWAYS ACTIVE for all server-side page components that accept `searchParams` props and perform conditional logic, data prefetching, or routing decisions based on URL query parameters.

### Rules

- **R-SEARCH-001** MUST: Page components that conditionally redirect based on search parameter presence MUST parse parameters through a centralized cache parser before evaluating redirect conditions.
- **R-SEARCH-002** MUST: Search parameter parsing MUST occur immediately after the async `searchParams` prop is received, before any conditional logic or data fetching operations.
- **R-SEARCH-003** MUST: Search parameter schemas MUST be defined in dedicated files co-located with page components to maintain clear ownership and facilitate schema discovery.
- **R-SEARCH-004** SHOULD: Implement standard error handling patterns for validation failures, such as redirecting to default page state or showing error boundaries.

### Verify

```bash
# Discover the project's static analysis configuration and execute the type-checking workflow
# to verify search parameter schemas match usage
find . -name "tsconfig.json" -o -name "eslint.config.*" -o -name ".eslintrc*" | head -1

# Locate the project's test suite and execute tests covering search parameter validation
find . -type f \( -name "*.test.ts" -o -name "*.test.tsx" -o -name "*.spec.ts" \) | grep -i "search\|param" | head -5

# Identify the project's linting configuration and run the linter
# to detect direct searchParams access without cache parser invocation
grep -r "searchParams" --include="*.ts" --include="*.tsx" | grep -v "parseSearchParams\|cache.*parser" | head -10
```

**Accept when:**
- All server-side page components that accept `searchParams` props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to `searchParams` properties occurs without prior cache parser invocation
- Search parameter schemas are co-located with their corresponding page components
- Error handling for invalid search parameters is implemented at the page level

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be checked before approving changes to server-side page components that handle search parameters.
</enforcement>