# Externalize HTTP Client Configuration via Environment Variables: Http Clients Use

These rules are ALWAYS ACTIVE for all external HTTP clients that communicate with third-party services and require authentication credentials or environment-specific endpoint URLs.

### Rules

- **R-HTTP-001** SHOULD: HTTP clients SHOULD use structured logging with context to record configuration errors and request failures.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'pom.xml' -o -name 'Gemfile' | head -1

# Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
find . -path '*/test*' -name '*http*' -o -path '*/test*' -name '*client*' | grep -E '\.(go|js|py|java|rb)$'

# Search the codebase for environment variable retrieval patterns and verify that credentials are not hard-coded or committed to version control
grep -r 'os\.getenv\|process\.env\|ENV\[\|System\.getenv' --include='*.go' --include='*.js' --include='*.py' --include='*.java' --include='*.rb' | grep -i 'url\|token\|key\|secret\|credential'

# Verify no hard-coded URLs or credentials in HTTP client implementations
grep -r 'https://\|http://' --include='*.go' --include='*.js' --include='*.py' --include='*.java' --include='*.rb' | grep -v 'localhost\|127\.0\.0\.1\|example\.com' | grep -v test | grep -v '//' | head -20
```

**Accept when:**
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration
- Structured logging is implemented to record configuration errors and request failures with context, with credential values redacted from log output

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client implementations MUST be reviewed against these rules before acceptance.
</enforcement>