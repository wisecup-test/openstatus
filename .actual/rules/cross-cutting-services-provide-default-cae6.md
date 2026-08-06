# Use Environment Variables for Runtime Configuration Values: Services Provide Default

These rules are ALWAYS ACTIVE for all service initialization code, configuration retrieval logic, and deployment-time configuration injection patterns.

### Rules

- **R-ENV-001** MUST: Retrieve all runtime configuration values that vary across environments from environment variables using the standard library accessor during service initialization or client construction.
- **R-ENV-002** MUST: Implement explicit validation for required environment variables, checking for presence and non-empty values, and provide clear error messages identifying which variables are missing or invalid.
- **R-ENV-003** MUST: Fail fast during service initialization when required environment variables are missing, rather than deferring retrieval until first use.
- **R-ENV-004** MUST: Ensure no sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- **R-ENV-005** MAY: Services MAY provide default values for non-sensitive configuration parameters when environment variables are absent.
- **R-ENV-006** SHOULD: Implement a configuration struct or object that encapsulates all environment variable retrieval and validation in a single location, providing a typed interface to the rest of the application.
- **R-ENV-007** SHOULD: Document all required and optional environment variables in deployment documentation, including expected formats, example values for non-sensitive parameters, and the impact of missing values.

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
find . -type f \( -name 'Dockerfile' -o -name '*.tf' -o -name 'docker-compose.yml' -o -name 'k8s*.yaml' \)

# Verify all environment variables referenced in application code are documented
grep -r 'os\.Getenv\|process\.env\|ENV\[' --include='*.go' --include='*.js' --include='*.py' | cut -d: -f2 | sort -u
```

**Accept when:**
- All runtime configuration values that vary across environments are retrieved from environment variables using the standard library accessor.
- No sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- Services validate required environment variables during initialization and fail with clear error messages when configuration is missing.
- All required environment variables are documented in deployment documentation with expected formats and example values.
- Configuration retrieval is centralized in a single location (configuration struct or dedicated module) rather than scattered throughout the codebase.
- Integration tests verify service initialization behavior with missing or invalid environment variables.
- Deployment checklists confirm all required environment variables are provisioned in target environments.

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review rejection is mandatory for pull requests that embed credentials or environment-specific configuration in source code. Build failure is mandatory when static analysis detects hardcoded sensitive values. Deployment rollback is mandatory when services fail initialization due to missing required environment variables.
</enforcement>