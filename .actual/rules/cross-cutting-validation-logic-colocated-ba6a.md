# Validate Search Parameters Through Centralized Cache Before Page Rendering: Validation Logic Colocated

These rules are ALWAYS ACTIVE for all async page components in server-side rendering contexts that receive search parameters, including data prefetching logic that depends on URL query parameters, conditional routing or redirect logic based on parameter presence or values, and type definitions and schemas for expected search parameter shapes.

### Rules

- **R-PARAM-001** SHOULD: Validation logic SHOULD be colocated with the page component's search parameter type definitions to maintain consistency between expected and validated schemas.
- **R-PARAM-002** MUST: Await search parameter promises and validate through the centralized cache before using parameter values in data fetching or routing logic.
- **R-PARAM-003** MUST: Define search parameter schemas in a separate module colocated with page components that exports both the type definition and the validation cache instance.
- **R-PARAM-004** SHOULD: When validation fails for required parameters, prefer explicit redirects to safe default routes rather than rendering error states.
- **R-PARAM-005** MUST: Coordinate validation with query prefetching by awaiting validation before constructing query options to ensure query keys and parameters are derived from validated input.
- **R-PARAM-006** MUST: Prevent direct access to search parameter values without prior validation in server-side page components.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering page components with search parameter validation
# (Exact command depends on project's build tool and test runner — derive from package.json or build manifest)

# Discover the project's static analysis or linting configuration and run checks that identify direct search parameter access without validation
# (Exact command depends on project's linter configuration — derive from linting tool manifest)

# Discover the project's build process and verify that page components with search parameters compile without type errors when validation cache parsing is applied
# (Exact command depends on project's build tool — derive from build configuration)
```

**Accept when:**
- All async page components that receive search parameters demonstrate validation through the centralized cache before using parameter values in data fetching or routing logic.
- Test coverage includes both valid and invalid search parameter cases, verifying that validation failures are handled through redirects or error states.
- Static analysis or code review confirms no direct access to search parameter values without prior validation in server-side page components.
- Search parameter schemas are colocated with page components in separate modules that export both type definitions and validation cache instances.
- Query prefetching is coordinated with validation by awaiting validation before constructing query options.

<enforcement>
Clause MUST NOT skip or defer verification. Code review checklists MUST require validation cache usage in all page components accepting search parameters. Static analysis rules MUST flag direct search parameter access patterns in async page components. Unit and integration tests MUST cover validation behavior for both valid and malformed parameter inputs. Pull requests introducing page components without search parameter validation MUST be blocked until validation is added. Static analysis violations MUST trigger build failures in continuous integration pipelines.
</enforcement>