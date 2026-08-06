# Use Environment Variables for Runtime Configuration Values: Environment Variable Names

These rules are ALWAYS ACTIVE for all service initialization code, configuration retrieval logic, and deployment integration points that handle runtime configuration values varying across environments.

### Rules

- **R-ENV-001** SHOULD: Environment variable names SHOULD use uppercase with underscore separators to follow conventional naming patterns.
- **R-ENV-002** MUST: Retrieve environment variables during service initialization or client construction to enable fail-fast behavior when required configuration is missing.
- **R-ENV-003** MUST: Implement explicit validation for required environment variables, checking for presence and non-empty values, and provide clear error messages identifying which variables are missing or invalid.
- **R-ENV-004** SHOULD: Consider implementing a configuration struct or object that encapsulates all environment variable retrieval and validation in a single location, providing a typed interface to the rest of the application.
- **R-ENV-005** MUST: Document all required and optional environment variables in deployment documentation, including expected formats, example values for non-sensitive parameters, and the impact of missing values.
- **R-ENV-006** MUST: Ensure no sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- **R-ENV-007** MUST: Implement logging filters to redact credential values and avoid echoing environment variable contents in error messages.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library module used for environment variable access
find . -name 'go.mod' -o -name 'package.json' -o -name 'requirements.txt' -o -name 'Gemfile' | head -1

# Locate test files that verify configuration retrieval behavior
find . -type f -name '*config*test*' -o -name '*test*config*' | grep -E '\.(go|js|py|rb)$'

# Identify the project's static analysis or linting configuration
find . -name '.golangci.yml' -o -name '.eslintrc*' -o -name 'pylintrc' -o -name '.rubocop.yml'

# Execute configured static analysis checks to verify no hardcoded credentials appear
# (command varies by project; example for Go: golangci-lint run)

# Locate deployment or infrastructure configuration
find . -type f \( -name 'Dockerfile' -o -name '*.yaml' -o -name '*.yml' -o -name 'terraform' \) | head -5

# Verify all environment variables referenced in application code are documented
grep -r 'os\.Getenv\|process\.env\|ENV\[' --include='*.go' --include='*.js' --include='*.py' | cut -d: -f2 | sort -u
```

**Accept when:**
- All runtime configuration values that vary across environments are retrieved from environment variables using the standard library accessor.
- No sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- Services validate required environment variables during initialization and fail with clear error messages when configuration is missing.
- All required environment variables are documented in deployment documentation with expected formats and example values.
- Environment variable names follow uppercase with underscore separator conventions.
- Configuration retrieval is centralized in a configuration struct or object providing a typed interface.
- Logging and error messages do not expose credential values from environment variables.

<enforcement>
Clause Code MUST NOT skip or defer verification of environment variable sourcing, validation, and documentation before approving changes to configuration retrieval logic or service initialization.
</enforcement>