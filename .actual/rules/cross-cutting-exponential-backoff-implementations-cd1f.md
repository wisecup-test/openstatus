# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Exponential Backoff Implementations

These rules are ALWAYS ACTIVE for all HTTP clients instantiated for external endpoint monitoring, health-check handlers that evaluate HTTP assertions, ping and region availability verification workflows, and integration test scenarios that exercise HTTP request/response cycles.

### Rules

- **R-EXP-001** SHOULD: Exponential backoff implementations should use context-aware retry functions that respect cancellation signals and deadline propagation.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool,
# then locate and inspect the lock or resolution artifact to determine exact
# resolved versions of retry and HTTP client dependencies.
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' -o -name 'pom.xml' -o -name 'Gemfile.lock' | head -5

# Discover the project's test execution tooling and run integration test suites
# that exercise HTTP client retry behavior with simulated transient failures
# and timeout conditions.
grep -r "integration" . --include="*.go" --include="*.sh" --include="Makefile" | grep -i test | head -10

# Discover the project's static analysis or linting configuration and verify
# that HTTP client instantiation sites include exponential backoff retry wrappers
# with configurable retry counts.
grep -r "http\.Client\|NewClient\|Do(" . --include="*.go" | grep -v vendor | head -20
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.
- HTTP client instantiation occurs within the retry operation closure to ensure each retry attempt uses a fresh client instance with independent timeout tracking.
- Timeout parameters sourced from monitor or request specifications are validated against minimum and maximum bounds before conversion to duration types.

<enforcement>
Clause Code MUST NOT skip or defer verification. All HTTP client instantiation sites must be audited for exponential backoff retry wrappers before code review approval.
</enforcement>