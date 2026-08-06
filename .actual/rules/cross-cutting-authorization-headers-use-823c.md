# Externalize HTTP Client Configuration via Environment Variables: Authorization Headers Use

These rules are ALWAYS ACTIVE for all HTTP clients that communicate with external third-party services and require authentication credentials.

### Rules

- **R-AUTH-001** SHOULD: Authorization headers SHOULD use bearer token format when transmitting API keys to external services.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library or HTTP client library used for environment variable retrieval
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' -o -name 'pom.xml' | head -1

# Locate the project's test suite and identify integration tests that verify HTTP client configuration from environment variables
find . -path '*/test*' -name '*http*' -o -path '*/test*' -name '*client*' | grep -E '\.(go|js|py|rb|java)$'

# Search the codebase for environment variable retrieval patterns and verify that credentials are not hard-coded or committed to version control
grep -r 'os\.Getenv\|process\.env\|os\.environ\|ENV\[' --include='*.go' --include='*.js' --include='*.py' --include='*.rb' --include='*.java' | grep -i 'auth\|token\|key\|secret'

# Verify bearer token format in authorization headers
grep -r 'Authorization.*Bearer\|bearer.*token' --include='*.go' --include='*.js' --include='*.py' --include='*.rb' --include='*.java'

# Check for hard-coded credentials or URLs
grep -r 'Authorization.*[A-Za-z0-9+/=]\{20,\}\|http.*://.*api' --include='*.go' --include='*.js' --include='*.py' --include='*.rb' --include='*.java' | grep -v test | grep -v mock
```

**Accept when:**
- All external HTTP clients retrieve endpoint URLs and credentials from environment variables without hard-coded fallbacks
- Client constructors validate required environment variables and fail with clear error messages when configuration is missing
- Authorization headers use bearer token format for API key transmission
- Integration tests demonstrate successful HTTP client initialization and request execution using environment variable configuration
- Credentials are not committed to version control and are not visible in source code

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for HTTP client implementations in scope.
</enforcement>