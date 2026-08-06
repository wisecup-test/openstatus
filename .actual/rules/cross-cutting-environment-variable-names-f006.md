# Retrieve Secrets from Environment Variables in Integration Test Contexts: Environment Variable Names

These rules are ALWAYS ACTIVE for integration test initialization code that constructs HTTP clients for external services, service adapter constructors requiring API keys or bearer tokens, cloud platform client initialization, and task scheduling client setup.

### Rules

- **R-ENV-001** MUST: Environment variable names for secrets SHALL use uppercase naming with underscore separators and include a semantic suffix indicating the credential type or target service.

### Verify

```bash
# Discover the project's test execution script and invoke it with integration test selection flags
# to verify that test setup code retrieves credentials from environment variables
find . -name "*test*" -o -name "*spec*" | head -5

# Discover the project's static analysis or linting configuration and execute it
# to verify that no hard-coded credential strings exist in integration test files
grep -r "password\|secret\|key\|token" --include="*test*" --include="*spec*" | grep -v "ENV\|getenv\|process.env" || echo "No hard-coded credentials detected"

# Discover the project's environment variable documentation or template configuration
# and verify that all secrets referenced in integration test code are documented
find . -name ".env*" -o -name "*env.template*" -o -name "*env.example*" | head -5
```

**Accept when:**
- All integration tests requiring external service authentication retrieve credentials exclusively from environment variables using standard library functions
- Test execution fails with clear error messages identifying missing environment variables when required credentials are not present
- No secret values are logged, stored in global variables, or persisted beyond the immediate initialization context where they are consumed
- Environment variable names follow the uppercase-with-underscores pattern and include semantic suffixes indicating credential type or target service

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration test code retrieving secrets MUST use environment variables with names conforming to R-ENV-001.
</enforcement>