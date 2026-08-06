# Isolate External HTTP Client Construction in Boundary Layer: Request Preparation Logic

These rules are ALWAYS ACTIVE for all HTTP client construction, request preparation logic, header setting, body serialization, and timeout configuration across external monitoring targets, third-party API calls, ping and health check operations, and HTTP job execution paths.

### Rules

- **R-BOUNDARY-001** SHOULD: Request preparation logic including header setting and body serialization SHOULD be separated from client construction to enable independent testing of each concern.

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
Claude Code MUST NOT skip or defer verification. Violations are tracked as technical debt and pull requests containing inline HTTP client construction outside the boundary layer are blocked until refactored.
</enforcement>