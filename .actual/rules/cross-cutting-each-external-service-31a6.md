# Retrieve Secrets from Environment Variables at Runtime: Each External Service

These rules are ALWAYS ACTIVE for integration test suites, client initialization code, HTTP request construction, and service account credential loading that authenticate with external services and cloud providers.

### Rules

- **R-SECRETS-001** MUST: Each external service client MUST document the required environment variable names in its initialization or constructor code.

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
Clause Code MUST NOT skip or defer verification. Violations are handled through code review rejection, CI pipeline failure on static analysis detection, and security incident response for accidentally committed secrets.
</enforcement>