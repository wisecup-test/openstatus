# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Custom Headers Added

These rules are ALWAYS ACTIVE for all HTTP client implementations, HTTP handler implementations, middleware components that inspect or modify request/response headers, and API client libraries that encapsulate external service communication.

### Rules

- **R-HTTP-HDR-001** MAY: Custom headers MAY be added to requests for tracing, correlation, or feature flag propagation.
- **R-HTTP-HDR-002** MUST: All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies.
- **R-HTTP-HDR-003** MUST: Authorization headers are present in all external service client requests and use bearer token format.
- **R-HTTP-HDR-004** SHOULD: Establish shared constants or configuration for standard header names to ensure consistency across service boundaries and reduce typo-related integration failures.
- **R-HTTP-HDR-005** SHOULD: Implement middleware or interceptor patterns to centralize header manipulation logic for cross-cutting concerns like authentication, tracing, and request identification.
- **R-HTTP-HDR-006** SHOULD: Create integration test suites that validate header contracts between services, including presence, format, and propagation of required headers across multi-hop request chains.
- **R-HTTP-HDR-007** SHOULD: Implement header sanitization in logging middleware to prevent sensitive data exposure through observability tooling.
- **R-HTTP-HDR-008** SHOULD: Encapsulate header access behind internal adapter interfaces to reduce migration friction if HTTP framework changes.

### Verify

```bash
# Discover and execute the project's test suite focusing on HTTP client and handler integration tests
# that validate header manipulation and query parameter access
find . -type f -name '*test*' -o -name '*spec*' | grep -E '(http|client|handler|integration)' | head -20

# Locate and run static analysis or linting rules that enforce header naming conventions
# and required header presence at service boundaries
grep -r "User-Agent" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | head -20
grep -r "Authorization" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | head -20
grep -r "Content-Type" --include="*.go" --include="*.js" --include="*.ts" --include="*.py" . | head -20

# Identify and execute end-to-end tests that verify header propagation across multi-service request flows
find . -type f \( -name '*e2e*' -o -name '*integration*' \) -name '*test*' | head -20
```

**Accept when:**
- All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies
- Authorization headers are present in all external service client requests and use bearer token format
- Integration tests pass demonstrating correct header propagation and query parameter access across service boundaries
- Shared constants or configuration exist for standard header names across service boundaries
- Middleware or interceptor patterns centralize header manipulation logic for cross-cutting concerns
- Integration test suites validate header contracts between services including presence, format, and propagation
- Header sanitization is implemented in logging middleware to prevent sensitive data exposure
- Header access is encapsulated behind internal adapter interfaces

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for HTTP client implementations, HTTP handler implementations, middleware components, and API client libraries within the configured scope. Integration tests validating header contracts must pass before code merge. Code review must verify User-Agent, Content-Type, and Authorization header presence. Runtime monitoring must alert on missing headers at service boundaries.
</enforcement>