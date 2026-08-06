# Standardize HTTP Response Body Access for Integration Testing: Http Client Implementations

These rules are ALWAYS ACTIVE for all HTTP client implementations, checker functions, handler logic, and integration test code that access request or response body fields across external service boundaries.

### Rules

- **R-HTTP-001** MUST: HTTP client implementations shall set User-Agent headers to identify the service making requests.
- **R-HTTP-002** MUST: Establish Body.Close() defer patterns immediately after successful HTTP response acquisition to prevent resource leaks.
- **R-HTTP-003** MUST: Pair all error logging with err.Error() output to maintain consistency with observed logging patterns in checker and handler implementations.
- **R-HTTP-004** SHOULD: Centralize User-Agent string construction to enable consistent service identification across all external HTTP calls.
- **R-HTTP-005** SHOULD: Implement fallback logic for missing Content-Type headers and document expected header behavior in service contracts.
- **R-HTTP-006** SHOULD: Centralize timeout configuration in shared client factory and audit existing timeout values for consistency.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP response Body field accesses are paired with Close() calls
# (Exact command to be discovered from project build configuration)

# Discover and run the project's integration test suite to validate User-Agent and Content-Type header handling
# (Exact command to be discovered from project test configuration)

# Discover and execute the project's linting configuration to detect HTTP client timeout omissions
# (Exact command to be discovered from project lint configuration)
```

**Accept when:**
- All HTTP response body accesses in checker, handler, and job packages use the Body field consistently
- Static analysis confirms no resource leaks from unclosed Body readers
- Integration tests pass with User-Agent headers present in all external service requests
- Content-Type validation occurs before body processing in all handler implementations
- All error logging includes err.Error() output
- Timeout configuration is centralized and consistent across HTTP client instantiations

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting code changes. Violations block pull requests until corrected.
</enforcement>