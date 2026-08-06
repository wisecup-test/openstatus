# Standardize HTTP Header Manipulation for Testing Strategy: Http Clients Configured

These rules are ALWAYS ACTIVE for all HTTP-based integration testing components, external service health check implementations, request and response validation logic, and client configuration for outbound HTTP requests.

### Rules

- **R-HTTP-001** MUST: HTTP clients MUST be configured with explicit timeout values to prevent indefinite blocking during external service checks.

### Verify

```bash
# Locate the project's test execution script and run the integration test suite
# to verify header manipulation patterns are functioning correctly
find . -name "*test*" -o -name "*spec*" | head -5

# Inspect test output logs to confirm request headers are being set as expected
# and response headers are being validated
grep -r "timeout" --include="*.go" --include="*.py" --include="*.js" --include="*.ts" | grep -i "http\|client" | head -10

# Search the codebase for HTTP client instantiation to verify timeout configuration
grep -r "Client\|client" --include="*.go" --include="*.py" --include="*.js" --include="*.ts" | grep -i "timeout" | head -10
```

**Accept when:**
- All integration tests pass with correct timeout configuration for HTTP clients
- Code inspection confirms explicit timeout values are set on all HTTP client instances
- Test logs demonstrate HTTP clients are not blocking indefinitely on external service calls
- No HTTP client instantiations exist without explicit timeout configuration in integration test code

<enforcement>
Claude Code MUST NOT skip or defer verification of timeout configuration on HTTP clients. All HTTP client instantiations in integration testing code MUST include explicit timeout values before code is considered compliant.
</enforcement>