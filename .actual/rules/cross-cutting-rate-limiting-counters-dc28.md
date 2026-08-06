# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Rate Limiting Counters

These rules are ALWAYS ACTIVE for all data access code that implements rate limiting, workflow state persistence, session management, or user-scoped entity queries in the workflow monitoring system.

### Rules

- **R-CACHE-001** MUST: Rate limiting counters and short-lived session state must use in-memory cache storage with explicit time-to-live expiration policies.
- **R-CACHE-002** MUST: When adding new cached data types, document the expiration policy rationale in code comments, including the business logic that determines the time-to-live value and the consistency tradeoffs accepted.
- **R-CACHE-003** MUST: For workflow state transitions, ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain state consistency.
- **R-CACHE-004** MUST: Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning.
- **R-CACHE-005** SHOULD: Implement cache expiration policies aligned with workflow step durations (3-day and 14-day intervals) to bound eventual consistency windows.
- **R-CACHE-006** SHOULD: Design workflow logic to tolerate eventual consistency within bounded time windows between cache and database state.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'go.mod' -o -name 'requirements.txt' -o -name 'pom.xml' -o -name 'Gemfile' | head -1

# 2. Identify the database client library version from lock artifact
# (e.g., package-lock.json, go.sum, requirements.lock, pom.lock, Gemfile.lock)
grep -E '(postgres|mysql|dynamodb|firestore|mongodb)' <lock-artifact> | head -5

# 3. Identify the cache client library version from lock artifact
# (e.g., redis, memcached, in-memory cache)
grep -E '(redis|memcached|cache)' <lock-artifact> | head -5

# 4. Verify database client API supports select operations and schema validation
# Locate official documentation for the exact resolved version

# 5. Verify cache client API supports key-value operations with expiration policies
# Confirm TTL/expiration is measured in seconds or compatible units

# 6. Locate and execute the project's test suite
find . -path '*/test*' -name '*test*.go' -o -name '*_test.py' -o -name '*Test.java' | head -10

# 7. Run workflow state persistence tests
# Run rate limiting counter operation tests
# Run cache expiration behavior tests
# Run schema validation error handling tests
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds (sub-millisecond for 15 tokens/second) and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- Schema validation tests confirm that malformed database records are caught at the storage boundary and do not propagate through application logic
- Structured logging for cache operations is present in code, including hit/miss status, key patterns, and expiration metadata

<enforcement>
Clause Code MUST NOT skip or defer verification. All data access code introducing rate limiting, workflow state, or session management MUST pass the verify commands and accept criteria before merging. Code review MUST require explicit justification for storage layer choice (cache vs database) on all new data access code. Pull requests adding data access code without schema validation or expiration policies are blocked until corrected.
</enforcement>