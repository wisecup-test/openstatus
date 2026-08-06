# Validate Search Parameters Through Centralized Cache Parser: Parameter Validation Independent

These rules are ALWAYS ACTIVE for server-side page components that receive search parameters from routing, async page functions that prefetch data based on query parameters, and route handlers where search parameters control application behavior or data access.

### Rules

- **R-PARAM-001** SHOULD: Parameter validation and independent query prefetching SHOULD be coordinated in parallel when no data dependencies exist between them.
- **R-PARAM-002** MUST: All page components that receive search parameters MUST parse them through the centralized cache parser before use.
- **R-PARAM-003** MUST: Search parameter schemas MUST be defined in a centralized location that exports typed cache parsers for each route or route group.
- **R-PARAM-004** MUST: Validation failures MUST be handled explicitly by checking for required parameters after parsing and implementing appropriate fallback behavior such as redirects or error pages.
- **R-PARAM-005** SHOULD: Page components SHOULD structure parameter parsing and prefetch operations using Promise.all when operations are independent, maintaining both safety and performance.

### Verify

```bash
# Discover and run the project's test execution script covering page components with search parameter validation
# (Test script location derived from dependency manifest)

# Discover and run the project's static analysis or type-checking script to verify type-safe search parameter access
# (Type-checking script location derived from build tool configuration)

# Discover and run the project's linting configuration to detect direct access to raw search parameters that bypass validation
# (Linter configuration location derived from project root)
```

**Accept when:**
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed
- No direct access to raw search parameters that bypasses validation is detected by linting

<enforcement>
Claude Code MUST NOT skip or defer verification. Static type checking in continuous integration MUST fail on unsafe parameter access. Code review MUST enforce validation of search parameter handling in new page components. Integration tests MUST exercise parameter validation paths and assert on validation behavior. Pull requests with unvalidated search parameter access MUST be blocked by code review.
</enforcement>