# Standardize Structured Logging with zerolog for Observability: Services Include Additional

These rules are ALWAYS ACTIVE for all HTTP, DNS, and TCP checker handler functions, job execution functions that perform protocol-specific health checks, client wrappers for external services such as analytics platforms, retry and backoff logic, and server initialization and lifecycle management code.

### Rules

- **R-LOGGING-001** MUST: All error logging statements in checker handlers and job executors use structured logging with error object attachment via logger.Error() or log.Ctx(ctx).Error() patterns.
- **R-LOGGING-002** MUST: Context propagation is verified through the request lifecycle from handler entry to error logging, ensuring request identifiers and event metadata are attached before invoking business logic.
- **R-LOGGING-003** MUST: No string concatenation or formatting is used for error message construction where structured fields are available; use structured field attachment instead.
- **R-LOGGING-004** MAY: Services MAY include additional structured fields for protocol-specific data such as DNS record types, HTTP status codes, or TCP connection states.
- **R-LOGGING-005** SHOULD: Establish a context initialization pattern in HTTP handlers that attaches request identifiers and event metadata before invoking business logic.
- **R-LOGGING-006** SHOULD: Define standard structured fields for each protocol type (HTTP status codes, DNS record types, TCP connection states) to ensure consistency across handlers.

### Verify

```bash
# Discover and execute the project's static analysis tooling to verify all error logging calls use structured logging methods
# (Tool and command to be derived from project repository)

# Locate the project's test suite and run integration tests that validate context propagation through handler chains
# (Test command to be derived from project repository)

# Identify the project's linting configuration and verify rules enforce context-aware logging patterns
# (Linting command to be derived from project repository)
```

**Accept when:**
- All error logging statements in checker handlers and job executors use structured logging with error object attachment
- Context propagation is verified through the request lifecycle from handler entry to error logging
- Static analysis confirms no string concatenation or formatting is used for error message construction where structured fields are available
- Log output format is machine-parseable JSON with consistent field attachment across all protocol handlers

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis in the continuous integration pipeline MUST fail if unstructured error logging is detected. Code review MUST block merge if logging patterns do not follow established conventions. Runtime monitoring MUST alert if log parsing failures indicate malformed output.
</enforcement>