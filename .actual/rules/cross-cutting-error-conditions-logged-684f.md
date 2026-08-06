# Standardize Structured Logging with zerolog for Observability: Error Conditions Logged

These rules are ALWAYS ACTIVE for all HTTP, DNS, and TCP checker handler functions, job execution functions, client wrappers for external services, retry and backoff logic, and server initialization code.

### Rules

- **R-LOGGING-001** MUST: All error conditions MUST be logged using structured logging methods that capture both the error message and contextual metadata.

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
Claude Code MUST NOT skip or defer verification. Violations are caught by automated static analysis in the CI pipeline, code review checklists, and integration tests that validate log output format and field presence.
</enforcement>