# Standardize Service Boundary Identification Through Request Context Retrieval: Error Logging Service

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS monitoring handlers that process external check requests, external client integrations that send events to analytics or third-party services, retry logic implementations that maintain request identity across multiple attempts, and health check endpoints that report service identity and regional information.

### Rules

- **R-SB-001** MUST: Error logging at service boundaries MUST use context-aware structured loggers that receive the request context as their first parameter.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# that verifies handler context retrieval across all service boundary types.
find . -name "*test*" -o -name "*integration*" | head -20

# Discover the static analysis or linting configuration and execute checks
# that verify context-aware logging is used in all error paths.
find . -name ".golangci.yml" -o -name "lint.yaml" -o -name ".eslintrc*" | head -10

# Search for context retrieval patterns to confirm all handlers follow
# the established pattern (e.g., c.Get, log.Ctx, logger.Error).
grep -r "c\.Get\|log\.Ctx\|logger\.Error" --include="*.go" | grep -E "(handler|boundary)" | head -20

# Verify context propagation in error logging statements.
grep -r "log\.Ctx(ctx)\.Error\|logger\.Error" --include="*.go" | wc -l
```

**Accept when:**
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.
- Code review checklist verification confirms new service boundaries follow the context retrieval pattern.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging. Code review identifies missing context usage and requests changes before approval. Integration test failures trigger build failures and prevent deployment.
</enforcement>