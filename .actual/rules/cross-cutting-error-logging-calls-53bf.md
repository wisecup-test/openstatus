# Standardize Structured Logging with zerolog for Observability: Error Logging Calls

These rules are ALWAYS ACTIVE for all HTTP, DNS, and TCP checker handler functions, job execution functions that perform protocol-specific health checks, client wrappers for external services, retry and backoff logic, and server initialization and lifecycle management code.

### Rules

- **R-LOGGING-001** MUST: Error logging calls MUST include the error object using the logging library's error attachment method rather than string formatting.

### Verify

```bash
# Discover and execute the project's static analysis tooling to verify all error logging calls use structured logging methods
# Locate the project's test suite and run integration tests that validate context propagation through handler chains
# Identify the project's linting configuration and verify rules enforce context-aware logging patterns
```

**Accept when:**
- All error logging statements in checker handlers and job executors use structured logging with error object attachment
- Context propagation is verified through the request lifecycle from handler entry to error logging
- Static analysis confirms no string concatenation or formatting is used for error message construction where structured fields are available

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations are caught by automated static analysis in the CI pipeline and code review checklist enforcement.
</enforcement>