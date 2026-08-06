# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Custom Headers Propagated

These rules are ALWAYS ACTIVE for HTTP request handlers that interact with external services, protocol handlers (HTTP, TCP, DNS) that require context propagation, service boundary implementations that manipulate request/response headers, and cache-layer abstractions used for request metadata access.

### Rules

- **R-HEADER-001** MUST: Custom headers MUST be propagated through the request chain by reading from and writing to the header collection using key-value access patterns.

### Verify

```bash
# Discover the project's test execution mechanism and run protocol handler test suites
# to verify cache-layer context propagation
find . -name '*test*' -o -name '*_test.*' | head -5

# Locate static analysis or linting configuration and execute checks to identify
# missing User-Agent or Content-Type header operations in external client code
grep -r "User-Agent" --include="*.go" --include="*.java" --include="*.ts" --include="*.py" .
grep -r "Content-Type" --include="*.go" --include="*.java" --include="*.ts" --include="*.py" .

# Identify integration test suites that exercise service boundaries and verify
# they validate header presence and correctness
find . -path '*/integration/*' -o -path '*/e2e/*' | grep -i test
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios
- Code review verification confirms new handlers use cache-layer interface for context access
- Static analysis checks detect no missing User-Agent or Content-Type headers in external client instantiation
- Integration test execution validates header propagation across service boundaries

<enforcement>
Clause Code MUST NOT skip or defer verification. Violations are handled by code review rejection for handlers that bypass cache-layer abstraction without documented justification, build failure on static analysis detection of external requests missing required headers, and test failure on integration tests detecting missing or incorrect context propagation. Exception process requires documentation of protocol-specific requirements, architectural review approval, and exception annotation in code with reference to approval.
</enforcement>