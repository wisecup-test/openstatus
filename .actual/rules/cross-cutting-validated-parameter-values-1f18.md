# Validate Search Parameters Through Centralized Cache Parser: Validated Parameter Values

These rules are ALWAYS ACTIVE for server-side page components that receive search parameters from routing, async page functions that prefetch data based on query parameters, and route handlers where search parameters control application behavior or data access.

### Rules

- **R-PARAM-001** MUST: Validated parameter values MUST be used for conditional logic such as redirects or data fetching decisions.
- **R-PARAM-002** MUST: All search parameters received from HTTP requests MUST be parsed through the centralized cache parser before use.
- **R-PARAM-003** MUST: Page components MUST await parameter parsing and coordinate with prefetch operations using Promise.all when operations are independent.
- **R-PARAM-004** MUST: Validation failures MUST be handled explicitly by checking for required parameters after parsing and implementing appropriate fallback behavior such as redirects or error pages.
- **R-PARAM-005** SHOULD: Search parameter schemas SHOULD be defined in a centralized location that exports typed cache parsers for each route or route group.

### Verify

```bash
# Discover and run the project's test execution script covering page components with search parameter validation
# Discover and run the project's static analysis or type-checking script to verify type-safe search parameter access
# Discover and run the project's linting configuration to detect direct access to raw search parameters that bypass validation
```

**Accept when:**
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed
- No direct access to raw search parameters exists that bypasses validation

<enforcement>
Claude Code MUST NOT skip or defer verification. Type checking failures in CI prevent merge until parameter access is properly validated. Pull requests with unvalidated search parameter access are blocked by code review.
</enforcement>