# Isolate External HTTP Client Construction in Boundary Layer: Boundary Layer Provide

These rules are ALWAYS ACTIVE for all HTTP client construction, timeout configuration, and external endpoint communication across handlers, job execution paths, and monitoring operations.

### Rules

- **R-BOUNDARY-001** MUST: Isolate all HTTP client construction for external monitoring targets, third-party APIs, ping operations, and health checks within a dedicated boundary layer package.
- **R-BOUNDARY-002** MUST: Ensure HTTP client instances returned from the boundary layer include associated cleanup functions to guarantee proper resource management and connection pooling behavior.
- **R-BOUNDARY-003** MUST: Apply security policies such as TLS configuration and user-agent identification consistently across all external HTTP calls through the boundary layer construction point.
- **R-BOUNDARY-004** SHOULD: Provide pre-configured client instances for common timeout patterns within the boundary layer to reduce duplication across handlers.
- **R-BOUNDARY-005** MUST: Implement test doubles that replicate production client behavior for timeout enforcement by tracking elapsed time and returning timeout errors when thresholds are exceeded.
- **R-BOUNDARY-006** MUST: Design the boundary layer interface to accept extensible configuration objects rather than fixed parameter lists to accommodate future transport-level requirements.
- **R-BOUNDARY-007** MUST: Prevent inline HTTP client construction outside the designated boundary layer through static analysis or linting rules.
- **R-BOUNDARY-008** MUST: Coordinate exponential backoff retry logic with HTTP client timeout configuration to ensure retry logic does not exceed client-level deadlines.

### Verify

```bash
# Locate and execute the project's integration test suite
# Verify HTTP client boundary layer behavior with test doubles injected
find . -name '*test*' -o -name '*spec*' | grep -E '\.(sh|py|js|go)$' | head -1 | xargs cat

# Discover the project's static analysis or linting configuration
# Verify that HTTP client construction outside the boundary layer is flagged
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.golangci.yml' -o -name 'ruleset.xml' | head -1 | xargs cat

# Identify the project's code coverage reporting mechanism
# Verify boundary layer client construction and test double implementations meet coverage threshold
find . -name 'coverage*' -o -name '.coveragerc' -o -name 'codecov.yml' | head -1 | xargs cat
```

**Accept when:**
- All integration tests pass with test doubles injected at the boundary layer, verifying timeout enforcement and header propagation without real network calls.
- Static analysis confirms no HTTP client construction occurs outside the designated boundary layer.
- Code coverage for boundary layer client construction and test double implementations meets or exceeds the project's defined threshold.
- Characterization tests capture current timeout and header handling behavior before refactoring and verify equivalent behavior after boundary layer adoption.
- Contract tests verify test double behavior matches production client behavior for timeout enforcement, header propagation, and error handling.

<enforcement>
Claude Code MUST NOT skip or defer verification. All integration tests with boundary layer test doubles MUST pass. Static analysis MUST confirm no inline HTTP client construction outside the boundary layer. Code coverage MUST meet the project's threshold. Pull requests containing inline HTTP client construction outside the boundary layer MUST be blocked until refactored.
</enforcement>