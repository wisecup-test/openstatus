# Validate Search Parameters Through Centralized Cache Before Page Rendering: Page Components Await

These rules are ALWAYS ACTIVE for all async page components in server-side rendering contexts that receive search parameters as promises, including data prefetching logic, conditional routing, and type definitions for expected search parameter shapes.

### Rules

- **R-PARAM-001** MUST: Page components MUST await the search parameter promise and pass it to the validation cache rather than accessing parameter values directly.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering page components with search parameter validation
# (Exact command depends on project's build tool and test runner — inspect package.json or build manifest)

# Discover the project's static analysis or linting configuration and run checks that identify direct search parameter access without validation
# (Exact command depends on project's linter configuration — inspect eslint/tsconfig or equivalent)

# Discover the project's build process and verify that page components with search parameters compile without type errors when validation cache parsing is applied
# (Exact command depends on project's build tool — inspect build manifest or tsconfig)
```

**Accept when:**
- All async page components that receive search parameters demonstrate validation through the centralized cache before using parameter values in data fetching or routing logic
- Test coverage includes both valid and invalid search parameter cases, verifying that validation failures are handled through redirects or error states
- Static analysis or code review confirms no direct access to search parameter values without prior validation in server-side page components

<enforcement>
Clause Code MUST NOT skip or defer verification. All page components receiving search parameters MUST be validated through the centralized cache before any data fetching or routing decisions. Violations trigger build failures and require security review before merge.
</enforcement>