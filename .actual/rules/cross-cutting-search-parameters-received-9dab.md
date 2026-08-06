# Validate Search Parameters Through Type-Safe Cache Before Processing: Search Parameters Received

These rules are ALWAYS ACTIVE for all server-side page components that receive search parameters from URL query strings.

### Rules

- **R-SEARCH-001** MUST: All search parameters received by server-side page components MUST be parsed through a type-safe validation cache before being used in application logic, data fetching, or routing decisions.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering search parameter validation logic
# (Exact command depends on project's build tool — inspect package.json or equivalent manifest)

# Discover the project's static analysis or linting configuration and run checks that detect unvalidated search parameter usage in server-side page components
# (Exact command depends on project's linter — inspect configuration files)

# Discover the project's code search capabilities and verify that all page components receiving search parameters include validation cache parsing before data operations
# (Use grep, ripgrep, or IDE search to locate all server-side page components and confirm validation patterns)
```

**Accept when:**
- All server-side page components that receive search parameters demonstrate validation cache parsing before any data fetching or business logic
- Test coverage includes validation of malformed, missing, and malicious search parameter inputs
- Static analysis or code review confirms no direct consumption of unvalidated search parameters in server-side rendering logic

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review MUST block merge of page components that consume search parameters without validation. Static analysis failures MUST trigger build failures in continuous integration. Security testing failures MUST be treated as high-priority defects requiring immediate remediation.
</enforcement>