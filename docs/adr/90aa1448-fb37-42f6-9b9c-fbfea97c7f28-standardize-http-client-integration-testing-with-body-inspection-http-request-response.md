# Standardize HTTP Client Integration Testing with Body Inspection: Http Request Response

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker application performs HTTP health checks and monitoring across external services, requiring validation of request and response bodies at service boundaries
- Integration testing patterns consistently inspect response.Body, res.Body, and data.Body fields across HTTP client interactions, indicating a need to verify payload integrity at API boundaries
- External client boundaries use standard library HTTP clients with custom headers (User-Agent, Content-Type, Authorization) and timeout configurations, establishing a pattern for outbound service communication
- The codebase demonstrates retry logic with exponential backoff and context-aware error logging, suggesting reliability requirements at service boundaries necessitate comprehensive integration testing

## Problem Statement

Service boundary interactions with external HTTP APIs require consistent integration testing to validate request construction, response handling, and error propagation, but without standardized body inspection and assertion patterns, teams may implement inconsistent validation approaches that fail to catch payload corruption, serialization errors, or contract violations at runtime.

## Decision

1. MUST: HTTP request and response handling MUST propagate context for cancellation and timeout enforcement across service boundaries

## Policy Block

- MUST HTTP request and response handling MUST propagate context for cancellation and timeout enforcement across service boundaries

In scope:
- HTTP client implementations that communicate with external services
- Integration test suites for service boundary interactions
- Request and response body serialization and deserialization logic
- HTTP header management for cross-service communication
- Timeout and retry configuration for external API calls

Out of scope:
- Internal function calls within the same service process
- Database client interactions
- Message queue or event stream consumers
- File system I/O operations
- In-memory cache operations

## Rationale

- The evidence shows consistent body inspection patterns across three files with 91.23% confidence, indicating an established architectural practice for validating payload integrity at service boundaries
- HTTP client configurations demonstrate explicit timeout management and custom header injection, revealing a deliberate approach to controlling external service interactions and identifying traffic sources
- The presence of retry logic with exponential backoff alongside body inspection suggests that integration testing must validate not only happy-path responses but also resilience mechanisms at API boundaries
- Context propagation and error logging patterns indicate that service boundary testing must verify both data correctness and operational observability across external calls

## Consequences

Positive:
- Integration tests will catch payload corruption, serialization errors, and contract violations before production deployment
- Consistent User-Agent headers enable external services to identify and monitor traffic patterns from this application
- Explicit timeout configurations prevent cascading failures and resource exhaustion when external services degrade
- Standardized body inspection patterns reduce cognitive load for developers implementing new service integrations

Negative:
- Integration tests that inspect response bodies require test fixtures or mock servers, increasing test complexity and maintenance burden
- Mandatory body inspection may slow test execution if tests perform actual network calls rather than using mocks
- Strict timeout requirements may require tuning for services with variable latency characteristics
- Retry logic with backoff increases the time window for transient failures to resolve but also increases total request latency on failure paths

## Alternatives

- Test only HTTP status codes without inspecting response bodies (rejected)
  Rejected because: Status code validation alone cannot detect payload corruption, serialization errors, or subtle contract violations that manifest in response body structure or content
  When valid: Acceptable only for health check endpoints that return no meaningful body content
- Use contract testing frameworks instead of integration tests with body inspection (deferred)
  Rejected because: Contract testing provides complementary value but does not replace integration testing for validating actual runtime behavior with real serialization and network stack
  When valid: Should be adopted in addition to integration tests for comprehensive boundary validation
- Implement service boundary testing only in end-to-end test suites (rejected)
  Rejected because: End-to-end tests provide insufficient isolation and feedback speed for developers working on service boundary logic, and their failure modes are harder to diagnose
  When valid: End-to-end tests complement but do not replace focused integration tests at service boundaries

## Risks

- Integration tests that perform real network calls may become flaky due to external service instability or network conditions
  Mitigation: Use test doubles or mock servers for integration tests, reserving real network calls for smoke tests or contract verification suites
  Owner: engineering team
- Overly strict timeout configurations may cause false failures for legitimate slow responses during peak load or cold start scenarios
  Mitigation: Establish timeout values based on percentile latency analysis of production traffic and provide configuration overrides for specific high-latency endpoints
  Owner: engineering team
- Body inspection tests may become brittle if they assert on exact payload structure rather than essential contract elements
  Mitigation: Design assertions to validate required fields and semantic correctness rather than exact JSON structure or field ordering
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
- Structure integration tests to create HTTP clients with production-equivalent timeout and header configurations, then validate both successful and error response bodies against expected contracts
- Implement test helpers that abstract common body inspection patterns to reduce duplication across integration test suites while maintaining consistency
- When implementing retry logic, ensure integration tests validate that retry attempts respect context cancellation and do not exceed configured maximum attempts

## Continuation Context


Verify commands:
- Discover the project's test execution mechanism and run the integration test suite for service boundary interactions
- Inspect integration test implementations to confirm response body inspection is present for all external HTTP client calls
- Review HTTP client configurations to verify explicit timeout values and User-Agent headers are set

Accept when:
- All integration tests for HTTP client service boundaries include assertions on response body content
- HTTP client configurations specify explicit timeout values and User-Agent headers
- Integration test suite executes successfully and validates both success and error response scenarios

## Enforcement

- Verified by: Code review verification that new HTTP client integrations include body inspection tests
- Verified by: Continuous integration pipeline execution of integration test suites
- Verified by: Static analysis to detect HTTP client instantiations without explicit timeout configuration
- Violation handling: Pull requests introducing HTTP client integrations without body inspection tests are blocked until tests are added
- Violation handling: HTTP clients without explicit timeouts trigger build warnings that must be resolved before merge
- Violation handling: Periodic audits identify service boundary code lacking integration test coverage for remediation
- Exception process: Exceptions for body inspection requirements may be granted for health check endpoints that return no meaningful payload
- Exception process: Exception requests must document the rationale and obtain approval from a technical lead
- Exception process: Approved exceptions are recorded with expiration dates for periodic review