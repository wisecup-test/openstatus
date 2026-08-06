# Source Secrets from Environment Variables at Runtime: Secrets Credentials Not

These rules are ALWAYS ACTIVE for all application components that authenticate to external HTTP APIs, service initialization code that constructs authenticated clients, task handlers and request processors that require credentials for third-party service integration, and configuration loading modules that prepare runtime context for authenticated operations.

### Rules

- **R-SECRETS-001** MUST NOT: Secrets and credentials MUST NOT be hardcoded in source files, configuration files committed to version control, or embedded in compiled binaries.
- **R-SECRETS-002** MUST: Implement centralized configuration loading that validates all required environment variables at application startup before initializing service clients, providing clear error messages that list missing variables without exposing their values.
- **R-SECRETS-003** MUST: Use structured logging with automatic redaction of fields matching secret patterns to prevent accidental credential exposure in logs while maintaining debuggability of configuration issues.
- **R-SECRETS-004** SHOULD: Document the complete list of required environment variables for each service component in deployment documentation, including expected format, purpose, and whether they are required or optional.
- **R-SECRETS-005** SHOULD: Consider implementing a configuration validation mode that checks environment variable presence and format without starting the full application, enabling pre-deployment validation.

### Verify

```bash
# Discover and execute the project's static analysis tooling to scan source files for hardcoded credential patterns or secret strings
# (Tool name and invocation to be derived from project repository)

# Locate the project's test suite and run integration tests that validate environment variable loading and missing-secret error handling
# (Test runner and test location to be derived from project repository)

# Identify the project's linting or security scanning configuration and verify it includes rules that detect hardcoded secrets or credentials in source code
# (Linting tool and configuration to be derived from project repository)
```

**Accept when:**
- Static analysis confirms no hardcoded credentials, API keys, or secret strings are present in source files committed to version control
- All service initialization code successfully retrieves required secrets from environment variables and fails fast with clear error messages when critical variables are missing
- Integration tests demonstrate that services correctly authenticate to external APIs using environment-sourced credentials and handle missing or invalid credentials gracefully

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before code changes are approved.
</enforcement>