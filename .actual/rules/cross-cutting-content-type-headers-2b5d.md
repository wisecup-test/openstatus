# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Content Type Headers

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication.

### Rules

- **R-CONTENT-TYPE-001** MUST: Content-Type headers MUST be validated on incoming requests and explicitly set on outgoing requests when body content is present.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|client|handler|integration)' | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r "Content-Type" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | grep -E '(header|Header)' | head -20

# Identify HTTP client implementations and verify Content-Type header handling
grep -r "User-Agent\|Authorization\|Content-Type" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | grep -E '(client|request|http)' | head -30
```

**Accept when:**
- All HTTP client implementations validate Content-Type headers on incoming requests
- All HTTP client implementations explicitly set Content-Type headers on outgoing requests when body content is present
- All HTTP handler implementations validate Content-Type headers on incoming requests
- Integration tests pass demonstrating correct Content-Type header validation and propagation across service boundaries
- Code review checklist items confirm Content-Type header operations are present in all HTTP client and handler implementations

<enforcement>
Claude Code MUST NOT skip or defer verification of Content-Type header validation and explicit header setting on requests with body content. All HTTP service boundary implementations MUST comply with R-CONTENT-TYPE-001.
</enforcement>