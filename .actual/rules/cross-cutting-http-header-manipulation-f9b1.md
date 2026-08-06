# Standardize HTTP Response Body Handling in Integration Testing: Http Header Manipulation

These rules are ALWAYS ACTIVE for all HTTP client wrappers, handler functions, job executors, response assertion evaluators, and event forwarding logic performing external health checks and monitoring requests in integration testing contexts.

### Rules

- **R-HTTP-001** SHOULD: HTTP header manipulation for integration testing should preserve Content-Type detection and allow custom header injection for user agent identification and authentication.

### Verify

```bash
# Discover the project's integration test suite location and execute the test runner with coverage reporting enabled to verify body handling patterns are exercised
# Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
# Identify the project's HTTP client usage across the checker service and validate that all response body access points include explicit close operations
```

**Accept when:**
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts
- Content-Type headers are preserved during response processing
- Custom headers for user agent identification and authentication are injected without corrupting existing header state

<enforcement>
Claude Code MUST NOT skip or defer verification. Static analysis failures block pull request merges until body handling is corrected. Code review identifies missing defer close patterns and requests changes before approval. Runtime monitoring detects connection pool exhaustion or file descriptor leaks and triggers alerts for investigation.
</enforcement>