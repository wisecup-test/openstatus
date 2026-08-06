# Standardize HTTP Client Integration Testing with Body Inspection: Http Client Integrations

These rules are ALWAYS ACTIVE for all HTTP client implementations that cross service boundaries, including integration test suites for service boundary interactions, request and response body serialization and deserialization logic, HTTP header management for cross-service communication, and timeout and retry configuration for external API calls.

### Rules

- **R-HTTP-001** MUST: All HTTP client integrations that cross service boundaries MUST implement integration tests that inspect response body content.
- **R-HTTP-002** MUST: HTTP client configurations MUST specify explicit timeout values and User-Agent headers.
- **R-HTTP-003** MUST: Integration tests MUST validate both successful and error response bodies against expected contracts.
- **R-HTTP-004** SHOULD: Implement test helpers that abstract common body inspection patterns to reduce duplication across integration test suites.
- **R-HTTP-005** SHOULD: Design assertions to validate required fields and semantic correctness rather than exact JSON structure or field ordering.
- **R-HTTP-006** SHOULD: Use test doubles or mock servers for integration tests, reserving real network calls for smoke tests or contract verification suites.
- **R-HTTP-007** SHOULD: When implementing retry logic, ensure integration tests validate that retry attempts respect context cancellation and do not exceed configured maximum attempts.

### Verify

```bash
# Discover the project's test execution mechanism and run the integration test suite
find . -name '*test*' -o -name '*_test.*' | head -5

# Inspect integration test implementations for response body inspection
grep -r 'response\.Body\|res\.Body\|data\.Body' --include='*_test.*' --include='*test*.go' .

# Review HTTP client configurations for explicit timeout values
grep -r 'Timeout\|timeout' --include='*.go' . | grep -i 'client\|http'

# Verify User-Agent headers are set in HTTP clients
grep -r 'User-Agent' --include='*.go' .

# Confirm integration test suite executes successfully
# (Run the project's standard test command)
```

**Accept when:**
- All integration tests for HTTP client service boundaries include assertions on response body content
- HTTP client configurations specify explicit timeout values and User-Agent headers
- Integration test suite executes successfully and validates both success and error response scenarios
- Body inspection tests assert on required fields and semantic correctness rather than exact payload structure
- Test helpers abstract common body inspection patterns to reduce duplication

<enforcement>
Clause Code MUST NOT skip or defer verification. Pull requests introducing HTTP client integrations without body inspection tests are blocked until tests are added. HTTP clients without explicit timeouts trigger build warnings that must be resolved before merge. Periodic audits identify service boundary code lacking integration test coverage for remediation.
</enforcement>