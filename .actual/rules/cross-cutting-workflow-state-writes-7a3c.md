# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Workflow State Writes

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, particularly those with multi-day execution windows, cron-triggered orchestration, cloud task queue integration, or high-frequency state synchronization.

### Rules

- **R-WF-CACHE-001** MUST: All workflow state writes to distributed cache MUST include explicit expiration policies measured in seconds to prevent unbounded growth.

### Verify

```bash
# Discover the project test suite and execute integration tests covering cache tier coordination and rate limiting behavior
find . -type f -name '*test*' -o -name '*spec*' | head -20

# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name '.pylintrc' -o -name 'pyproject.toml' -o -name '.flake8' \) | head -10

# Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures
find . -type f \( -name 'prometheus.yml' -o -name 'datadog.yml' -o -name 'monitoring.yml' -o -name 'observability.yml' \) | head -10
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow state writes must be audited for explicit TTL/expiration policies before code is committed. Static analysis and integration tests are mandatory gates in the CI pipeline.
</enforcement>