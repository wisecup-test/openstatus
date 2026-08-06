# Use Environment Variables for Runtime Configuration Secrets: Services Provide Default

These rules are ALWAYS ACTIVE for all service initialization code that requires runtime configuration, HTTP client constructors, database connection initialization, external service integrations, and cloud service clients.

### Rules

- **R-ENV-001** MAY: Services MAY provide default values for non-sensitive configuration when environment variables are absent.
- **R-ENV-002** MUST: Retrieve environment variables during service initialization or client constructor execution, storing values in struct fields or configuration objects rather than accessing environment variables repeatedly during request processing.
- **R-ENV-003** MUST: Implement validation logic that checks for required environment variables at startup and returns descriptive errors identifying which variables are missing, preventing silent failures or cryptic runtime errors.
- **R-ENV-004** MUST: Document all required environment variables for each service including their purpose, expected format, and whether they contain sensitive values, maintaining this documentation alongside deployment configuration.
- **R-ENV-005** MUST NOT: Hardcode credentials, API keys, or environment-specific configuration values in source files.
- **R-ENV-006** MUST: Implement logging and error handling that redacts environment variable values, using structured logging with explicit field control rather than dumping entire environment maps.

### Verify

```bash
# Discover the project's static analysis configuration and execute the configured linter
# to detect hardcoded credentials or configuration values in source files
linter_config=$(find . -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' | head -1)
if [ -n "$linter_config" ]; then
  echo "Running static analysis with discovered config: $linter_config"
  # Execute project's configured linter
fi

# Locate the project's test suite and execute integration tests
# that verify services fail gracefully when required environment variables are missing
test_dir=$(find . -type d -name '*test*' -o -name '*tests*' | head -1)
if [ -d "$test_dir" ]; then
  echo "Running integration tests from: $test_dir"
  # Execute project's test suite
fi

# Identify the project's code search tooling and scan for environment variable
# retrieval patterns to confirm they occur during initialization
echo "Scanning for environment variable retrieval patterns..."
grep -r "os\.Getenv\|process\.env\|getenv\|ENV\[" --include="*.go" --include="*.js" --include="*.py" --include="*.java" . 2>/dev/null | grep -v test | head -20
```

**Accept when:**
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request
- All required environment variables are documented with purpose, expected format, and sensitivity classification
- Logging and error handling redact sensitive environment variable values

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merging until hardcoded values are removed. Code review must verify environment variable handling before approval.
</enforcement>