# Adopt Multi-Layer Caching with Rate Limiting for Workflow Reliability: Data Retrieved Persistent

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active and governs all workflow monitoring implementations that require reliability guarantees through caching and rate limiting.

## Context

- Workflow monitoring systems require reliable state management across distributed components to prevent data loss and ensure consistent execution
- High-frequency workflow operations demand rate limiting to protect downstream services from overload while maintaining responsiveness
- Multi-layer caching strategies separate transient in-memory state from persistent distributed cache to optimize read performance and reduce latency
- Cloud-based task orchestration requires integration with external queue services while maintaining local state coherence
- Schema validation at cache boundaries ensures data integrity when persisting workflow state across system restarts

## Problem Statement

Workflow monitoring systems must maintain reliable state across distributed components while handling high-frequency operations without overwhelming downstream services. Without coordinated caching and rate limiting, systems risk data inconsistency, service degradation, and workflow execution failures during peak load or transient failures.

## Decision

1. MUST: All data retrieved from persistent storage MUST be validated against defined schemas before insertion into cache layers

## Policy Block

- MUST All data retrieved from persistent storage MUST be validated against defined schemas before insertion into cache layers

In scope:
- Workflow monitoring systems with multi-day execution windows
- Cron-triggered or scheduled workflow orchestration
- Systems integrating cloud task queue services
- High-frequency state synchronization between database and cache
- Rate-limited API operations protecting downstream services

Out of scope:
- Stateless request-response workflows without persistence requirements
- Single-tier caching architectures without distributed cache
- Workflows with execution windows under one hour
- Systems without rate limiting requirements

Exceptions:
- EXC-001: Workflow operations are read-only and do not modify state
- EXC-002: Development or testing environments with synthetic load

## Rationale

- Evidence shows coordinated use of in-memory Map structures and distributed cache with explicit 30-day expiration policies, indicating deliberate separation of hot and cold data paths
- Rate limiter configuration with 15 tokens per second interval demonstrates proactive protection against service overload during workflow execution bursts
- Schema validation using parse operations on workspace data ensures type safety at cache boundaries, preventing corruption from schema drift
- Integration with cloud task queue services and multi-step workflow coordination indicates requirements for reliable asynchronous execution across extended time windows

## Consequences

Positive:
- Reduced latency for frequently accessed workflow state through in-memory caching
- Protection of downstream services from overload through token bucket rate limiting
- Data integrity guarantees through schema validation at cache boundaries
- Resilience to transient failures through persistent distributed cache with explicit TTLs
- Scalable workflow orchestration through separation of coordination logic and state management

Negative:
- Increased system complexity from managing two cache tiers with different consistency guarantees
- Potential cache coherence issues if invalidation logic is not correctly implemented across tiers
- Rate limiting may introduce artificial delays during legitimate traffic spikes
- Memory pressure from in-memory cache growth if eviction policies are not properly configured
- Operational overhead from monitoring cache hit rates, rate limit violations, and expiration policies

## Alternatives

- Single-tier distributed cache only without in-memory layer (rejected)
  Rejected because: Network latency for every cache access would degrade workflow responsiveness and increase load on distributed cache service
  When valid: Valid for workflows with infrequent state access where latency is not critical
- Database-only state management without caching (rejected)
  Rejected because: Database query latency and connection pool exhaustion would limit throughput for high-frequency workflow operations
  When valid: Valid for low-frequency workflows with simple state requirements and no rate limiting needs
- Event sourcing with append-only log for workflow state (deferred)
  Rejected because: Adds complexity of event replay and projection maintenance without clear evidence of audit or time-travel requirements
  When valid: Valid when workflow history reconstruction or audit trails are explicit requirements

## Risks

- Cache stampede during distributed cache failures causing database overload
  Mitigation: Implement circuit breakers and fallback logic to degrade gracefully when distributed cache is unavailable
  Owner: Engineering team
- Rate limiter state inconsistency in multi-instance deployments leading to effective rate limit multiplication
  Mitigation: Use distributed rate limiting with shared state or accept per-instance limits with documented aggregate behavior
  Owner: Engineering team
- Schema validation failures blocking workflow execution after database schema migrations
  Mitigation: Implement schema versioning and backward-compatible validation with graceful degradation paths
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
- Configure cache expiration policies based on workflow execution windows, ensuring TTL exceeds maximum expected workflow duration with safety margin
- Implement cache key naming conventions that encode entity hierarchy to enable efficient pattern-based invalidation during bulk operations
- Monitor rate limiter rejection rates and adjust token bucket parameters based on observed traffic patterns and downstream service capacity
- Establish cache warming strategies for predictable workflow schedules to prevent cold-start latency spikes

## Continuation Context


Verify commands:
- Discover the project test suite and execute integration tests covering cache tier coordination and rate limiting behavior
- Discover the project linting configuration and execute static analysis to verify cache expiration policies are explicitly set on all distributed cache writes
- Discover the project monitoring configuration and verify metrics collection for cache hit rates, rate limit violations, and schema validation failures

Accept when:
- All workflow state writes to distributed cache include explicit expiration configuration
- Rate limiting is applied to workflow operations with documented token bucket parameters
- Schema validation is performed on all data retrieved from persistent storage before cache insertion
- Integration tests demonstrate correct behavior under cache failure and rate limit scenarios

## Enforcement

- Verified by: Automated integration tests in continuous integration pipeline
- Verified by: Static analysis rules checking for cache expiration policy presence
- Verified by: Code review checklist items for rate limiting and schema validation
- Verified by: Runtime monitoring alerts for cache coherence violations and rate limit breaches
- Violation handling: CI pipeline fails if integration tests for cache and rate limiting do not pass
- Violation handling: Pull requests blocked if static analysis detects missing cache expiration policies
- Violation handling: Production alerts trigger incident response for cache coherence or rate limit violations
- Violation handling: Post-incident reviews required for any workflow reliability failures
- Exception process: Request exception through architecture review board with documented rationale
- Exception process: Provide evidence that alternative approach meets reliability requirements
- Exception process: Document exception in workflow configuration with approval reference and expiration date
- Exception process: Schedule follow-up review to assess exception validity after implementation