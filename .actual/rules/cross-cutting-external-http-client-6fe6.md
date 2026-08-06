# Isolate External HTTP Client Boundaries with Environment-Driven Configuration: External Http Client

These rules are ALWAYS ACTIVE for all external HTTP client implementations that communicate with services outside the deployment boundary.

### Rules

- **R-EX-HTTP-001** MUST: External HTTP client instances MUST be configured using environment variables for endpoint URLs and authentication credentials.
- **R-EX-HTTP-002** MUST: Constructor functions for external clients MUST accept HTTP client instances as parameters to enable injection of custom transports with timeout, TLS, and connection pooling configuration.
- **R-EX-HTTP-003** MUST: Environment variable validation MUST occur during client initialization with clear error messages indicating which variables are missing or malformed.
- **R-EX-HTTP-004** SHOULD: Structured logging at HTTP boundaries SHOULD include request identifiers, HTTP method, target URL, status code, and error details to support distributed tracing and debugging.
- **R-EX-HTTP-005** SHOULD: Explicit timeout configuration SHOULD be implemented on HTTP client instances and retry logic with exponential backoff SHOULD be added at call sites.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests
# that verify external client configuration from environment variables
echo "Running integration tests for external HTTP client configuration..."

# Discover the project's static analysis tooling and verify that environment
# variable usage is documented and validated at startup
echo "Verifying environment variable usage and validation..."

# Discover the project's logging configuration and verify that HTTP client
# errors are captured with structured context
echo "Verifying structured error logging at HTTP boundaries..."
```

**Accept when:**
- All external HTTP clients successfully load configuration from environment variables and fail fast with clear errors when configuration is missing
- Integration tests demonstrate successful HTTP request construction with headers, query parameters, and authentication tokens
- Structured error logs are emitted for HTTP client failures with sufficient context for debugging
- Constructor functions accept HTTP client instances as parameters for dependency injection
- Startup validation checks required environment variables are present and well-formed before accepting traffic

<enforcement>
Clause Code MUST NOT skip or defer verification. All external HTTP client implementations MUST comply with R-EX-HTTP-001 through R-EX-HTTP-005. Pull requests introducing external clients without environment-driven configuration are rejected during code review. Missing integration test coverage for HTTP boundaries blocks merge.
</enforcement>