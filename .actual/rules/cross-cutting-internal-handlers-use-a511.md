# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Internal Handlers Use

These rules are ALWAYS ACTIVE for all internal API handlers, HTTP client wrappers, job execution modules, and ping/health check handlers that perform HTTP operations.

### Rules

- **R-INTERNAL-001** MUST: All internal API handlers MUST use contextual logger methods to record error conditions.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -1

# Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
find . -path '*/test*' -name '*handler*test*' -o -path '*/test*' -name '*http*test*' | head -5

# Identify the log aggregation query tool and run queries that detect error log entries from internal API modules
grep -r 'logger\.Error\|log\.Ctx.*Error' --include='*.go' --include='*.ts' --include='*.js' --include='*.py' . 2>/dev/null | wc -l
```

**Accept when:**
- Static analysis reports no violations of logger initialization or error logging patterns in internal API handlers
- Integration tests demonstrate error logs contain expected context fields and error messages from HTTP operation failures
- Log queries successfully retrieve error entries with structured fields from all internal API handler modules
- All error handling paths in HTTP request handlers invoke contextual logger methods

<enforcement>
Claude Code MUST NOT skip or defer verification. All internal API handlers must be audited for compliance with contextual logger method usage before code is committed.
</enforcement>