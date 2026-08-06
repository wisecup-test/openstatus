# Externalize HTTP Client Configuration via Environment Variables: Http Clients Construct

These rules are ALWAYS ACTIVE for all external HTTP clients that communicate with third-party services and require runtime configuration for endpoint URLs and authentication credentials.

### Rules

- **R-HTTP-001** MAY: HTTP clients MAY construct request URLs dynamically using query parameters for flexible request composition.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
find . -name "go.mod" -o -name "package.json" -o -name "requirements.txt" -o -name "pom.xml" -o -name "Gemfile" | head -1

# Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
find . -path "./node_modules" -prune -o -path "./vendor" -prune -o -type f \( -name "*_test.go" -o -name "*.test.js" -o -name "test_*.py" \) -print | grep -i http | head -5

# Search the codebase for environment variable retrieval patterns and verify that credentials are not hard-coded or committed to version control
grep -r "os\.Getenv\|process\.env\|ENV\[" --include="*.go" --include="*.js" --include="*.py" --include="*.java" . 2>/dev/null | grep -i "url\|token\|key\|secret\|credential" | head -10

# Verify no hard-coded URLs or credentials in HTTP client implementations
grep -r "http://\|https://" --include="*.go" --include="*.js" --include="*.py" --include="*.java" . 2>/dev/null | grep -v "example\.com\|localhost\|test" | grep -v "//" | head -5
```

**Accept when:**
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration
- No credentials or service URLs are committed to version control
- Environment variable naming follows a consistent convention with service-specific prefixes

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review MUST block merge requests that hard-code credentials or service URLs. Static analysis failures in CI pipelines MUST prevent deployment of code with configuration violations.
</enforcement>