# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Http Client Operations

These rules are ALWAYS ACTIVE for all HTTP client operations, internal API handlers, HTTP request handlers, HTTP client wrappers, external service integrations, job execution modules performing HTTP operations, and ping and health check handlers.

### Rules

- **R-HTTP-001** MUST: HTTP client operations that fail MUST log errors through the context-bound logger instance.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
# (Exact commands depend on project build tool and linter configuration — derive from repository)

# Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
# (Test discovery depends on project test framework — derive from repository)

# Identify the log aggregation query tool and run queries that detect error log entries from internal API modules
# (Query syntax depends on log aggregation platform — derive from repository)
```

**Accept when:**
- Static analysis reports no violations of logger initialization or error logging patterns
- Integration tests demonstrate error logs contain expected context fields and error messages
- Log queries successfully retrieve error entries with structured fields from all internal API handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client error paths MUST be validated to ensure context-bound logger invocation before code is committed.
</enforcement>