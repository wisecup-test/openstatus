# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Http Client Timeout

These rules are ALWAYS ACTIVE for all HTTP client instantiation sites that invoke external endpoints for monitoring, health-check operations, ping verification, region availability checks, and integration test scenarios.

### Rules

- **R-HTTP-TIMEOUT-001** MUST: HTTP client timeout configuration must be derived from operation-specific timeout parameters expressed in milliseconds and converted to the appropriate duration type.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Gemfile' -o -name 'Gemfile.lock' \) | head -5

# Locate and inspect the lock or resolution artifact to determine exact resolved versions
if [ -f 'go.sum' ]; then grep -E '(http|retry|client)' go.sum | head -10; fi

# Discover the project's test execution tooling and run integration test suites
find . -maxdepth 2 -type f -name '*test*' | grep -E '\.(go|js|py|java)$' | head -5

# Verify HTTP client instantiation sites include exponential backoff retry wrappers
grep -r 'http\.Client\|NewClient\|HTTPClient' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | grep -v test | head -10

# Verify timeout configuration is derived from millisecond parameters
grep -r 'timeout.*ms\|millisecond\|Duration.*timeout' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | head -10
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.
- Timeout parameters are validated against minimum and maximum bounds before conversion to duration types.
- HTTP client instantiation occurs within the retry operation closure to ensure each retry attempt uses a fresh client instance with independent timeout tracking.

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiation sites must be audited for compliance with R-HTTP-TIMEOUT-001 before code is committed.
</enforcement>