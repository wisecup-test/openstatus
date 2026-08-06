# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: Systems Use Schema

These rules are ALWAYS ACTIVE for all workflow monitoring systems that execute multi-step processes over extended time periods (3-14 days) and require user state management in distributed workflow environments.

### Rules

- **R-CACHE-001** MAY: Systems MAY use schema validation on workspace or user data retrieved from cache to ensure data integrity.
- **R-CACHE-002** MUST: Initialize in-memory map at workflow entry point before database queries.
- **R-CACHE-003** MUST: Populate in-memory map during initial user data fetch and reuse for all subsequent lookups within the execution context.
- **R-CACHE-004** MUST: Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations.
- **R-CACHE-005** MUST: Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants, ensuring TTL values equal or exceed the maximum workflow step duration.
- **R-CACHE-006** MUST: Implement circuit breaker pattern with fallback to database-only mode for distributed cache unavailability.
- **R-CACHE-007** SHOULD: Implement cache size limits with LRU eviction policy for in-memory maps.
- **R-CACHE-008** SHOULD: Monitor process memory usage and set alerts at 80% threshold.
- **R-CACHE-009** SHOULD: Configure distributed cache client with connection timeouts and retry policies.

### Verify

```bash
# Locate the project's workflow monitoring module and identify the cache initialization logic
grep -r "usersMap\.set\|redis\.set" --include="*.js" --include="*.ts" src/

# Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins
grep -B5 -A5 "LaunchMonitorWorkflow\|Step3Days\|Step14Days" src/ --include="*.js" --include="*.ts"

# Inspect distributed cache write operations and extract the configured expiration values
grep -r "TTL\|expir\|30.*day" --include="*.js" --include="*.ts" src/ | grep -i cache

# Confirm that TTL values equal or exceed the maximum workflow step duration
grep -r "tokensPerInterval\|rate.*limit" --include="*.js" --include="*.ts" src/

# Examine the project's test suite for cache layer integration tests
find test/ -name "*cache*" -o -name "*workflow*" | xargs grep -l "cache.*miss\|expir\|cross.*instance"

# Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination
npm test -- --grep "cache|expir|distributed"
```

**Accept when:**
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state
- Both in-memory and distributed cache structures are instantiated before workflow execution begins
- TTL values in distributed cache configuration equal or exceed the maximum workflow step duration
- Cache initialization occurs at workflow entry point before any database queries
- Integration tests validate cache hit rates and distributed state persistence across process boundaries

<enforcement>
Clause Code MUST NOT skip or defer verification. All rules in this file are mandatory for workflow monitoring systems matching the defined scope. Violations detected during code review or integration testing must be remediated before merge.
</enforcement>