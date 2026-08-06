# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: Memory Cache Structures

These rules are ALWAYS ACTIVE for workflow monitoring systems that execute multi-step processes over extended time periods (3-14 days) requiring user state management in distributed environments with rate limiting enforcement across workflow instances.

### Rules

- **R-CACHE-001** MUST: In-memory cache structures SHALL be populated from database queries and used for subsequent lookups within the same execution context.
- **R-CACHE-002** MUST: Initialize in-memory map at workflow entry point before database queries are executed.
- **R-CACHE-003** MUST: Populate in-memory map during initial user data fetch and reuse for all subsequent lookups within the execution context.
- **R-CACHE-004** MUST: Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants.
- **R-CACHE-005** MUST: Configure distributed cache TTL to exceed maximum workflow duration by safety margin (e.g., 30 days for 14-day workflows).
- **R-CACHE-006** SHOULD: Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations.
- **R-CACHE-007** SHOULD: Implement cache size limits with LRU eviction policy for in-memory structures in high-user-count scenarios.
- **R-CACHE-008** SHOULD: Implement circuit breaker pattern with fallback to database-only mode for distributed cache unavailability.
- **R-CACHE-009** SHOULD: Monitor process memory usage and set alerts at 80% threshold for in-memory cache growth.
- **R-CACHE-010** SHOULD: Implement monitoring alerts for cache misses during active workflow execution.

### Verify

```bash
# Locate the project's workflow monitoring module and identify the cache initialization logic
grep -r "usersMap\|cache.*init" --include="*.js" --include="*.ts" src/

# Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins
grep -B5 -A5 "LaunchMonitorWorkflow\|workflow.*start" src/ | grep -E "usersMap|redis|cache"

# Inspect distributed cache write operations and extract the configured expiration values
grep -r "redis\.set\|cache\.set" --include="*.js" --include="*.ts" src/ | grep -E "EX|PX|TTL|expir"

# Confirm that TTL values equal or exceed the maximum workflow step duration
grep -r "Step3Days\|Step14Days\|StepPaused" --include="*.js" --include="*.ts" src/ | head -20

# Examine the project's test suite for cache layer integration tests
find test/ -name "*cache*" -o -name "*workflow*" | xargs grep -l "cache.*hit\|cache.*miss\|expir"

# Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination
npm test -- --grep "cache|expir|distributed"
```

**Accept when:**
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state
- Cache initialization occurs before any workflow state access operations
- TTL configuration for distributed cache exceeds maximum workflow step duration by documented safety margin
- Integration tests pass for cache miss scenarios, expiration behavior, and cross-instance state coordination

<enforcement>
Clause Code MUST NOT skip or defer verification. All R-CACHE rules are mandatory for workflow state management implementations. Violations detected during code review or production monitoring must trigger remediation or exception process.
</enforcement>