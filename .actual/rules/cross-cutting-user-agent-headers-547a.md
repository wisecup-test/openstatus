# Standardize HTTP Header Manipulation for Testing Strategy: User Agent Headers

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, request and response validation logic, and client configuration for outbound HTTP requests.

### Rules

- **R-UA-001** MUST: User-Agent headers MUST be set to identify the checking service to external endpoints.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*check*" | grep -E "\.(sh|py|go|js)$" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "User-Agent" --include="*test*" --include="*check*" .

# Search the codebase for header manipulation method invocations to verify
# consistent usage patterns across all HTTP checker components
grep -r "SetHeader\|Header(\|headers\[" --include="*.go" --include="*.py" --include="*.js" . | grep -i "user-agent"
```

**Accept when:**
- All integration tests pass with correct header configuration for both requests and responses
- Code inspection confirms consistent use of header manipulation interfaces across all HTTP checker components
- Test logs demonstrate proper User-Agent header values are set on outbound requests and validated on inbound responses
- No checker component omits User-Agent header configuration for external service calls

<enforcement>
Claude Code MUST NOT skip or defer verification. Integration test failures due to missing or incorrect User-Agent headers block merge requests. Code review feedback requires correction of non-standard header manipulation patterns. Static analysis warnings for missing User-Agent headers are escalated to errors.
</enforcement>