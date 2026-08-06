# Source Secrets from Environment Variables at Runtime: Authorization Headers Authentication

These rules are ALWAYS ACTIVE for all application components that authenticate to external HTTP APIs, service initialization code that constructs authenticated clients, task handlers and request processors that require credentials for third-party service integration, and configuration loading modules that prepare runtime context for authenticated operations.

### Rules

- **R-AUTH-001** SHOULD: Authorization headers and authentication tokens constructed from environment-sourced secrets SHOULD be set immediately before use and SHOULD NOT be stored in long-lived data structures.

### Verify

```bash
# Discover and execute the project's static analysis tooling to scan source files for hardcoded credential patterns or secret strings
# (Tool and command to be derived from project repository)

# Locate the project's test suite and run integration tests that validate environment variable loading and missing-secret error handling
# (Test command to be derived from project repository)

# Identify the project's linting or security scanning configuration and verify it includes rules that detect hardcoded secrets or credentials in source code
# (Linting command to be derived from project repository)
```

**Accept when:**
- Static analysis confirms no hardcoded credentials, API keys, or secret strings are present in source files committed to version control
- All service initialization code successfully retrieves required secrets from environment variables and fails fast with clear error messages when critical variables are missing
- Integration tests demonstrate that services correctly authenticate to external APIs using environment-sourced credentials and handle missing or invalid credentials gracefully

<enforcement>
Claude Code MUST NOT skip or defer verification. Automated static analysis in continuous integration pipeline MUST scan for hardcoded secrets and credential patterns. Code review MUST verify that new external service integrations use environment variables for credentials. Security scanning tools MUST detect exposed secrets in source code, configuration files, and container images. CI pipeline MUST fail and block merge if static analysis detects hardcoded credentials or secret patterns in code changes.
</enforcement>