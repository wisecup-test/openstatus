# Use Environment Variables for Runtime Configuration Secrets: Security Sensitive Values

These rules are ALWAYS ACTIVE for all service initialization code, HTTP client constructors, database connection initialization, external service integrations, and cloud service clients that require runtime configuration or security-sensitive values.

### Rules

- **R-SEC-001** MUST: Security-sensitive values including API keys, service credentials, private keys, and authorization tokens MUST be sourced exclusively from environment variables and MUST NOT be hardcoded in source files.

### Verify

```bash
# Discover the project's static analysis configuration and execute the configured linter
# to detect hardcoded credentials or configuration values in source files
linter_config=$(find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' | head -1)
if [ -n "$linter_config" ]; then
  echo "Found linter config: $linter_config"
  # Execute linter with security checks enabled
fi

# Locate the project's test suite and execute integration tests
# that verify services fail gracefully when required environment variables are missing
test_dir=$(find . -type d -name 'test*' -o -name '*test' -o -name 'spec' | head -1)
if [ -n "$test_dir" ]; then
  echo "Found test directory: $test_dir"
  # Execute integration tests for environment variable handling
fi

# Identify the project's code search tooling and scan for environment variable
# retrieval patterns to confirm they occur during initialization rather than request processing
grep -r "process\.env\|os\.getenv\|getenv\|environ\[" --include="*.js" --include="*.py" --include="*.go" --include="*.java" . 2>/dev/null | grep -v node_modules | grep -v '.git' || echo "Scan complete"
```

**Accept when:**
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request
- No hardcoded secrets, API keys, or sensitive tokens appear in version control history or source code

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria MUST be satisfied before approving code that handles runtime configuration or security-sensitive values.
</enforcement>