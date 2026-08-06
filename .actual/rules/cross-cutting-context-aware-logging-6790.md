# Standardize Error Logging with Structured Logger Methods: Context Aware Logging

These rules are ALWAYS ACTIVE for all error handling paths in HTTP checker handlers, DNS lookup handlers, TCP connection handlers, ping handlers, backoff retry operations, external client request failures, and assertion evaluation failures.

### Rules

- **R-LOG-001** MUST: Context-aware logging MUST retrieve the logger instance from the request context to preserve correlation identifiers and request metadata.
- **R-LOG-002** MUST: All error conditions in checker handlers and job implementations MUST emit structured log entries at error level.
- **R-LOG-003** MUST: Error logs MUST include context correlation identifiers when context is available.
- **R-LOG-004** SHOULD: Establish a context propagation pattern where all handler functions receive context as the first parameter and pass it to job implementations and external client calls.
- **R-LOG-005** SHOULD: Create helper functions that wrap common error logging patterns with protocol-specific structured fields to reduce code duplication across checker implementations.
- **R-LOG-006** SHOULD: Configure log output format and destination through environment variables or configuration files to support different deployment environments without code changes.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner
find . -type f -name '*test*' -o -name '*_test.go' | head -5

# Search the codebase for error handling patterns
grep -r 'return err' --include='*.go' | grep -v 'log\|Log' | head -20

# Inspect the logging configuration initialization code
grep -r 'log\.Ctx\|logger\.' --include='*.go' | head -20

# Verify error returns are accompanied by structured log statements
grep -B2 'return err' --include='*.go' -r . | grep -E 'log\.Ctx|logger\.Error' | wc -l
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type
- Code review checklist confirms error logging for all error return paths
- Static analysis rules detect error returns without corresponding log statements
- Integration test assertions validate error log output format and content

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review MUST block merge requests missing error logging in new error handling paths. Static analysis failures MUST trigger build warnings that escalate to errors after grace period. Production monitoring MUST alert on unstructured error messages or missing correlation identifiers.
</enforcement>