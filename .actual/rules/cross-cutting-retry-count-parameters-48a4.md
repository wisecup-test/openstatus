# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Retry Count Parameters

These rules are ALWAYS ACTIVE for all HTTP client instantiation sites that invoke external endpoints for monitoring, health-check operations, ping and region availability verification, and integration test scenarios that exercise HTTP request/response cycles.

### Rules

- **R-RETRY-001** MAY: Retry count parameters may be sourced from request query parameters, cached event data, or monitor configuration specifications.
- **R-RETRY-002** MUST: HTTP client instantiation should occur within the retry operation closure to ensure each retry attempt uses a fresh client instance with independent timeout tracking.
- **R-RETRY-003** MUST: Timeout parameters sourced from monitor or request specifications should be validated against minimum and maximum bounds before conversion to duration types to prevent misconfiguration.
- **R-RETRY-004** MUST: Error logging within retry operations should include structured context fields such as operation identifiers, retry attempt counts, and elapsed time to facilitate debugging and operational analysis.
- **R-RETRY-005** MUST: All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- **R-RETRY-006** MUST: Context-aware retry functions must check cancellation status before each retry attempt and propagate cancellation errors immediately to prevent retry operations from continuing after caller has abandoned the request.
- **R-RETRY-007** SHOULD: Implement response body size limits and streaming evaluation for large payloads, with early termination on assertion failure to prevent excessive memory consumption.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'package.json' -o -name 'pom.xml' -o -name 'Gemfile' -o -name 'Cargo.toml' \) | head -1

# Locate and inspect the lock or resolution artifact to determine exact resolved versions
find . -maxdepth 2 -type f \( -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pom.lock' -o -name 'Gemfile.lock' -o -name 'Cargo.lock' \) | head -1

# Discover the project's test execution tooling and run integration test suites
find . -maxdepth 2 -type f -name '*test*' | grep -E '\.(sh|yaml|yml|json)$' | head -5

# Discover the project's static analysis or linting configuration
find . -maxdepth 2 -type f \( -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name '.rubocop.yml' \) | head -1

# Search for HTTP client instantiation sites without retry mechanisms
grep -r 'http\.Client\|NewClient\|http\.Get\|http\.Post' . --include='*.go' --include='*.js' --include='*.ts' --include='*.py' 2>/dev/null | grep -v 'retry\|backoff\|exponential' | head -10
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.
- Timeout parameters are validated against minimum and maximum bounds before conversion to duration types.
- Context cancellation signals are properly respected and propagated by retry operations.
- Response body handling includes size limits or streaming evaluation for large payloads.

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiation sites must be audited for compliance with exponential backoff retry requirements. Integration tests must be executed to confirm retry behavior under transient failure conditions. Code review verification must confirm retry logic is present before merging changes that introduce new HTTP clients.
</enforcement>