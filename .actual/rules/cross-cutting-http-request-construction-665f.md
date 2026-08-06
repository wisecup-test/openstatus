# Isolate External HTTP Client Boundaries with Environment-Driven Configuration: Http Request Construction

These rules are ALWAYS ACTIVE for all HTTP client wrappers that communicate with external services outside the deployment boundary, including configuration loading for service endpoints and authentication tokens, request construction including headers and query parameters, and error handling at external integration points.

### Rules

- **R-HTTP-001** MUST: HTTP request construction MUST include query parameter encoding and header configuration before client execution.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests
# that verify external client configuration from environment variables
find . -name '*test*' -o -name '*_test.*' | head -5

# Discover the project's static analysis tooling and verify that environment
# variable usage is documented and validated at startup
grep -r 'os.Getenv\|os.LookupEnv' --include='*.go' | head -10

# Discover the project's logging configuration and verify that HTTP client
# errors are captured with structured context
grep -r 'log\|slog\|zap' --include='*.go' | grep -i 'http\|client' | head -10
```

**Accept when:**
- All external HTTP clients successfully load configuration from environment variables and fail fast with clear errors when configuration is missing
- Integration tests demonstrate successful HTTP request construction with headers, query parameters, and authentication tokens
- Structured error logs are emitted for HTTP client failures with sufficient context for debugging
- Constructor functions for external clients accept HTTP client instances as parameters to enable injection of custom transports
- Environment variable validation occurs during client initialization with clear error messages

<enforcement>
Clause R-HTTP-001 verification is mandatory. Code review must confirm that all external HTTP clients follow environment-driven configuration patterns. Integration test coverage for HTTP boundaries must be present. Pull requests introducing external clients without environment-driven configuration are rejected. Missing integration test coverage blocks merge.
</enforcement>