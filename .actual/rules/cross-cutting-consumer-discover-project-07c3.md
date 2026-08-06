# Validate Search Parameters Through Centralized Cache Parser: Consumer Discover Project

These rules are ALWAYS ACTIVE for server-side page components that receive search parameters from routing, async page functions that prefetch data based on query parameters, and route handlers where search parameters control application behavior or data access.

### Rules

- **R-DISCOVER-001** MUST: The consumer MUST discover the project's dependency lock artifact and resolve the exact installed version of the search parameter validation library before implementing validation logic.
- **R-VALIDATE-002** MUST: All page components that receive search parameters MUST parse them through the centralized cache parser before use.
- **R-VALIDATE-003** MUST: Search parameter validation MUST occur at the boundary where external input enters the system, before routing decisions or data fetching operations.
- **R-VALIDATE-004** MUST: Validation failures MUST be handled explicitly by checking for required parameters after parsing and implementing appropriate fallback behavior such as redirects or error pages.
- **R-VALIDATE-005** SHOULD: Page components SHOULD coordinate parameter parsing with prefetch operations using Promise.all when operations are independent to maintain both safety and performance.
- **R-VALIDATE-006** SHOULD: Search parameter schemas SHOULD be defined in a centralized location that exports typed cache parsers for each route or route group, ensuring schema definitions are co-located with their validation logic.

### Verify

```bash
# Discover the project's test execution script in the dependency manifest and run the test suite
# covering page components with search parameter validation

# Discover the project's static analysis or type-checking script and execute it to verify that
# search parameter access is type-safe throughout page components

# Discover the project's linting configuration and run the linter to detect any direct access
# to raw search parameters that bypasses validation
```

**Accept when:**
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed
- No direct access to raw search parameters that bypasses validation is detected by linting

<enforcement>
Claude Code MUST NOT skip or defer verification. Static type checking in continuous integration MUST fail on unsafe parameter access. Code review MUST enforce validation of search parameter handling in new page components. Integration tests MUST exercise parameter validation paths and assert on validation behavior. Pull requests with unvalidated search parameter access MUST be blocked by code review.
</enforcement>