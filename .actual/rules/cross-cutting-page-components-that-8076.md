# Validate Search Parameters Through Centralized Cache Parser: Page Components That

These rules are ALWAYS ACTIVE for all server-side page components that receive search parameters from the framework router, and for route handlers that process URL query strings.

### Rules

- **R-CACHE-001** MUST: All page components that receive search parameters MUST parse them through the centralized cache parser before consuming any parameter values.
- **R-CACHE-002** MUST: Route handlers that process URL query strings MUST validate parameters through the centralized cache parser before passing them to data access layers or application logic.
- **R-CACHE-003** MUST: Data fetching logic that depends on search parameter values MUST await cache parser resolution before executing queries or API calls.
- **R-CACHE-004** SHOULD: Structure page components to await cache parser resolution before any conditional logic or data fetching operations.
- **R-CACHE-005** SHOULD: For optional parameters or those with default values, ensure the cache parser schema explicitly declares these defaults rather than handling them in component logic after parsing.

### Verify

```bash
# Discover the project's static analysis configuration and execute linting/type-checking
# to verify search parameter usage patterns
grep -r "searchParamsCache\.parse" --include="*.ts" --include="*.tsx" src/

# Locate test files corresponding to page components and execute the test suite
# to verify cache parser integration
find . -name "*.test.ts" -o -name "*.test.tsx" | xargs grep -l "searchParamsCache"

# Search the codebase for page component definitions that accept search parameters
# and verify each invokes the cache parser before parameter consumption
grep -r "searchParams" --include="*.tsx" src/app/ | grep -v "searchParamsCache.parse"
```

**Accept when:**
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Test coverage includes validation of cache parser integration for all page components with search parameters
- No page components are found that access search parameters without prior cache parser invocation

<enforcement>
Claude Code MUST NOT skip or defer verification. All page components accepting search parameters MUST be validated against the cache parser invocation requirement before code is considered compliant.
</enforcement>