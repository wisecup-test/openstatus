# Standardize Service Boundary Identification Through Request Context Retrieval: Service Boundaries Query

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS monitoring handlers that process external check requests, external client integrations that send events to analytics or third-party services, retry logic implementations that maintain request identity across multiple attempts, and health check endpoints that report service identity and regional information.

### Rules

- **R-SB-001** MAY: Service boundaries MAY query additional request parameters from the framework context to enrich logging output with request-specific metadata.
- **R-SB-002** MUST: All handler types (HTTP, TCP, DNS) retrieve request context using framework-specific context mechanisms (c.Get, c.Set) before processing external monitoring requests.
- **R-SB-003** MUST: Service boundaries propagate request context to structured context-aware loggers (log.Ctx(ctx).Error(), logger.Error()) for all error logging at boundary crossings.
- **R-SB-004** MUST: Request context retrieval patterns be consistent across all handler types to enable end-to-end observability for monitoring operations.
- **R-SB-005** SHOULD: Service boundary abstractions encapsulate context retrieval, header inspection, and logger initialization to reduce boilerplate and ensure consistency.
- **R-SB-006** SHOULD: Establish naming conventions for context keys to prevent collisions and make context usage self-documenting in code reviews.
- **R-SB-007** SHOULD: Document expected context fields for each handler type in interface definitions or handler registration code to serve as a contract for new implementations.
- **R-SB-008** MUST: Retry logic with exponential backoff maintain stable request identity across multiple attempts by preserving context through c.Set('event', t) operations.
- **R-SB-009** MUST NOT: Rely on global request ID generation or global state for request context storage in concurrent or multi-threaded environments.
- **R-SB-010** MUST NOT: Pass event identifiers as explicit function parameters through all layers; use framework context mechanisms instead.

### Verify

```bash
# Locate and run the project's integration test suite for handler context retrieval
find . -name '*test*' -o -name '*spec*' | grep -i integration | head -1 | xargs -I {} bash -c 'cd $(dirname {}) && npm test || go test ./... || python -m pytest'

# Discover and execute static analysis or linting configuration
if [ -f .eslintrc* ] || [ -f eslint.config.* ]; then npx eslint . --format json; fi
if [ -f .golangci.yml ]; then golangci-lint run; fi
if [ -f pylintrc ] || [ -f .pylintrc ]; then pylint $(find . -name '*.py' -type f); fi

# Search for context retrieval patterns to confirm handler compliance
grep -r "c\.Get\|c\.Set\|log\.Ctx\|logger\.Error" --include="*.go" --include="*.js" --include="*.ts" . | grep -E "(handler|boundary|service)" | head -20

# Verify context propagation in error paths
grep -r "log\.Ctx(ctx)" --include="*.go" . | wc -l
grep -r "logger\.Error" --include="*.go" . | wc -l
```

**Accept when:**
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.
- Context retrieval patterns are consistent across all handler implementations.
- No global state or explicit parameter passing is used for request context propagation in concurrent environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. All handler implementations MUST be audited for context retrieval compliance before code review approval. Static analysis failures MUST block pull request merging until context retrieval is added to flagged handlers.
</enforcement>