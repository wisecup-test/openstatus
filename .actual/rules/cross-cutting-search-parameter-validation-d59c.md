# Validate Search Parameters Through Centralized Cache Parser: Search Parameter Validation

These rules are ALWAYS ACTIVE for server-side page components that receive search parameters from routing, async page functions that prefetch data based on query parameters, and route handlers where search parameters control application behavior or data access.

### Rules

- **R-SEARCH-001** MUST: Search parameter validation MUST occur before any data fetching operations that depend on parameter values.
- **R-SEARCH-002** MUST: All page components that receive search parameters parse them through the centralized cache parser before use.
- **R-SEARCH-003** MUST: Search parameter schemas be defined in a centralized location that exports typed cache parsers for each route or route group.
- **R-SEARCH-004** SHOULD: Structure page components to await parameter parsing and coordinate with prefetch operations using Promise.all when operations are independent.
- **R-SEARCH-005** SHOULD: Handle validation failures explicitly by checking for required parameters after parsing and implementing appropriate fallback behavior such as redirects or error pages.
- **R-SEARCH-006** SHOULD: Wrap parser calls in try-catch blocks that map validation errors to appropriate HTTP responses or redirect to error pages with context.

### Verify

```bash
# Discover and run the project's test execution script covering page components with search parameter validation
# (Test script location must be discovered from the project's dependency manifest)

# Discover and run the project's static analysis or type-checking script to verify type-safe search parameter access
# (Type-checking script location must be discovered from the project's dependency manifest)

# Discover and run the project's linting configuration to detect direct access to raw search parameters that bypass validation
# (Linting script location must be discovered from the project's dependency manifest)
```

**Accept when:**
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed
- No direct access to raw search parameters that bypasses validation is detected by linting

<enforcement>
Claude Code MUST NOT skip or defer verification. Static type checking in continuous integration MUST fail on unsafe parameter access. Code review MUST enforce validation of search parameter handling in new page components. Integration tests MUST exercise parameter validation paths and assert on validation behavior. Pull requests with unvalidated search parameter access MUST be blocked by code review.
</enforcement>