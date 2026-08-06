# Use Environment Variables for Runtime Configuration Values: Configuration Retrieval Occur

These rules are ALWAYS ACTIVE for all service initialization code, client construction, and configuration retrieval logic that handles runtime configuration values varying across deployment environments.

### Rules

- **R-CONFIG-001** MUST: Configuration retrieval MUST occur during service initialization or client construction to fail fast if required values are missing.
- **R-CONFIG-002** MUST: All runtime configuration values that vary across environments (service endpoints, authentication credentials, project identifiers, feature flags) MUST be retrieved from environment variables using the standard library accessor.
- **R-CONFIG-003** MUST: No sensitive credentials or environment-specific endpoints SHALL be embedded in source code or version control.
- **R-CONFIG-004** MUST: Services MUST validate required environment variables during initialization and fail with clear error messages identifying which variables are missing or invalid.
- **R-CONFIG-005** MUST: All required and optional environment variables MUST be documented in deployment documentation with expected formats, example values for non-sensitive parameters, and impact of missing values.
- **R-CONFIG-006** SHOULD: Implement a configuration struct or object that encapsulates all environment variable retrieval and validation in a single location, providing a typed interface to the rest of the application.
- **R-CONFIG-007** SHOULD: Implement logging filters to redact credential values and avoid echoing environment variable contents in error messages.

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
find . -type f \( -name 'Dockerfile' -o -name '*.yaml' -o -name '*.yml' -o -name 'docker-compose*' -o -name 'terraform*' \) | head -5

# Verify environment variables referenced in code are documented in deployment targets
grep -r 'os\.Getenv\|process\.env\|ENV\[' --include='*.go' --include='*.js' --include='*.py' --include='*.rb' | cut -d: -f2 | sort -u
```

**Accept when:**
- All runtime configuration values that vary across environments are retrieved from environment variables using the standard library accessor.
- No sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- Services validate required environment variables during initialization and fail with clear error messages when configuration is missing.
- All required environment variables are documented in deployment documentation with expected formats and example values.
- Static analysis checks confirm no hardcoded credentials or environment-specific values appear in source files.
- Integration tests verify service initialization behavior with missing or invalid environment variables.
- Deployment checklist verification confirms all required environment variables are provisioned in target environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review rejection is mandatory for pull requests that embed credentials or environment-specific configuration in source code. Build failure is mandatory when static analysis detects hardcoded sensitive values. Deployment rollback is mandatory when services fail initialization due to missing required environment variables.
</enforcement>