# Standardize HTTP Client Configuration for External Service Boundaries: Response Bodies External

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including request/response handling, error logging, retry logic, timeout configuration, and context propagation across service boundaries.

### Rules

- **R-EX-001** SHOULD: Response bodies from external HTTP calls SHOULD be properly closed to prevent resource leaks.
- **R-EX-002** MUST: All HTTP client instantiations for external service communication MUST include explicit timeout configuration based on target service response time characteristics plus network latency buffer.
- **R-EX-003** MUST: Error responses from external HTTP clients MUST be logged with structured context including request identifiers, target service URLs, HTTP status codes, and error messages.
- **R-EX-004** MUST: User-Agent headers MUST be set on outbound requests to identify the calling service.
- **R-EX-005** SHOULD: Retry logic with exponential backoff SHOULD be implemented using libraries that support context-aware cancellation to prevent retry storms during graceful shutdown.
- **R-EX-006** SHOULD: Request and response headers SHOULD be manipulated to establish service identity and content negotiation contracts at external service boundaries.

### Verify

```bash
# Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit timeout configuration
# (Exact command to be discovered from project build configuration)

# Discover and execute the project's test suite to validate that error logging occurs at external client boundaries with structured context
# (Exact command to be discovered from project build configuration)

# Discover and execute the project's integration tests to verify retry behavior with exponential backoff under simulated transient failure conditions
# (Exact command to be discovered from project build configuration)
```

**Accept when:**
- All HTTP client instantiations for external service communication include explicit timeout configuration
- Response bodies from external HTTP calls are properly closed to prevent resource leaks
- Error responses from external HTTP clients are logged with structured context including error details
- User-Agent headers are set on outbound requests to identify the calling service
- Retry behavior with exponential backoff is validated under simulated transient failure conditions

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for external HTTP client implementations. Violations must be addressed before code review approval.
</enforcement>