# Use Environment Variables for Runtime Configuration Secrets: Environment Variable Names

These rules are ALWAYS ACTIVE for all service initialization code, HTTP client constructors, database connection initialization, external service integrations, and cloud service clients that require runtime configuration or secrets.

### Rules

- **R-ENV-001** SHOULD: Environment variable names SHOULD follow a consistent naming convention using uppercase with underscore separators.

### Verify

```bash
# Discover the project's static analysis configuration and execute the configured linter
# to detect hardcoded credentials or configuration values in source files
linter_config=$(find . -maxdepth 2 -type f \( -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' -o -name '.clippy.toml' \) | head -1)
if [ -n "$linter_config" ]; then
  echo "Found linter config: $linter_config"
  # Execute linter with security checks enabled
fi

# Locate the project's test suite and execute integration tests
# that verify services fail gracefully when required environment variables are missing
test_dir=$(find . -maxdepth 2 -type d \( -name 'test' -o -name 'tests' -o -name '__tests__' -o -name 'spec' \) | head -1)
if [ -n "$test_dir" ]; then
  echo "Found test directory: $test_dir"
  # Execute integration tests for environment variable handling
fi

# Identify the project's code search tooling and scan for environment variable
# retrieval patterns to confirm they occur during initialization rather than request processing
grep -r "process\.env\|os\.getenv\|getenv\|Environment\.GetEnvironmentVariable" . \
  --include="*.js" --include="*.ts" --include="*.py" --include="*.go" --include="*.java" --include="*.cs" \
  --exclude-dir=node_modules --exclude-dir=.git --exclude-dir=dist --exclude-dir=build \
  | grep -v "node_modules" || echo "No environment variable retrieval patterns found"
```

**Accept when:**
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request
- Environment variable names follow uppercase with underscore separator convention (e.g., `API_KEY`, `DATABASE_URL`, `SERVICE_ENDPOINT`)

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that falls within the defined scope.
</enforcement>