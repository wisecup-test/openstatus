# Standardize HTTP Response Body Handling in Integration Testing: Http Response Body

These rules are ALWAYS ACTIVE for all HTTP client wrappers, handler functions, job executors, response assertion evaluators, and event forwarding logic that perform external HTTP health checks and monitoring requests in integration testing contexts.

### Rules

- **R-HTTP-001** MUST: All HTTP response body readers obtained from external client calls must be explicitly closed using defer patterns immediately after error checking to prevent resource leaks.

### Verify

```bash
# Discover the project's integration test suite location and execute the test runner with coverage reporting enabled
# to verify body handling patterns are exercised
find . -type f -name '*_test.go' -o -name '*_integration_test.go' | head -5

# Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
grep -r "bodyclose\|errcheck" . --include=".golangci.yml" --include=".golangci.yaml" --include="Makefile" 2>/dev/null || echo "(no linting config found)"

# Identify the project's HTTP client usage across the checker service and validate that all response body access points
# include explicit close operations
grep -rn "response\.Body\|res\.Body\|req\.Body" . --include="*.go" | grep -v "defer" | head -10
```

**Accept when:**
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merges until body handling is corrected. Code review identifies missing defer close patterns and requests changes before approval.
</enforcement>