# Standardize Error Logging with Structured Logger Methods: Error Conditions Checker

These rules are ALWAYS ACTIVE for all error handling paths in checker handlers and job implementations across HTTP, DNS, TCP, and ping protocol implementations.

### Rules

- **R-ERR-001** MUST: All error conditions in checker handlers and job implementations MUST be logged using the structured logger's error-level method with context propagation.
- **R-ERR-002** MUST: Error logs MUST include context correlation identifiers when context is available through log.Ctx(ctx).Error() or equivalent structured logger patterns.
- **R-ERR-003** MUST: All handler functions MUST receive context as the first parameter and pass it to job implementations and external client calls.
- **R-ERR-004** SHOULD: Create helper functions that wrap common error logging patterns with protocol-specific structured fields to reduce code duplication across checker implementations.
- **R-ERR-005** SHOULD: Configure log output format and destination through environment variables or configuration files to support different deployment environments without code changes.

### Verify

```bash
# Locate the project's test suite directory and execute the integration test runner
find . -type f -name '*test*' -o -name '*_test.go' | head -5

# Search the codebase for error handling patterns
grep -r 'return err' --include='*.go' | grep -v 'log\|Error' | head -20

# Confirm all error returns are accompanied by structured log statements
grep -r 'log\.Ctx\|logger\.Error' --include='*.go' | wc -l

# Inspect the logging configuration initialization code
find . -type f -name '*log*' -o -name '*config*' | grep -E '\.(go|yaml|toml)$' | head -10

# Verify context propagation in function signatures
grep -r 'func.*ctx context\.Context' --include='*.go' | wc -l
```

**Accept when:**
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type (HTTP, DNS, TCP, ping)
- All handler functions receive context as the first parameter
- No error return paths exist without corresponding structured log statements

<enforcement>
Code review MUST block merge requests missing error logging in new error handling paths. Static analysis MUST detect error returns without corresponding log statements. Integration tests MUST validate error log output format and content. Claude Code MUST NOT skip or defer verification of these rules.
</enforcement>