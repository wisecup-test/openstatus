# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Cache Keys Follow

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, including multi-day execution windows, cron-triggered orchestration, cloud task queue integration, and high-frequency state synchronization.

### Rules

- **R-CACHE-001** SHOULD: Cache keys SHOULD follow hierarchical naming conventions that encode entity type and identifier for efficient invalidation.

### Verify

```bash
# Discover the project test suite and execute integration tests covering cache tier coordination and rate limiting behavior
# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
# Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios
- Cache key naming conventions encode entity hierarchy to enable efficient pattern-based invalidation

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow state writes must include explicit cache expiration policies, and integration tests must pass before code is accepted.
</enforcement>