# Retrieve Secrets from Environment Variables at Runtime: Integration Tests Validate

These rules are ALWAYS ACTIVE for integration test suites that connect to external HTTP APIs, client initialization code that authenticates with cloud service providers, HTTP request construction that requires bearer tokens or API keys, and service account credential loading for cloud platform authentication.

### Rules

- **R-SECRETS-001** SHOULD: Integration tests SHOULD validate the presence of required environment variables before attempting authentication operations.
- **R-SECRETS-002** SHOULD: Client initialization code should retrieve environment variables once during construction and store them in private fields, avoiding repeated lookups during request execution.
- **R-SECRETS-003** SHOULD: Integration test setup should document all required environment variables in test documentation or README files, including their purpose and expected format.
- **R-SECRETS-004** SHOULD: Consider implementing a validation function that checks for required environment variables at test suite initialization and fails fast with clear error messages before attempting authentication.

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
Claude Code MUST NOT skip or defer verification. Code review verification that new client code retrieves secrets from environment variables is mandatory. Static analysis scanning for hardcoded credentials in source files is mandatory. Integration test execution in CI that validates environment variable configuration is mandatory.
</enforcement>