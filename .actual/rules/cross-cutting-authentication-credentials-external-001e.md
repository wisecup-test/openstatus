# Externalize HTTP Client Configuration via Environment Variables: Authentication Credentials External

These rules are ALWAYS ACTIVE for all external HTTP client implementations that communicate with third-party services and require authentication credentials or environment-specific endpoint configuration.

### Rules

- **R-AUTH-001** MUST: Authentication credentials for external HTTP clients MUST be retrieved from environment variables and MUST NOT be committed to version control.
- **R-AUTH-002** MUST: Client constructors MUST retrieve environment variables during initialization and store them as private fields, failing fast with descriptive errors if required variables are missing.
- **R-AUTH-003** MUST: Service endpoint URLs that vary across deployment environments MUST be retrieved from environment variables and MUST NOT be hard-coded.
- **R-AUTH-004** SHOULD: Use structured logging with context to record configuration retrieval and validation, ensuring credential values are redacted from log output.
- **R-AUTH-005** SHOULD: Document all required environment variables in the client package documentation, including expected format, example values, and environment-specific variations.
- **R-AUTH-006** MAY: Local development environments may use default localhost URLs when environment variables are not set (EXC-001).
- **R-AUTH-007** MAY: Integration tests may inject configuration programmatically rather than via environment variables (EXC-002).

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'pom.xml' | head -1

# Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
find . -path '*/test*' -name '*http*client*' -o -path '*/test*' -name '*integration*' | grep -E '\.(go|js|py|java)$'

# Search the codebase for environment variable retrieval patterns
grep -r 'os\.Getenv\|process\.env\|os\.environ\|System\.getenv' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | grep -i 'url\|endpoint\|auth\|token\|credential\|key'

# Verify that credentials are not hard-coded in HTTP client implementations
grep -r 'http.*://.*:.*@' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | grep -v test | grep -v example

# Search for hard-coded bearer tokens or API keys
grep -r 'Authorization.*Bearer' --include='*.go' --include='*.js' --include='*.py' --include='*.java' | grep -v 'getenv\|environ\|process\.env' | grep -v test
```

**Accept when:**
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration
- No credentials or service URLs are committed to version control
- Structured logging redacts credential values from output

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for external HTTP client implementations. Code review and static analysis MUST block merge requests that violate R-AUTH-001 or R-AUTH-003. Exceptions require written approval from the engineering team lead and MUST be documented in the implementation with clear comments.
</enforcement>