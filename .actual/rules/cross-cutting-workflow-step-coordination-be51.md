# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Workflow Step Coordination

These rules are ALWAYS ACTIVE for all workflow monitoring implementations that require reliability guarantees through caching and rate limiting, particularly those managing multi-day execution lifecycles with distributed state coordination.

### Rules

- **R-WF-001** MUST: Workflow step coordination MUST use explicit step identifiers and state machines to manage multi-day execution lifecycles.
- **R-WF-002** MUST: All workflow state writes to distributed cache MUST include explicit expiration configuration.
- **R-WF-003** MUST: Rate limiting MUST be applied to workflow operations with documented token bucket parameters.
- **R-WF-004** MUST: Schema validation MUST be performed on all data retrieved from persistent storage before cache insertion.
- **R-WF-005** MUST: Cache expiration policies MUST be configured based on workflow execution windows, ensuring TTL exceeds maximum expected workflow duration with safety margin.
- **R-WF-006** MUST: Cache key naming conventions MUST encode entity hierarchy to enable efficient pattern-based invalidation during bulk operations.
- **R-WF-007** SHOULD: Implement cache warming strategies for predictable workflow schedules to prevent cold-start latency spikes.
- **R-WF-008** SHOULD: Monitor rate limiter rejection rates and adjust token bucket parameters based on observed traffic patterns and downstream service capacity.

### Verify

```bash
# Discover and execute the project test suite for integration tests covering cache tier coordination and rate limiting behavior
find . -name '*test*' -o -name '*spec*' | head -20

# Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
find . -name '.eslintrc*' -o -name 'eslint.config.*' -o -name 'pyproject.toml' -o -name '.flake8' -o -name 'tox.ini' | head -10

# Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures
find . -name '*monitoring*' -o -name '*metrics*' -o -name '*observability*' | grep -E '\.(yml|yaml|json|toml)$' | head -10

# Verify all workflow state writes include explicit expiration configuration
grep -r 'cache.*set\|cache.*put\|distributed.*cache' --include='*.ts' --include='*.js' --include='*.py' | grep -v 'expir\|ttl\|TTL' | wc -l

# Verify rate limiting is configured with documented parameters
grep -r 'rate.*limit\|token.*bucket\|RateLimit' --include='*.ts' --include='*.js' --include='*.py' | head -20

# Verify schema validation on cache retrieval
grep -r 'cache.*get\|retrieve.*cache' --include='*.ts' --include='*.js' --include='*.py' | grep -E 'validate|parse|schema' | head -20
```

**Accept when:**
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios
- Static analysis confirms cache expiration policies are explicitly set on all distributed cache writes
- Metrics collection is configured for cache hit rates, rate limit violations, and schema validation failures
- Workflow step coordination uses explicit step identifiers and state machines for multi-day execution windows

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for workflow monitoring implementations within scope. Integration tests, static analysis, and runtime monitoring must pass before code is accepted.
</enforcement>