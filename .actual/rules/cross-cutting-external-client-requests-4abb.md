# Standardize Service Boundary Identification Through Request Context Retrieval: External Client Requests

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS monitoring handlers that process external check requests, external client integrations that send events to analytics or third-party services, retry logic implementations, and health check endpoints that report service identity.

### Rules

- **R-SB-001** SHOULD: External client requests SHOULD set identifying headers to distinguish service-originated traffic from other sources.

### Verify

```bash
# Locate and run the integration test suite that verifies handler context retrieval
find . -name '*test*' -o -name '*spec*' | grep -i integration | head -1 | xargs -I {} bash -c 'cd $(dirname {}) && npm test || go test ./... || python -m pytest'

# Search for context retrieval patterns across all handler implementations
grep -r "c\.Get\|c\.Set\|req\.Header\.Get\|response\.Header\.Get" --include="*.go" --include="*.js" --include="*.ts" .

# Verify context-aware logging is used in error paths
grep -r "log\.Ctx\|logger\.Error" --include="*.go" --include="*.js" --include="*.ts" . | grep -v test

# Check for static analysis or linting configuration
find . -name ".eslintrc*" -o -name "golangci.yml" -o -name "pylintrc" | head -1
```

**Accept when:**
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.
- All external client requests include identifying headers in outbound requests to analytics or third-party services.

<enforcement>
Claude Code MUST NOT skip or defer verification. All handler implementations MUST retrieve request context before processing external requests. Code review MUST verify that new service boundaries follow the context retrieval pattern. Integration test failures MUST block deployment.
</enforcement>