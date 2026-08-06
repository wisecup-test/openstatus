# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: Workflow Initialization Data

These rules are ALWAYS ACTIVE for workflow monitoring systems that execute multi-step processes over extended time periods (3-14 days) and require user state management in distributed workflow environments with rate limiting enforcement across workflow instances.

### Rules

- **R-CACHE-001** SHOULD: Workflow initialization data SHOULD be persisted to distributed cache before launching asynchronous workflow steps to ensure state availability across process boundaries.
- **R-CACHE-002** MUST: Initialize in-memory map at workflow entry point before database queries and populate during initial user data fetch for reuse in all subsequent lookups within the execution context.
- **R-CACHE-003** MUST: Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations during debugging or maintenance.
- **R-CACHE-004** MUST: Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants, ensuring TTL values equal or exceed the maximum workflow step duration defined in workflow contracts.
- **R-CACHE-005** SHOULD: Configure distributed cache TTL to exceed maximum workflow duration by safety margin (e.g., 30 days for 14-day workflows) to prevent premature state loss.
- **R-CACHE-006** SHOULD: Implement cache size limits with LRU eviction policy and monitor process memory usage with alerts at 80% threshold to prevent unbounded in-memory cache growth.
- **R-CACHE-007** SHOULD: Implement circuit breaker pattern with fallback to database-only mode for distributed cache unavailability scenarios.
- **R-CACHE-008** SHOULD: Add distributed cache health checks to deployment readiness probes and configure cache client with connection timeouts and retry policies.

### Verify

```bash
# Locate the project's workflow monitoring module and identify the cache initialization logic
grep -r "usersMap\.set\|redis\.set" --include="*.js" --include="*.ts" src/

# Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins
grep -B5 -A5 "LaunchMonitorWorkflow\|Step3Days\|Step14Days" src/ --include="*.js" --include="*.ts"

# Inspect distributed cache write operations and extract the configured expiration values
grep -r "TTL\|expir\|setex" --include="*.js" --include="*.ts" src/ | grep -E "30.*day|86400|2592000"

# Confirm that TTL values equal or exceed the maximum workflow step duration
grep -r "tokensPerInterval\|rate.*limit" --include="*.js" --include="*.ts" src/

# Examine the project's test suite for cache layer integration tests
find . -path ./node_modules -prune -o -name "*.test.js" -o -name "*.test.ts" -o -name "*.spec.js" -o -name "*.spec.ts" | xargs grep -l "cache\|redis\|usersMap"

# Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination
npm test -- --testPathPattern="cache|workflow" 2>&1 | tee test-results.log
```

**Accept when:**
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state
- Both cache layers (in-memory map and distributed store) are instantiated before workflow execution begins
- TTL values in distributed cache configuration equal or exceed maximum workflow step duration
- Integration tests pass for cache miss scenarios, expiration behavior, and cross-instance state coordination
- Cache access latency metrics show microsecond-level performance for in-memory lookups
- Memory consumption monitoring shows controlled growth with LRU eviction policy active

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for workflow initialization data caching implementations. Violations detected during code review or production monitoring must trigger remediation or exception process.
</enforcement>