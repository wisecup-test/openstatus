# Retrieve Secrets from Environment Variables in Integration Test Contexts: Integration Test Contexts

These rules are ALWAYS ACTIVE for integration test initialization code that constructs HTTP clients for external services, service adapter constructors requiring API keys or bearer tokens, cloud platform client initialization, and task scheduling client setup.

### Rules

- **R-SECRETS-001** MAY: Integration test contexts MAY log the presence or absence of required environment variables for debugging purposes but SHALL NOT log the secret values themselves.
- **R-SECRETS-002** MUST: All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions.
- **R-SECRETS-003** MUST: Test execution fail with clear error messages identifying missing environment variables when required credentials are not present.
- **R-SECRETS-004** MUST: No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed.
- **R-SECRETS-005** MUST: Create a template environment configuration file documenting all required environment variable names with descriptive comments explaining their purpose, expected format, and which external services they authenticate to.
- **R-SECRETS-006** MUST: Implement a test setup helper function that validates the presence of all required environment variables and returns a structured error listing missing credentials before any HTTP client construction occurs.
- **R-SECRETS-007** MUST: When constructing authorization headers from environment-sourced secrets, use formatted string construction to ensure consistent bearer token or API key header structure across all external service clients.

### Verify

```bash
# Discover the project's test execution script and invoke it with integration test selection flags
# to verify that test setup code retrieves credentials from environment variables

# Discover the project's static analysis or linting configuration and execute it
# to verify that no hard-coded credential strings exist in integration test files

# Discover the project's environment variable documentation or template configuration
# and verify that all secrets referenced in integration test code are documented
# with their expected names and formats
```

**Accept when:**
- All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions
- Test execution fails with clear error messages identifying missing environment variables when required credentials are not present
- No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed
- Template environment configuration file exists documenting all required environment variable names
- Test setup helper function validates presence of required environment variables before client construction
- Authorization headers use formatted string construction for consistent structure

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review verification that integration test setup code uses environment variable retrieval for all secrets is mandatory. Static analysis rules detecting hard-coded credential patterns in test files are mandatory. CI pipeline validation that integration tests fail appropriately when required environment variables are not configured is mandatory.
</enforcement>