# Standardize Error Logging with Structured Logger Methods: Non Critical Errors

These rules are ALWAYS ACTIVE for all error handling paths in checker handlers, job implementations, and external client call sites across HTTP, DNS, TCP, and ping protocol implementations.

### Rules

- **R-NONCRIT-001** MAY: Non-critical errors in assertion evaluation or response parsing MAY be logged at warning level instead of error level.
- **R-NONCRIT-002** MUST: All error conditions in checker handlers and job implementations emit structured log entries using context-aware logging patterns (log.Ctx(ctx).Error() or equivalent).
- **R-NONCRIT-003** MUST: Error logs include context correlation identifiers when context is available in the call chain.
- **R-NONCRIT-004** SHOULD: Extract error strings with err.Error() to provide human-readable error messages while maintaining structured log format for automated parsing.
- **R-NONCRIT-005** SHOULD: Create helper functions that wrap common error logging patterns with protocol-specific structured fields to reduce code duplication across checker implementations.
- **R-NONCRIT-006** MUST: Establish a context propagation pattern where all handler functions receive context as the first parameter and pass it to job implementations and external client calls.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner
find . -type f -name '*test*.go' -o -name '*_test.go' | head -5

# Search the codebase for error handling patterns
grep -r "return err" --include="*.go" | grep -v "log\|Log" | head -10

# Confirm all error returns are accompanied by structured log statements
grep -r "log\.Ctx(ctx)\.Error\|logger\.Error" --include="*.go" | wc -l

# Inspect the logging configuration initialization code
find . -type f -name "*.go" | xargs grep -l "zerolog\|log.*init" | head -5

# Verify context propagation in function signatures
grep -r "func.*ctx context\.Context" --include="*.go" | wc -l
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error or warning level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type (HTTP, DNS, TCP, ping)
- Context is propagated as the first parameter through all handler and job implementation function calls
- Helper functions exist that wrap common error logging patterns with protocol-specific structured fields
- No error return paths exist without corresponding structured log statements

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review approval and must be validated before merge.
</enforcement>