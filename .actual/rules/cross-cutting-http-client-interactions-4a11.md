# Standardize Service Boundary Identification Through Request Context Retrieval: Http Client Interactions

These rules are ALWAYS ACTIVE for all HTTP client interactions at service boundaries, including external client integrations, retry logic implementations, and health check endpoints that process external monitoring requests.

### Rules

- **R-HTTP-001** MUST: HTTP client interactions at service boundaries MUST inspect response headers to extract metadata required for logging and downstream processing.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# that verifies handler context retrieval across all service boundary types.
find . -name "*test*" -o -name "*integration*" | head -5

# Discover the static analysis or linting configuration and execute checks
# that verify context-aware logging is used in all error paths.
find . -name ".golangci.yml" -o -name "lint.yaml" -o -name ".eslintrc*"

# Search for context retrieval patterns to confirm all handlers follow
# the established pattern (framework-specific: c.Get, c.Set patterns).
grep -r "c\.Get\|c\.Set\|req\.Header\.Get\|response\.Header\.Get" --include="*.go" .

# Verify context-aware logging is used in error paths.
grep -r "log\.Ctx\|logger\.Error" --include="*.go" . | grep -v test
```

**Accept when:**
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.
- Response header inspection is consistently applied across all HTTP client interactions at service boundaries.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging until context retrieval and header inspection are added to flagged handlers. Code review must verify new service boundaries follow the context retrieval and header inspection pattern. Integration test failures trigger build failures and prevent deployment.
</enforcement>