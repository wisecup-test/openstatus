# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Data Retrieved Persistent

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, particularly those with multi-day execution windows, cron-triggered orchestration, cloud task queue integration, or high-frequency state synchronization.

### Rules

- **R-CACHE-001** MUST: All data retrieved from persistent storage MUST be validated against defined schemas before insertion into cache layers.

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

<enforcement>
Claude Code MUST NOT skip or defer verification. All data retrieved from persistent storage MUST be validated against defined schemas before insertion into cache layers. Violations block pull requests and fail CI pipelines.
</enforcement>