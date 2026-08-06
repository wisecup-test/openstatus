# Validate Search Parameters Through Centralized Cache Before Page Rendering: Async Page Components

These rules are ALWAYS ACTIVE for all async page components in server-side rendering contexts that receive search parameters as promises, including data prefetching logic, conditional routing, and type definitions for expected search parameter shapes.

### Rules

- **R-ASYNC-PAGE-001** MUST: All async page components that accept search parameters as promises MUST parse those parameters through a centralized validation cache before using them in any data fetching, routing, or rendering logic.

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
Clause Code MUST NOT skip or defer verification. All async page components receiving search parameters must be audited for centralized cache validation before merging. Static analysis violations trigger build failures. Security review is required for any documented exceptions.
</enforcement>