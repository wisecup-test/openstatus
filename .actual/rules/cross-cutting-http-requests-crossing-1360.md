# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Http Requests Crossing

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication crossing service boundaries.

### Rules

- **R-HTTP-001** MUST: All HTTP requests crossing service boundaries MUST explicitly set User-Agent headers to identify the calling service.
- **R-HTTP-002** MUST: All external service client requests MUST include Authorization headers using bearer token format.
- **R-HTTP-003** MUST: All HTTP client implementations MUST validate Content-Type on requests with bodies.
- **R-HTTP-004** SHOULD: Establish shared constants or configuration for standard header names to ensure consistency across service boundaries and reduce typo-related integration failures.
- **R-HTTP-005** SHOULD: Implement middleware or interceptor patterns to centralize header manipulation logic for cross-cutting concerns like authentication, tracing, and request identification.
- **R-HTTP-006** SHOULD: Create integration test suites that validate header contracts between services, including presence, format, and propagation of required headers across multi-hop request chains.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -i http | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r "User-Agent" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | head -20

# Identify and execute end-to-end tests that verify header propagation across multi-service request flows
grep -r "Authorization" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | grep -i header | head -20
```

**Accept when:**
- All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies
- Authorization headers are present in all external service client requests and use bearer token format
- Integration tests pass demonstrating correct header propagation and query parameter access across service boundaries
- Shared constants or configuration for standard header names are established and documented
- Middleware or interceptor patterns centralize header manipulation logic for cross-cutting concerns
- Integration test suites validate header contracts between services across multi-hop request chains

<enforcement>
Clause Code MUST NOT skip or defer verification. All HTTP requests crossing service boundaries MUST be audited for explicit User-Agent headers, Authorization headers in bearer token format, and Content-Type validation. Integration tests MUST pass before merge.
</enforcement>