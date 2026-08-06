# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Cache Entries User

These rules are ALWAYS ACTIVE for all code that implements data access patterns for workflow state, rate limiting counters, user-scoped queries, and session state management.

### Rules

- **R-CACHE-001** MUST: Cache entries for user-scoped workflow data must include expiration windows aligned with the longest workflow step duration to prevent stale reads.
- **R-CACHE-002** MUST: Document the expiration policy rationale in code comments, including the business logic that determines the time-to-live value and the consistency tradeoffs accepted when adding new cached data types.
- **R-CACHE-003** MUST: Ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain state consistency for workflow state transitions.
- **R-CACHE-004** MUST: Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning.
- **R-CACHE-005** MUST: Provide explicit justification for storage layer choice (cache vs database) on all new data access code during code review.
- **R-CACHE-006** MUST: Implement schema validation at storage boundaries for all database result sets to enforce data contracts and prevent malformed data propagation.
- **R-CACHE-007** SHOULD: Implement cache expiration policies aligned with workflow step durations and add monitoring for cache hit rates and staleness metrics.
- **R-CACHE-008** SHOULD: Implement fallback rate limiting using database-backed counters with relaxed limits and circuit breakers to detect cache failures.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'Cargo.lock' -o -name 'pom.xml' -o -name 'build.gradle' | head -5

# 2. Identify the database client library version from lock artifact
grep -E '(database|db|sql|postgres|mysql)' $(find . -name '*.lock' -o -name 'Gemfile.lock' -o -name 'go.sum' | head -1) | head -10

# 3. Identify the cache client library version from lock artifact
grep -E '(cache|redis|memcached)' $(find . -name '*.lock' -o -name 'Gemfile.lock' -o -name 'go.sum' | head -1) | head -10

# 4. Locate and execute workflow state persistence tests
find . -path '*/test*' -name '*workflow*' -o -path '*/test*' -name '*state*' | grep -E '\.(test|spec)\.(js|py|go|rb|java)$' | head -5

# 5. Locate and execute rate limiting tests
find . -path '*/test*' -name '*rate*' -o -path '*/test*' -name '*limit*' | grep -E '\.(test|spec)\.(js|py|go|rb|java)$' | head -5

# 6. Locate and execute cache expiration tests
find . -path '*/test*' -name '*cache*' -o -path '*/test*' -name '*expir*' | grep -E '\.(test|spec)\.(js|py|go|rb|java)$' | head -5

# 7. Verify schema validation patterns in database access code
grep -r 'schema\|validate\|Schema' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' --include='*.java' | grep -E '(select|query|fetch)' | head -10

# 8. Verify cache expiration configuration
grep -r 'expir\|ttl\|TTL' --include='*.js' --include='*.py' --include='*.go' --include='*.rb' --include='*.java' | head -10
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- Database client library version is confirmed and its select operations and schema validation patterns are verified against official documentation
- Cache client library version is confirmed and its key-value operations with expiration policies (measured in seconds) are verified against official documentation
- All new data access code includes explicit storage layer choice justification in code review
- Schema validation is implemented at all storage boundaries for database result sets
- Structured logging for cache operations (hit/miss status, key patterns, expiration metadata) is present in the codebase

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for code that implements data access patterns within the configured scope. Violations must be corrected before code is merged to production branches.
</enforcement>