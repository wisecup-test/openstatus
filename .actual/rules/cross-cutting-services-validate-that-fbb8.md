# Use Environment Variables for Runtime Configuration Secrets: Services Validate That

These rules are ALWAYS ACTIVE for all service initialization code that requires runtime configuration, HTTP client constructors, database connection initialization, external service integrations, and cloud service clients.

### Rules

- **R-ENV-001** SHOULD: Services SHOULD validate that required environment variables are present and non-empty during initialization and fail fast with descriptive error messages if missing.

### Verify

```bash
# Discover the project's static analysis configuration and execute the configured linter
# to detect hardcoded credentials or configuration values in source files
linter_config=$(find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'golangci.yml' | head -1)
if [ -n "$linter_config" ]; then
  echo "Found linter config: $linter_config"
  # Execute linter (command varies by project)
fi

# Locate the project's test suite and execute integration tests
# that verify services fail gracefully when required environment variables are missing
test_dir=$(find . -type d -name 'test*' -o -name '*test' -o -name 'tests' | head -1)
if [ -d "$test_dir" ]; then
  echo "Found test directory: $test_dir"
  # Execute test suite (command varies by project)
fi

# Identify the project's code search tooling and scan for environment variable
# retrieval patterns to confirm they occur during initialization rather than request processing
grep -r "os\.getenv\|process\.env\|environ\[" --include="*.go" --include="*.js" --include="*.py" --include="*.java" . 2>/dev/null | grep -v node_modules | head -20
```

**Accept when:**
- Static analysis reports no hardcoded credentials, API keys, or environment-specific configuration values in source files
- Integration tests confirm that services detect missing required environment variables at initialization and fail with descriptive error messages
- Code review confirms that environment variable retrieval occurs during service initialization and values are stored for reuse rather than retrieved per-request

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before approving changes that introduce new configuration or secrets handling.
</enforcement>