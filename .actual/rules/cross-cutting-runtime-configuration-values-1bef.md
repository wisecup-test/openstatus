# Use Environment Variables for Runtime Configuration Secrets: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all service initialization code, HTTP client constructors, database connection initialization, external service integrations, and cloud service clients that require runtime configuration or secrets.

### Rules

- **R-CONFIG-001** MUST: All runtime configuration values that vary by environment MUST be retrieved from environment variables using the standard library environment access functions.
- **R-CONFIG-002** MUST: Retrieve environment variables during service initialization or client constructor execution, storing values in struct fields or configuration objects rather than accessing environment variables repeatedly during request processing.
- **R-CONFIG-003** MUST: Implement validation logic that checks for required environment variables at startup and returns descriptive errors identifying which variables are missing, preventing silent failures or cryptic runtime errors.
- **R-CONFIG-004** MUST: Document all required environment variables for each service including their purpose, expected format, and whether they contain sensitive values, maintaining this documentation alongside deployment configuration.
- **R-CONFIG-005** MUST: Implement logging and error handling that redacts environment variable values. Use structured logging with explicit field control rather than dumping entire environment maps.

### Verify

```bash
# Discover the project's static analysis configuration and execute the configured linter
# to detect hardcoded credentials or configuration values in source files
linter_config=$(find . -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' | head -1)
if [ -n "$linter_config" ]; then
  echo "Found linter config: $linter_config"
  # Execute linter (tool-specific command derived from project)
fi

# Locate the project's test suite and execute integration tests
# that verify services fail gracefully when required environment variables are missing
test_dir=$(find . -type d -name '*test*' -o -name '*tests*' | head -1)
if [ -d "$test_dir" ]; then
  echo "Found test directory: $test_dir"
  # Execute integration tests (tool-specific command derived from project)
fi

# Identify the project's code search tooling and scan for environment variable
# retrieval patterns to confirm they occur during initialization rather than request processing
grep -r 'os\.Getenv\|process\.env\|getenv' --include='*.go' --include='*.js' --include='*.py' . | grep -v node_modules | grep -v '.git'
```

**Accept when:**
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request
- All required environment variables are documented with purpose, expected format, and sensitivity classification
- Logging and error handling redact sensitive environment variable values

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory and must be verified before accepting code changes that involve runtime configuration or secrets handling.
</enforcement>