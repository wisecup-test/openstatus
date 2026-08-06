# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Handlers Enrich Log

These rules are ALWAYS ACTIVE for all HTTP request handlers, HTTP client wrappers, job execution modules, and error handling paths in internal APIs across the checker application.

### Rules

- **R-HANDLERS-001** MAY: Handlers MAY enrich log entries with request-specific metadata from headers or query parameters.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
find . -name ".golangci.yml" -o -name "golangci.yml" -o -name "lint.yaml" | head -1

# Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
find . -path "*/test*" -name "*handler*test.go" -o -path "*/test*" -name "*client*test.go" | head -5

# Identify log aggregation query tool and run queries that detect error log entries from internal API modules
grep -r "logger\.Error\|log\.Ctx.*Error" --include="*.go" | grep -E "(handler|client|job)" | wc -l
```

**Accept when:**
- Static analysis reports no violations of logger initialization or error logging patterns
- Integration tests demonstrate error logs contain expected context fields and error messages
- Log queries successfully retrieve error entries with structured fields from all internal API handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. All three acceptance criteria must be confirmed before approving changes to HTTP handlers, HTTP clients, or job execution modules that involve error handling.
</enforcement>