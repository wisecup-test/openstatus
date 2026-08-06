# Standardize HTTP Header Manipulation for Testing Strategy: Retry Logic Exponential

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, request and response validation logic, and client configuration for outbound HTTP requests.

### Rules

- **R-RETRY-001** SHOULD: Retry logic with exponential backoff SHOULD be implemented for transient failure scenarios in external service validation.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*spec*" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "User-Agent\|Content-Type" --include="*test*.go" --include="*_test.go" | head -10

# Search the codebase for header manipulation method invocations to verify
# consistent usage patterns across all checker components
grep -r "Header\|SetHeader" --include="*.go" | grep -v vendor | head -20

# Verify retry logic with exponential backoff is present in HTTP client operations
grep -r "exponential\|backoff\|retry" --include="*.go" | grep -v vendor | head -10
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms consistent use of header manipulation interfaces across all HTTP checker components
- Test logs demonstrate proper header values are set on outbound requests and validated on inbound responses
- Retry logic with exponential backoff is implemented and verified in external service validation code
- Request and response headers are logged during test execution for visibility into header contracts

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration tests must pass and code inspection must confirm consistent header manipulation patterns and exponential backoff retry logic before accepting changes.
</enforcement>