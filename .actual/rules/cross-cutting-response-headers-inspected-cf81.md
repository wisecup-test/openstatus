# Standardize HTTP Header Manipulation for Testing Strategy: Response Headers Inspected

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, and request/response validation logic within the project.

### Rules

- **R-HTTP-001** MUST: Response headers MUST be inspected using the response object's header retrieval interface to validate service behavior.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*check*" | grep -E "\.(sh|py|go|js)$" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "header" --include="*test*" --include="*check*" . | grep -i "response\|inspect" | head -10

# Search the codebase for header manipulation method invocations to verify
# consistent usage patterns across all HTTP checker components
grep -r "\.Header\|GetHeader\|response\.Header" --include="*.go" --include="*.py" --include="*.js" . | head -15
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms consistent use of header retrieval interfaces across all HTTP checker components
- Test logs demonstrate proper response header values are being validated on inbound responses
- No inconsistent header inspection patterns are detected across checker implementations

<enforcement>
Claude Code MUST NOT skip or defer verification. Header inspection patterns must be validated before accepting any HTTP checker component implementation.
</enforcement>