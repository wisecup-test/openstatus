# Adopt Structured Logging with Context Propagation for Error Reporting: Retry Operations Log

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS checker handler implementations, external client interaction code, retry logic and backoff operations, and server initialization and health check endpoint implementations.

### Rules

- **R-RETRY-LOG-001** SHOULD: Retry operations SHOULD log errors at each attempt to provide visibility into backoff behavior and failure patterns.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
# (Exact command depends on project's linter configuration — inspect .actual/config or build tool manifest)

# Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions
# (Run integration tests covering timeout, retry, and external service unavailability scenarios)

# Identify the project's log output validation tooling and verify that error logs contain required structured fields
# (Validate error logs contain correlation identifiers and structured error data)
```

**Accept when:**
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review checklist MUST require verification of context-aware error logging in all new handler implementations. Static analysis rules in continuous integration MUST detect error logging calls without context propagation. Integration test coverage MUST be required for error scenarios in all protocol checker implementations. Pull requests with violations identified by static analysis MUST be blocked from merge until corrected.
</enforcement>