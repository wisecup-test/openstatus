# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Workflow State Transitions

These rules are ALWAYS ACTIVE for all workflow state management, rate limiting, and data access code that coordinates distributed task execution, manages user workflow state, or implements rate-limited operations.

### Rules

- **R-WST-001** MUST: All workflow state transitions that govern task scheduling or execution flow must persist to durable storage before acknowledging the state change to upstream callers.
- **R-WST-002** MUST: Implement schema validation on all database result sets to enforce data contracts at storage boundaries and prevent malformed data propagation.
- **R-WST-003** MUST: Document the expiration policy rationale in code comments for all new cached data types, including the business logic that determines time-to-live values and consistency tradeoffs.
- **R-WST-004** MUST: Ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain state consistency.
- **R-WST-005** MUST: Implement structured logging for all cache operations including hit/miss status, key patterns, and expiration metadata.
- **R-WST-006** SHOULD: Use cache-backed storage for high-frequency read operations (rate limiting counters, session state) to achieve sub-millisecond latency.
- **R-WST-007** SHOULD: Use durable database storage for workflow state spanning multi-day intervals (3-day, 14-day windows) to survive process restarts and infrastructure failures.
- **R-WST-008** SHOULD: Implement cache expiration policies aligned with workflow step durations to bound eventual consistency windows.
- **R-WST-009** MAY: Adopt write-through cache patterns for entity types with high read-to-write ratios where cache consistency is critical.

### Verify

```bash
# 1. Discover the project's dependency manifest and lock artifact
find . -name 'package.json' -o -name 'Gemfile.lock' -o -name 'go.sum' -o -name 'requirements.txt' -o -name 'Cargo.lock' | head -1

# 2. Identify the database client library version from lock artifact
# (Exact command depends on build tool; inspect lock file for database client entry)

# 3. Verify database client API supports select operations and schema validation
# (Fetch official documentation for identified version)

# 4. Identify the cache client library version from lock artifact
# (Exact command depends on build tool; inspect lock file for cache client entry)

# 5. Verify cache client API supports key-value operations with expiration policies
# (Fetch official documentation for identified version)

# 6. Locate and execute workflow state persistence tests
find . -path '*/test*' -name '*workflow*' -o -path '*/test*' -name '*state*' | grep -E '\.(test|spec)\.(js|py|go|rb)$'

# 7. Execute rate limiting counter operation tests
find . -path '*/test*' -name '*rate*limit*' -o -path '*/test*' -name '*counter*' | grep -E '\.(test|spec)\.(js|py|go|rb)$'

# 8. Execute cache expiration behavior tests
find . -path '*/test*' -name '*cache*' -o -path '*/test*' -name '*expir*' | grep -E '\.(test|spec)\.(js|py|go|rb)$'
```

**Accept when:**
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks
- Database client library version is confirmed to support select operations and schema validation patterns
- Cache client library version is confirmed to support key-value operations with expiration policies measured in seconds
- All new data access code includes explicit justification for storage layer choice (cache vs database)
- All new cached data types include documented expiration policy rationale in code comments
- All workflow state transitions include schema validation on database reads

<enforcement>
Claude Code MUST NOT skip or defer verification. Code review checklist MUST require explicit justification for storage layer choice on all new data access code. Automated tests MUST cover cache expiration behavior and database schema validation for all entity types. Pull requests adding data access code without schema validation or expiration policies MUST be blocked until corrected.
</enforcement>