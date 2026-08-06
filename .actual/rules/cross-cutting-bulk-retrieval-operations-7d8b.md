# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Bulk Retrieval Operations

These rules are ALWAYS ACTIVE for all data access code that implements workflow state persistence, rate limiting, user-scoped queries, and session state management across distributed task execution systems.

### Rules

- **R-BULK-001** SHOULD: Bulk retrieval operations that scan user collections should use database select operations rather than cache enumeration to ensure consistency.
- **R-BULK-002** MUST: Document expiration policy rationale in code comments when adding new cached data types, including business logic that determines time-to-live values and consistency tradeoffs accepted.
- **R-BULK-003** MUST: Ensure database writes complete and return success before initiating side effects such as task queue enqueues or external API calls to maintain workflow state consistency.
- **R-BULK-004** MUST: Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning.
- **R-BULK-005** MUST: Provide explicit justification for storage layer choice (cache vs database) on all new data access code during code review.
- **R-BULK-006** MUST: Implement cache expiration policies aligned with workflow step durations and add monitoring for cache hit rates and staleness metrics.
- **R-BULK-007** MUST: Implement fallback rate limiting using database-backed counters with relaxed limits and circuit breakers to detect cache failures.
- **R-BULK-008** MUST: Wrap schema validation in error boundaries with structured logging and implement dead-letter queues for malformed records.

### Verify

```bash
# Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'requirements.txt' -o -name 'pom.xml' | head -1

# Identify the database client library version and verify select operations API
# (Exact command depends on build tool discovered above)

# Identify the cache client library version and verify key-value operations with expiration
# (Exact command depends on build tool discovered above)

# Locate and execute workflow state persistence tests
find . -path '*/test*' -name '*workflow*' -o -path '*/test*' -name '*state*' | grep -E '\.(test|spec)\.(js|py|go|java)$'

# Execute rate limiting counter operation tests
find . -path '*/test*' -name '*rate*limit*' -o -path '*/test*' -name '*counter*' | grep -E '\.(test|spec)\.(js|py|go|java)$'

# Execute schema validation error handling tests
find . -path '*/test*' -name '*schema*' -o -path '*/test*' -name '*validation*' | grep -E '\.(test|spec)\.(js|py|go|java)$'

# Verify cache expiration behavior is tested
grep -r 'expir' . --include='*.test.*' --include='*.spec.*' | head -5

# Verify database schema validation is tested
grep -r 'schema.*validat\|validat.*schema' . --include='*.test.*' --include='*.spec.*' | head -5
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- Schema validation tests confirm that malformed records are caught at storage boundaries and logged appropriately
- Code review checklist confirms explicit justification for storage layer choice on all new data access code
- Monitoring and alerting are configured for cache hit rates, staleness metrics, and schema validation failure rates

<enforcement>
Claude Code MUST NOT skip or defer verification. All rules in this file are mandatory for data access implementations within scope. Violations must be corrected before code review approval.
</enforcement>