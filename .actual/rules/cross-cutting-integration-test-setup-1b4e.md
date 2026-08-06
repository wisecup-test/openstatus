# Retrieve Secrets from Environment Variables in Integration Test Contexts: Integration Test Setup

These rules are ALWAYS ACTIVE for integration test initialization code that constructs HTTP clients for external services, service adapter constructors requiring API keys or bearer tokens, cloud platform client initialization, and task scheduling client setup.

### Rules

- **R-INT-SEC-001** SHOULD: Integration test setup code SHOULD validate the presence of required environment variables before constructing authenticated clients and SHOULD fail fast with descriptive error messages when credentials are missing.

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

<enforcement>
Clause Code MUST NOT skip or defer verification of environment variable retrieval patterns in integration test setup code.
</enforcement>