# Isolate External HTTP Client Construction in Boundary Layer: Http Client Construction

These rules are ALWAYS ACTIVE for all HTTP client construction in handler and job execution paths, including health checks, monitoring operations, and third-party API calls to external endpoints.

### Rules

- **R-HTTP-001** MUST: HTTP client construction MUST accept timeout configuration as an explicit parameter and apply it to the client instance.

### Verify

```bash
# Locate the project's test execution script or build automation configuration and execute the integration test suite to verify HTTP client boundary layer behavior.
# Discover the project's static analysis or linting configuration and execute it to verify that HTTP client construction outside the boundary layer is flagged.
# Identify the project's code coverage reporting mechanism and verify that boundary layer client construction and test double implementations achieve the project's coverage threshold.
```

**Accept when:**
- All integration tests pass with test doubles injected at the boundary layer, verifying timeout enforcement and header propagation without real network calls.
- Static analysis confirms no HTTP client construction occurs outside the designated boundary layer.
- Code coverage for boundary layer client construction and test double implementations meets or exceeds the project's defined threshold.

<enforcement>
Claude Code MUST NOT skip or defer verification. Violations are blocked until refactored. Existing violations are tracked as technical debt and prioritized for refactoring. Integration test failures trigger build failures and prevent deployment.
</enforcement>