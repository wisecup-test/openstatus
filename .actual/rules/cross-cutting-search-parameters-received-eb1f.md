# Validate Search Parameters Through Centralized Cache Parser: Search Parameters Received

These rules are ALWAYS ACTIVE for all server-side page components that accept searchParams props and perform data prefetching, conditional routing, or redirects based on URL parameters.

### Rules

- **R-SEARCH-001** MUST: All search parameters received by server-side page components MUST be parsed through a centralized cache parser before use in conditional logic, data fetching, or routing decisions.

### Verify

```bash
# Discover the project's static analysis configuration and execute the type-checking workflow
# to verify search parameter schemas match usage
find . -name "tsconfig.json" -o -name "eslint.config.*" -o -name ".eslintrc*" | head -1

# Locate the project's test suite and execute tests covering search parameter validation
find . -name "*.test.ts" -o -name "*.test.tsx" -o -name "*.spec.ts" | grep -i "search\|param" | head -5

# Identify the project's linting configuration and run the linter
# to detect direct searchParams access without cache parser invocation
grep -r "searchParams" --include="*.ts" --include="*.tsx" | grep -v "cache\|parser" | head -10
```

**Accept when:**
- All server-side page components that accept searchParams props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to searchParams properties occurs without prior cache parser invocation
- Search parameter schemas are defined in dedicated files co-located with page components
- Cache parser invocation occurs immediately after the async searchParams prop is received, before any conditional logic or data fetching operations

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking and linting must pass in the continuous integration pipeline before deployment. Code review must verify cache parser usage in all page components. Violations block merge approval and deployment.
</enforcement>