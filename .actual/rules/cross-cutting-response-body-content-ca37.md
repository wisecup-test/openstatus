# Standardize HTTP Response Body Handling in Integration Testing: Response Body Content

These rules are ALWAYS ACTIVE for all HTTP client wrappers, handler functions, job executors, and response assertion evaluators performing external health checks and monitoring requests in integration testing contexts.

### Rules

- **R-BODY-001** MUST: Response body content required for assertion evaluation must be read into memory buffers before processing to enable multiple reads and consistent error handling.
- **R-BODY-002** MUST: All HTTP response body readers must be closed via defer statements immediately after error checking to prevent resource leaks.
- **R-BODY-003** MUST: HTTP client instances must configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts.
- **R-BODY-004** SHOULD: Establish reusable body handling utilities that encapsulate the defer close pattern, buffer reading, and error propagation to reduce boilerplate across handler and job implementations.
- **R-BODY-005** SHOULD: Document the relationship between request body serialization, header manipulation, and response body parsing to ensure consistent data flow through assertion evaluation pipelines.

### Verify

```bash
# Discover the project's integration test suite location and execute the test runner with coverage reporting enabled
# to verify body handling patterns are exercised
find . -type f -name '*_test.go' -o -name '*_integration_test.go' | head -5

# Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
grep -r "bodyclose\|errcheck" . --include=".golangci.yml" --include=".golangci.yaml" --include="Makefile" 2>/dev/null || echo "(no linting config found)"

# Identify the project's HTTP client usage across the checker service and validate that all response body access points
# include explicit close operations
grep -r "response\.Body\|res\.Body\|req\.Body" . --include="*.go" | grep -v "defer" | head -10
```

**Accept when:**
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking.
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures.
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts.
- Static analysis tooling detects no unclosed HTTP response body readers in the codebase.
- Code review checklist enforces explicit body close patterns in all HTTP client usage.
- Integration test coverage reports validate that body handling paths are exercised.

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merges until body handling is corrected. Code review identifies missing defer close patterns and requests changes before approval. Runtime monitoring detects connection pool exhaustion or file descriptor leaks and triggers alerts for investigation.
</enforcement>