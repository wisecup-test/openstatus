# Standardize HTTP Header Manipulation for Testing Strategy: Request Response Body

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, request and response validation logic, and client configuration for outbound HTTP requests.

### Rules

- **R-HTTP-001** MUST: Request and response body content MUST be accessible for assertion evaluation and validation logic.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*check*" | grep -E "\.(sh|py|go|js)$" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "request.*header\|response.*header" . --include="*.go" --include="*.py" --include="*.js" | head -10

# Search the codebase for header manipulation method invocations to verify
# consistent usage patterns across all HTTP checker components
grep -r "SetHeader\|Header\|Headers" . --include="*.go" --include="*.py" --include="*.js" | grep -v node_modules | head -15
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms consistent use of header manipulation interfaces across all HTTP checker components
- Test logs demonstrate proper header values are set on outbound requests and validated on inbound responses
- Request and response body content is accessible and validated in assertion logic

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test failures due to incorrect header or body configuration block merge requests. Code review feedback requires correction of non-standard header manipulation patterns. Static analysis warnings are escalated to errors for critical header operations.
</enforcement>