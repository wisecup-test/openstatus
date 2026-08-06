# Standardize HTTP Client Integration Testing with Body Inspection: Http Client Implementations

These rules are ALWAYS ACTIVE for all HTTP client implementations that communicate with external services, including integration test suites for service boundary interactions, request and response body serialization and deserialization logic, HTTP header management for cross-service communication, and timeout and retry configuration for external API calls.

### Rules

- **R-HTTP-001** MUST: HTTP client implementations MUST set explicit User-Agent headers to identify the calling service.
- **R-HTTP-002** MUST: All integration tests for HTTP client service boundaries MUST include assertions on response body content.
- **R-HTTP-003** MUST: HTTP client configurations MUST specify explicit timeout values.
- **R-HTTP-004** SHOULD: Integration tests SHOULD create HTTP clients with production-equivalent timeout and header configurations, then validate both successful and error response bodies against expected contracts.
- **R-HTTP-005** SHOULD: Test helpers SHOULD abstract common body inspection patterns to reduce duplication across integration test suites while maintaining consistency.
- **R-HTTP-006** SHOULD: When implementing retry logic, integration tests SHOULD validate that retry attempts respect context cancellation and do not exceed configured maximum attempts.

### Verify

```bash
# Discover the project's test execution mechanism and run the integration test suite for service boundary interactions
# Inspect integration test implementations to confirm response body inspection is present for all external HTTP client calls
# Review HTTP client configurations to verify explicit timeout values and User-Agent headers are set
```

**Accept when:**
- All integration tests for HTTP client service boundaries include assertions on response body content
- HTTP client configurations specify explicit timeout values and User-Agent headers
- Integration test suite executes successfully and validates both success and error response scenarios

<enforcement>
Clause Code MUST NOT skip or defer verification. Pull requests introducing HTTP client integrations without body inspection tests are blocked until tests are added. HTTP clients without explicit timeouts trigger build warnings that must be resolved before merge. Periodic audits identify service boundary code lacking integration test coverage for remediation. Exceptions for body inspection requirements may be granted for health check endpoints that return no meaningful payload, but exception requests must document the rationale and obtain approval from a technical lead.
</enforcement>