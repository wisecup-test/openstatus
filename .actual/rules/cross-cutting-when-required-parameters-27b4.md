# Validate Search Parameters Through Centralized Cache Before Page Rendering: When Required Parameters

These rules are ALWAYS ACTIVE for all async page components in server-side rendering contexts that receive search parameters as promises, including data prefetching logic that depends on URL query parameters, conditional routing or redirect logic based on parameter presence or values, and type definitions and schemas for expected search parameter shapes.

### Rules

- **R-PARAM-001** MUST: When required parameters are missing or invalid after validation, page components MUST handle the failure case explicitly through redirects or error states rather than proceeding with undefined values.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering page components with search parameter validation
# (Exact command depends on project's build tool and test runner — derive from package.json or build manifest)

# Discover the project's static analysis or linting configuration and run checks that identify direct search parameter access without validation
# (Exact command depends on project's linter configuration — derive from eslint/tsconfig/similar)

# Discover the project's build process and verify that page components with search parameters compile without type errors when validation cache parsing is applied
# (Exact command depends on project's build tool — derive from package.json or build manifest)
```

**Accept when:**
- All async page components that receive search parameters demonstrate validation through the centralized cache before using parameter values in data fetching or routing logic
- Test coverage includes both valid and invalid search parameter cases, verifying that validation failures are handled through redirects or error states
- Static analysis or code review confirms no direct access to search parameter values without prior validation in server-side page components

<enforcement>
Claude Code MUST NOT skip or defer verification. All page components accepting search parameters MUST be validated through the centralized cache before any data fetching or routing decisions occur. Pull requests introducing page components without search parameter validation are blocked until validation is added. Static analysis violations trigger build failures in continuous integration pipelines.
</enforcement>