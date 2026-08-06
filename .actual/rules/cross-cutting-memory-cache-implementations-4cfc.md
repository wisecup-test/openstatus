# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Memory Cache Implementations

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, including multi-day execution windows, cron-triggered orchestration, cloud task queue integration, high-frequency state synchronization, and rate-limited API operations.

### Rules

- **R-CACHE-001** SHOULD: In-memory cache implementations SHOULD use native collection types with set operations for constant-time lookups.

### Verify

```bash
# Discover the project test suite and execute integration tests covering cache tier coordination and rate limiting behavior
find . -type f -name '*test*' -o -name '*spec*' | head -20

# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
find . -type f \( -name '.eslintrc*' -o -name 'eslint.config.*' -o -name '.pylintrc' -o -name 'pyproject.toml' -o -name '.flake8' \) | head -10

# Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures
find . -type f \( -name '*monitoring*' -o -name '*metrics*' -o -name '*observability*' \) | head -10
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios
- In-memory cache implementations use native collection types with set operations for O(1) lookups
- Cache key naming conventions encode entity hierarchy to enable efficient pattern-based invalidation
- Rate limiter rejection rates are monitored and token bucket parameters are adjusted based on observed traffic patterns

<enforcement>
Claude Code MUST NOT skip or defer verification. All workflow state writes must include explicit cache expiration policies. Rate limiting configuration must be documented with token bucket parameters. Schema validation must be confirmed on all persistent storage reads before cache insertion. Integration tests must pass before code is accepted.
</enforcement>