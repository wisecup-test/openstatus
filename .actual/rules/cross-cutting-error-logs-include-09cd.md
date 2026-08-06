# Standardize Error Logging with Structured Logger Methods: Error Logs Include

These rules are ALWAYS ACTIVE for all error handling paths in HTTP checker handlers, DNS lookup handlers, TCP connection handlers, ping handlers, backoff retry operations, external client request failures, and assertion evaluation failures.

### Rules

- **R-ERRLOG-001** SHOULD: Error logs SHOULD include additional structured fields such as operation type, target endpoint, and retry attempt number when available.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner to verify error logging behavior in failure scenarios
find . -type f -name '*test*' -o -name '*_test.*' | head -5

# Search the codebase for error handling patterns and confirm all error returns are accompanied by structured log statements
grep -r "return err" --include="*.go" | grep -v "log\|Log" | head -10

# Inspect the logging configuration initialization code to verify context propagation is enabled and error level is configured
grep -r "log\.Ctx\|logger\.Error" --include="*.go" | head -10
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type

<enforcement>
Clause Code MUST NOT skip or defer verification. All error return paths MUST be accompanied by structured log statements with context propagation and protocol-specific fields.
</enforcement>