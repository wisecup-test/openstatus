# Standardize Error Logging with Structured Logger Methods: Error Logging Not

These rules are ALWAYS ACTIVE for all error handling paths in HTTP checker handlers, DNS lookup handlers, TCP connection handlers, ping handlers, backoff retry operations, external client request failures, and assertion evaluation failures across the distributed health checking service.

### Rules

- **R-ERR-001** MUST_NOT: Error logging MUST NOT block the error handling flow or suppress error propagation to callers.
- **R-ERR-002** MUST: All error conditions in checker handlers and job implementations emit structured log entries at error level.
- **R-ERR-003** MUST: Error logs include context correlation identifiers when context is available via log.Ctx(ctx).Error() patterns.
- **R-ERR-004** MUST: All handler functions receive context as the first parameter and pass it to job implementations and external client calls.
- **R-ERR-005** SHOULD: Create helper functions that wrap common error logging patterns with protocol-specific structured fields to reduce code duplication.
- **R-ERR-006** SHOULD: Configure log output format and destination through environment variables or configuration files to support different deployment environments.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner
find . -type f -name '*test*' -o -name '*_test.go' | head -5

# Search the codebase for error handling patterns
grep -r "return err" --include="*.go" | grep -v "log\|Error" | head -20

# Confirm all error returns are accompanied by structured log statements
grep -r "log\.Ctx(ctx)\.Error\|logger\.Error" --include="*.go" | wc -l

# Inspect the logging configuration initialization code
find . -type f -name "*log*" -o -name "*config*" | grep -E "\.(go|yaml|toml)$" | head -10

# Verify context propagation is enabled
grep -r "context\.Context" --include="*.go" | grep "func.*ctx" | head -10
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type (HTTP, DNS, TCP, ping)
- No error return paths exist without corresponding structured log statements
- Context is propagated as the first parameter through all handler and job implementation functions
- Error logging does not block error propagation to callers

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review MUST enforce error logging for all error return paths. Static analysis rules MUST detect error returns without corresponding log statements. Integration tests MUST validate error log output format and content before merge.
</enforcement>