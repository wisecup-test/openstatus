# Retrieve Secrets from Environment Variables at Runtime: Authentication Credentials Not

These rules are ALWAYS ACTIVE for all integration test suites, client initialization code, HTTP request construction, and service account credential loading that authenticate with external services or cloud providers.

### Rules

- **R-AUTH-001** MUST NOT: Authentication credentials MUST NOT be hardcoded in source files, committed to version control, or embedded in compiled binaries.
- **R-AUTH-002** MUST: Client initialization code should retrieve environment variables once during construction and store them in private fields, avoiding repeated lookups during request execution.
- **R-AUTH-003** MUST: Integration test setup should document all required environment variables in test documentation or README files, including their purpose and expected format.
- **R-AUTH-004** SHOULD: Implement a validation function that checks for required environment variables at test suite initialization and fails fast with clear error messages before attempting authentication.
- **R-AUTH-005** SHOULD: Implement error handling that reports missing variables without exposing values, and configure logging systems to redact environment variable contents.

### Verify

```bash
# Discover the project's test execution script and run integration tests with required environment variables set to verify authentication succeeds
# (Exact command depends on project's build tool and test runner)

# Discover the project's static analysis or linting configuration and verify it includes checks for hardcoded secrets or credentials in source files
# (Exact command depends on project's static analysis tool)

# Discover the project's client initialization code and verify all external service clients retrieve authentication material from environment variables
# (Manual code review or grep-based verification)
```

**Accept when:**
- All integration tests that authenticate with external services retrieve credentials exclusively from environment variables
- Static analysis confirms no authentication secrets are hardcoded in source files or committed to version control
- Client initialization code documents the required environment variable names and validates their presence before use
- Error handling reports missing variables without exposing their values
- Environment variable names are documented and consistent across development, CI, and production environments

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review rejection is mandatory for pull requests that hardcode secrets or bypass environment variable retrieval. CI pipeline failure is mandatory when static analysis detects potential hardcoded credentials.
</enforcement>