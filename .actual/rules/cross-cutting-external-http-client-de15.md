# Standardize HTTP Client Configuration for External Service Boundaries: External Http Client

These rules are ALWAYS ACTIVE for all HTTP client instantiations at external service boundaries, including request/response header manipulation, error handling, retry logic, timeout configuration, and context propagation for distributed tracing.

### Rules

- **R-EX-001** SHOULD: External HTTP client operations SHOULD implement retry logic with exponential backoff for transient failure scenarios.
- **R-EX-002** MUST: All HTTP client instantiations for external service communication MUST include explicit timeout configuration based on target service response time characteristics plus network latency buffer.
- **R-EX-003** MUST: Error responses from external HTTP clients MUST be logged with structured context including request identifiers, target service URLs, HTTP status codes, and error messages.
- **R-EX-004** MUST: User-Agent headers MUST be set on outbound requests to identify the calling service.
- **R-EX-005** SHOULD: Retry logic with exponential backoff SHOULD support context-aware cancellation to prevent retry storms during graceful shutdown scenarios.
- **R-EX-006** SHOULD: Exponential backoff implementations SHOULD include maximum retry limits and jitter in backoff calculations to distribute retry load temporally.

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
- Retry logic with exponential backoff is implemented for transient failure scenarios
- Integration tests validate retry behavior under simulated transient failure conditions

<enforcement>
Claude Code MUST NOT skip or defer verification. All HTTP client instantiations at external service boundaries MUST comply with R-EX-002, R-EX-003, and R-EX-004. Violations block pull requests until configuration is added. Missing error logging triggers code review feedback. Integration test failures prevent production deployment. Exceptions require architectural review, documentation, and approval reference with expiration review date.
</enforcement>