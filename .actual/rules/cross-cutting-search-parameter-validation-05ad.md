# Validate Search Parameters Through Type-Safe Cache Before Processing: Search Parameter Validation

These rules are ALWAYS ACTIVE for all server-side page components that receive search parameters as promises from URL query strings, and for any data prefetching operations that depend on parameter values.

### Rules

- **R-PARAM-001** MUST: Search parameter validation MUST occur before any asynchronous data prefetching operations that depend on parameter values.

### Verify

```bash
# Discover the project's test runner configuration and execute the test suite covering search parameter validation logic
# (Exact command depends on project's build tool and test runner — consult package.json or build manifest)

# Discover the project's static analysis or linting configuration and run checks that detect unvalidated search parameter usage in server-side page components
# (Exact command depends on project's linter configuration — consult .eslintrc, tsconfig.json, or equivalent)

# Discover the project's code search capabilities and verify that all page components receiving search parameters include validation cache parsing before data operations
# Example: grep -r "searchParams" src/ --include="*.ts" --include="*.tsx" | grep -v "validate\|parse" to identify potential gaps
```

**Accept when:**
- All server-side page components that receive search parameters demonstrate validation cache parsing before any data fetching or business logic
- Test coverage includes validation of malformed, missing, and malicious search parameter inputs
- Static analysis or code review confirms no direct consumption of unvalidated search parameters in server-side rendering logic
- Validation schemas are co-located with page component definitions
- Invalid or missing parameters are handled with explicit redirects rather than undefined behavior

<enforcement>
Clause Code MUST NOT skip or defer verification. All page components receiving search parameters MUST validate them through a type-safe cache before any data prefetching. Code review MUST block merge of components that consume unvalidated search parameters. Static analysis failures MUST trigger build failures. Security testing failures MUST be treated as high-priority defects. Exceptions require documented justification, security review approval, and compensating controls recorded in the architectural decision log.
</enforcement>