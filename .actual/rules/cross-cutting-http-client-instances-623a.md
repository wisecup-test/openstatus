# Standardize HTTP Response Body Handling in Integration Testing: Http Client Instances

These rules are ALWAYS ACTIVE for all HTTP client instances used in integration testing contexts, including HTTP client wrappers performing external health checks, handler functions processing incoming check requests, job executors running scheduled or triggered HTTP monitoring tasks, response assertion evaluators, and event forwarding logic sending response data to analytics systems.

### Rules

- **R-HTTP-001** MUST: HTTP client instances used in integration testing must configure explicit timeout values derived from monitor or request configuration to prevent indefinite blocking.

### Verify

```bash
# Discover the project's integration test suite location and execute the test runner with coverage reporting enabled
# to verify body handling patterns are exercised
find . -type f -name '*_test.*' -o -name 'test_*' | head -5

# Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
grep -r "defer.*Close\|defer.*close" --include="*.go" | grep -i "response\|body" | wc -l

# Identify the project's HTTP client usage across the checker service and validate that all response body access points
# include explicit close operations
grep -r "http\.Client\|http\.Get\|http\.Post" --include="*.go" | grep -v "defer" | head -10
```

**Accept when:**
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merges until body handling is corrected. Code review identifies missing defer close patterns and requests changes before approval.
</enforcement>