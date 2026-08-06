# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Request Identifiers Event

These rules are ALWAYS ACTIVE for all HTTP request handlers, protocol handlers (HTTP, TCP, DNS), and service boundary implementations that manipulate request/response headers or propagate context across handler chains.

### Rules

- **R-REQID-001** SHOULD: Request identifiers and event tracking data SHOULD be injected into the cache layer early in the request lifecycle for downstream handler access.

### Verify

```bash
# Discover the project's test execution mechanism and run protocol handler test suites
# to verify cache-layer context propagation
find . -name '*test*' -o -name '*_test.*' | head -5

# Locate static analysis or linting configuration and execute checks to identify
# missing User-Agent or Content-Type header operations in external client code
grep -r "User-Agent" --include="*.go" --include="*.java" --include="*.ts" --include="*.js" .
grep -r "Content-Type" --include="*.go" --include="*.java" --include="*.ts" --include="*.js" .

# Identify integration test suites that exercise service boundaries and verify they
# validate header presence and correctness
find . -path '*/integration/*' -o -path '*/e2e/*' | grep -i test
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios

<enforcement>
Code review verification that new handlers use cache-layer interface for context access is mandatory. Static analysis checks for missing User-Agent or Content-Type headers in external client instantiation must pass. Integration test execution validating header propagation across service boundaries must succeed. Claude Code MUST NOT skip or defer verification.
</enforcement>