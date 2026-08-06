# Adopt Structured Logging with Context Propagation for Error Reporting: Http Client Operations

These rules are ALWAYS ACTIVE for all HTTP client operations, external service interactions, retry logic, and error handling paths in checker service implementations across all protocol handlers (HTTP, TCP, DNS) and event publishing code.

### Rules

- **R-HTTP-001** MUST: HTTP client operations that interact with external services MUST log errors with sufficient context to identify the request, response metadata, and timing information.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.lintrc*' | head -1 | xargs -I {} sh -c 'echo "Found linting config: {}"; cat {}'

# Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*error*' | grep -E '\.(go|java|py|ts)$' | head -5

# Identify the project's log output validation tooling and verify that error logs contain required structured fields
grep -r "structured.*log\|context.*log\|error.*log" . --include='*.go' --include='*.java' --include='*.py' --include='*.ts' | grep -E 'handler|client|retry' | head -10

# Verify context propagation in error logging calls
grep -r "log.*error\|Error.*log" . --include='*.go' --include='*.java' --include='*.py' --include='*.ts' | grep -v test | wc -l
```

**Accept when:**
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions (timeout, retry, external service unavailability)
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context
- Error logs contain required structured fields including request identifiers, event types, regional information, timing data, and protocol-specific details
- Error sanitization utilities strip sensitive information from error messages and response bodies before logging

<enforcement>
Clause Code MUST NOT skip or defer verification. All error logging in HTTP client operations, external service interactions, and retry logic MUST include context propagation and structured error metadata. Violations identified by static analysis or code review block merge until corrected. Runtime detection of uncorrelated error logs triggers alerts for investigation.
</enforcement>