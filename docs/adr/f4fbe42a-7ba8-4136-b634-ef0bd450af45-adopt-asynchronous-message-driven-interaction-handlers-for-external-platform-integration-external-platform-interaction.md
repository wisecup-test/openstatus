# Adopt Asynchronous Message-Driven Interaction Handlers for External Platform Integration: External Platform Interaction

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is always active and governs all agent behavior when implementing external platform integration handlers.

## Context

- External platform integrations require handling asynchronous user interactions with stateful workflows that span multiple request-response cycles
- Integration handlers must coordinate between ephemeral HTTP requests and persistent state stores to maintain conversation context across platform API calls
- Platform-specific SDKs provide asynchronous APIs for updating UI elements and managing conversation threads, requiring non-blocking execution patterns
- Temporary action state must be cached with expiration policies to prevent stale interactions while supporting concurrent user actions across multiple threads

## Problem Statement

External platform integrations receive user interactions as discrete HTTP events, but must maintain stateful workflows across multiple asynchronous operations including state retrieval, validation, business logic execution, and platform API calls to update UI elements. Without a consistent concurrency model, handlers risk blocking operations, race conditions in state management, and inconsistent error handling across the integration surface.

## Decision

1. MUST: All external platform interaction handlers must implement asynchronous function signatures to support non-blocking execution of state retrieval, validation, and platform API calls

## Policy Block

- MUST All external platform interaction handlers must implement asynchronous function signatures to support non-blocking execution of state retrieval, validation, and platform API calls

In scope:
- HTTP route handlers receiving external platform interaction webhooks
- State management operations for temporary interaction data with expiration policies
- Platform SDK API calls for updating conversation UI elements and thread messages
- Validation logic for inbound platform payloads and cached state schemas

Out of scope:
- Synchronous request-response APIs without stateful workflow requirements
- Long-running background jobs managed by dedicated task queues
- Real-time streaming connections or WebSocket handlers
- Internal service-to-service RPC calls without external platform constraints

## Rationale

- The evidence shows three integration handler files implementing asynchronous functions with consistent patterns: state retrieval from cache, validation, execution, and platform API updates
- Cache layer operations use time-to-live expiration and atomic consume operations, indicating awareness of race conditions and stale state risks in concurrent environments
- Platform SDK calls are awaited and wrapped in error handling that triggers compensating UI updates, demonstrating non-blocking execution with failure recovery
- The separation of memory-backed and cache-backed state stores with identical interfaces indicates intentional support for both production and testing scenarios

## Consequences

Positive:
- Non-blocking execution prevents request timeouts and improves throughput for concurrent user interactions across multiple conversation threads
- Atomic state consumption with expiration prevents duplicate processing of the same user action and automatically cleans up stale interaction data
- Consistent error handling with compensating platform API calls provides clear user feedback when operations fail
- Interface-based state store abstraction enables testing without external cache dependencies while maintaining production behavior

Negative:
- Asynchronous execution increases code complexity and requires careful error propagation across multiple await boundaries
- Cache layer dependency introduces operational complexity and potential failure modes requiring fallback strategies
- Time-to-live expiration policies require tuning to balance memory usage against user experience for slow interactions
- Compensating platform API calls for error states add latency to failure paths and may themselves fail, requiring nested error handling

## Alternatives

- Synchronous blocking handlers with in-memory state (rejected)
  Rejected because: Platform SDK APIs are inherently asynchronous and blocking would cause request timeouts; in-memory state does not survive process restarts or scale across multiple instances
  When valid: Single-instance development environments with no production traffic requirements
- Event-driven architecture with message queue for all interactions (rejected)
  Rejected because: Platform webhook contracts require synchronous HTTP acknowledgment within timeout windows; queueing would violate platform API requirements and delay user feedback
  When valid: Platforms that support asynchronous webhook acknowledgment with separate callback URLs
- Hybrid approach with synchronous acknowledgment and asynchronous background processing (deferred)
  Rejected because: Current interaction complexity does not justify the operational overhead of background workers; may be reconsidered if processing time exceeds platform timeout limits
  When valid: Interactions requiring long-running operations that exceed platform webhook timeout constraints

## Risks

- Cache layer outages cause all interaction handlers to fail, preventing users from completing any actions through the platform integration
  Mitigation: Implement circuit breaker pattern with fallback to synchronous error responses; monitor cache availability and alert on degradation; consider read-through cache with database backing for critical state
  Owner: Platform Integration Team
- Race conditions between concurrent interactions on the same thread may cause state corruption if cache operations are not truly atomic
  Mitigation: Verify cache layer provides atomic compare-and-swap or transaction semantics; implement optimistic locking with version numbers in cached state; add integration tests for concurrent access patterns
  Owner: Engineering Team
- Unhandled promise rejections in asynchronous handlers may cause silent failures without user notification or error logging
  Mitigation: Wrap all async handler entry points in try-catch blocks; implement global unhandled rejection handlers; ensure all platform API error responses trigger compensating UI updates
  Owner: Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define state store interfaces with put, get, consume, and findByThread operations; implement both cache-backed and memory-backed variants for production and testing
- Structure handlers with clear phases: parse and validate inbound payload, retrieve cached state, execute business logic, update platform UI, consume state atomically
- Configure cache expiration policies based on expected user interaction latency; monitor expired action rates to detect if TTL is too aggressive
- Implement structured error logging with context including thread identifiers, user identifiers, and action types to support debugging of asynchronous failures

## Continuation Context


Verify commands:
- Discover the project's integration test suite and execute tests covering concurrent interaction handling with cache-backed state stores
- Locate the project's static analysis configuration and verify it enforces async function signatures for all route handlers
- Identify the project's error tracking configuration and confirm unhandled promise rejection handlers are registered at application startup

Accept when:
- All integration handler functions use async signatures and await all cache operations and platform API calls
- State store implementations provide atomic consume operations that delete state after retrieval
- Error handling in handlers triggers compensating platform API calls to update UI with failure messages
- Integration tests demonstrate correct behavior under concurrent interactions on the same conversation thread

## Enforcement

- Verified by: Static analysis rules enforcing async function signatures for route handlers
- Verified by: Code review checklist requiring verification of error handling and compensating API calls
- Verified by: Integration test suite coverage requirements for concurrent interaction scenarios
- Violation handling: Pull requests failing static analysis checks are blocked from merge
- Violation handling: Code review identifies missing error handling or synchronous blocking operations and requests changes
- Violation handling: Production incidents caused by synchronous handlers or missing error handling trigger post-incident reviews and remediation
- Exception process: Exceptions require architectural review demonstrating that synchronous execution meets platform timeout requirements and does not impact throughput
- Exception process: Exception approval must include monitoring plan for request latency and error rates
- Exception process: Approved exceptions are documented in code comments with rationale and re-evaluation criteria