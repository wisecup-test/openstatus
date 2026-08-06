# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Implementations Integrate Cloud

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, particularly those integrating cloud task queue services for asynchronous workflow execution.

### Rules

- **R-CACHE-001** MAY: Implementations MAY integrate with cloud task queue services for asynchronous workflow execution.
- **R-CACHE-002** MUST: All workflow state writes to distributed cache include explicit expiration configuration with TTL exceeding maximum expected workflow duration plus safety margin.
- **R-CACHE-003** MUST: Rate limiting is applied to workflow operations with documented token bucket parameters (baseline: 15 tokens per second interval).
- **R-CACHE-004** MUST: Schema validation is performed on all data retrieved from persistent storage before cache insertion.
- **R-CACHE-005** SHOULD: Implement cache key naming conventions that encode entity hierarchy to enable efficient pattern-based invalidation during bulk operations.
- **R-CACHE-006** SHOULD: Monitor rate limiter rejection rates and adjust token bucket parameters based on observed traffic patterns and downstream service capacity.
- **R-CACHE-007** SHOULD: Establish cache warming strategies for predictable workflow schedules to prevent cold-start latency spikes.
- **R-CACHE-008** SHOULD: Implement circuit breakers and fallback logic to degrade gracefully when distributed cache is unavailable.

### Verify

```bash
# Discover and execute the project test suite for integration tests covering cache tier coordination and rate limiting behavior
find . -name '*test*' -o -name '*spec*' | head -5

# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set
find . -name '.eslintrc*' -o -name 'pylintrc' -o -name '.flake8' -o -name 'tox.ini' | head -3

# Discover the project monitoring configuration and verify metrics collection
find . -name '*monitoring*' -o -name '*metrics*' -o -name '*observability*' | head -5

# Verify cache expiration policies are present in distributed cache writes
grep -r 'expir\|ttl\|TTL' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' | grep -i cache

# Verify rate limiting configuration is documented
grep -r 'rate.*limit\|token.*bucket\|rateLimit' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' --include='*.md'

# Verify schema validation on cache retrieval
grep -r 'validate\|schema\|parse' --include='*.js' --include='*.ts' --include='*.py' --include='*.go' | grep -i cache
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios
- Cache hit rates, rate limit violations, and schema validation failures are collected as metrics
- Circuit breaker and fallback logic is implemented for distributed cache unavailability

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules marked MUST are mandatory and must be verified before accepting implementation. Rules marked SHOULD represent best practices and should be verified where applicable to the project context.
</enforcement>