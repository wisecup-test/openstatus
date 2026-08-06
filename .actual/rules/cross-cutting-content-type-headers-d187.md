# Standardize HTTP Response Body Access for Integration Testing: Content Type Headers

These rules are ALWAYS ACTIVE for all HTTP checker implementations, handler functions processing ping and monitoring requests, integration test code accessing request or response body fields, and client wrappers performing assertion evaluation on body content.

### Rules

- **R-CTYPES-001** MUST: Content-Type headers shall be validated via Header.Get before processing request or response bodies.
- **R-CTYPES-002** MUST: Establish Body.Close() defer patterns immediately after successful HTTP response acquisition to prevent resource leaks.
- **R-CTYPES-003** MUST: Pair all error logging with err.Error() output to maintain consistency with observed logging patterns in checker and handler implementations.
- **R-CTYPES-004** SHOULD: Centralize User-Agent string construction to enable consistent service identification across all external HTTP calls.

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

<enforcement>
Clause Code MUST NOT skip or defer verification. Pull requests introducing inconsistent body access patterns are blocked until corrected. Missing User-Agent or Content-Type validation triggers code review feedback. Resource leak detection from unclosed Body readers fails continuous integration checks.
</enforcement>