# Standardize HTTP Client Configuration for External Service Boundaries: Http Clients Accept

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including error handling, request/response header manipulation, retry logic, and timeout configuration.

### Rules

- **R-HTTP-001** SHOULD: HTTP clients SHOULD accept context parameters to enable request cancellation and distributed tracing.
- **R-HTTP-002** MUST: All HTTP client instantiations for external service communication MUST include explicit timeout configuration.
- **R-HTTP-003** MUST: Error responses from external HTTP clients MUST be logged with structured context including error details.
- **R-HTTP-004** MUST: User-Agent headers MUST be set on outbound requests to identify the calling service.
- **R-HTTP-005** SHOULD: Retry logic with exponential backoff SHOULD be applied at external client boundaries to handle transient failures.
- **R-HTTP-006** SHOULD: Request and response headers SHOULD be manipulated to establish service identity and content negotiation contracts.

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
- Error responses from external HTTP clients are logged with structured context including error details
- User-Agent headers are set on outbound requests to identify the calling service
- HTTP clients accept context parameters for request cancellation and distributed tracing
- Retry logic with exponential backoff is implemented for transient failure handling

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiations at external service boundaries MUST comply with rules R-HTTP-001 through R-HTTP-006 before code is committed.
</enforcement>