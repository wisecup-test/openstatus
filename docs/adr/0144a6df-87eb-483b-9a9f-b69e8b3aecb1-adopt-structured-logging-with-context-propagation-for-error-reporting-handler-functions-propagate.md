# Adopt Structured Logging with Context Propagation for Error Reporting: Handler Functions Propagate

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service performs health checks across HTTP, TCP, and DNS protocols, requiring consistent error visibility across multiple handler types and network operations
- Error conditions arise from external client interactions, timeout scenarios, and protocol-specific failures that must be captured with sufficient context for debugging distributed health check operations
- The service integrates with external event collection systems that require structured error data including timing information, response metadata, and failure details
- Context propagation through request handlers enables correlation of errors with specific check executions, regions, and retry attempts across the observability pipeline
- The codebase demonstrates consistent use of context-aware logging patterns with error extraction methods across six files handling checker operations, client interactions, and event publishing

## Problem Statement

Health check services executing distributed protocol validations across multiple regions require a standardized approach to error logging that preserves request context, enables correlation across retry attempts, and provides structured error data to downstream analytics systems without coupling handler logic to specific logging implementations.

## Decision

1. SHOULD: Handler functions SHOULD propagate context from the request framework to logging calls to maintain correlation across the request lifecycle

## Policy Block

- SHOULD Handler functions SHOULD propagate context from the request framework to logging calls to maintain correlation across the request lifecycle

In scope:
- All HTTP, TCP, and DNS checker handler implementations
- External client interaction code including HTTP client operations and event publishing
- Retry logic and backoff operations that may fail multiple times
- Server initialization and health check endpoint implementations

Out of scope:
- Success-path logging that does not involve error conditions
- Debug or trace-level logging for non-error diagnostic information
- Metrics collection and performance monitoring that does not use the logging subsystem

## Rationale

- The evidence shows consistent application of context-aware error logging across six files with 90.65% confidence, indicating an established architectural pattern rather than isolated usage
- The pattern appears in critical paths including HTTP checker handlers, TCP handlers, DNS handlers, external client code, and event publishing, demonstrating system-wide adoption for error visibility
- Integration with external event collection systems and the presence of structured data extraction patterns indicate that error logging serves both operational debugging and analytics use cases
- The co-occurrence of error logging with retry logic, timeout handling, and external client operations suggests the pattern addresses the specific challenges of distributed health check reliability and observability

## Consequences

Positive:
- Consistent error reporting across all protocol handlers enables unified monitoring and alerting for health check failures
- Context propagation through the logging subsystem provides automatic correlation of errors with request identifiers, regions, and check configurations
- Structured error extraction supports downstream analytics and pattern detection in the event collection system
- Standardized error logging reduces cognitive load for developers implementing new checker types or debugging existing failures

Negative:
- Dependency on context propagation requires careful handling of context objects throughout the request lifecycle to avoid losing correlation data
- Structured logging frameworks may introduce performance overhead in high-frequency error scenarios or when logging large error payloads
- Changes to the logging library or context extraction patterns require coordinated updates across multiple handler implementations

## Alternatives

- Use unstructured string-based logging with manual context formatting in each handler (rejected)
  Rejected because: Manual context formatting is error-prone, inconsistent across handlers, and does not support structured querying in analytics systems
  When valid: Valid only for prototype code or services without downstream analytics requirements
- Implement centralized error collection through middleware without context-aware logging (rejected)
  Rejected because: Middleware-only error collection loses fine-grained context from retry attempts, external client failures, and protocol-specific error conditions that occur deep in the call stack
  When valid: Valid for services with simple request-response patterns and no retry logic
- Adopt distributed tracing with span-based error recording instead of logging (deferred)
  Rejected because: Tracing provides complementary observability but does not replace the need for structured error logs for analytics and long-term storage in event collection systems
  When valid: Valid as an additional observability layer to complement structured logging

## Risks

- Context loss during error propagation through retry logic or goroutine boundaries may result in uncorrelated error logs
  Mitigation: Establish code review guidelines requiring explicit context propagation in all asynchronous operations and retry loops
  Owner: engineering team
- High error rates during outages may generate excessive log volume impacting performance or storage costs
  Mitigation: Implement rate limiting or sampling for error logs during sustained failure conditions while preserving error metrics
  Owner: engineering team
- Sensitive information in error messages or response bodies may be logged inadvertently
  Mitigation: Establish error sanitization patterns and code review checks for PII or credential exposure in logged error data
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
- Establish a context propagation pattern where the request framework context is extracted at handler entry and passed to all downstream operations including external client calls and retry logic
- Define structured logging fields for common error metadata including request identifiers, event types, regional information, timing data, and protocol-specific details to ensure consistency across handlers
- Implement error sanitization utilities that strip sensitive information from error messages and response bodies before logging, particularly for authentication headers and request payloads

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting rules that verify context propagation patterns in error logging calls
- Locate the project's test suite and run integration tests that validate error logging behavior under failure conditions including timeout, retry, and external service unavailability scenarios
- Identify the project's log output validation tooling and verify that error logs contain required structured fields including context identifiers and error details

Accept when:
- All error logging calls in handler functions extract context using the context-aware logger pattern and include error object details
- Integration tests demonstrate that error logs contain correlation identifiers and structured error data under simulated failure conditions
- Static analysis confirms no error logging calls bypass the context propagation pattern or log errors without structured context

## Enforcement

- Verified by: Code review checklist requiring verification of context-aware error logging in all new handler implementations
- Verified by: Static analysis rules in continuous integration that detect error logging calls without context propagation
- Verified by: Integration test coverage requirements for error scenarios in all protocol checker implementations
- Violation handling: Pull requests with error logging violations identified by static analysis are blocked from merge until corrected
- Violation handling: Code review findings for missing context propagation require revision before approval
- Violation handling: Runtime detection of uncorrelated error logs triggers alerts for investigation and remediation
- Exception process: Exception requests must document why context-aware logging is not feasible for the specific error scenario
- Exception process: Architecture review approval required for exceptions that affect core handler implementations or external client interactions
- Exception process: Approved exceptions must include compensating controls such as alternative correlation mechanisms or enhanced error metadata