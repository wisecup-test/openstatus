# Standardize HTTP Response Body Handling in Integration Testing: Request Response Body

These rules are ALWAYS ACTIVE for all HTTP client wrappers, handler functions, job executors, and response assertion evaluators performing external health checks and monitoring requests in integration testing contexts.

### Rules

- **R-BODY-001** SHOULD: Request and response body data structures used in integration testing should be serializable to JSON for logging, event forwarding, and assertion evaluation.

### Verify

```bash
# Discover the project's integration test suite location and execute the test runner with coverage reporting enabled
find . -type f -name '*_test.go' -o -name '*_integration_test.go' | head -5

# Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
grep -r "defer.*Close" --include="*.go" | grep -i "response\|body" | wc -l

# Identify the project's HTTP client usage across the checker service
grep -r "http\.Client\|http\.Get\|http\.Post\|response\.Body" --include="*.go" | grep -v test | head -10

# Validate that all response body access points include explicit close operations
grep -r "response\.Body" --include="*.go" -A 3 | grep -c "defer.*Close"
```

**Accept when:**
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts
- Response body data structures are serializable to JSON and used consistently across logging, event forwarding, and assertion evaluation paths

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merges until body handling is corrected. Code review must identify missing defer close patterns and request changes before approval.
</enforcement>