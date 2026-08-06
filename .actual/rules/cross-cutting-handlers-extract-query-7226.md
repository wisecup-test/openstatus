# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Handlers Extract Query

These rules are ALWAYS ACTIVE for all handler implementations that interact with external services, protocol handlers (HTTP, TCP, DNS) that require context propagation, service boundary implementations that manipulate request/response headers, and cache-layer abstractions used for request metadata access.

### Rules

- **R-HANDLER-001** MAY: Handlers MAY extract query parameters through the cache-layer interface when protocol-agnostic access patterns are beneficial.
- **R-HANDLER-002** MUST: All external HTTP requests MUST include User-Agent headers for service identification at boundary crossings.
- **R-HANDLER-003** MUST: Content-Type headers MUST be validated on inbound requests and set on outbound structured data to prevent deserialization errors.
- **R-HANDLER-004** SHOULD: Handler implementations SHOULD use the cache-layer interface (c.Get, c.Set, c.Query) for event tracking and request identifier propagation rather than direct protocol-specific operations.
- **R-HANDLER-005** SHOULD: Cache-layer keys SHOULD follow naming conventions with handler-specific prefixes to prevent key collisions and context data corruption.
- **R-HANDLER-006** MUST NOT: Handlers MUST NOT bypass the cache-layer abstraction for context access without documented justification and architectural review approval.

### Verify

```bash
# Discover and run protocol handler test suites to verify cache-layer context propagation
find . -type f -name '*_test.*' | grep -E '(handler|protocol)' | head -5

# Locate static analysis or linting configuration and execute checks for missing User-Agent or Content-Type headers
find . -type f \( -name '.golangci.yml' -o -name 'lint.yaml' -o -name '.eslintrc*' \) | head -3

# Identify integration test suites that exercise service boundaries
find . -type f -name '*integration*test*' -o -name '*e2e*test*' | head -5

# Search for external client instantiation patterns
grep -r 'req\.Header\.Set\|req\.Header\.Get\|response\.Header\.Get' --include='*.go' --include='*.ts' --include='*.js' | head -10

# Verify cache-layer interface usage in handlers
grep -r 'c\.Get\|c\.Set\|c\.Query' --include='*.go' --include='*.ts' --include='*.js' | grep -i handler | head -10
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios
- Cache-layer key naming conventions are documented and consistently applied across all handler implementations
- No handlers bypass the cache-layer abstraction without documented justification and architectural review approval

<enforcement>
Clause Code MUST NOT skip or defer verification. Code review MUST reject handlers that bypass cache-layer abstraction without documented justification. Build MUST fail on static analysis detection of external requests missing required headers. Integration tests MUST validate header propagation across service boundaries.
</enforcement>