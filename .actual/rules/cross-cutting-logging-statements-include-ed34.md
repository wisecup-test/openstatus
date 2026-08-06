# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Logging Statements Include

These rules are ALWAYS ACTIVE for HTTP request handlers in internal APIs, HTTP client wrappers and external service integrations, job execution modules performing HTTP operations, ping and health check handlers, and error handling paths in request processing pipelines.

### Rules

- **R-LOG-001** SHOULD: Logging statements SHOULD include structured context fields derived from the request context.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -1 | xargs -I {} sh -c 'echo "Found linting config: {}"; cat {}'

# Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
find . -path '*/test*' -name '*handler*test*' -o -path '*/test*' -name '*http*test*' | head -5

# Identify log aggregation query tool and verify error log entries from internal API modules
grep -r "log\.Ctx\|logger\.Error" --include="*.go" --include="*.ts" --include="*.js" . | grep -E "(handler|client|job|ping)" | head -10
```

**Accept when:**
- Static analysis reports no violations of logger initialization or error logging patterns
- Integration tests demonstrate error logs contain expected context fields and error messages
- Log queries successfully retrieve error entries with structured fields from all internal API handlers
- All error handling paths in HTTP handlers invoke context-bound logger methods with structured fields

<enforcement>
Claude Code MUST NOT skip or defer verification. All error paths in HTTP handlers, clients, and job execution modules MUST include structured context logging before proceeding.
</enforcement>