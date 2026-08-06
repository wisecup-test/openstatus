# Standardize Service Boundary Identification Through Request Context Retrieval: Service Boundary Handlers

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS monitoring handlers that process external check requests, external client integrations that send events to analytics or third-party services, retry logic implementations, and health check endpoints that report service identity and regional information.

### Rules

- **R-SBH-001** MUST: All service boundary handlers MUST retrieve request-scoped event context using the framework's context retrieval mechanism before processing external requests.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# that verifies handler context retrieval across all service boundary types.
find . -name "*test*" -o -name "*integration*" | head -20

# Discover the static analysis or linting configuration and execute checks
# that verify context-aware logging is used in all error paths.
find . -name ".golangci.yml" -o -name "lint.yaml" -o -name ".eslintrc*" | head -10

# Search for context retrieval patterns to confirm all handlers follow
# the established pattern (e.g., c.Get('event'), log.Ctx(ctx))
grep -r "c\.Get\|log\.Ctx\|logger\.Error" --include="*.go" | grep -E "(handler|boundary)" | head -20
```

**Accept when:**
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging until context retrieval is added to flagged handlers. Code review identifies missing context usage and requests changes before approval. Integration test failures trigger build failures and prevent deployment to production environments.
</enforcement>