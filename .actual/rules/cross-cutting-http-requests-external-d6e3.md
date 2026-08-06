# Standardize HTTP Client Configuration for External Service Boundaries: Http Requests External

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including request header manipulation, error handling, retry logic, timeout configuration, and context propagation for distributed tracing.

### Rules

- **R-HTTP-EXT-001** MUST: HTTP requests to external services MUST set User-Agent headers to identify the calling service.
- **R-HTTP-EXT-002** MUST: HTTP clients for external service calls MUST configure explicit timeout values based on the expected response time characteristics of the target service plus a reasonable buffer for network latency.
- **R-HTTP-EXT-003** MUST: Error responses from external HTTP clients MUST be logged with structured context including request identifiers, target service URLs, HTTP status codes, and error messages.
- **R-HTTP-EXT-004** SHOULD: Implement retry logic using exponential backoff that supports context-aware cancellation to prevent retry storms during graceful shutdown scenarios.
- **R-HTTP-EXT-005** SHOULD: Implement maximum retry limits and consider jitter in backoff calculations to distribute retry load temporally and prevent amplification of load on downstream services.

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
- User-Agent headers are set on outbound requests to identify the calling service
- Error responses from external HTTP clients are logged with structured context including error details
- Retry behavior with exponential backoff is validated under simulated transient failure conditions
- Integration tests confirm timeout and retry behavior function as specified

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiations at external service boundaries MUST satisfy R-HTTP-EXT-001, R-HTTP-EXT-002, and R-HTTP-EXT-003 before code review approval. Violations block pull requests until remediated.
</enforcement>