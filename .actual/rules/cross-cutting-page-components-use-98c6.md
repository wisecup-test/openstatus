# Validate Search Parameters Through Centralized Cache Parser: Page Components Use

These rules are ALWAYS ACTIVE for all server-side page components that accept `searchParams` props and perform data prefetching, conditional routing, or redirects based on URL query parameters.

### Rules

- **R-CACHE-001** MAY: Page components MAY use parsed search parameters for query prefetching in parallel with the parsing operation.
- **R-CACHE-002** MUST: Page components that accept `searchParams` props MUST invoke the centralized cache parser before using parameter values in conditional logic or data fetching operations.
- **R-CACHE-003** MUST: Search parameter schemas MUST be defined in dedicated files co-located with page components to maintain clear ownership and facilitate schema discovery.
- **R-CACHE-004** MUST: For pages with conditional redirects, the parsing operation MUST complete before evaluating redirect conditions to prevent routing decisions based on unvalidated input.
- **R-CACHE-005** MUST NOT: Page components MUST NOT access `searchParams` properties directly without prior cache parser invocation.

### Verify

```bash
# Discover the project's static analysis configuration and execute the type-checking workflow
# to verify search parameter schemas match usage
find . -name "tsconfig.json" -o -name "eslint.config.*" -o -name ".eslintrc*" | head -1

# Locate the project's test suite and execute tests covering search parameter validation
find . -type f \( -name "*.test.ts" -o -name "*.test.tsx" -o -name "*.spec.ts" -o -name "*.spec.tsx" \) | grep -i "search\|param" | head -5

# Identify the project's linting configuration and run the linter
# to detect direct searchParams access without cache parser invocation
grep -r "searchParams" --include="*.ts" --include="*.tsx" | grep -v "parseSearchParams\|cache.*parser" | head -10
```

**Accept when:**
- All server-side page components that accept `searchParams` props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to `searchParams` properties occurs without prior cache parser invocation
- Search parameter schemas are co-located with their corresponding page components
- Redirect conditions are evaluated only after parsing operations complete

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking and linting violations block deployment. Code review MUST verify cache parser usage in all page components before merge approval.
</enforcement>