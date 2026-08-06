# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Memory Cache Keys

These rules are ALWAYS ACTIVE for all code implementing data access patterns, cache operations, workflow state persistence, rate limiting, and schema validation across the workflow monitoring system.

### Rules

- **R-CACHE-001** SHOULD: In-memory cache keys should encode entity type, identifier, and scope to enable efficient invalidation and prevent key collisions across namespaces.
- **R-CACHE-002** MUST: Document the expiration policy rationale in code comments for all new cached data types, including the business logic that determines the time-to-live value and the consistency tradeoffs accepted.
- **R-CACHE-003** MUST: Ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain state consistency for workflow state transitions.
- **R-CACHE-004** MUST: Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning.
- **R-CACHE-005** MUST: Provide explicit justification for storage layer choice (cache vs database) on all new data access code during code review.
- **R-CACHE-006** MUST: Implement cache expiration policies aligned with workflow step durations and add monitoring for cache hit rates and staleness metrics.
- **R-CACHE-007** MUST: Implement fallback rate limiting using database-backed counters with relaxed limits and add circuit breakers to detect cache failures.
- **R-CACHE-008** MUST: Wrap schema validation in error boundaries with structured logging and implement dead-letter queues for malformed records.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'requirements.txt' -o -name 'pom.xml' | head -5

# 2. Identify the database client library version from lock artifact
grep -E '(postgres|mysql|sqlite|dynamodb|mongodb)' $(find . -name '*.lock' -o -name 'go.sum' -o -name 'requirements.txt') | head -3

# 3. Identify the cache client library version from lock artifact
grep -E '(redis|memcached|cache)' $(find . -name '*.lock' -o -name 'go.sum' -o -name 'requirements.txt') | head -3

# 4. Locate and execute workflow state persistence tests
find . -path '*/test*' -name '*workflow*' -o -path '*/test*' -name '*persistence*' | head -5

# 5. Locate and execute rate limiting tests
find . -path '*/test*' -name '*rate*limit*' -o -path '*/test*' -name '*token*bucket*' | head -5

# 6. Locate and execute cache expiration tests
find . -path '*/test*' -name '*cache*' -o -path '*/test*' -name '*expir*' | head -5

# 7. Verify schema validation patterns in database result handling
grep -r 'schema.*validat' --include='*.py' --include='*.js' --include='*.go' --include='*.java' . | head -5

# 8. Verify cache key encoding patterns
grep -r 'cache.*key' --include='*.py' --include='*.js' --include='*.go' --include='*.java' . | head -5
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- Schema validation error handling tests confirm that malformed records are caught at storage boundaries and logged appropriately
- Code review checklist confirms explicit storage layer justification (cache vs database) for all new data access code
- Monitoring and alerting are configured for cache hit rates, staleness metrics, and schema validation failure rates
- Fallback rate limiting with database-backed counters and circuit breakers are implemented and tested

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code review approval. Violations must be corrected before merge. Production incidents caused by cache-database consistency issues trigger post-mortem reviews and pattern documentation updates.
</enforcement>