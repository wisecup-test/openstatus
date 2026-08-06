# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Handler Implementations Separate

These rules are ALWAYS ACTIVE for all handler implementations that interact with external services, protocol handlers (HTTP, TCP, DNS) that require context propagation, and service boundary implementations that manipulate request/response headers.

### Rules

- **R-HANDLER-001** SHOULD: Handler implementations SHOULD separate protocol-specific header manipulation from business logic by using the cache-layer abstraction for context access.

### Verify

```bash
# Discover the project's test execution mechanism and run protocol handler test suites
# to verify cache-layer context propagation
find . -name '*test*' -o -name '*_test.*' | head -5

# Locate static analysis or linting configuration and execute checks to identify
# missing User-Agent or Content-Type header operations in external client code
grep -r "User-Agent" --include="*.go" --include="*.py" --include="*.js" . 2>/dev/null | head -10
grep -r "Content-Type" --include="*.go" --include="*.py" --include="*.js" . 2>/dev/null | head -10

# Identify integration test suites that exercise service boundaries and verify
# they validate header presence and correctness
find . -path '*/test*' -name '*integration*' -o -path '*/test*' -name '*boundary*' 2>/dev/null
```

**Accept when:**
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios

<enforcement>
Code review verification that new handlers use cache-layer interface for context access is MANDATORY. Static analysis checks for missing User-Agent or Content-Type headers in external client instantiation are MANDATORY. Integration test execution validating header propagation across service boundaries is MANDATORY. Claude Code MUST NOT skip or defer verification.
</enforcement>