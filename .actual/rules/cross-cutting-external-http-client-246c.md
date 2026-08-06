# Externalize HTTP Client Configuration via Environment Variables: External Http Client

These rules are ALWAYS ACTIVE for all external HTTP client implementations that communicate with third-party services and require runtime configuration of endpoint URLs and authentication credentials.

### Rules

- **R-EX-001** MUST: External HTTP client implementations MUST retrieve service endpoint URLs from environment variables at runtime rather than hard-coding them in source code.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'pom.xml' -o -name 'build.gradle' | head -1

# Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
find . -path '*/test*' -name '*http*' -o -path '*/test*' -name '*client*' | grep -E '\.(go|js|py|java)$'

# Search the codebase for environment variable retrieval patterns and verify that credentials are not hard-coded or committed to version control
grep -r 'os\.getenv\|process\.env\|os\.environ\|System\.getenv' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | grep -i 'url\|endpoint\|token\|key\|credential'

# Verify no hard-coded URLs or credentials in HTTP client files
grep -r 'https://\|http://' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | grep -v 'example\|localhost\|test\|mock' | grep -v '//' | head -20
```

**Accept when:**
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration
- No credentials or service URLs are committed to version control in HTTP client implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. All external HTTP client implementations MUST be reviewed against R-EX-001 before acceptance.
</enforcement>