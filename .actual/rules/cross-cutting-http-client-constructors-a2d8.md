# Externalize HTTP Client Configuration via Environment Variables: Http Client Constructors

These rules are ALWAYS ACTIVE for all HTTP clients that communicate with external third-party services, client implementations that require authentication credentials, service endpoint URLs that vary across deployment environments, and configuration values that contain sensitive data or secrets.

### Rules

- **R-HTTP-001** MUST: HTTP client constructors MUST validate that required environment variables are present and non-empty before attempting to make requests.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
find . -name 'package.json' -o -name 'go.mod' -o -name 'pom.xml' -o -name 'build.gradle' -o -name 'Cargo.toml' | head -1

# Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
find . -path '*/test*' -name '*http*client*' -o -path '*/test*' -name '*integration*' | grep -i http

# Search the codebase for environment variable retrieval patterns and verify that credentials are not hard-coded or committed to version control
grep -r 'process\.env\|os\.getenv\|System\.getenv\|std::env::var' --include='*.js' --include='*.ts' --include='*.go' --include='*.java' --include='*.rs' . | grep -i 'http\|client\|url\|auth\|token\|credential' | head -20

# Verify no hard-coded URLs or credentials in HTTP client files
grep -r 'https://\|http://' --include='*.js' --include='*.ts' --include='*.go' --include='*.java' --include='*.rs' . | grep -v 'test\|mock\|example\|localhost' | grep -i 'client\|http' | wc -l
```

**Accept when:**
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration
- No credentials or service URLs are committed to version control

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client implementations MUST be reviewed against R-HTTP-001 before acceptance.
</enforcement>