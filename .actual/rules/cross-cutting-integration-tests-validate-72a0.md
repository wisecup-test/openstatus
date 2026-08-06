# Standardize HTTP Client Integration Testing with Body Inspection: Integration Tests Validate

These rules are ALWAYS ACTIVE for all HTTP client implementations that communicate with external services, integration test suites for service boundary interactions, request and response body serialization and deserialization logic, HTTP header management for cross-service communication, and timeout and retry configuration for external API calls.

### Rules

- **R-HTTPCI-001** SHOULD: Integration tests SHOULD validate both successful response bodies and error response bodies to ensure complete contract coverage.

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
Clause Code MUST NOT skip or defer verification. Code review verification that new HTTP client integrations include body inspection tests is mandatory. Continuous integration pipeline execution of integration test suites is mandatory. Static analysis to detect HTTP client instantiations without explicit timeout configuration is mandatory. Pull requests introducing HTTP client integrations without body inspection tests are blocked until tests are added. HTTP clients without explicit timeouts trigger build warnings that must be resolved before merge. Periodic audits identify service boundary code lacking integration test coverage for remediation.
</enforcement>