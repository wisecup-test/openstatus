# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Error Logging Occur

These rules are ALWAYS ACTIVE for all HTTP request handlers, HTTP client wrappers, job execution modules, ping and health check handlers, and error handling paths in request processing pipelines within internal APIs.

### Rules

- **R-ERRLOG-001** SHOULD: Error logging SHOULD occur at the boundary where errors are first detected or handled.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -1 | xargs -I {} sh -c 'echo "Found config: {}"; cat {}'

# Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
find . -path '*/test*' -name '*handler*test*' -o -path '*/test*' -name '*client*test*' | head -5

# Identify log aggregation query tool and run queries that detect error log entries from internal API modules
grep -r "logger\.Error\|log\.Ctx.*Error" --include="*.go" --include="*.ts" --include="*.js" --include="*.py" . 2>/dev/null | wc -l
```

**Accept when:**
- Static analysis reports no violations of logger initialization or error logging patterns in HTTP handlers and client modules
- Integration tests demonstrate error logs contain expected context fields and error messages for HTTP operation failures
- Log queries successfully retrieve error entries with structured fields from all internal API handlers and external service integrations
- All error paths in HTTP request processing pipelines invoke contextual logger methods (logger.Error() or log.Ctx(ctx).Error())

<enforcement>
Claude Code MUST NOT skip or defer verification. All error handling paths in scope MUST be audited for compliance with R-ERRLOG-001 before code is considered complete.
</enforcement>