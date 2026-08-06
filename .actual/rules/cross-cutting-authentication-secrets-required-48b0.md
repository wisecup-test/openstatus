# Retrieve Secrets from Environment Variables at Runtime: Authentication Secrets Required

These rules are ALWAYS ACTIVE for all integration test suites, client initialization code, HTTP request construction, and service account credential loading that requires authentication with external services or cloud providers.

### Rules

- **R-AUTH-001** MUST: All authentication secrets required for integration tests and external service clients MUST be retrieved from environment variables at runtime using the standard library environment access functions.

### Verify

```bash
# Discover the project's test execution script and run integration tests with required environment variables set to verify authentication succeeds
# Discover the project's static analysis or linting configuration and verify it includes checks for hardcoded secrets or credentials in source files
# Discover the project's client initialization code and verify all external service clients retrieve authentication material from environment variables
```

**Accept when:**
- All integration tests that authenticate with external services retrieve credentials exclusively from environment variables
- Static analysis confirms no authentication secrets are hardcoded in source files or committed to version control
- Client initialization code documents the required environment variable names and validates their presence before use

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review rejection is mandatory for pull requests that hardcode secrets or bypass environment variable retrieval. CI pipeline failure is mandatory when static analysis detects potential hardcoded credentials.
</enforcement>