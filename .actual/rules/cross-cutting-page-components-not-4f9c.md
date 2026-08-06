# Validate Search Parameters Through Centralized Cache Parser: Page Components Not

These rules are ALWAYS ACTIVE for all server-side page components that accept search parameters from the framework router, route handlers that process URL query strings, and data fetching logic that depends on search parameter values.

### Rules

- **R-SEARCH-001** MUST NOT: Page components MUST NOT access raw search parameter values directly without validation through the centralized cache parser.
- **R-SEARCH-002** MUST: Page components that accept search parameters MUST await cache parser resolution before any conditional logic or data fetching operations.
- **R-SEARCH-003** MUST: Cache parser schema definitions MUST explicitly declare optional parameters and default values rather than handling them in component logic after parsing.
- **R-SEARCH-004** MUST: All search parameter validation MUST occur at page component boundaries before data is passed to application logic, database queries, or API calls.
- **R-SEARCH-005** SHOULD: Asynchronous parsing operations SHOULD be coordinated with Promise.all to ensure validation completes before downstream operations.

### Verify

```bash
# Discover the project's static analysis configuration and execute linting/type-checking
# to verify search parameter usage patterns
grep -r "searchParamsCache" --include="*.ts" --include="*.tsx" src/

# Search for page component definitions that accept search parameters
grep -r "searchParams" --include="*.ts" --include="*.tsx" src/app/ | grep -E "(page\.(ts|tsx)|route\.(ts|tsx))"

# Verify each page component invokes the cache parser before parameter consumption
grep -B5 -A5 "searchParams" src/app/**/page.tsx | grep -E "(searchParamsCache\.parse|await.*parse)"

# Locate and execute test files for page components
find . -name "*.test.ts" -o -name "*.test.tsx" | xargs grep -l "searchParams"

# Run type checking to ensure search parameters are accessed only after cache parser resolution
```

**Accept when:**
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Type checking enforcement confirms search parameters are accessed only after cache parser resolution
- Test coverage includes validation of cache parser integration for all page components with search parameters
- No page components are found accessing raw search parameter values without prior cache parser invocation

<enforcement>
Claude Code MUST NOT skip or defer verification. All page components accepting search parameters MUST be scanned for cache parser invocation before parameter access. Type checking MUST pass. Code review MUST verify cache parser integration for any changes involving search parameters.
</enforcement>