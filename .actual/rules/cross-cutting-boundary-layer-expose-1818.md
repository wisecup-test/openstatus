# Isolate External HTTP Client Construction in Boundary Layer: Boundary Layer Expose

These rules are ALWAYS ACTIVE for all HTTP client construction, timeout configuration, and external endpoint communication in handler logic, job execution paths, and monitoring operations.

### Rules

- **R-BOUNDARY-001** MUST: The boundary layer MUST expose an interface or factory function that returns HTTP client instances, enabling test doubles to be injected during integration testing.
- **R-BOUNDARY-002** MUST: All HTTP requests to external monitoring targets MUST use the boundary layer client factory.
- **R-BOUNDARY-003** MUST: All HTTP requests to third-party APIs MUST use the boundary layer client factory.
- **R-BOUNDARY-004** MUST: Ping and health check operations MUST use the boundary layer client factory.
- **R-BOUNDARY-005** MUST: HTTP job execution paths MUST use the boundary layer client factory.
- **R-BOUNDARY-006** MUST: The boundary layer interface MUST accept extensible configuration objects rather than fixed parameter lists to accommodate future transport options.
- **R-BOUNDARY-007** MUST: The boundary layer factory MUST return both the client instance and any associated cleanup functions to ensure proper resource management and connection pooling behavior.
- **R-BOUNDARY-008** SHOULD: Test doubles SHOULD replicate production client behavior for timeout enforcement by tracking elapsed time and returning timeout errors when thresholds are exceeded.
- **R-BOUNDARY-009** SHOULD: Security policies such as TLS settings and user-agent identification SHOULD be applied consistently through the boundary layer construction point.

### Verify

```bash
# Locate and execute the project's integration test suite
find . -name '*test*' -o -name '*spec*' | grep -E '\.(sh|py|js|go)$' | head -1 | xargs bash

# Discover and execute static analysis or linting configuration
if [ -f '.eslintrc' ] || [ -f 'pylintrc' ] || [ -f '.golangci.yml' ]; then
  echo "Static analysis configuration found"
fi

# Identify code coverage reporting mechanism
if [ -f 'coverage.xml' ] || [ -f '.coveragerc' ]; then
  echo "Code coverage configuration found"
fi

# Search for inline HTTP client construction outside boundary layer
grep -r 'http\.Client\|NewRequest\|Do(' --include='*.go' --include='*.js' --include='*.py' | grep -v 'boundary\|factory' || echo "No inline clients detected outside boundary layer"
```

**Accept when:**
- All integration tests pass with test doubles injected at the boundary layer, verifying timeout enforcement and header propagation without real network calls.
- Static analysis confirms no HTTP client construction occurs outside the designated boundary layer.
- Code coverage for boundary layer client construction and test double implementations meets or exceeds the project's defined threshold.
- Boundary layer interface accepts extensible configuration objects and returns both client instance and cleanup functions.
- All external HTTP calls (monitoring targets, third-party APIs, health checks, job execution) route through the boundary layer factory.

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and violations block deployment. Static analysis and integration tests MUST pass before accepting changes.
</enforcement>