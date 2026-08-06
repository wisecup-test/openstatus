# Adopt Structured Logging with Context Propagation for Error Reporting: Handler Functions Propagate

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS checker handler implementations, external client interaction code, retry logic, and server initialization code that performs error logging.

### Rules

- **R-HANDLER-PROPAGATE-001** SHOULD: Handler functions SHOULD propagate context from the request framework to logging calls to maintain correlation across the request lifecycle.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
# (Exact tool and configuration location must be derived from the project repository)

# Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions including timeout, retry, and external service unavailability scenarios
# (Test runner and test file locations must be derived from the project repository)

# Identify the project's log output validation tooling and verify that error logs contain required structured fields including context identifiers and error details
# (Validation tool location must be derived from the project repository)
```

**Accept when:**
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context

<enforcement>
Code review checklist MUST verify context-aware error logging in all new handler implementations. Static analysis rules in continuous integration MUST detect error logging calls without context propagation. Integration test coverage MUST be required for error scenarios in all protocol checker implementations. Pull requests with violations identified by static analysis MUST be blocked from merge until corrected.
</enforcement>