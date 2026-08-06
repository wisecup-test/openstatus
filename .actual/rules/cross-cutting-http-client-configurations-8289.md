# Standardize HTTP Client Integration Testing with Body Inspection: Http Client Configurations

These rules are ALWAYS ACTIVE for all HTTP client implementations that communicate with external services, integration test suites for service boundary interactions, request and response body serialization and deserialization logic, HTTP header management for cross-service communication, and timeout and retry configuration for external API calls.

### Rules

- **R-HTTP-001** MUST: HTTP client configurations MUST specify explicit timeout values to prevent unbounded blocking at service boundaries.

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
Clause Code MUST NOT skip or defer verification. Pull requests introducing HTTP client integrations without body inspection tests are blocked until tests are added. HTTP clients without explicit timeouts trigger build warnings that must be resolved before merge.
</enforcement>