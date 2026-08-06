# Isolate External HTTP Client Construction in Boundary Layer: Request Preparation Logic

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system performs HTTP health checks and monitoring operations against external endpoints, requiring configurable timeout behavior and retry logic.
- HTTP client construction is distributed across multiple handlers and job execution paths, with timeout values derived from request parameters or monitor configuration.
- Integration testing requires access to response bodies and headers to validate assertions and evaluate success criteria.
- External client boundaries must be testable in isolation to verify timeout enforcement, header propagation, and error handling without invoking real network calls.
- The codebase uses exponential backoff retry patterns coordinated with HTTP client timeout configuration to ensure resilience.

## Problem Statement

HTTP client construction is embedded directly in handler and job execution logic, coupling timeout configuration, header management, and transport settings to business logic. This makes it difficult to test timeout enforcement, retry coordination, and header propagation in isolation, and prevents consistent application of security policies such as TLS configuration and user-agent identification across all external HTTP calls.

## Decision

1. SHOULD: Request preparation logic including header setting and body serialization SHOULD be separated from client construction to enable independent testing of each concern.

## Policy Block

- SHOULD Request preparation logic including header setting and body serialization SHOULD be separated from client construction to enable independent testing of each concern.

In scope:
- All HTTP requests to external monitoring targets
- All HTTP requests to third-party APIs
- Ping and health check operations
- HTTP job execution paths

Out of scope:
- Internal service-to-service communication using framework-provided clients
- HTTP server configuration and request handling
- WebSocket or streaming protocol clients

## Rationale

- Six files across checker handlers, HTTP job execution, and client modules demonstrate inline HTTP client construction with timeout configuration, indicating a distributed pattern that couples transport concerns to business logic.
- Integration test evidence shows response body and header access patterns that require testable client boundaries to validate assertion evaluation and success criteria without network dependencies.
- Exponential backoff retry coordination with HTTP client timeouts requires consistent timeout application to prevent retry logic from exceeding client-level deadlines.
- Centralizing client construction in a boundary layer enables consistent application of security policies such as user-agent identification and TLS configuration across all external HTTP calls.

## Consequences

Positive:
- HTTP client behavior including timeout enforcement and header propagation can be tested in isolation using test doubles without invoking real network calls.
- Timeout configuration and retry logic coordination can be validated independently of handler and job execution paths.
- Security policies such as TLS settings and user-agent identification can be applied consistently across all external HTTP calls through a single construction point.
- Refactoring transport-level concerns such as connection pooling or tracing instrumentation requires changes only in the boundary layer.

Negative:
- Introduces an additional abstraction layer that must be maintained and documented.
- Requires refactoring existing handler and job execution code to use the boundary layer instead of inline client construction.
- May increase initial development complexity for simple HTTP operations that do not require custom configuration.
- Test doubles must accurately replicate production client behavior to avoid false confidence in integration tests.

## Alternatives

- Continue inline HTTP client construction in handlers and job execution paths (rejected)
  Rejected because: Couples transport configuration to business logic, making timeout enforcement and header propagation difficult to test in isolation and preventing consistent application of security policies.
  When valid: For prototypes or single-use scripts where testability and policy consistency are not required.
- Use a global singleton HTTP client with fixed timeout configuration (rejected)
  Rejected because: Prevents per-request timeout configuration required for monitoring operations with varying timeout requirements, and makes testing with different timeout scenarios difficult.
  When valid: For systems where all external HTTP calls have identical timeout and transport requirements.
- Inject HTTP client instances through dependency injection at handler initialization (deferred)
  When valid: Can be combined with the boundary layer approach to provide handler-level client customization while maintaining centralized construction logic.

## Risks

- Test doubles may not accurately replicate production HTTP client behavior, leading to false confidence in integration tests.
  Mitigation: Implement contract tests that verify test double behavior matches production client behavior for timeout enforcement, header propagation, and error handling.
  Owner: engineering team
- Boundary layer abstraction may not accommodate future transport-level requirements such as HTTP/2 or custom connection pooling.
  Mitigation: Design the boundary layer interface to accept extensible configuration objects rather than fixed parameter lists, enabling future transport options without interface changes.
  Owner: engineering team
- Refactoring existing code to use the boundary layer may introduce regressions in timeout behavior or header handling.
  Mitigation: Implement characterization tests that capture current behavior before refactoring, and verify equivalent behavior after boundary layer adoption.
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
- Identify all locations where HTTP client instances are constructed inline by searching for timeout configuration patterns and client instantiation. Extract construction logic into a factory function or interface in a dedicated boundary package.
- Design the boundary layer interface to return both the client instance and any associated cleanup functions to ensure proper resource management and connection pooling behavior.
- Implement test doubles that replicate production client behavior for timeout enforcement by tracking elapsed time and returning timeout errors when thresholds are exceeded, enabling integration tests to verify retry coordination without real network delays.

## Continuation Context


Verify commands:
- Locate the project's test execution script or build automation configuration and execute the integration test suite to verify HTTP client boundary layer behavior.
- Discover the project's static analysis or linting configuration and execute it to verify that HTTP client construction outside the boundary layer is flagged.
- Identify the project's code coverage reporting mechanism and verify that boundary layer client construction and test double implementations achieve the project's coverage threshold.

Accept when:
- All integration tests pass with test doubles injected at the boundary layer, verifying timeout enforcement and header propagation without real network calls.
- Static analysis confirms no HTTP client construction occurs outside the designated boundary layer.
- Code coverage for boundary layer client construction and test double implementations meets or exceeds the project's defined threshold.

## Enforcement

- Verified by: Continuous integration pipeline executes integration tests with boundary layer test doubles to verify isolation and timeout behavior.
- Verified by: Code review process verifies that new HTTP client usage follows boundary layer construction patterns.
- Verified by: Static analysis or linting rules flag inline HTTP client construction outside the boundary layer.
- Violation handling: Pull requests containing inline HTTP client construction outside the boundary layer are blocked until refactored.
- Violation handling: Existing violations are tracked as technical debt items and prioritized for refactoring based on test coverage gaps.
- Violation handling: Integration test failures indicating boundary layer contract violations trigger build failures and prevent deployment.
- Exception process: Exception requests must document why the boundary layer cannot accommodate the use case and propose an alternative approach that maintains testability.
- Exception process: Exceptions require approval from the engineering team lead and must include a plan for eventual migration to the boundary layer.
- Exception process: Approved exceptions are documented in code comments with references to the exception approval and migration plan.