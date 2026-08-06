# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Http Clients That

These rules are ALWAYS ACTIVE for all HTTP clients that invoke external endpoints for monitoring, health-check, or assertion evaluation operations.

### Rules

- **R-HTTP-001** MUST: All HTTP clients that invoke external endpoints for monitoring, health-check, or assertion evaluation operations must wrap the invocation in an exponential backoff retry mechanism.
- **R-HTTP-002** MUST: HTTP client instantiation should occur within the retry operation closure to ensure each retry attempt uses a fresh client instance with independent timeout tracking.
- **R-HTTP-003** MUST: Timeout parameters sourced from monitor or request specifications must be validated against minimum and maximum bounds before conversion to duration types to prevent misconfiguration.
- **R-HTTP-004** MUST: Error logging within retry operations must include structured context fields such as operation identifiers, retry attempt counts, and elapsed time to facilitate debugging and operational analysis.
- **R-HTTP-005** SHOULD: Implement context-aware retry functions that check cancellation status before each retry attempt and propagate cancellation errors immediately.
- **R-HTTP-006** SHOULD: Implement response body size limits and streaming evaluation for large payloads, with early termination on assertion failure.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'package.json' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Gemfile' \) | head -1

# Locate and inspect the lock or resolution artifact to determine exact resolved versions
find . -maxdepth 2 -type f \( -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pom.lock' -o -name 'Gemfile.lock' \) | head -1

# Discover the project's test execution tooling and run integration test suites
find . -maxdepth 2 -type f -name '*test*' | grep -E '\.(go|js|py|java)$' | head -5

# Discover the project's static analysis or linting configuration
find . -maxdepth 2 -type f \( -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name 'checkstyle.xml' \) | head -1

# Verify HTTP client instantiation sites include exponential backoff retry wrappers
grep -r 'http\.Client\|NewClient\|Do(' . --include='*.go' --include='*.js' --include='*.py' | grep -v 'test' | head -10
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.
- Timeout parameters are validated against minimum and maximum bounds before conversion to duration types.
- Context cancellation signals are properly respected in retry implementations.

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiation sites must be audited for compliance with R-HTTP-001 through R-HTTP-006 before code is committed.
</enforcement>