# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Content Type Headers

These rules are ALWAYS ACTIVE for all HTTP request handlers that interact with external services, protocol handlers (HTTP, TCP, DNS) that require context propagation, service boundary implementations that manipulate request/response headers, and cache-layer abstractions used for request metadata access.

### Rules

- **R-HEADER-001** MUST: Content-Type headers MUST be validated on inbound requests and set explicitly on outbound requests when transmitting structured data.
- **R-HEADER-002** MUST: All external HTTP client instantiations MUST include User-Agent headers for service identification at boundary crossings.
- **R-HEADER-003** MUST: Handler implementations MUST use the cache-layer interface (c.Get, c.Set, c.Query) for request context propagation rather than direct protocol coupling.
- **R-HEADER-004** MUST: Cache-layer keys MUST follow handler-specific naming conventions with prefixes to prevent key collisions across handlers.
- **R-HEADER-005** MUST: External client wrappers MUST validate that Content-Type is set when request body is present before transmission.

### Verify

```bash
# Discover and run protocol handler test suites to verify cache-layer context propagation
find . -name '*test*' -type f | grep -E '(handler|protocol)' | head -5

# Locate static analysis or linting configuration and execute checks for missing User-Agent or Content-Type headers
find . -name '.lint*' -o -name 'lint.yaml' -o -name '.golangci.yml' 2>/dev/null

# Identify integration test suites that exercise service boundaries
find . -name '*integration*test*' -type f | head -5

# Search for external client instantiation patterns to verify header manipulation
grep -r 'req\.Header\.Set\|req\.Header\.Get\|response\.Header\.Get' --include='*.go' | head -10

# Verify cache-layer interface usage in handlers
grep -r 'c\.Get\|c\.Set\|c\.Query' --include='*.go' | head -10
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios
- Cache-layer key naming conventions are documented and consistently applied across all handler implementations
- External client wrappers validate Content-Type presence when request bodies are present

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review acceptance.
</enforcement>