# Standardize Error Logging with Structured Logger Methods: Error Log Statements

These rules are ALWAYS ACTIVE for all error handling paths in checker handlers, job implementations, and external client call sites across HTTP, DNS, TCP, and ping protocol implementations.

### Rules

- **R-ERRLOG-001** MUST: Error log statements MUST extract the error message using the error's string representation method and attach it to the structured log entry.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner to verify error logging behavior in failure scenarios
find . -type f -name '*test*' -o -name '*_test.*' | head -5

# Search the codebase for error handling patterns and confirm all error returns are accompanied by structured log statements
grep -r 'return err' --include='*.go' | grep -v 'log\|Log' | head -10

# Inspect the logging configuration initialization code to verify context propagation is enabled and error level is configured
grep -r 'log\.Ctx\|logger\.Error\|zerolog' --include='*.go' | grep -i 'init\|config' | head -5
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type

<enforcement>
Clause R-ERRLOG-001 verification is mandatory. Code review MUST block merge requests missing error logging in new error handling paths. Static analysis failures MUST trigger build warnings that escalate to errors after grace period. Production monitoring MUST alert on unstructured error messages or missing correlation identifiers.
</enforcement>