# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Rate Limiting Applied

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, including multi-day execution windows, cron-triggered orchestration, cloud task queue integration, and high-frequency state synchronization.

### Rules

- **R-CACHE-001** MUST: Rate limiting MUST be applied to workflow operations with token bucket configuration specifying tokens per interval and interval duration.
- **R-CACHE-002** MUST: All workflow state writes to distributed cache MUST include explicit expiration configuration.
- **R-CACHE-003** MUST: Schema validation MUST be performed on all data retrieved from persistent storage before cache insertion.
- **R-CACHE-004** SHOULD: Configure cache expiration policies based on workflow execution windows, ensuring TTL exceeds maximum expected workflow duration with safety margin.
- **R-CACHE-005** SHOULD: Implement cache key naming conventions that encode entity hierarchy to enable efficient pattern-based invalidation during bulk operations.
- **R-CACHE-006** SHOULD: Monitor rate limiter rejection rates and adjust token bucket parameters based on observed traffic patterns and downstream service capacity.
- **R-CACHE-007** SHOULD: Establish cache warming strategies for predictable workflow schedules to prevent cold-start latency spikes.

### Verify

```bash
# Discover the project test suite and execute integration tests covering cache tier coordination and rate limiting behavior
# (Exact command derived from project repository)

# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
# (Exact command derived from project repository)

# Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures
# (Exact command derived from project repository)
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before code acceptance. Static analysis and integration tests must pass in the CI pipeline.
</enforcement>