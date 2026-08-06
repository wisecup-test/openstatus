# Standardize HTTP Response Body Handling in Integration Testing: Integration Test Implementations

These rules are ALWAYS ACTIVE for all HTTP client wrappers, handler functions, job executors, response assertion evaluators, and event forwarding logic performing external health checks and monitoring requests in integration testing contexts.

### Rules

- **R-BODY-001** MUST: Close all HTTP response body readers via defer statements immediately after error checking to prevent resource leaks in long-running checker services.
- **R-BODY-002** MUST: Read entire response bodies into buffers before processing for assertions, logging, or event forwarding to ensure complete content capture.
- **R-BODY-003** MUST: Configure HTTP client timeouts derived from monitor or request configuration rather than using default or infinite timeouts.
- **R-BODY-004** SHOULD: Establish standard error handling patterns with explicit checks after body read operations and consistent logging of read failures.
- **R-BODY-005** SHOULD: Implement response size limits in HTTP client configuration and reject responses exceeding thresholds before full body read to mitigate memory pressure.
- **R-BODY-006** MAY: Cache request metadata in context or framework storage for retry coordination and event correlation in integration test implementations.

### Verify

```bash
# Discover the project's integration test suite location and execute the test runner with coverage reporting enabled
find . -type f -name '*_test.go' -o -name '*_integration_test.go' | head -5

# Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
grep -r "defer.*Close" --include="*.go" | grep -i "response\|body" | wc -l

# Identify the project's HTTP client usage across the checker service and validate response body access points
grep -r "response\.Body\|res\.Body\|req\.Body" --include="*.go" | grep -v "defer" | head -10

# Verify timeout configuration is not using defaults
grep -r "http\.Client\|http\.DefaultClient" --include="*.go" | grep -v "timeout\|Timeout" | head -5
```

**Accept when:**
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts
- Static analysis tooling detects no unclosed HTTP response body readers in the codebase
- Code review checklist confirms explicit body close patterns in all HTTP client usage
- Integration test coverage reports validate that body handling paths are exercised

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merges until body handling is corrected. Code review identifies missing defer close patterns and requests changes before approval. Runtime monitoring detects connection pool exhaustion or file descriptor leaks and triggers alerts for investigation.
</enforcement>