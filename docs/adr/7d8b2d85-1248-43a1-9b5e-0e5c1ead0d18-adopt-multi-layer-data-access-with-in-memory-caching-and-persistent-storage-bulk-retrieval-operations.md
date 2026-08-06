# Adopt Multi-Layer Data Access with In-Memory Caching and Persistent Storage: Bulk Retrieval Operations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The workflow monitoring system coordinates asynchronous task execution using cloud task queues and requires reliable state management across distributed components
- User workflow state must persist beyond transient failures and support retrieval patterns that span multiple time windows (3-day, 14-day, and paused states)
- The system implements rate limiting (15 tokens per second) to protect downstream services, requiring fast access to request counters without database round-trips
- Workspace data undergoes validation using schema parsers, indicating structured data contracts between storage and application layers
- The codebase imports database client libraries and cache client libraries, establishing a dual-storage architecture for different data lifecycle requirements

## Problem Statement

Workflow monitoring systems that coordinate distributed task execution require reliable data access patterns that balance consistency, performance, and fault tolerance. Single-layer storage architectures cannot simultaneously satisfy low-latency read requirements for rate limiting, durable persistence for workflow state, and efficient bulk retrieval for user-scoped queries without introducing either performance bottlenecks or consistency risks.

## Decision

1. SHOULD: Bulk retrieval operations that scan user collections should use database select operations rather than cache enumeration to ensure consistency

## Policy Block

- SHOULD Bulk retrieval operations that scan user collections should use database select operations rather than cache enumeration to ensure consistency

In scope:
- Workflow state persistence for task queue coordination
- Rate limiting counter storage and retrieval
- User and workspace entity queries with schema validation
- Session state and temporary data with defined expiration windows

Out of scope:
- Static configuration data that does not change during runtime
- Logging and observability data streams
- Binary asset storage or file uploads
- Real-time streaming data that bypasses persistence layers

## Rationale

- The evidence shows explicit use of both in-memory cache operations with expiration policies and database select queries, indicating intentional separation of storage concerns based on data lifecycle and access patterns
- Rate limiting at 15 tokens per second requires sub-millisecond read latency that database round-trips cannot reliably provide, necessitating cache-backed counter storage
- Workflow steps spanning 3-day and 14-day intervals require durable persistence that survives process restarts and infrastructure failures, which in-memory storage cannot guarantee
- Schema validation on database result sets demonstrates a reliability pattern that enforces data contracts at storage boundaries, reducing propagation of malformed data through the application

## Consequences

Positive:
- Rate limiting and session state operations achieve sub-millisecond latency by avoiding database round-trips for high-frequency reads
- Workflow state persists reliably across process restarts and infrastructure failures, enabling long-running task coordination
- Schema validation at storage boundaries prevents malformed data from propagating through application logic, reducing runtime errors
- Explicit cache expiration policies prevent unbounded memory growth and ensure stale data is automatically purged

Negative:
- Dual-storage architecture increases operational complexity, requiring monitoring and maintenance of both database and cache infrastructure
- Cache invalidation logic must be carefully coordinated with database writes to prevent consistency anomalies between storage layers
- Developers must understand data lifecycle requirements to correctly choose between cache and database storage for new features
- Cache failures can degrade rate limiting effectiveness or force fallback to slower database-backed implementations

## Alternatives

- Use database-only storage with aggressive connection pooling and read replicas for all data access patterns (rejected)
  Rejected because: Database round-trip latency cannot reliably satisfy sub-millisecond read requirements for rate limiting at 15 tokens per second, and connection pool exhaustion under high concurrency would introduce request queuing delays
  When valid: Valid for systems with relaxed latency requirements (>10ms acceptable) and lower request rates where database performance is sufficient
- Use cache-only storage with periodic snapshots to durable storage for disaster recovery (rejected)
  Rejected because: Cache-only storage cannot guarantee durability for workflow state that must survive infrastructure failures, and snapshot intervals introduce potential data loss windows for in-flight state transitions
  When valid: Valid for ephemeral data where loss is acceptable and state can be reconstructed from external sources
- Implement write-through cache pattern where all writes go to database first, then populate cache on read misses (deferred)
  Rejected because: Not rejected; this pattern complements the current architecture and may be adopted for specific entity types where cache-aside semantics are insufficient
  When valid: Valid for entity types with high read-to-write ratios where cache consistency is critical and write latency is acceptable

## Risks

- Cache and database state divergence due to failed cache writes or missed invalidations, leading to stale reads that violate workflow state invariants
  Mitigation: Implement cache expiration policies aligned with workflow step durations, add monitoring for cache hit rates and staleness metrics, and design workflow logic to tolerate eventual consistency within bounded time windows
  Owner: engineering team
- Cache infrastructure outages degrade rate limiting effectiveness, potentially allowing request floods that overwhelm downstream services
  Mitigation: Implement fallback rate limiting using database-backed counters with relaxed limits, add circuit breakers to detect cache failures, and establish runbooks for cache recovery procedures
  Owner: engineering team
- Schema validation failures on database reads cause cascading errors if not properly handled, potentially blocking workflow progression
  Mitigation: Wrap schema validation in error boundaries with structured logging, implement dead-letter queues for malformed records, and add monitoring alerts for validation failure rates
  Owner: engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- When adding new cached data types, document the expiration policy rationale in code comments, including the business logic that determines the time-to-live value and the consistency tradeoffs accepted
- For workflow state transitions, ensure database writes complete and return success before initiating any side effects such as task queue enqueues or external API calls to maintain state consistency
- Implement structured logging for cache operations that includes hit/miss status, key patterns, and expiration metadata to enable operational debugging and capacity planning

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and lock artifact, then identify the database client library version and verify its API compatibility with select operations and schema validation patterns
- Discover the project's cache client library version and verify its API supports key-value operations with expiration policies measured in seconds
- Locate the project's test suite and execute tests covering workflow state persistence, rate limiting counter operations, and schema validation error handling

Accept when:
- All workflow state persistence tests pass, demonstrating that state survives simulated process restarts and can be retrieved with correct schema validation
- Rate limiting tests demonstrate counter operations complete within acceptable latency bounds and correctly enforce token bucket semantics
- Cache expiration tests verify that entries are automatically purged after their time-to-live expires and do not cause memory leaks

## Enforcement

- Verified by: Code review checklist requiring explicit justification for storage layer choice (cache vs database) on all new data access code
- Verified by: Automated tests covering cache expiration behavior and database schema validation for all entity types
- Verified by: Architecture review for new features that introduce stateful workflows or rate-limited operations
- Violation handling: Pull requests that add data access code without schema validation or expiration policies are blocked until corrected
- Violation handling: Production incidents caused by cache-database consistency issues trigger post-mortem reviews and pattern documentation updates
- Violation handling: Monitoring alerts for cache hit rate degradation or schema validation failure spikes trigger on-call investigation
- Exception process: Exceptions for single-layer storage must document the specific performance or consistency requirements that justify deviation from the dual-storage pattern
- Exception process: Architecture review board approval required for new storage technologies or access patterns not covered by existing database and cache infrastructure
- Exception process: Temporary exceptions during prototyping must include a migration plan to compliant patterns before production deployment