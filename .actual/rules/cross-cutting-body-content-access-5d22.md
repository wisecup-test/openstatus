# Standardize HTTP Response Body Access for Integration Testing: Body Content Access

These rules are ALWAYS ACTIVE for all HTTP checker implementations, handler functions processing ping and monitoring requests, integration test code accessing request or response body fields, and client wrappers performing assertion evaluation on body content.

### Rules

- **R-BODY-001** SHOULD: Body content access should be paired with error logging that includes err.Error() output.
- **R-BODY-002** MUST: Establish Body.Close() defer patterns immediately after successful HTTP response acquisition to prevent resource leaks.
- **R-BODY-003** SHOULD: Centralize User-Agent string construction to enable consistent service identification across all external HTTP calls.
- **R-BODY-004** SHOULD: Pair all error logging with err.Error() output to maintain consistency with observed logging patterns in checker and handler implementations.

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
Clause Code MUST NOT skip or defer verification. Static analysis tools scanning for HTTP response Body field access patterns, code review checklist items for User-Agent and Content-Type header validation, and integration test suite execution validating body access and header handling are mandatory before acceptance.
</enforcement>