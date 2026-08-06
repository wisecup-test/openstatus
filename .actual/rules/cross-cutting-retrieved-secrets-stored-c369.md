# Source Secrets from Environment Variables at Runtime: Retrieved Secrets Stored

These rules are ALWAYS ACTIVE for all application components that authenticate to external HTTP APIs, service initialization code that constructs authenticated clients, task handlers and request processors that require credentials for third-party service integration, and configuration loading modules that prepare runtime context for authenticated operations.

### Rules

- **R-SECRETS-001** MUST: Retrieved secrets MUST be stored in memory only for the duration of their use and MUST NOT be logged, printed to standard output, or persisted to disk.
- **R-SECRETS-002** MUST: Implement centralized configuration loading that validates all required environment variables at application startup before initializing service clients, providing clear error messages that list missing variables without exposing their values.
- **R-SECRETS-003** MUST: Use structured logging with automatic redaction of fields matching secret patterns to prevent accidental credential exposure in logs while maintaining debuggability of configuration issues.
- **R-SECRETS-004** SHOULD: Document the complete list of required environment variables for each service component in deployment documentation, including expected format, purpose, and whether they are required or optional.
- **R-SECRETS-005** SHOULD: Consider implementing a configuration validation mode that checks environment variable presence and format without starting the full application, enabling pre-deployment validation.

### Verify

```bash
# Discover and execute the project's static analysis tooling to scan source files for hardcoded credential patterns or secret strings
find . -type f \( -name '*.py' -o -name '*.js' -o -name '*.ts' -o -name '*.go' -o -name '*.java' \) -exec grep -l 'api[_-]?key\|secret[_-]?key\|password\s*=' {} \; 2>/dev/null || echo "No obvious hardcoded patterns found"

# Locate the project's test suite and run integration tests that validate environment variable loading and missing-secret error handling
if [ -f pytest.ini ] || [ -d tests ]; then pytest -v tests/ -k "env\|secret\|config" 2>/dev/null; fi
if [ -f package.json ]; then npm test -- --testNamePattern="env|secret|config" 2>/dev/null; fi

# Identify the project's linting or security scanning configuration and verify it includes rules that detect hardcoded secrets or credentials in source code
if [ -f .pre-commit-config.yaml ]; then grep -i "secret\|credential" .pre-commit-config.yaml; fi
if [ -f .bandit ]; then cat .bandit; fi
if [ -f .semgrep.yml ]; then cat .semgrep.yml; fi
```

**Accept when:**
- Static analysis confirms no hardcoded credentials, API keys, or secret strings are present in source files committed to version control.
- All service initialization code successfully retrieves required secrets from environment variables and fails fast with clear error messages when critical variables are missing.
- Integration tests demonstrate that services correctly authenticate to external APIs using environment-sourced credentials and handle missing or invalid credentials gracefully.
- Structured logging is configured with redaction rules for known secret environment variable names.
- Configuration validation at startup checks for required environment variables before service initialization.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that sources secrets from environment variables at runtime. Violations block merge and trigger security team notification.
</enforcement>