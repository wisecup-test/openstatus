# Standardize Service Boundary Identification Through Request Context Retrieval: Service Boundary Definitions

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS monitoring handlers that process external check requests, external client integrations that send events to analytics or third-party services, retry logic implementations, and health check endpoints that report service identity.

### Rules

- **R-SB-001** SHOULD: Service boundary definitions SHOULD store event correlation identifiers in request context immediately upon request receipt to enable tracing across retry attempts.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# that verifies handler context retrieval across all service boundary types.
find . -name "*test*" -o -name "*integration*" | head -20

# Discover the static analysis or linting configuration and execute checks
# that verify context-aware logging is used in all error paths.
find . -name ".golangci.yml" -o -name "lint.yaml" -o -name ".eslintrc*" | head -10

# Search for context retrieval patterns to confirm all handlers follow
# the established pattern (c.Get, c.Set, log.Ctx, logger.Error).
grep -r "c\.Get\|c\.Set\|log\.Ctx\|logger\.Error" --include="*.go" | head -30

# Verify context propagation in retry logic with exponential backoff.
grep -r "backoff\.Retry\|c\.Set.*event" --include="*.go" | head -20
```

**Accept when:**
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.
- Context retrieval patterns (c.Get, c.Set) are consistently used across all handler implementations.
- Header inspection patterns (req.Header.Get, response.Header.Get) are present in both inbound and outbound request processing.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging. Code review must verify context retrieval patterns. Integration test failures prevent deployment to production.
</enforcement>