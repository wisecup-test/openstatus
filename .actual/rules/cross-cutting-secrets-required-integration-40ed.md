# Retrieve Secrets from Environment Variables in Integration Test Contexts: Secrets Required Integration

These rules are ALWAYS ACTIVE for all integration test initialization code that constructs HTTP clients for external services, service adapter constructors requiring API keys or bearer tokens, cloud platform client initialization, and task scheduling client setup.

### Rules

- **R-SECRETS-001** MUST: All secrets required for integration test execution SHALL be retrieved from environment variables using the standard library environment access function.
- **R-SECRETS-002** MUST: Implement explicit validation of required environment variables during test setup and fail fast with clear error messages identifying missing credentials before any HTTP client construction occurs.
- **R-SECRETS-003** MUST: When constructing authorization headers from environment-sourced secrets, use formatted string construction to ensure consistent bearer token or API key header structure across all external service clients.
- **R-SECRETS-004** SHOULD: Create a template environment configuration file documenting all required environment variable names with descriptive comments explaining their purpose, expected format, and which external services they authenticate to.
- **R-SECRETS-005** SHOULD: Implement structured logging with secret redaction filters and validate that error handling code does not include environment variable values in exception messages.
- **R-SECRETS-006** SHOULD: Maintain template configuration files with placeholder values, document the required environment variables separately, and configure version control ignore rules to exclude environment files.

### Verify

```bash
# Discover the project's test execution script and invoke it with integration test selection flags
# to verify that test setup code retrieves credentials from environment variables
./scripts/test.sh --integration

# Discover the project's static analysis or linting configuration and execute it
# to verify that no hard-coded credential strings exist in integration test files
./scripts/lint.sh --check-secrets

# Discover the project's environment variable documentation or template configuration
# and verify that all secrets referenced in integration test code are documented
grep -r "os\.getenv\|process\.env\|System\.getenv" tests/integration/ | wc -l
```

**Accept when:**
- All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions.
- Test execution fails with clear error messages identifying missing environment variables when required credentials are not present.
- No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed.
- Environment variable template documentation exists and lists all secrets referenced in integration test code.
- Static analysis confirms no hard-coded credential patterns exist in integration test files.

<enforcement>
Claude Code MUST NOT skip or defer verification of these rules. All integration test code retrieving secrets MUST use environment variables exclusively. Missing or misconfigured environment variables MUST result in fast-fail with clear error messages. Secret values MUST NOT appear in logs, error messages, or global state.
</enforcement>