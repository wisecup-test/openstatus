# Use Environment Variables for Runtime Configuration Secrets: Environment Variable Retrieval

These rules are ALWAYS ACTIVE for all service initialization code, HTTP client constructors, database connection initialization, external service integrations, and cloud service clients that require runtime configuration or secrets.

### Rules

- **R-ENV-001** MUST: Environment variable retrieval MUST occur during service initialization or client construction, not at request-processing time.
- **R-ENV-002** MUST: Implement validation logic that checks for required environment variables at startup and returns descriptive errors identifying which variables are missing.
- **R-ENV-003** MUST: Store retrieved environment variable values in struct fields or configuration objects rather than accessing environment variables repeatedly during request processing.
- **R-ENV-004** MUST: Document all required environment variables for each service including their purpose, expected format, and whether they contain sensitive values.
- **R-ENV-005** SHOULD: Implement logging and error handling that redacts environment variable values to prevent leaking sensitive data.
- **R-ENV-006** SHOULD: Establish and document a naming convention for environment variables across services.

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
test_dir=$(find . -type d -name 'test*' -o -name '*_test' -o -name 'tests' | head -1)
if [ -n "$test_dir" ]; then
  echo "Found test directory: $test_dir"
  # Execute integration tests (tool-specific command derived from project)
fi

# Identify the project's code search tooling and scan for environment variable
# retrieval patterns to confirm they occur during initialization rather than request processing
grep -r 'os\.Getenv\|getenv\|environ' --include='*.go' --include='*.js' --include='*.py' . | grep -v 'test' | head -20
```

**Accept when:**
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request
- All required environment variables are documented with purpose, format, and sensitivity level

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the defined scope. Violations must be identified during code review and remediated before approval.
</enforcement>