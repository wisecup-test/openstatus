# Retrieve Secrets from Environment Variables in Integration Test Contexts: Authorization Headers Bearer

These rules are ALWAYS ACTIVE for integration test initialization code that constructs HTTP clients for external services, service adapter constructors requiring API keys or bearer tokens, cloud platform client initialization, and task scheduling client setup requiring authentication secrets.

### Rules

- **R-AUTH-001** SHOULD: Authorization headers and bearer tokens constructed from environment-sourced secrets SHOULD be assembled using formatted string construction to ensure consistent header structure.

### Verify

```bash
# Discover the project's test execution script and invoke it with integration test selection flags
# to verify that test setup code retrieves credentials from environment variables
./scripts/test.sh --integration

# Discover the project's static analysis or linting configuration and execute it
# to verify that no hard-coded credential strings exist in integration test files
make lint

# Discover the project's environment variable documentation or template configuration
# and verify that all secrets referenced in integration test code are documented
grep -r "ENV" docs/integration-tests.md || echo "Check template .env.example"
```

**Accept when:**
- All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions
- Test execution fails with clear error messages identifying missing environment variables when required credentials are not present
- No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed
- Authorization headers are constructed using formatted string construction (e.g., `f"Bearer {token}"` or equivalent)
- Environment variable template files document all required secret names with descriptive comments

<enforcement>
Clause Code MUST NOT skip or defer verification of environment variable retrieval patterns in integration test setup code. All secrets MUST be sourced from environment variables; hard-coded credentials are violations requiring immediate remediation.
</enforcement>