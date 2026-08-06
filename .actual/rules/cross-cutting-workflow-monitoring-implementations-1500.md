# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Workflow Monitoring Implementations

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, including multi-day execution windows, cron-triggered orchestration, cloud task queue integration, and high-frequency state synchronization.

### Rules

- **R-WFM-001** MUST: Workflow monitoring implementations MUST use a two-tier caching architecture with in-memory cache for hot data and distributed cache for persistent state.
- **R-WFM-002** MUST: All workflow state writes to distributed cache MUST include explicit expiration configuration with TTL exceeding maximum expected workflow duration plus safety margin.
- **R-WFM-003** MUST: Rate limiting MUST be applied to workflow operations with documented token bucket parameters (baseline: 15 tokens per second interval).
- **R-WFM-004** MUST: Schema validation MUST be performed on all data retrieved from persistent storage before cache insertion to ensure type safety at cache boundaries.
- **R-WFM-005** MUST: Cache key naming conventions MUST encode entity hierarchy to enable efficient pattern-based invalidation during bulk operations.
- **R-WFM-006** SHOULD: Implement circuit breakers and fallback logic to degrade gracefully when distributed cache is unavailable to prevent cache stampede during failures.
- **R-WFM-007** SHOULD: Monitor rate limiter rejection rates and adjust token bucket parameters based on observed traffic patterns and downstream service capacity.
- **R-WFM-008** SHOULD: Establish cache warming strategies for predictable workflow schedules to prevent cold-start latency spikes.

### Verify

```bash
# Discover and execute the project test suite for integration tests covering cache tier coordination and rate limiting behavior
find . -name '*test*' -o -name '*spec*' | head -5

# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'tox.ini' | head -5

# Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures
find . -name 'prometheus.yml' -o -name 'datadog.yml' -o -name 'monitoring.yaml' | head -5
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios
- Cache key naming conventions encode entity hierarchy for pattern-based invalidation
- Circuit breaker and fallback logic is implemented for distributed cache unavailability
- Cache hit rates, rate limit violations, and schema validation failures are monitored and alerted

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules R-WFM-001 through R-WFM-008 are mandatory for workflow monitoring implementations in scope. CI pipeline MUST fail if integration tests for cache and rate limiting do not pass. Pull requests MUST be blocked if static analysis detects missing cache expiration policies. Production alerts MUST trigger incident response for cache coherence or rate limit violations.
</enforcement>