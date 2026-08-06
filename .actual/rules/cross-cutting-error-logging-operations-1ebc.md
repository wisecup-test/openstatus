# Adopt Structured Logging with Context Propagation for Error Reporting: Error Logging Operations

These rules are ALWAYS ACTIVE for all error logging operations in HTTP, TCP, and DNS checker handler implementations, external client interaction code, retry logic, and server initialization code.

### Rules

- **R-LOGGING-001** MUST: All error logging operations MUST extract context using the context-aware logger retrieval pattern and invoke error-level logging methods with the error object.

### Verify

```bash
# Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
find . -name '.golangci.yml' -o -name 'golangci.yaml' -o -name '.eslintrc*' -o -name 'pylintrc' | head -1 | xargs -I {} sh -c 'echo "Found config: {}"; cat {}'

# Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*error*' | grep -E '\.(go|js|py)$' | head -5

# Identify the project's log output validation tooling and verify that error logs contain required structured fields
grep -r "error.*log\|log.*error" . --include="*.go" --include="*.js" --include="*.py" | grep -E "context|ctx" | head -10
```

**Accept when:**
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging operations must be reviewed for context propagation compliance before code approval.
</enforcement>