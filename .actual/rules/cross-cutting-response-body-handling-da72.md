# Isolate External HTTP Client Boundaries with Environment-Driven Configuration: Response Body Handling

These rules are ALWAYS ACTIVE for all HTTP client implementations that communicate with external services outside the deployment boundary, including configuration loading for service endpoints, request construction, error handling, and structured logging at external integration points.

### Rules

- **R-HTTP-001** SHOULD: Response body handling SHOULD use streaming interfaces to support efficient processing of large payloads.

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
- Response body handling implementations use streaming interfaces where applicable to support large payloads

<enforcement>
Clause Code MUST NOT skip or defer verification. All external HTTP client implementations MUST be reviewed for compliance with environment-driven configuration patterns and streaming response body handling before merge.
</enforcement>