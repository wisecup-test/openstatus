# Adopt Structured Logging with Context Propagation for Error Reporting: Logging Implementations Include

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS checker handler implementations, external client interaction code, retry logic and backoff operations, and server initialization code that involves error conditions.

### Rules

- **R-LOG-001** MAY: Logging implementations MAY include structured fields for request identifiers, event types, and regional information when available from the request context.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
# (Exact tool and command to be derived from project repository)

# Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions including timeout, retry, and external service unavailability scenarios
# (Exact test runner and command to be derived from project repository)

# Identify the project's log output validation tooling and verify that error logs contain required structured fields including context identifiers and error details
# (Exact validation tool and command to be derived from project repository)
```

**Accept when:**
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review checklist, static analysis rules in continuous integration, and integration test coverage requirements are mandatory. Pull requests with violations identified by static analysis are blocked from merge until corrected.
</enforcement>