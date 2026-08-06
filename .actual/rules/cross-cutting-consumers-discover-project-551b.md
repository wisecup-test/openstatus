# Validate Search Parameters Through Centralized Cache Before Page Rendering: Consumers Discover Project

These rules are ALWAYS ACTIVE for all async page components in server-side rendering contexts that receive search parameters as promises, including data prefetching logic, conditional routing, and type definitions for expected search parameter shapes.

### Rules

- **R-VALIDATE-001** MUST: Consumers MUST discover the project's dependency lock file and resolve the exact installed version of the search parameter validation library before implementing validation logic.
- **R-VALIDATE-002** MUST: All async page components that receive search parameters MUST validate them through the centralized cache before using parameter values in data fetching or routing logic.
- **R-VALIDATE-003** MUST: Search parameter validation MUST occur at the page entry point before any data fetching, query construction, or conditional routing decisions.
- **R-VALIDATE-004** MUST: Validation failures for required parameters MUST result in explicit redirects to safe default routes rather than rendering error states.
- **R-VALIDATE-005** MUST: Query prefetching coordination MUST await validation before constructing query options to ensure query keys and parameters are derived from validated input.
- **R-VALIDATE-006** SHOULD: Define search parameter schemas colocated with page components in a separate module that exports both the type definition and the validation cache instance.
- **R-VALIDATE-007** SHOULD: Implement shared validation utilities for common parameter patterns to prevent inconsistent validation behavior across page components.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering page components with search parameter validation
# (Consumer must identify test runner from project configuration)

# Discover the project's static analysis or linting configuration and run checks that identify direct search parameter access without validation
# (Consumer must identify linter/analyzer from project configuration)

# Discover the project's build process and verify that page components with search parameters compile without type errors when validation cache parsing is applied
# (Consumer must identify build tool from project configuration)
```

**Accept when:**
- All async page components that receive search parameters demonstrate validation through the centralized cache before using parameter values in data fetching or routing logic
- Test coverage includes both valid and invalid search parameter cases, verifying that validation failures are handled through redirects or error states
- Static analysis or code review confirms no direct access to search parameter values without prior validation in server-side page components
- The exact installed version of the search parameter validation library has been discovered from the project's dependency lock file
- Validation cache definitions are colocated with search parameter type definitions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting code changes. Static analysis rules that flag direct search parameter access patterns in async page components must be run. Code review checklists must confirm validation cache usage in all applicable page components.
</enforcement>