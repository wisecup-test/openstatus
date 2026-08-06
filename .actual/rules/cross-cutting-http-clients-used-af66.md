# Standardize HTTP Client Configuration for External Service Boundaries: Http Clients Used

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including error handling, request/response header manipulation, retry logic, and timeout configuration across service integrations.

### Rules

- **R-HTTP-001** MUST: HTTP clients used for external service communication MUST configure explicit timeout values to prevent unbounded blocking operations.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit timeout configuration
# Discover and execute the project's test suite to validate that error logging occurs at external client boundaries with structured context
# Discover and execute the project's integration tests to verify retry behavior with exponential backoff under simulated transient failure conditions
```

**Accept when:**
- All HTTP client instantiations for external service communication include explicit timeout configuration
- Error responses from external HTTP clients are logged with structured context including error details
- User-Agent headers are set on outbound requests to identify the calling service

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis scanning, code review checklists, and integration test coverage are mandatory before accepting HTTP client implementations. Pull requests introducing HTTP clients without explicit timeouts MUST be blocked.
</enforcement>