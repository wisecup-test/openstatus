# Use Environment Variables for Runtime Configuration Values: Services Validate That

These rules are ALWAYS ACTIVE for all service initialization code, configuration retrieval logic, and deployment integration points that handle runtime configuration values varying across environments.

### Rules

- **R-ENV-001** SHOULD: Services SHOULD validate that all required environment variables are present and non-empty before proceeding with initialization.
- **R-ENV-002** MUST: All runtime configuration values that vary across environments MUST be retrieved from environment variables using the standard library accessor.
- **R-ENV-003** MUST: No sensitive credentials or environment-specific endpoints MUST be embedded in source code or version control.
- **R-ENV-004** MUST: Services MUST fail fast with clear error messages identifying which environment variables are missing or invalid during initialization.
- **R-ENV-005** SHOULD: Configuration retrieval and validation SHOULD be encapsulated in a single location (configuration struct or object) providing a typed interface to the rest of the application.
- **R-ENV-006** MUST: All required and optional environment variables MUST be documented in deployment documentation with expected formats and example values for non-sensitive parameters.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library module used for environment variable access
find . -name "go.mod" -o -name "package.json" -o -name "requirements.txt" -o -name "Gemfile" | head -1

# Locate test files that verify configuration retrieval behavior
find . -type f -name "*config*test*" -o -name "*test*config*" | grep -E "\.(go|js|py|rb)$"

# Identify the project's static analysis or linting configuration
find . -name ".golangci.yml" -o -name ".eslintrc*" -o -name "pylintrc" -o -name ".rubocop.yml" | head -1

# Execute configured static analysis checks to verify no hardcoded credentials
# (Tool-specific command derived from manifest and linting config)

# Locate deployment or infrastructure configuration
find . -type f \( -name "*.tf" -o -name "*.yaml" -o -name "*.yml" -o -name "Dockerfile" -o -name "docker-compose.yml" \) | head -5

# Verify environment variables referenced in application code are documented
grep -r "os\.Getenv\|process\.env\|ENV\[" . --include="*.go" --include="*.js" --include="*.py" --include="*.rb" | cut -d: -f1 | sort -u
```

**Accept when:**
- All runtime configuration values that vary across environments are retrieved from environment variables using the standard library accessor.
- No sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- Services validate required environment variables during initialization and fail with clear error messages when configuration is missing.
- All required environment variables are documented in deployment documentation with expected formats and example values.
- Configuration retrieval and validation logic is centralized and provides a typed interface to the application.
- Static analysis checks confirm no hardcoded credentials or environment-specific values appear in source files.
- Integration tests verify service initialization behavior with missing or invalid environment variables.
- Deployment checklist verification confirms all required environment variables are provisioned in target environments.

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review rejection is mandatory for pull requests that embed credentials or environment-specific configuration in source code. Build failure is mandatory when static analysis detects hardcoded sensitive values. Deployment rollback is mandatory when services fail initialization due to missing required environment variables.
</enforcement>