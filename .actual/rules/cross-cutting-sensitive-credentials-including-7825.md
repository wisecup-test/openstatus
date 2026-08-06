# Use Environment Variables for Runtime Configuration Values: Sensitive Credentials Including

These rules are ALWAYS ACTIVE for all service initialization code, configuration retrieval logic, and deployment manifests that handle runtime configuration values, authentication credentials, API keys, service endpoints, and environment-specific parameters.

### Rules

- **R-CRED-001** MUST: Sensitive credentials including API keys, authentication tokens, and service account keys MUST be sourced from environment variables and MUST NOT be embedded in source code or version control.
- **R-CRED-002** MUST: Retrieve environment variables during service initialization or client construction to enable fail-fast behavior when required configuration is missing, rather than deferring retrieval until first use.
- **R-CRED-003** MUST: Implement explicit validation for required environment variables, checking for presence and non-empty values, and provide clear error messages identifying which variables are missing or invalid.
- **R-CRED-004** SHOULD: Implement a configuration struct or object that encapsulates all environment variable retrieval and validation in a single location, providing a typed interface to the rest of the application.
- **R-CRED-005** SHOULD: Document all required and optional environment variables in deployment documentation, including expected formats, example values for non-sensitive parameters, and the impact of missing values.

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

# Search for hardcoded credentials or environment-specific values in source code
grep -r 'api[_-]?key\|password\|secret\|token' . --include='*.go' --include='*.js' --include='*.py' --include='*.rb' | grep -v test | grep -v vendor | grep -v node_modules

# Locate deployment or infrastructure configuration
find . -name 'docker-compose.yml' -o -name 'Dockerfile' -o -name '*.tf' -o -name 'k8s*.yaml' -o -name '.env.example'

# Verify environment variables referenced in application code are documented in deployment targets
grep -r 'os\.Getenv\|process\.env\|os\.environ\|ENV\[' . --include='*.go' --include='*.js' --include='*.py' --include='*.rb' | grep -oE '"[A-Z_]+"|\x27[A-Z_]+\x27' | sort -u
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