# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Retry Operations Accept

These rules are ALWAYS ACTIVE for all HTTP client instantiation sites that invoke external endpoints for monitoring, health-check operations, ping verification, region availability checks, and integration test scenarios.

### Rules

- **R-RETRY-001** MUST: Retry operations must accept a configurable maximum retry count parameter that governs the upper bound of retry attempts.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'go.mod' -o -name 'package.json' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Gemfile' -o -name 'requirements.txt' -o -name 'Cargo.toml' \) | head -1

# Locate and inspect the lock or resolution artifact to determine exact resolved versions
find . -maxdepth 2 -type f \( -name 'go.sum' -o -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'pom.lock' -o -name 'Gemfile.lock' -o -name 'Cargo.lock' \) | head -1

# Discover the project's test execution tooling and run integration test suites
find . -maxdepth 2 -type f -name '*test*' | grep -E '\.(sh|yaml|yml|json)$' | head -5

# Discover static analysis or linting configuration
find . -maxdepth 2 -type f \( -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' \) | head -1

# Search for HTTP client instantiation patterns without retry wrappers
grep -r 'http\.Client\|NewClient\|http\.Get\|http\.Post' . --include='*.go' --include='*.js' --include='*.ts' --include='*.py' 2>/dev/null | grep -v 'retry\|backoff\|exponential' | head -20
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.
- Timeout parameters sourced from monitor or request specifications are validated against minimum and maximum bounds before conversion to duration types.
- HTTP client instantiation occurs within the retry operation closure to ensure each retry attempt uses a fresh client instance with independent timeout tracking.

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiation sites must be audited for exponential backoff retry wrapper presence and configurable retry count parameters before code is accepted.
</enforcement>