# Use Environment Variables for Runtime Configuration Values: Runtime Configuration Values

These rules are ALWAYS ACTIVE for all runtime configuration retrieval code that varies across deployment environments (development, staging, production).

### Rules

- **R-CONFIG-001** MUST: Runtime configuration values that vary across deployment environments MUST be retrieved from environment variables using the standard library accessor.
- **R-CONFIG-002** MUST: Retrieve environment variables during service initialization or client construction to enable fail-fast behavior when required configuration is missing.
- **R-CONFIG-003** MUST: Implement explicit validation for required environment variables, checking for presence and non-empty values, and provide clear error messages identifying which variables are missing or invalid.
- **R-CONFIG-004** SHOULD: Implement a configuration struct or object that encapsulates all environment variable retrieval and validation in a single location, providing a typed interface to the rest of the application.
- **R-CONFIG-005** MUST: Document all required and optional environment variables in deployment documentation, including expected formats, example values for non-sensitive parameters, and the impact of missing values.
- **R-CONFIG-006** MUST: No sensitive credentials or environment-specific endpoints are embedded in source code or version control.

### Verify

```bash
# Discover the project's dependency manifest and identify the standard library module used for environment variable access
find . -name "go.mod" -o -name "package.json" -o -name "requirements.txt" -o -name "Gemfile" | head -1

# Locate test files that verify configuration retrieval behavior
find . -type f -name "*config*test*" -o -name "*test*config*" | grep -E "\.(go|js|py|rb)$"

# Identify the project's static analysis or linting configuration
find . -name ".golangci.yml" -o -name ".eslintrc*" -o -name "pylintrc" -o -name ".rubocop.yml"

# Execute configured static analysis checks to verify no hardcoded credentials
# (command varies by language/tool; example for Go)
grep -r "const.*=.*['\"]" --include="*.go" | grep -iE "(password|secret|key|token|credential)" || echo "No obvious hardcoded credentials found"

# Locate deployment or infrastructure configuration
find . -name "*.tf" -o -name "*.yaml" -o -name "*.yml" -o -name "Dockerfile" -o -name "docker-compose.yml" | head -5

# Verify environment variables referenced in code are documented
grep -r "os\.Getenv\|process\.env\|ENV\[" --include="*.go" --include="*.js" --include="*.py" | cut -d: -f2 | sort -u
```

**Accept when:**
- All runtime configuration values that vary across environments are retrieved from environment variables using the standard library accessor.
- No sensitive credentials or environment-specific endpoints are embedded in source code or version control.
- Services validate required environment variables during initialization and fail with clear error messages when configuration is missing.
- All required environment variables are documented in deployment documentation with expected formats and example values.
- Configuration retrieval is centralized in a single location (configuration struct/object) providing a typed interface.
- Static analysis confirms no hardcoded credentials or environment-specific values appear in source files.

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review rejection is mandatory for pull requests that embed credentials or environment-specific configuration in source code. Build failure is mandatory when static analysis detects hardcoded sensitive values.
</enforcement>