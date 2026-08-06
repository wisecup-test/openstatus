# Isolate External HTTP Client Boundaries with Environment-Driven Configuration: Http Client Boundaries

These rules are ALWAYS ACTIVE for all HTTP client implementations that communicate with external services outside the deployment boundary, including configuration loading for service endpoints and authentication tokens, request construction, and error handling at external integration points.

### Rules

- **R-HTTP-001** MUST: HTTP client boundaries MUST implement structured error logging with context propagation for observability.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests that verify external client configuration from environment variables
# Discover the project's static analysis tooling and verify that environment variable usage is documented and validated at startup
# Discover the project's logging configuration and verify that HTTP client errors are captured with structured context
```

**Accept when:**
- All external HTTP clients successfully load configuration from environment variables and fail fast with clear errors when configuration is missing
- Integration tests demonstrate successful HTTP request construction with headers, query parameters, and authentication tokens
- Structured error logs are emitted for HTTP client failures with sufficient context for debugging

<enforcement>
Clause Code MUST NOT skip or defer verification. All external HTTP clients must follow environment-driven configuration patterns with structured error logging. Pull requests introducing external clients without these patterns are rejected during code review. Missing integration test coverage for HTTP boundaries blocks merge.
</enforcement>