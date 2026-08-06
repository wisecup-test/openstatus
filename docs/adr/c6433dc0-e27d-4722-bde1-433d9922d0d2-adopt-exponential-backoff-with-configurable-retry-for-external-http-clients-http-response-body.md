# Adopt Exponential Backoff with Configurable Retry for External HTTP Clients: Http Response Body

Status: proposed
Date: 2025-01-17
Deciders: Detection Pipeline (automated)

## Context

- HTTP monitoring and health-check services require resilient external client behavior to handle transient network failures, service unavailability, and timeout conditions without immediate failure propagation.
- The checker service performs HTTP assertions against external endpoints with configurable timeout constraints, requiring retry logic that respects both operation-level timeout budgets and request-level timeout parameters.
- Integration testing workflows depend on reliable HTTP client behavior that can distinguish between permanent failures and transient errors, enabling accurate service health evaluation.
- The codebase demonstrates consistent use of exponential backoff retry patterns across multiple HTTP client instantiation sites, with timeout configuration derived from monitor or request specifications.

## Problem Statement

External HTTP clients in monitoring and health-check workflows must handle transient failures gracefully while respecting timeout constraints, but naive retry implementations can amplify cascading failures, exhaust timeout budgets prematurely, or mask permanent errors as transient conditions.

## Decision

1. SHOULD: HTTP response body content should be captured and made available for assertion evaluation regardless of retry attempt count.

## Policy Block

- SHOULD HTTP response body content should be captured and made available for assertion evaluation regardless of retry attempt count.

In scope:
- HTTP clients instantiated for external endpoint monitoring
- Health-check handlers that evaluate HTTP assertions
- Ping and region availability verification workflows
- Integration test scenarios that exercise HTTP request/response cycles

Out of scope:
- Internal service-to-service communication within the same trust boundary
- Database client connections and query retry logic
- Message queue consumer retry behavior
- File system I/O operations

## Rationale

- The evidence shows consistent application of exponential backoff retry patterns across three distinct handler and job implementations, indicating an established architectural pattern for resilience.
- Timeout configuration is consistently derived from monitor or request specifications and converted to millisecond-based duration types, demonstrating a standardized approach to timeout budget management.
- The pattern separates retry orchestration from HTTP client instantiation, allowing independent configuration of connection timeout and retry behavior while maintaining clear boundaries between external client concerns and business logic.
- Integration testing workflows depend on reliable HTTP client behavior that captures response bodies and error conditions across retry attempts, enabling accurate assertion evaluation and failure diagnosis.

## Consequences

Positive:
- Transient network failures and temporary service unavailability are handled gracefully without immediate failure propagation to monitoring dashboards or alerting systems.
- Exponential backoff reduces load on struggling downstream services by spacing retry attempts with increasing delays, preventing retry storms.
- Configurable retry counts enable operation-specific tuning of resilience behavior based on SLA requirements and timeout budgets.
- Consistent retry patterns across handlers and jobs simplify testing, debugging, and operational reasoning about failure modes.

Negative:
- Exponential backoff increases end-to-end latency for operations that ultimately fail, potentially delaying error detection and alerting.
- Retry logic adds complexity to timeout budget calculation, requiring careful coordination between per-request timeouts and total operation deadlines.
- Aggressive retry configurations can mask permanent failures as transient conditions, delaying incident response and root cause analysis.
- Context cancellation and deadline propagation must be carefully implemented to prevent retry operations from exceeding caller timeout expectations.

## Alternatives

- Implement fixed-interval retry with constant delay between attempts (rejected)
  Rejected because: Fixed-interval retry does not provide backpressure relief for struggling downstream services and can contribute to cascading failure scenarios by maintaining constant request rate during outages.
  When valid: May be appropriate for internal service-to-service communication within the same trust boundary where retry storms are not a concern.
- Delegate retry logic to external infrastructure components such as service mesh or API gateway (rejected)
  Rejected because: The checker service requires application-level control over retry behavior to coordinate timeout budgets, capture response bodies across attempts, and evaluate assertions with full visibility into retry history.
  When valid: Valid for stateless request forwarding scenarios where application-level retry context is not required.
- Implement circuit breaker pattern to fail fast after detecting sustained failure rates (deferred)
  Rejected because: Circuit breaker pattern complements exponential backoff but requires additional state management and failure rate tracking infrastructure not present in current evidence.
  When valid: Should be considered as a future enhancement when monitoring multiple endpoints with shared failure domains.

## Risks

- Misconfigured retry counts or timeout parameters can cause operations to exceed caller deadline expectations, resulting in cascading timeout failures.
  Mitigation: Implement comprehensive timeout budget validation in configuration parsing and enforce maximum retry count limits based on operation-level SLA requirements.
  Owner: engineering team
- Exponential backoff implementations may not respect context cancellation signals, causing retry operations to continue after caller has abandoned the request.
  Mitigation: Use context-aware retry functions that check cancellation status before each retry attempt and propagate cancellation errors immediately.
  Owner: engineering team
- Retry logic that captures and evaluates response bodies across multiple attempts may consume excessive memory for large response payloads.
  Mitigation: Implement response body size limits and streaming evaluation for large payloads, with early termination on assertion failure.
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
- HTTP client instantiation should occur within the retry operation closure to ensure each retry attempt uses a fresh client instance with independent timeout tracking.
- Timeout parameters sourced from monitor or request specifications should be validated against minimum and maximum bounds before conversion to duration types to prevent misconfiguration.
- Error logging within retry operations should include structured context fields such as operation identifiers, retry attempt counts, and elapsed time to facilitate debugging and operational analysis.

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the build tool, then locate and inspect the lock or resolution artifact to determine exact resolved versions of retry and HTTP client dependencies.
- Discover the project's test execution tooling and run integration test suites that exercise HTTP client retry behavior with simulated transient failures and timeout conditions.
- Discover the project's static analysis or linting configuration and verify that HTTP client instantiation sites include exponential backoff retry wrappers with configurable retry counts.

Accept when:
- All HTTP clients that invoke external endpoints for monitoring or health-check operations are wrapped in exponential backoff retry mechanisms with configurable maximum retry counts.
- Integration tests demonstrate successful retry behavior for transient failures and proper timeout budget enforcement across retry attempts.
- Error logging captures retry attempt context and operation identifiers for all failed HTTP invocations.

## Enforcement

- Verified by: Automated integration tests that exercise HTTP client retry behavior with simulated failure scenarios
- Verified by: Code review verification that new HTTP client instantiation sites include exponential backoff retry wrappers
- Verified by: Static analysis rules that detect HTTP client instantiation without retry mechanisms
- Violation handling: Pull requests introducing HTTP clients without retry mechanisms are blocked until retry logic is added
- Violation handling: Existing violations are tracked as technical debt items and prioritized based on endpoint criticality and failure rate history
- Violation handling: Production incidents involving unhandled transient failures trigger immediate remediation and retry logic addition
- Exception process: Exception requests must document the specific endpoint characteristics that make retry behavior inappropriate
- Exception process: Exceptions require approval from both the service owner and the platform reliability team
- Exception process: Approved exceptions are documented in code comments with rationale and reviewed quarterly for continued validity