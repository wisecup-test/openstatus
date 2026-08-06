# Standardize Public API Contract Definitions for HTTP Testing: Public Contracts Assertion

These rules are ALWAYS ACTIVE for HTTP checker handlers, job implementations, integration test code that validates external HTTP service responses, protocol buffer service definitions for private location communication, ping and health check endpoints that evaluate HTTP assertions, and client wrapper code that interacts with external HTTP APIs.

### Rules

- **R-PUB-001** MUST: Public API contracts for assertion evaluation MUST separate number-based and string-based comparators into distinct functions.

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
- Number-based and string-based comparator functions are implemented as separate, distinct functions

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations are blocked in pull requests until refactored to use standardized contracts. Existing violations are tracked in technical debt backlog. Exceptions require architecture review board approval and must be documented in handler code comments.
</enforcement>