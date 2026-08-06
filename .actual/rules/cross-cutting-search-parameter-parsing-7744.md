# Validate Search Parameters Through Centralized Cache Parser: Search Parameter Parsing

These rules are ALWAYS ACTIVE for all server-side page components that accept search parameters, route handlers that process URL query strings, and data fetching logic that depends on search parameter values.

### Rules

- **R-SEARCH-001** MUST: Search parameter parsing MUST occur before any data fetching operations that depend on parameter values.
- **R-SEARCH-002** MUST: All server-side page components that accept search parameters MUST invoke the centralized cache parser before accessing parameter values.
- **R-SEARCH-003** MUST: Search parameters from untrusted client input via URL query strings MUST be validated through the centralized searchParamsCache parser before being consumed by application logic or passed to data access layers.
- **R-SEARCH-004** SHOULD: Asynchronous parsing operations SHOULD be coordinated with Promise.all to ensure parsing completes before downstream operations that depend on parameter values.
- **R-SEARCH-005** SHOULD: Optional or default-valued parameters SHOULD be explicitly declared in the cache parser schema rather than handled in component logic after parsing.
- **R-SEARCH-006** MUST: Cache parser invocations MUST be wrapped in error boundaries with fallback behavior for validation failures.

### Verify

```bash
# Discover the project's static analysis configuration and execute linting/type-checking
# to verify search parameter usage patterns

# Locate test files corresponding to page components and execute the test suite
# to verify cache parser integration

# Search the codebase for page component definitions that accept search parameters
# and verify each invokes the cache parser before parameter consumption
grep -r "searchParams" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.jsx" | grep -v "searchParamsCache.parse"
```

**Accept when:**
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Test coverage includes validation of cache parser integration for all page components with search parameters
- Type checking enforcement ensures search parameters are accessed only after cache parser resolution
- No page components are detected accessing search parameters without cache parser validation

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis in continuous integration MUST scan for page components with unvalidated search parameter access. Code review MUST verify cache parser invocation for any page component changes involving search parameters. Violations MUST block merge requests and fail the CI pipeline.
</enforcement>