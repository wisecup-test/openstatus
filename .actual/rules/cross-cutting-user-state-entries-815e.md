# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: User State Entries

These rules are ALWAYS ACTIVE for all workflow monitoring systems that execute multi-step processes over extended time periods and maintain user state across distributed execution contexts.

### Rules

- **R-CACHE-001** MUST: User state entries in distributed cache SHALL be configured with expiration policies matching the maximum workflow duration plus a safety margin.
- **R-CACHE-002** MUST: Initialize in-memory map at workflow entry point before database queries.
- **R-CACHE-003** MUST: Populate in-memory map during initial user data fetch and reuse for all subsequent lookups within the execution context.
- **R-CACHE-004** MUST: Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations.
- **R-CACHE-005** MUST: Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants.
- **R-CACHE-006** SHOULD: Implement cache size limits with LRU eviction policy for in-memory layer in high-user-count scenarios.
- **R-CACHE-007** SHOULD: Monitor process memory usage and set alerts at 80% threshold.
- **R-CACHE-008** SHOULD: Implement circuit breaker pattern with fallback to database-only mode for distributed cache unavailability.
- **R-CACHE-009** SHOULD: Add distributed cache health checks to deployment readiness probes.
- **R-CACHE-010** SHOULD: Configure cache client with connection timeouts and retry policies.

### Verify

```bash
# Locate the project's workflow monitoring module and identify the cache initialization logic
grep -r "usersMap\.set\|redis\.set" --include="*.js" --include="*.ts" src/

# Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins
grep -B5 -A5 "LaunchMonitorWorkflow\|Step3Days\|Step14Days\|StepPaused" src/ | grep -E "cache|map|redis"

# Inspect distributed cache write operations and extract the configured expiration values
grep -r "TTL\|expir\|setex" --include="*.js" --include="*.ts" src/ | grep -v test

# Confirm that TTL values equal or exceed the maximum workflow step duration
grep -r "30.*day\|14.*day\|3.*day" --include="*.js" --include="*.ts" src/

# Examine the project's test suite for cache layer integration tests
find . -path ./node_modules -prune -o -name "*.test.js" -o -name "*.test.ts" -o -name "*.spec.js" -o -name "*.spec.ts" | xargs grep -l "cache\|redis\|usersMap"

# Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination
npm test -- --testPathPattern="cache|state" 2>&1 | tee test-results.log
```

**Accept when:**
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state
- Both cache layers are initialized before any workflow state access occurs
- Distributed cache TTL values are explicitly configured to exceed maximum workflow duration by a documented safety margin
- Integration tests pass validating cache hit rates and distributed state persistence across process boundaries

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for workflow user state implementations. Violations detected during code review or integration testing must be remediated before merge. Production monitoring must track cache access latency and memory consumption metrics continuously.
</enforcement>