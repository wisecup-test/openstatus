# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Application Layer Caching

These rules are ALWAYS ACTIVE for all data access code that implements workflow state persistence, rate limiting, user and workspace entity queries, or session state management within the application.

### Rules

- **R-CACHE-001** MAY: Application-layer caching using in-process data structures may supplement persistent cache layers for read-heavy access patterns within a single request lifecycle.
- **R-CACHE-002** MUST: Document the expiration policy rationale in code comments for all new cached data types, including the business logic that determines the time-to-live value and the consistency tradeoffs accepted.
- **R-CACHE-003** MUST: Ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain workflow state consistency.
- **R-CACHE-004** MUST: Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning.
- **R-CACHE-005** MUST: Implement cache expiration policies aligned with workflow step durations and add monitoring for cache hit rates and staleness metrics.
- **R-CACHE-006** MUST: Wrap schema validation in error boundaries with structured logging and implement dead-letter queues for malformed records.
- **R-CACHE-007** MUST: Provide explicit justification for storage layer choice (cache vs database) on all new data access code during code review.
- **R-CACHE-008** MUST: Implement automated tests covering cache expiration behavior and database schema validation for all entity types.
- **R-CACHE-009** MUST: Require architecture review for new features that introduce stateful workflows or rate-limited operations.

### Verify

```bash
# Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'requirements.txt' -o -name 'pom.xml' | head -1

# Identify the database client library version and verify API compatibility
# (Exact command depends on build tool discovered above)

# Identify the cache client library version and verify expiration policy support
# (Exact command depends on build tool discovered above)

# Locate and execute the project's test suite
find . -path '*/test*' -name '*test*' -o -path '*/spec*' -name '*spec*' | head -5

# Verify workflow state persistence tests pass
# Verify rate limiting counter operation tests pass
# Verify cache expiration tests pass
# Verify schema validation error handling tests pass
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- Schema validation tests verify that malformed records are caught and logged without cascading errors
- Code review checklist confirms explicit justification for storage layer choice on all new data access code
- Architecture review confirms new stateful workflows or rate-limited operations comply with dual-storage pattern

<enforcement>
Claude Code MUST NOT skip or defer verification. All data access code must pass automated tests covering cache expiration and schema validation. Code review must confirm storage layer justification. Production incidents caused by cache-database consistency issues trigger post-mortem reviews.
</enforcement>