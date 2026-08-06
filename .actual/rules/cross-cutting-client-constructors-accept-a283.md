# Isolate External HTTP Client Boundaries with Environment-Driven Configuration: Client Constructors Accept

These rules are ALWAYS ACTIVE for all HTTP client wrapper implementations that communicate with external services outside the deployment boundary, including configuration loading for service endpoints and authentication tokens, request construction, and error handling at external integration points.

### Rules

- **R-CLIENT-001** MAY: Client constructors MAY accept pre-configured HTTP client instances to support custom transport, timeout, and retry policies.
- **R-CLIENT-002** MUST: Constructor functions for external clients should accept HTTP client instances as parameters to enable injection of custom transports with timeout, TLS, and connection pooling configuration.
- **R-CLIENT-003** MUST: Environment variable validation should occur during client initialization with clear error messages indicating which variables are missing or malformed.
- **R-CLIENT-004** MUST: Structured logging at HTTP boundaries should include request identifiers, HTTP method, target URL, status code, and error details to support distributed tracing and debugging.
- **R-CLIENT-005** MUST: All external HTTP clients must load configuration from environment variables and fail fast with clear errors when configuration is missing.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests
# that verify external client configuration from environment variables
find . -name '*test*' -o -name '*_test.*' | head -5

# Discover the project's static analysis tooling and verify that environment
# variable usage is documented and validated at startup
grep -r "env\." --include="*.go" --include="*.py" --include="*.js" | grep -i "getenv\|environ" | head -10

# Discover the project's logging configuration and verify that HTTP client
# errors are captured with structured context
grep -r "log\|logger" --include="*.go" --include="*.py" --include="*.js" | grep -i "http\|client\|request" | head -10
```

**Accept when:**
- All external HTTP clients successfully load configuration from environment variables and fail fast with clear errors when configuration is missing
- Integration tests demonstrate successful HTTP request construction with headers, query parameters, and authentication tokens
- Structured error logs are emitted for HTTP client failures with sufficient context for debugging
- Constructor functions accept pre-configured HTTP client instances as parameters
- Environment variable validation occurs during client initialization with descriptive error messages

<enforcement>
Claude Code MUST NOT skip or defer verification. All external HTTP client implementations MUST comply with these rules before merge. Code review verification, integration test coverage, and static analysis checks are mandatory.
</enforcement>