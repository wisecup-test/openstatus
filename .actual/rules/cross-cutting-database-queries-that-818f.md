# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Database Queries That

These rules are ALWAYS ACTIVE for all database query code, cache operations, and data access patterns that retrieve user or workspace entities, manage workflow state, or enforce rate limiting.

### Rules

- **R-CACHE-001** MUST: Database queries that retrieve user or workspace entities must apply schema validation to all result sets before propagating data to application logic.
- **R-CACHE-002** MUST: When adding new cached data types, document the expiration policy rationale in code comments, including the business logic that determines the time-to-live value and the consistency tradeoffs accepted.
- **R-CACHE-003** MUST: For workflow state transitions, ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain state consistency.
- **R-CACHE-004** SHOULD: Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning.
- **R-CACHE-005** SHOULD: Implement cache expiration policies aligned with workflow step durations, add monitoring for cache hit rates and staleness metrics, and design workflow logic to tolerate eventual consistency within bounded time windows.
- **R-CACHE-006** SHOULD: Implement fallback rate limiting using database-backed counters with relaxed limits, add circuit breakers to detect cache failures, and establish runbooks for cache recovery procedures.

### Verify

```bash
# Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'requirements.txt' -o -name 'Cargo.lock' | head -1

# Identify the database client library version and verify API compatibility
# (Exact command depends on build tool discovered above)

# Identify the cache client library version and verify expiration policy support
# (Exact command depends on build tool discovered above)

# Locate and execute the project's test suite
find . -path '*/test*' -name '*test*' -o -path '*/spec*' -name '*spec*' | head -5

# Verify workflow state persistence tests pass
# Verify rate limiting counter operation tests pass
# Verify cache expiration tests pass
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- All database queries retrieving user or workspace entities include schema validation before data propagation
- New cached data types include documented expiration policy rationale in code comments
- Workflow state transitions ensure database writes complete before side effects are initiated

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review checklist must require explicit justification for storage layer choice (cache vs database) on all new data access code. Automated tests must cover cache expiration behavior and database schema validation for all entity types. Architecture review required for new features introducing stateful workflows or rate-limited operations. Pull requests adding data access code without schema validation or expiration policies are blocked until corrected.
</enforcement>