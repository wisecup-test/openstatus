# Standardize HTTP Header Manipulation for Testing Strategy: Http Request Headers

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, and request/response validation logic within the project's test suite.

### Rules

- **R-HTTP-001** MUST: HTTP request headers MUST be set programmatically using the request object's header manipulation interface before client execution.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*spec*" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "header" --include="*test*.go" --include="*_test.go" | head -20

# Search the codebase for header manipulation method invocations
# to verify consistent usage patterns across all HTTP checker components
grep -r "Header\|SetHeader\|Get(\"" --include="*.go" | grep -i test | head -20
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms consistent use of header manipulation interfaces across all HTTP checker components
- Test logs demonstrate proper header values are set on outbound requests and validated on inbound responses

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test failures due to incorrect header configuration block merge requests. Code review feedback requires correction of non-standard header manipulation patterns. Static analysis warnings are escalated to errors for critical header operations.
</enforcement>