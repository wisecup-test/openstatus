# Implement Dual-Layer Caching with In-Memory and Distributed Store for Workflow User State: Distributed Cache Keys

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Workflow monitoring systems require fast access to user state across multiple execution steps spanning days or weeks
- Database queries for user lookups introduce latency and load when executed repeatedly within a single workflow execution context
- Distributed cache stores provide persistence and cross-process visibility for workflow state that must survive process restarts
- In-memory maps offer microsecond-level access times for frequently accessed user data within a single execution context
- Rate limiting enforcement requires coordination across distributed workflow instances to prevent per-user quota violations

## Problem Statement

Workflow monitoring systems that execute multi-step processes over extended time periods (3-14 days) must maintain user state efficiently while balancing access speed, persistence requirements, and cross-instance coordination. Single-layer caching strategies either sacrifice speed (distributed-only) or reliability (memory-only), creating a tradeoff between performance and fault tolerance in long-running workflow orchestration.

## Decision

1. MUST: Distributed cache keys SHALL encode entity type and identifier to enable namespace isolation and prevent key collisions

## Policy Block

- MUST Distributed cache keys SHALL encode entity type and identifier to enable namespace isolation and prevent key collisions

In scope:
- Long-running workflow orchestration systems with multi-day execution spans
- User state management in distributed workflow environments
- Rate limiting enforcement across workflow instances
- Workflow step coordination requiring persistent state

Out of scope:
- Short-lived request-response cycles under one minute
- Single-process applications without distributed execution requirements
- Stateless workflow systems
- Cache-aside patterns for general application data

## Rationale

- Evidence shows explicit dual-layer caching with usersMap.set() for in-memory storage and redis.set() for distributed persistence with 30-day expiration matching workflow duration
- Workflow step contracts (LaunchMonitorWorkflow, Step3Days, Step14Days, StepPaused) indicate multi-day execution requiring state persistence beyond process lifetime
- Rate limiting implementation with tokensPerInterval configuration requires distributed coordination to enforce quotas across workflow instances
- Database select operations combined with in-memory map population demonstrate cache-warming pattern to reduce repeated database load

## Consequences

Positive:
- Microsecond-level access times for user state within execution context via in-memory map lookups
- Workflow state survives process restarts and enables cross-instance coordination through distributed cache persistence
- Automatic state cleanup through cache expiration eliminates manual garbage collection logic
- Rate limiting enforcement achieves consistency across distributed workflow instances

Negative:
- Increased architectural complexity from managing two cache layers with different consistency guarantees
- Memory footprint grows linearly with active user count in each workflow process
- Cache expiration misconfiguration risks premature state loss in long-running workflows
- Potential cache coherency issues if in-memory and distributed layers diverge due to concurrent updates

## Alternatives

- Use distributed cache only without in-memory layer (rejected)
  Rejected because: Network round-trip latency for distributed cache access (1-5ms) becomes prohibitive when user state is accessed hundreds of times within a single workflow execution context, degrading overall workflow performance
  When valid: Acceptable for workflows with infrequent state access (fewer than 10 lookups per execution) where network latency overhead is negligible
- Use in-memory cache only with database fallback (rejected)
  Rejected because: Workflow state is lost on process restart or crash, breaking multi-day workflow execution continuity and requiring expensive state reconstruction from database
  When valid: Suitable for short-lived workflows completing within a single process lifetime (under 1 hour) where state persistence is unnecessary
- Store all workflow state in database with connection pooling (rejected)
  Rejected because: Database query latency (5-50ms) and connection pool contention create bottlenecks when workflow steps require frequent state access, and database load increases linearly with workflow concurrency
  When valid: Appropriate for low-throughput workflows (fewer than 10 concurrent executions) where database performance is sufficient and cache infrastructure is unavailable

## Risks

- Cache expiration set shorter than maximum workflow duration causes premature state loss and workflow failures
  Mitigation: Configure distributed cache TTL to exceed maximum workflow duration by safety margin (e.g., 30 days for 14-day workflows). Implement monitoring alerts for cache misses during active workflow execution.
  Owner: Engineering team
- In-memory cache grows unbounded consuming excessive memory in high-user-count scenarios
  Mitigation: Implement cache size limits with LRU eviction policy. Monitor process memory usage and set alerts at 80% threshold. Consider cache partitioning for very large user bases.
  Owner: Engineering team
- Distributed cache unavailability causes workflow initialization failures despite database availability
  Mitigation: Implement circuit breaker pattern with fallback to database-only mode. Add distributed cache health checks to deployment readiness probes. Configure cache client with connection timeouts and retry policies.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Initialize in-memory map at workflow entry point before database queries. Populate map during initial user data fetch and reuse for all subsequent lookups within the execution context.
- Structure distributed cache keys with hierarchical namespace encoding entity type and identifier to enable pattern-based queries and bulk operations during debugging or maintenance.
- Set distributed cache expiration using seconds-based TTL calculation derived from workflow step duration constants to maintain consistency between workflow timing and cache lifecycle.

## Continuation Context


Verify commands:
- Locate the project's workflow monitoring module and identify the cache initialization logic. Verify that both in-memory and distributed cache structures are instantiated before workflow execution begins.
- Inspect distributed cache write operations and extract the configured expiration values. Confirm that TTL values equal or exceed the maximum workflow step duration defined in workflow contracts.
- Examine the project's test suite for cache layer integration tests. Execute tests covering cache miss scenarios, expiration behavior, and cross-instance state coordination.

Accept when:
- Workflow execution logs demonstrate in-memory cache hits for repeated user lookups within a single execution context without additional database queries
- Distributed cache entries persist for the configured TTL period and are accessible across multiple workflow process instances
- Rate limiting enforcement correctly coordinates quota consumption across distributed workflow instances using shared cache state

## Enforcement

- Verified by: Code review verification that workflow initialization establishes both cache layers before state access
- Verified by: Integration tests validating cache hit rates and distributed state persistence across process boundaries
- Verified by: Performance monitoring tracking cache access latency and memory consumption metrics
- Violation handling: Pull requests introducing workflow state access without dual-layer caching are rejected during code review
- Violation handling: Production monitoring alerts trigger when cache miss rates exceed threshold indicating improper cache usage
- Violation handling: Architecture review board evaluates violations and determines remediation timeline based on impact severity
- Exception process: Submit exception request documenting specific workflow requirements that preclude dual-layer caching
- Exception process: Architecture review board evaluates performance and reliability tradeoffs of proposed alternative approach
- Exception process: Approved exceptions require explicit documentation in workflow module and monitoring plan for alternative state management strategy