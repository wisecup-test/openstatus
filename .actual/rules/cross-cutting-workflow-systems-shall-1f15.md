# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: Workflow Systems Shall

These rules are ALWAYS ACTIVE for all workflow monitoring systems that execute multi-step processes over extended time periods (3-14 days) and require user state management in distributed environments.

### Rules

- **R-CACHE-001** MUST: Workflow systems SHALL implement a dual-layer cache architecture combining in-memory storage for process-local access and distributed cache storage for cross-process persistence.
- **R-CACHE-002** MUST: Initialize in-memory map at workflow entry point before database queries. Populate map during initial user data fetch and reuse for all subsequent lookups within the execution context.
- **R-CACHE-003** MUST: Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations during debugging or maintenance.
- **R-CACHE-004** MUST: Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants to maintain consistency between workflow timing and cache lifecycle.
- **R-CACHE-005** MUST: Configure distributed cache TTL to exceed maximum workflow duration by safety margin (e.g., 30 days for 14-day workflows).
- **R-CACHE-006** SHOULD: Implement cache size limits with LRU eviction policy for in-memory layer to prevent unbounded memory growth in high-user-count scenarios.
- **R-CACHE-007** SHOULD: Implement circuit breaker pattern with fallback to database-only mode for distributed cache unavailability scenarios.
- **R-CACHE-008** SHOULD: Monitor process memory usage and set alerts at 80% threshold for in-memory cache consumption.
- **R-CACHE-009** SHOULD: Add distributed cache health checks to deployment readiness probes and configure cache client with connection timeouts and retry policies.

### Verify

```bash
# Locate the project's workflow monitoring module and identify the cache initialization logic
grep -r "usersMap\|cache.*init" --include="*.js" --include="*.ts" src/

# Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins
grep -B5 -A5 "LaunchMonitorWorkflow\|workflow.*start" src/ | grep -E "usersMap\.set|redis\.set"

# Inspect distributed cache write operations and extract the configured expiration values
grep -r "redis\.set\|cache.*expire\|TTL" --include="*.js" --include="*.ts" src/ | grep -E "30.*day|86400|expir"

# Confirm that TTL values equal or exceed the maximum workflow step duration
grep -r "Step3Days\|Step14Days\|StepPaused" --include="*.js" --include="*.ts" src/ | head -20

# Examine the project's test suite for cache layer integration tests
find test/ -name "*cache*" -o -name "*workflow*" | xargs grep -l "cache.*miss\|expir\|cross.*instance"

# Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination
npm test -- --grep "cache|expir|distributed"
```

**Accept when:**
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state
- Code review verification confirms workflow initialization establishes both cache layers before state access
- Integration tests validate cache hit rates and distributed state persistence across process boundaries
- Performance monitoring tracks cache access latency and memory consumption metrics within acceptable thresholds

<enforcement>
Claude Code MUST NOT skip or defer verification. All R-CACHE rules marked MUST are non-negotiable for workflow systems in scope. Violations detected during code review or integration testing MUST be remediated before merge. Cache miss rates exceeding configured thresholds MUST trigger production monitoring alerts.
</enforcement>