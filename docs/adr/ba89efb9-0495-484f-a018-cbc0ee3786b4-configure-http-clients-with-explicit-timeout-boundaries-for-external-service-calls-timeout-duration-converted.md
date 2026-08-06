# Configure HTTP Clients with Explicit Timeout Boundaries for External Service Calls: Timeout Duration Converted

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Activation

This ADR is ALWAYS ACTIVE and governs all HTTP client instantiation for external service communication.

## Context

- The system performs HTTP-based health checks, monitoring, and external service evaluation across multiple handlers and job execution contexts
- External service calls introduce unbounded latency risk without explicit timeout configuration, potentially blocking goroutines indefinitely
- The codebase integrates exponential backoff retry logic for resilience, requiring coordinated timeout boundaries to prevent cascading delays
- Multiple handler implementations instantiate HTTP clients with varying timeout strategies, creating inconsistent failure behavior across the system

## Problem Statement

Without explicit timeout configuration on HTTP clients used for external service communication, the system risks resource exhaustion, unbounded request latency, and unpredictable failure modes when downstream services become unresponsive or experience degraded performance.

## Decision

1. MUST: Timeout duration MUST be converted from the configuration unit to the standard library duration type using the appropriate constructor

## Policy Block

- MUST Timeout duration MUST be converted from the configuration unit to the standard library duration type using the appropriate constructor

In scope:
- All HTTP client instantiation for external service health checks
- All HTTP client instantiation for monitoring job execution
- All HTTP client instantiation for assertion evaluation against external endpoints
- All HTTP client instantiation for region ping operations

Out of scope:
- Internal service-to-service communication using non-HTTP protocols
- HTTP server configuration and listener timeout settings
- Database client connection timeout configuration
- Message queue or event stream client timeout settings

## Rationale

- The IR evidence shows consistent pattern of HTTP client instantiation with explicit Timeout field configuration across three distinct handler and job contexts
- All observed instances convert timeout values from millisecond or second units to duration types, indicating deliberate timeout boundary enforcement
- The pattern co-occurs with exponential backoff retry logic in the same files, demonstrating coordinated resilience strategy
- Explicit timeout configuration prevents goroutine leaks and resource exhaustion when external services experience degraded performance or network partitions

## Consequences

Positive:
- Bounded resource consumption prevents goroutine exhaustion under external service degradation scenarios
- Predictable failure modes enable consistent error handling and observability across handler contexts
- Coordinated timeout and retry configuration prevents cascading delays in monitoring and health check workflows
- Request-scoped timeout parameterization enables per-endpoint tuning based on expected service characteristics

Negative:
- Aggressive timeout values may cause false negatives for legitimately slow external services during high load periods
- Timeout configuration proliferation across multiple handlers increases maintenance burden and configuration drift risk
- Timeout-retry interaction complexity requires careful tuning to avoid premature abandonment or excessive retry attempts

## Alternatives

- Use default HTTP client with no explicit timeout configuration (rejected)
  Rejected because: Default HTTP client has no timeout, allowing indefinite blocking on unresponsive external services and risking goroutine exhaustion
  When valid: Never valid for production external service communication
- Configure timeout at context deadline level only, relying on request context cancellation (rejected)
  Rejected because: Context cancellation does not prevent HTTP client from establishing and holding connections; explicit client timeout provides defense-in-depth
  When valid: May be used in conjunction with client timeout for additional cancellation signal propagation
- Use shared singleton HTTP client with global timeout configuration (deferred)
  Rejected because: Would simplify client lifecycle management but prevents per-request timeout tuning observed in current evidence
  When valid: Valid if timeout requirements become uniform across all external service call contexts

## Risks

- Timeout values may become stale as external service performance characteristics evolve, causing increased false positive timeout errors
  Mitigation: Implement timeout value observability and alerting on timeout error rate trends; establish periodic timeout tuning review process
  Owner: engineering team
- Inconsistent timeout configuration across handlers may create confusing failure behavior for operators debugging external service issues
  Mitigation: Document timeout rationale per handler context; centralize timeout constant definitions with explanatory comments
  Owner: engineering team
- Timeout-retry interaction may cause total request latency to exceed acceptable bounds when retry count and timeout are not coordinated
  Mitigation: Establish and enforce maximum total elapsed time budget that accounts for retry attempts multiplied by per-attempt timeout
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
- Locate all HTTP client instantiation sites in handler and job execution code; verify each instantiation includes explicit Timeout field configuration
- Extract timeout configuration parameters from request structures or configuration constants; ensure timeout values are not duplicated as magic numbers across files
- Review timeout values in conjunction with retry backoff configuration to ensure maximum total elapsed time remains within acceptable service-level bounds
- Add observability instrumentation to track timeout error rates per handler context, enabling data-driven timeout tuning decisions

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis verification to confirm all HTTP client instantiations include explicit Timeout field configuration
- Discover and execute the project's integration test suite to verify timeout behavior under simulated external service latency scenarios
- Discover and execute the project's configuration validation to ensure timeout parameters are present in all required handler and job contexts

Accept when:
- All HTTP client instantiations for external service communication include explicit Timeout field configuration derived from parameters or constants
- No HTTP client instantiation uses default zero-value timeout or omits Timeout field
- Integration tests demonstrate timeout enforcement under simulated unresponsive external service conditions

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline scanning for HTTP client instantiation patterns
- Verified by: Code review checklist requiring explicit timeout verification for all external service communication changes
- Verified by: Integration test coverage requirements for timeout behavior in handler and job execution paths
- Violation handling: Static analysis failures block merge until HTTP client timeout configuration is added
- Violation handling: Code review rejection for any external service communication lacking explicit timeout boundaries
- Violation handling: Post-deployment monitoring alerts on timeout error rate anomalies trigger incident review
- Exception process: Exception requests must document specific external service characteristics justifying alternative timeout strategy
- Exception process: Architecture review approval required for any HTTP client instantiation without explicit timeout
- Exception process: Approved exceptions must include compensating controls such as circuit breaker or bulkhead isolation