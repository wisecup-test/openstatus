# Standardize HTTP Client Configuration for External Service Boundaries: Http Clients Configure

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including error handling, request/response header manipulation, retry logic, and timeout configuration.

### Rules

- **R-HTTP-001** MUST: Configure explicit timeout values on all HTTP clients instantiated for external service communication based on target service response time characteristics plus network latency buffer.
- **R-HTTP-002** MUST: Implement structured error logging at external HTTP client boundaries including request identifiers, target service URLs, HTTP status codes, and error messages.
- **R-HTTP-003** MUST: Set User-Agent headers on outbound HTTP requests to identify the calling service.
- **R-HTTP-004** SHOULD: Implement retry logic using exponential backoff for transient failures at external client boundaries with context-aware cancellation support.
- **R-HTTP-005** SHOULD: Implement maximum retry limits and jitter in backoff calculations to prevent retry amplification during incidents.
- **R-HTTP-006** MAY: Configure custom TLS settings when communicating with external services requiring specific security parameters.

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
- Retry logic with exponential backoff is implemented for transient failures
- Maximum retry limits and jitter are applied to prevent retry storms

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiations must be reviewed against R-HTTP-001 through R-HTTP-006. Static analysis, test suite execution, and integration test validation are mandatory before code acceptance.
</enforcement>