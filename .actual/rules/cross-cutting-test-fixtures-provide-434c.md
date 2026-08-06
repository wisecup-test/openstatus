# Retrieve Secrets from Environment Variables at Runtime: Test Fixtures Provide

These rules are ALWAYS ACTIVE for integration test suites, client initialization code, HTTP request construction, and service account credential loading that authenticate with external services and cloud providers.

### Rules

- **R-ENV-001** MAY: Test fixtures MAY provide default or mock values for environment variables in non-production test scenarios.
- **R-ENV-002** MUST: Client initialization code retrieve environment variables once during construction and store them in private fields, avoiding repeated lookups during request execution.
- **R-ENV-003** MUST: Integration test setup document all required environment variables in test documentation or README files, including their purpose and expected format.
- **R-ENV-004** SHOULD: Implement a validation function that checks for required environment variables at test suite initialization and fails fast with clear error messages before attempting authentication.
- **R-ENV-005** MUST: All integration tests that authenticate with external services retrieve credentials exclusively from environment variables.
- **R-ENV-006** MUST: Error handling report missing variables without exposing values, and configure logging systems to redact environment variable contents.

### Verify

```bash
# Discover the project's test execution script and run integration tests with required environment variables set
# to verify authentication succeeds

# Discover the project's static analysis or linting configuration and verify it includes checks for hardcoded
# secrets or credentials in source files

# Discover the project's client initialization code and verify all external service clients retrieve
# authentication material from environment variables
```

**Accept when:**
- All integration tests that authenticate with external services retrieve credentials exclusively from environment variables
- Static analysis confirms no authentication secrets are hardcoded in source files or committed to version control
- Client initialization code documents the required environment variable names and validates their presence before use
- Error handling does not expose secret values in logs or error messages
- Required environment variables are documented with purpose and expected format

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review MUST reject pull requests that hardcode secrets or bypass environment variable retrieval. CI pipeline MUST fail when static analysis detects potential hardcoded credentials. Security incident response process MUST be triggered for any secrets accidentally committed to version control.
</enforcement>