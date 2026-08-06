# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Http Request Handlers

These rules are ALWAYS ACTIVE for all HTTP request handlers that interact with external services, protocol handlers (HTTP, TCP, DNS) that require context propagation, and service boundary implementations that manipulate request/response headers.

### Rules

- **R-HTTP-001** MUST: All HTTP request handlers MUST use the cache-layer interface to retrieve and store request context data including event tracking and request identifiers.
- **R-HTTP-002** MUST: External HTTP requests MUST include User-Agent headers with service identification.
- **R-HTTP-003** MUST: Content-Type headers MUST be validated on inbound requests and set on outbound structured data requests.
- **R-HTTP-004** SHOULD: Establish naming conventions for cache-layer keys with handler-specific prefixes to prevent key collisions.
- **R-HTTP-005** SHOULD: Implement validation in external client wrappers to verify Content-Type is set when request body is present.

### Verify

```bash
# Discover and run protocol handler test suites to verify cache-layer context propagation
find . -type f -name '*test*' -path '*/handler*' | head -5

# Locate static analysis or linting configuration and execute checks for missing User-Agent or Content-Type headers
find . -type f \( -name '.golangci.yml' -o -name 'lint.yaml' -o -name '.eslintrc*' \) | head -3

# Identify integration test suites that exercise service boundaries
find . -type f -name '*integration*test*' | head -5

# Search for external client instantiation patterns
grep -r 'req\.Header\.Set\|req\.Header\.Get\|response\.Header\.Get' --include='*.go' --include='*.ts' --include='*.js' | head -10

# Verify cache-layer interface usage in handlers
grep -r 'c\.Get\|c\.Set\|c\.Query' --include='*.go' --include='*.ts' --include='*.js' | head -10
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling.
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data.
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios.
- Static analysis checks identify no missing User-Agent or Content-Type headers in external client code.
- Integration test suites validate header presence and correctness across service boundaries.

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review rejection is mandatory for handlers that bypass cache-layer abstraction without documented justification. Build failure is mandatory on static analysis detection of external requests missing required headers. Test failure is mandatory on integration tests detecting missing or incorrect context propagation.
</enforcement>