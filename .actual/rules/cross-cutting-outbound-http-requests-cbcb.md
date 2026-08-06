# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Outbound Http Requests

These rules are ALWAYS ACTIVE for all HTTP request handlers that interact with external services, protocol handlers (HTTP, TCP, DNS) that require context propagation, and service boundary implementations that manipulate request/response headers.

### Rules

- **R-OUTBOUND-HTTP-001** MUST: Outbound HTTP requests to external services MUST set a User-Agent header identifying the service and version.

### Verify

```bash
# Discover the project's test execution mechanism and run protocol handler test suites
# to verify cache-layer context propagation
find . -name '*test*' -o -name '*_test.*' | head -5

# Locate static analysis or linting configuration and execute checks to identify
# missing User-Agent or Content-Type header operations in external client code
grep -r "User-Agent" --include="*.go" --include="*.java" --include="*.py" --include="*.ts" --include="*.js" .

# Identify integration test suites that exercise service boundaries and verify they
# validate header presence and correctness
grep -r "external.*client\|service.*boundary" --include="*test*" .
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios
- Static analysis checks confirm no external client instantiations lack required User-Agent or Content-Type headers
- Integration tests validate header propagation across all service boundaries

<enforcement>
Clause R-OUTBOUND-HTTP-001 verification is mandatory. Code review MUST reject handlers that instantiate external HTTP clients without User-Agent headers. Build MUST fail on static analysis detection of missing headers. Integration tests MUST validate header presence and correctness before merge.
</enforcement>