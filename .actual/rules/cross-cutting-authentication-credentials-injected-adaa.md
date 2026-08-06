# Isolate External HTTP Client Boundaries with Environment-Driven Configuration: Authentication Credentials Injected

These rules are ALWAYS ACTIVE for all HTTP client wrappers that communicate with external services outside the deployment boundary, configuration loading for service endpoints and authentication tokens, request construction including headers and query parameters, and error handling at external integration points.

### Rules

- **R-AUTH-001** MUST: Authentication credentials MUST be injected via request headers using bearer token patterns when communicating with authenticated external services.

### Verify

```bash
# Discover the project's test execution mechanism and run integration tests
# that verify external client configuration from environment variables
find . -name '*test*' -o -name '*_test.*' | head -5

# Discover the project's static analysis tooling and verify that environment
# variable usage is documented and validated at startup
grep -r "environment" . --include="*.go" --include="*.md" | grep -i "validation\|startup" | head -10

# Discover the project's logging configuration and verify that HTTP client
# errors are captured with structured context
grep -r "log\|error" . --include="*.go" | grep -i "http\|client" | head -10
```

**Accept when:**
- All external HTTP clients successfully load configuration from environment variables and fail fast with clear errors when configuration is missing
- Integration tests demonstrate successful HTTP request construction with headers, query parameters, and authentication tokens
- Structured error logs are emitted for HTTP client failures with sufficient context for debugging
- Bearer token authentication is consistently applied via request headers across all authenticated external service integrations
- Environment variable validation occurs during client initialization with clear error messages

<enforcement>
Claude Code MUST NOT skip or defer verification. All external HTTP clients communicating with authenticated services MUST inject credentials via bearer token headers. Violations block merge.
</enforcement>