# Standardize HTTP Client Configuration for External Service Boundaries: Http Clients Configure

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- HTTP clients are instantiated at external service boundaries with explicit timeout configurations to prevent unbounded resource consumption
- Error handling at external client boundaries consistently logs error details using structured logging methods
- Request and response headers are manipulated to establish service identity and content negotiation contracts
- Retry logic with exponential backoff is applied at external client boundaries to handle transient failures
- Context propagation through HTTP clients enables distributed tracing and request lifecycle management

## Problem Statement

External HTTP client interactions lack standardized configuration patterns for timeouts, error handling, retry behavior, and observability, leading to inconsistent failure modes, difficult debugging, and unpredictable resource consumption across service boundaries.

## Decision

1. MAY: HTTP clients MAY configure custom TLS settings when communicating with external services requiring specific security parameters

## Policy Block

- MAY HTTP clients MAY configure custom TLS settings when communicating with external services requiring specific security parameters

In scope:
- HTTP client instantiation for external service communication
- Error logging at service boundary crossings
- Request header manipulation for service identification
- Retry and backoff logic for external calls
- Timeout configuration for outbound HTTP requests

Out of scope:
- Internal service-to-service communication within the same process
- Database client configuration
- Message queue or event stream clients
- File system or local resource access
- WebSocket or streaming protocol clients

## Rationale

- The evidence shows consistent patterns of HTTP client instantiation with explicit timeout configuration across 6 files, indicating a deliberate approach to resource management at external boundaries
- Structured error logging using methods that accept error context appears at every external client boundary, enabling operational visibility into failure modes
- User-Agent header manipulation and custom header propagation demonstrate service identity establishment as a cross-cutting concern
- Exponential backoff retry logic appears in multiple handlers, indicating recognition of transient failure patterns in distributed systems

## Consequences

Positive:
- Explicit timeout configuration prevents cascading failures and resource exhaustion when external services become unresponsive
- Structured error logging at boundaries provides consistent operational visibility and debugging capability across all external integrations
- Standardized retry behavior with exponential backoff improves system resilience against transient network and service failures
- Context propagation enables distributed tracing and request correlation across service boundaries

Negative:
- Timeout values must be carefully tuned per service to balance responsiveness against legitimate long-running operations
- Retry logic adds complexity and can amplify load on already-stressed downstream services if not properly bounded
- Structured logging at every boundary increases log volume and storage requirements
- Explicit client configuration at each boundary creates maintenance overhead when timeout or retry policies need adjustment

## Alternatives

- Use global default HTTP client without explicit timeout configuration (rejected)
  Rejected because: Global defaults provide no protection against unbounded blocking on unresponsive external services, leading to resource exhaustion and cascading failures
  When valid: Only appropriate for internal development or testing environments where external service reliability is guaranteed
- Implement circuit breaker pattern instead of simple retry with backoff (deferred)
  Rejected because: Circuit breakers add significant complexity and state management overhead; current retry approach provides sufficient resilience for observed failure patterns
  When valid: Should be reconsidered when external service failure rates exceed thresholds where retry amplification becomes problematic
- Centralize HTTP client configuration through dependency injection or factory pattern (deferred)
  Rejected because: Current inline configuration provides explicit visibility at each boundary; centralization would require additional abstraction layers
  When valid: Becomes valuable when timeout and retry policies need dynamic adjustment or when client count exceeds maintainability thresholds

## Risks

- Inconsistent timeout values across different external service boundaries may lead to unpredictable failure cascades when multiple services experience degradation simultaneously
  Mitigation: Document timeout selection rationale for each external service and establish guidelines for timeout value selection based on service SLAs
  Owner: engineering team
- Retry logic with exponential backoff can amplify load on downstream services during incidents, potentially prolonging outages
  Mitigation: Implement maximum retry limits and consider jitter in backoff calculations to distribute retry load temporally
  Owner: engineering team
- Structured logging at every external boundary may generate excessive log volume under high traffic, increasing storage costs and reducing signal-to-noise ratio
  Mitigation: Implement log sampling or rate limiting for successful operations while preserving full logging for error conditions
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
- When instantiating HTTP clients for external service calls, configure timeout values based on the expected response time characteristics of the target service plus a reasonable buffer for network latency
- Implement retry logic using exponential backoff libraries that support context-aware cancellation to prevent retry storms during graceful shutdown scenarios
- Structure error logs to include request identifiers, target service URLs, HTTP status codes, and error messages to enable correlation across distributed traces

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit timeout configuration
- Discover and execute the project's test suite to validate that error logging occurs at external client boundaries with structured context
- Discover and execute the project's integration tests to verify retry behavior with exponential backoff under simulated transient failure conditions

Accept when:
- All HTTP client instantiations for external service communication include explicit timeout configuration
- Error responses from external HTTP clients are logged with structured context including error details
- User-Agent headers are set on outbound requests to identify the calling service

## Enforcement

- Verified by: Static analysis scanning for HTTP client instantiation patterns without timeout configuration
- Verified by: Code review checklist verification for external service boundary implementations
- Verified by: Integration test coverage requirements for retry and timeout behavior
- Violation handling: Pull requests introducing HTTP clients without explicit timeouts are blocked until configuration is added
- Violation handling: Missing error logging at external boundaries triggers code review feedback requiring structured logging implementation
- Violation handling: Integration tests failing retry or timeout validation prevent deployment to production environments
- Exception process: Exceptions for timeout configuration require architectural review and documentation of alternative resource management strategy
- Exception process: Exceptions for error logging require justification based on sensitivity or compliance constraints
- Exception process: All exceptions must be documented in code comments with approval reference and expiration review date