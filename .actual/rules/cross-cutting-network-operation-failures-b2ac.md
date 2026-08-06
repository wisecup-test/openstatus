# Standardize Error Logging with Structured Logger Methods: Network Operation Failures

These rules are ALWAYS ACTIVE for all error handling paths in HTTP, DNS, TCP, and ping checker handlers and job implementations, backoff retry operations, external client request failures, and assertion evaluation failures.

### Rules

- **R-NETLOG-001** SHOULD: Network operation failures in HTTP, DNS, and TCP checks SHOULD log both the error and the target resource identifier using structured logger methods with context correlation.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner
find . -type f -name '*test*' -o -name '*_test.go' | head -5

# Search the codebase for error handling patterns
grep -r "return err" --include="*.go" | grep -v "log\|Log" | head -20

# Inspect logging configuration initialization code
find . -type f -name '*.go' | xargs grep -l "log\.Ctx\|logger\.Error" | head -10

# Verify error logs contain structured fields
grep -r "log\.Ctx(ctx)\.Error" --include="*.go" | wc -l
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type (HTTP, DNS, TCP, ping)
- Error returns are accompanied by structured log statements using log.Ctx(ctx).Error() or equivalent patterns
- Error logs include both the error message (via err.Error()) and the target resource identifier

<enforcement>
Claude Code MUST NOT skip or defer verification. All error handling paths in scope MUST include structured logging with context correlation before code is considered complete.
</enforcement>