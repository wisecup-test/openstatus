# Standardize HTTP Response Body Access for Integration Testing: Http Clients Performing

These rules are ALWAYS ACTIVE for all HTTP client implementations, checker functions, handler logic, and integration test code that access request or response body fields across external service boundaries.

### Rules

- **R-HTTP-001** SHOULD: HTTP clients performing external service checks should implement timeout constraints using duration-based configuration.
- **R-HTTP-002** MUST: Establish Body.Close() defer patterns immediately after successful HTTP response acquisition to prevent resource leaks.
- **R-HTTP-003** SHOULD: Centralize User-Agent string construction to enable consistent service identification across all external HTTP calls.
- **R-HTTP-004** SHOULD: Pair all error logging with err.Error() output to maintain consistency with observed logging patterns in checker and handler implementations.
- **R-HTTP-005** SHOULD: Implement fallback logic for missing Content-Type headers and document expected header behavior in service contracts.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP response Body field accesses are paired with Close() calls
# Discover and run the project's integration test suite to validate User-Agent and Content-Type header handling
# Discover and execute the project's linting configuration to detect HTTP client timeout omissions
```

**Accept when:**
- All HTTP response body accesses in checker, handler, and job packages use the Body field consistently
- Static analysis confirms no resource leaks from unclosed Body readers
- Integration tests pass with User-Agent headers present in all external service requests
- Content-Type validation occurs before body processing in all handler implementations
- Timeout configuration is centralized in shared client factory with consistent values across all HTTP client instantiations

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client implementations MUST be audited for Body.Close() patterns, timeout configuration, and header handling before code review approval.
</enforcement>