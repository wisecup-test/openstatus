# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Error Logging Calls

These rules are ALWAYS ACTIVE for all HTTP request handlers, HTTP client wrappers, job execution modules, ping and health check handlers, and error handling paths in request processing pipelines within internal APIs.

### Rules

- **R-ERRLOG-001** MUST: Error logging calls MUST extract error messages using the error interface method that returns string representations.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -1

# Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
find . -path '*/test*' -name '*handler*test*' -o -path '*/test*' -name '*http*test*' | head -5

# Identify the log aggregation query tool and run queries that detect error log entries from internal API modules
grep -r 'logger\.Error\|log\.Ctx.*Error' --include='*.go' --include='*.ts' --include='*.js' . | wc -l
```

**Accept when:**
- Static analysis reports no violations of logger initialization or error logging patterns
- Integration tests demonstrate error logs contain expected context fields and error messages
- Log queries successfully retrieve error entries with structured fields from all internal API handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging calls in HTTP handlers and client wrappers must be validated against R-ERRLOG-001 before code is committed.
</enforcement>