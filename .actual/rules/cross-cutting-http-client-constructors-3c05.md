# Retrieve Secrets from Environment Variables in Integration Test Contexts: Http Client Constructors

These rules are ALWAYS ACTIVE for integration test initialization code that constructs HTTP clients for external services, service adapter constructors requiring API keys or bearer tokens, cloud platform client initialization, and task scheduling client setup.

### Rules

- **R-SECRETS-001** MUST: HTTP client constructors and external service adapters that require authentication SHALL accept secrets as parameters rather than performing environment lookups internally.

### Verify

```bash
# Discover the project's test execution script and invoke it with integration test selection flags
# to verify that test setup code retrieves credentials from environment variables
find . -name "*test*" -o -name "*spec*" | head -5

# Discover the project's static analysis or linting configuration and execute it
# to verify that no hard-coded credential strings exist in integration test files
grep -r "password\|api[_-]?key\|secret\|token" --include="*test*" --include="*spec*" | grep -v "env\|ENV\|getenv" || echo "No obvious hard-coded credentials found"

# Discover the project's environment variable documentation or template configuration
# and verify that all secrets referenced in integration test code are documented
find . -name ".env*" -o -name "*env*.template" -o -name "*env*.example" | head -10
```

**Accept when:**
- All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions
- Test execution fails with clear error messages identifying missing environment variables when required credentials are not present
- No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed
- HTTP client constructors accept secrets as constructor parameters rather than performing internal environment variable lookups
- Authorization headers are constructed using formatted string assembly from parameter-passed secrets
- Test setup helper functions validate the presence of all required environment variables before any HTTP client construction occurs

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration test code constructing HTTP clients for external services MUST be reviewed to confirm secrets are passed as parameters and retrieved from environment variables at the test setup boundary, not embedded in client constructors.
</enforcement>