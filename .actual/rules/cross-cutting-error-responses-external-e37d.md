# Standardize HTTP Client Configuration for External Service Boundaries: Error Responses External

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including error handling, request/response header manipulation, retry logic, timeout configuration, and context propagation for distributed tracing.

### Rules

- **R-EX-001** MUST: Error responses from external HTTP clients MUST be logged using structured logging methods that capture error context.

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
Claude Code MUST NOT skip or defer verification. All HTTP client error responses at external service boundaries MUST include structured logging with error context before code is accepted.
</enforcement>