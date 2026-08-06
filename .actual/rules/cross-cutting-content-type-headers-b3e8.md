# Standardize HTTP Header Manipulation for Testing Strategy: Content Type Headers

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, request and response validation logic, and client configuration for outbound HTTP requests.

### Rules

- **R-CONTENT-TYPE-001** MUST: Content-Type headers MUST be validated on both request and response objects to ensure correct content negotiation.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*spec*" | grep -E "\.(sh|py|go|js)$" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "Content-Type" --include="*test*" --include="*spec*" .

# Search the codebase for header manipulation method invocations to verify
# consistent usage patterns across all checker components
grep -r "SetHeader\|Header(\|AddHeader" --include="*.go" --include="*.py" --include="*.js" . | grep -i content-type
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms Content-Type headers are validated on both request and response objects across all HTTP checker components
- Test logs demonstrate proper Content-Type values are set on outbound requests and validated on inbound responses
- No inconsistent Content-Type header manipulation patterns are detected across checker implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test failures due to incorrect Content-Type header configuration block merge requests. Code review feedback requires correction of non-standard Content-Type header validation patterns.
</enforcement>