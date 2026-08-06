# Adopt Cache Layer for Event Data in Handler Functions: Cache Get Operations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Handler functions in the checker application process HTTP, TCP, and DNS health check requests that require access to event metadata across multiple operations within a single request lifecycle
- Event data is retrieved and stored using cache operations (Get and Set) to avoid redundant database queries or external service calls during request processing
- The cache layer provides a request-scoped storage mechanism that persists data between handler initialization and response generation
- Query parameters are extracted from incoming requests and used to populate or retrieve cached event data, establishing a pattern of cache-first data access

## Problem Statement

Handler functions require efficient access to event metadata throughout the request lifecycle without incurring the cost of repeated database queries or external service calls, while maintaining consistency of event data across multiple operations within a single request context.

## Decision

1. SHOULD: Cache Get operations SHOULD return nil or empty values when the requested key does not exist, allowing handlers to distinguish between cache hits and misses

## Policy Block

- SHOULD Cache Get operations SHOULD return nil or empty values when the requested key does not exist, allowing handlers to distinguish between cache hits and misses

In scope:
- HTTP checker handler functions
- TCP checker handler functions
- DNS checker handler functions
- Event metadata storage and retrieval
- Request-scoped data caching

Out of scope:
- Long-term persistent storage
- Cross-request data sharing
- Distributed caching across service instances
- Cache invalidation strategies
- Cache eviction policies

## Rationale

- The pattern is observed across three handler files (checker.go, tcp.go, dns.go) with consistent cache operations (Get, Set, Query) indicating a deliberate architectural choice for request-scoped data management
- Cache layer usage reduces redundant data retrieval operations within a single request lifecycle, improving handler performance and reducing load on upstream data sources
- Request-scoped caching provides a clean separation between transient request data and persistent storage, simplifying handler logic and reducing coupling to database or external service implementations
- The pattern supports resilience strategies (backoff.Retry) by ensuring event data remains consistent across retry attempts within the same request context

## Consequences

Positive:
- Reduced latency for handler operations by eliminating redundant data retrieval calls within a single request
- Simplified handler logic by providing a consistent interface for accessing event metadata without direct database or service coupling
- Improved request consistency by ensuring all operations within a request lifecycle work with the same event data snapshot
- Enhanced testability by allowing cache layer to be mocked or stubbed independently of persistent storage

Negative:
- Increased memory footprint per request due to cache storage overhead
- Potential for stale data if cache entries are not properly invalidated when upstream data changes during request processing
- Additional complexity in understanding data flow as developers must track both cache and persistent storage interactions
- Risk of cache key collisions if naming conventions are not strictly enforced across handler implementations

## Alternatives

- Direct database or service calls for each data access operation (rejected)
  Rejected because: Would result in redundant queries within a single request lifecycle, increasing latency and load on upstream systems, particularly problematic when combined with retry logic that could amplify the number of calls
  When valid: When data consistency requirements mandate real-time reads or when request processing is guaranteed to access each data item exactly once
- Pass event data as function parameters through the handler call chain (rejected)
  Rejected because: Would require modifying function signatures across the handler stack and increase coupling between handler layers, making the code less maintainable and harder to extend with new data requirements
  When valid: When the handler call chain is shallow and the set of required data is stable and well-defined
- Use a distributed cache shared across service instances (rejected)
  Rejected because: Introduces network latency and operational complexity for data that only needs to persist for the duration of a single request, and does not provide the request-scoped isolation required by the handler pattern
  When valid: When event data needs to be shared across multiple service instances or persisted beyond individual request lifecycles

## Risks

- Cache key naming collisions between different handler types or data types could cause data corruption or incorrect handler behavior
  Mitigation: Establish and enforce a cache key naming convention that includes data type prefixes or namespaces, and implement cache key validation in handler initialization code
  Owner: engineering team
- Memory leaks if cache entries are not properly cleaned up after request completion, particularly in long-running service instances processing high request volumes
  Mitigation: Ensure cache implementation provides automatic cleanup on request completion and implement monitoring for memory usage patterns correlated with request volume
  Owner: engineering team
- Inconsistent cache behavior across different handler implementations if cache operations are not standardized, leading to subtle bugs and maintenance challenges
  Mitigation: Create shared cache access utilities or interfaces that enforce consistent Get, Set, and Query patterns across all handler implementations
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
- Implement cache Get operations at the beginning of handler functions to check for existing event data before performing expensive retrieval operations, and handle nil or empty return values appropriately
- Use cache Set operations immediately after retrieving or computing event data from upstream sources to ensure subsequent operations within the same request can access the cached data
- Establish a consistent cache key naming convention across all handler types to prevent collisions and improve code maintainability, documenting the convention in handler implementation guidelines

## Continuation Context


Verify commands:
- Discover the project's test suite location and execute handler tests that verify cache Get operations return expected values for known cache keys
- Discover the project's test suite location and execute handler tests that verify cache Set operations persist data accessible by subsequent Get operations within the same request context
- Discover the project's static analysis or linting configuration and execute checks that verify cache operations follow established key naming conventions

Accept when:
- All handler tests pass, demonstrating that cache Get and Set operations correctly store and retrieve event data within request lifecycles
- Static analysis confirms that cache key names follow the established naming convention across all handler implementations
- Integration tests verify that handlers using cache operations exhibit reduced latency compared to direct data retrieval approaches

## Enforcement

- Verified by: Automated test suite execution in continuous integration pipeline verifying cache operation correctness
- Verified by: Code review checklist items requiring verification of cache Get and Set patterns in new handler implementations
- Verified by: Static analysis tools checking for cache key naming convention compliance
- Violation handling: Pull requests introducing handler code that bypasses cache layer for event data retrieval are rejected during code review
- Violation handling: Test failures indicating incorrect cache operation usage block merge to main branch
- Violation handling: Static analysis violations for cache key naming conventions trigger build failures
- Exception process: Exceptions require written justification documenting why direct data access is necessary and why cache layer cannot satisfy the requirement
- Exception process: Architecture review board approval required for exceptions that introduce alternative data access patterns
- Exception process: Approved exceptions must be documented in code comments explaining the rationale and any compensating controls