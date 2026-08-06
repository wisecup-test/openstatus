# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Authorization Credentials Propagated

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication.

### Rules

- **R-AUTH-001** MUST: Authorization credentials MUST be propagated via request headers using bearer token format when authenticating to external services.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|client|handler|integration)' | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r "Authorization" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | grep -i "header\|bearer" | head -20

# Identify and execute end-to-end tests that verify header propagation across multi-service request flows
find . -type f \( -name '*e2e*' -o -name '*integration*' \) -print | head -20
```

**Accept when:**
- All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies
- Authorization headers are present in all external service client requests and use bearer token format
- Integration tests pass demonstrating correct header propagation and query parameter access across service boundaries
- Code review checklist items for HTTP client and handler implementations include header operation verification
- Static analysis rules detect missing or malformed header operations

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client and handler implementations MUST include bearer token Authorization headers. Integration tests validating header contracts are mandatory before merge.
</enforcement>