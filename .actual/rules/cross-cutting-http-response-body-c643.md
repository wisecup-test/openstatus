# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Http Response Body

These rules are ALWAYS ACTIVE for all HTTP clients instantiated for external endpoint monitoring, health-check handlers that evaluate HTTP assertions, ping and region availability verification workflows, and integration test scenarios that exercise HTTP request/response cycles.

### Rules

- **R-HTTP-001** SHOULD: HTTP response body content should be captured and made available for assertion evaluation regardless of retry attempt count.

### Verify

```bash
# Discover the project's dependency manifest and identify the build tool
find . -maxdepth 2 -type f \( -name 'package.json' -o -name 'go.mod' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Gemfile' -o -name 'requirements.txt' \) | head -1

# Locate and inspect the lock or resolution artifact to determine exact resolved versions
find . -maxdepth 2 -type f \( -name 'package-lock.json' -o -name 'yarn.lock' -o -name 'go.sum' -o -name 'pom.lock' -o -name 'Gemfile.lock' -o -name 'requirements.lock' \) | head -1

# Discover the project's test execution tooling and run integration test suites
find . -maxdepth 2 -type f \( -name 'Makefile' -o -name 'tox.ini' -o -name '.github/workflows/*.yml' -o -name 'jest.config.js' \) | head -1

# Verify HTTP client instantiation sites include exponential backoff retry wrappers
grep -r "exponential.*backoff\|retry.*backoff\|backoff.*retry" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . 2>/dev/null | grep -v node_modules | head -20

# Verify response body capture in retry logic
grep -r "response.*body\|body.*capture\|capture.*response" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . 2>/dev/null | grep -v node_modules | head -20
```

**Accept when:**
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.
- Response body content is captured and available for assertion evaluation across all retry attempts.

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiation sites must be audited to confirm response body capture is implemented regardless of retry attempt count.
</enforcement>