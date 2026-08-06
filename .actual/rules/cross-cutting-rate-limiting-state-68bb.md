# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: Rate Limiting State

These rules are ALWAYS ACTIVE for all workflow monitoring systems that execute multi-step processes over extended time periods and require user state management with rate limiting enforcement across distributed workflow instances.

### Rules

- **R-CACHE-001** SHOULD: Rate limiting state SHOULD be stored in distributed cache with expiration matching the rate limit window to enable automatic cleanup.
- **R-CACHE-002** MUST: Initialize in-memory map at workflow entry point before database queries.
- **R-CACHE-003** MUST: Populate in-memory map during initial user data fetch and reuse for all subsequent lookups within the execution context.
- **R-CACHE-004** MUST: Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations.
- **R-CACHE-005** MUST: Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants to maintain consistency between workflow timing and cache lifecycle.
- **R-CACHE-006** MUST: Configure distributed cache TTL to exceed maximum workflow duration by safety margin (e.g., 30 days for 14-day workflows).
- **R-CACHE-007** SHOULD: Implement cache size limits with LRU eviction policy to prevent unbounded memory growth.
- **R-CACHE-008** SHOULD: Implement circuit breaker pattern with fallback to database-only mode for distributed cache unavailability.
- **R-CACHE-009** SHOULD: Add distributed cache health checks to deployment readiness probes.
- **R-CACHE-010** SHOULD: Configure cache client with connection timeouts and retry policies.

### Verify

```bash
# Locate the project's workflow monitoring module and identify the cache initialization logic
grep -r "usersMap\.set\|redis\.set" --include="*.js" --include="*.ts" src/

# Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins
grep -r "new Map\|new Redis\|cache.*init" --include="*.js" --include="*.ts" src/ | grep -E "(workflow|launch|monitor)"

# Inspect distributed cache write operations and extract the configured expiration values
grep -r "expire\|ttl\|EX\|PX" --include="*.js" --include="*.ts" src/ | grep -i cache

# Confirm that TTL values equal or exceed the maximum workflow step duration
grep -r "30.*day\|14.*day\|3.*day\|86400\|1209600" --include="*.js" --include="*.ts" src/

# Examine the project's test suite for cache layer integration tests
find . -path ./node_modules -prune -o -name "*.test.js" -o -name "*.test.ts" -o -name "*.spec.js" -o -name "*.spec.ts" | xargs grep -l "cache\|redis\|usersMap"

# Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination
npm test -- --testNamePattern="cache|expiration|distributed"
```

**Accept when:**
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state
- Both cache layers are instantiated before workflow execution begins
- TTL values equal or exceed the maximum workflow step duration defined in workflow contracts
- Cache miss rates remain below configured threshold during active workflow execution
- Memory consumption stays within acceptable bounds with LRU eviction policy active

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for workflow state management implementations. Code review MUST validate dual-layer cache initialization before workflow state access. Integration tests MUST cover cache hit rates and distributed state persistence. Production monitoring MUST track cache access latency and memory consumption metrics.
</enforcement>