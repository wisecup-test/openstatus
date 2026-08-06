# Standardize Public API Contract Definitions for HTTP Testing: Handler Implementations Expose

These rules are ALWAYS ACTIVE for all HTTP checker handlers, integration test code, protocol buffer service definitions, ping and health check endpoints, and client wrapper code that interacts with external HTTP APIs.

### Rules

- **R-HTTP-001** SHOULD: Handler implementations SHOULD expose IsSuccessful methods on response contracts to encapsulate success criteria.

### Verify

```bash
# Discover the project's test execution script and run integration tests for HTTP checker handlers to verify contract compliance
# Locate the project's static analysis configuration and execute type checking to confirm all handlers use standardized contract types
# Identify the project's assertion validation test suite and execute it to verify separation of number and string comparator functions
```

**Accept when:**
- All HTTP checker handlers use standardized contract types for request and response structures
- Integration tests pass with consistent assertion evaluation across all handler implementations
- Static analysis confirms no direct use of raw HTTP client types in handler validation logic

<enforcement>
Claude Code MUST NOT skip or defer verification. Continuous integration pipeline executes type checking and integration tests on every commit. Code review process verifies new handlers use standardized contract types. Automated static analysis flags direct use of raw HTTP client types in validation code.
</enforcement>