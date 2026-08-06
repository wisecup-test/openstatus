# Validate Search Parameters Through Centralized Cache Before Page Rendering: Search Parameter Validation

These rules are ALWAYS ACTIVE for all async page components in server-side rendering contexts that receive search parameters, including data prefetching logic that depends on URL query parameters, conditional routing or redirect logic based on parameter presence or values, and type definitions and schemas for expected search parameter shapes.

### Rules

- **R-PARAM-001** MUST: Search parameter validation MUST occur before initiating any query prefetch operations or conditional redirects.
- **R-PARAM-002** MUST: Define search parameter schemas colocated with page components, typically in a separate module that exports both the type definition and the validation cache instance.
- **R-PARAM-003** MUST: Await validation before constructing query options to ensure that query keys and parameters are derived from validated input, preventing cache pollution from malformed parameters.
- **R-PARAM-004** SHOULD: When validation fails for required parameters, prefer explicit redirects to safe default routes rather than rendering error states.
- **R-PARAM-005** SHOULD: Implement validation through a centralized search parameter cache mechanism rather than ad-hoc validation in individual consumers.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering page components with search parameter validation
# (Exact command depends on project's build tool and test runner — derive from package.json or build manifest)

# Discover the project's static analysis or linting configuration and run checks that identify direct search parameter access without validation
# (Exact command depends on project's linter configuration — derive from eslint/tsconfig or equivalent)

# Discover the project's build process and verify that page components with search parameters compile without type errors when validation cache parsing is applied
# (Exact command depends on project's build tool — derive from build manifest)
```

**Accept when:**
- All async page components that receive search parameters demonstrate validation through the centralized cache before using parameter values in data fetching or routing logic
- Test coverage includes both valid and invalid search parameter cases, verifying that validation failures are handled through redirects or error states
- Static analysis or code review confirms no direct access to search parameter values without prior validation in server-side page components
- Search parameter schemas are colocated with page components and synchronized with validation logic
- Query prefetching is coordinated with validation by awaiting validation before constructing query options

<enforcement>
Clause Code MUST NOT skip or defer verification. All page components accepting search parameters MUST demonstrate centralized validation before data fetching or routing decisions. Pull requests introducing page components without search parameter validation are blocked until validation is added. Static analysis violations trigger build failures in continuous integration pipelines.
</enforcement>