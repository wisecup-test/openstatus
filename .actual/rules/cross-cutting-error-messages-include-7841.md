# Adopt Structured Logging with Context Propagation for Error Reporting: Error Messages Include

These rules are ALWAYS ACTIVE for all HTTP, TCP, and DNS checker handler implementations, external client interaction code, retry logic and backoff operations, and server initialization code that involves error conditions.

### Rules

- **R-STRUCT-LOG-001** MUST: Error messages MUST include the string representation of the error obtained through the error extraction method.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
# (Exact command depends on project's linter configuration — consult project's build tool and CI configuration)

# Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions
# (Exact command depends on project's test framework — consult project's build tool configuration)

# Identify the project's log output validation tooling and verify that error logs contain required structured fields
# (Exact command depends on project's logging and validation infrastructure)
```

**Accept when:**
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions (timeout, retry, external service unavailability)
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context
- Error messages in all protocol checker implementations (HTTP, TCP, DNS) include the string representation of extracted errors

<enforcement>
Clause R-STRUCT-LOG-001 is mandatory. Code review, static analysis in CI, and integration test coverage must verify compliance before merge. Violations block pull requests. Exceptions require architecture review approval and documented compensating controls.
</enforcement>