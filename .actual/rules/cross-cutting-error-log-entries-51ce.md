# Standardize Structured Error Logging with Context Propagation: Error Log Entries

These rules are ALWAYS ACTIVE for all handler functions that process HTTP, DNS, TCP, or ping monitoring requests; client interaction code that performs external HTTP requests or network operations; retry logic implementations using backoff strategies; public API contract implementations that expose error states; and integration points with third-party APIs where errors must be captured.

### Rules

- **R-LOGGING-001** MUST: Error log entries MUST include the error message through a dedicated error field rather than string concatenation.

### Verify

```bash
# Discover the project's dependency manifest and lock file, then locate the verification script that validates structured logging patterns across handler implementations
find . -name 'go.mod' -o -name 'go.sum' -o -name 'package.json' -o -name 'package-lock.json' | head -5

# Identify the project's static analysis or linting configuration and execute the rule that checks for context-aware error logging method usage
find . -name '.golangci.yml' -o -name 'eslintrc*' -o -name '.lintrc*' | head -5

# Locate the integration test suite for protocol handlers and run tests that verify error log output includes structured fields and context correlation
find . -path '*/test*' -name '*handler*test*' -o -path '*/test*' -name '*error*test*' | head -10
```

**Accept when:**
- All error logging call sites in handler functions use context-aware structured logging methods with error field extraction (e.g., `log.Ctx(ctx).Error().Err(err)` patterns)
- Static analysis confirms no error logging uses string concatenation or unstructured output methods
- Integration tests verify that error logs from retry operations include correlation identifiers and can be queried by structured fields
- Error log entries across HTTP, DNS, TCP, and ping handlers consistently extract error messages into dedicated fields rather than embedding them in log messages

<enforcement>
Claude Code MUST NOT skip or defer verification. All error logging patterns must be validated against R-LOGGING-001 before code review approval.
</enforcement>