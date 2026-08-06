# Standardize HTTP Response Body Handling in Integration Testing: Integration Test Implementations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service performs HTTP health checks and monitoring across multiple handlers and job execution paths, requiring consistent validation of response bodies, headers, and timing data
- Integration testing patterns emerged across seven files handling HTTP client interactions, response body parsing, and assertion evaluation against external endpoints
- The codebase uses standard library HTTP clients with configurable timeouts and retry logic via exponential backoff, necessitating uniform body handling across success and failure paths
- Response bodies are consumed for assertion evaluation, logging, and event forwarding to analytics systems, creating multiple points where body handling must be coordinated

## Problem Statement

HTTP response body handling in integration testing contexts lacks standardization across checker handlers, job executors, and client wrappers, leading to inconsistent resource cleanup, potential memory leaks from unclosed body readers, and fragile assertion logic that may fail when response formats vary across external endpoints.

## Decision

1. MAY: Integration test implementations may cache request metadata in context or framework storage for retry coordination and event correlation

## Policy Block

- MAY Integration test implementations may cache request metadata in context or framework storage for retry coordination and event correlation

In scope:
- HTTP client wrappers performing external health checks and monitoring requests
- Handler functions processing incoming check requests and forwarding to external endpoints
- Job executors running scheduled or triggered HTTP monitoring tasks
- Response assertion evaluators comparing actual response bodies against expected criteria
- Event forwarding logic sending response data to analytics or observability systems

Out of scope:
- Unit tests mocking HTTP responses without actual network calls
- Internal service-to-service communication using RPC or message queues
- Static file serving or asset delivery endpoints
- WebSocket or streaming response handling
- GraphQL or gRPC client implementations

## Rationale

- Seven files across checker handlers, HTTP client wrappers, and job executors consistently reference response.Body, res.Body, and req.Body fields, indicating a pervasive pattern of body handling in integration testing contexts
- The evidence shows explicit timeout configuration and exponential backoff retry logic, demonstrating that HTTP clients are used in production monitoring scenarios where resource cleanup and error handling are critical
- Response bodies are consumed for multiple purposes including assertion evaluation, error logging, and event forwarding to external analytics systems, requiring standardized handling to prevent data loss or corruption
- The pattern emerges in both synchronous handler paths and asynchronous job execution contexts, indicating that body handling standards must apply uniformly across different execution models

## Consequences

Positive:
- Prevents resource leaks from unclosed HTTP response body readers in long-running checker services
- Enables consistent assertion evaluation logic across different handler and job execution paths
- Facilitates reliable event forwarding and logging by ensuring response body content is fully captured before processing
- Improves debuggability by standardizing how response data is extracted and made available for error reporting

Negative:
- Requires reading entire response bodies into memory buffers, increasing memory footprint for large responses
- Adds boilerplate defer and error checking code to every HTTP client interaction point
- May introduce latency in assertion evaluation paths due to full body buffering before processing
- Complicates streaming response scenarios where incremental processing would be more efficient

## Alternatives

- Use streaming response body processing with incremental assertion evaluation (rejected)
  Rejected because: The evidence shows response bodies are consumed for multiple purposes including logging and event forwarding, which require complete body content and would be incompatible with streaming consumption
  When valid: When monitoring endpoints return very large responses and assertions can be evaluated on partial content
- Implement automatic body closing via custom HTTP client wrapper with finalizers (rejected)
  Rejected because: Finalizers are non-deterministic and may delay resource cleanup, which is unacceptable in high-frequency monitoring scenarios where connection pool exhaustion is a risk
  When valid: In low-frequency testing scenarios where explicit resource management is less critical
- Adopt a third-party HTTP testing library that abstracts body handling (deferred)
  Rejected because: The current implementation uses standard library HTTP clients consistently across all evidence files, and migration would require significant refactoring without clear evidence of inadequacy
  When valid: If future requirements demand advanced features like automatic retries, circuit breaking, or response recording that justify the dependency cost

## Risks

- Large response bodies from monitored endpoints may cause memory pressure or out-of-memory conditions when buffered entirely
  Mitigation: Implement response size limits in HTTP client configuration and reject responses exceeding thresholds before full body read
  Owner: engineering team
- Inconsistent error handling across body read operations may result in partial data being used for assertions or logged incompletely
  Mitigation: Establish standard error handling patterns with explicit checks after body read operations and consistent logging of read failures
  Owner: engineering team
- Timeout configurations may be insufficient for slow external endpoints, causing false negatives in monitoring results
  Mitigation: Derive timeout values from monitor configuration rather than hardcoding, and implement timeout tuning based on historical response time data
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
- Establish a standard pattern for HTTP client instantiation that includes timeout configuration, custom transport settings for TLS, and connection pooling parameters appropriate for monitoring workloads
- Create reusable body handling utilities that encapsulate the defer close pattern, buffer reading, and error propagation to reduce boilerplate across handler and job implementations
- Document the relationship between request body serialization, header manipulation, and response body parsing to ensure consistent data flow through assertion evaluation pipelines

## Continuation Context


Verify commands:
- Discover the project's integration test suite location and execute the test runner with coverage reporting enabled to verify body handling patterns are exercised
- Locate the project's static analysis or linting configuration and run the toolchain to detect unclosed response body readers
- Identify the project's HTTP client usage across the checker service and validate that all response body access points include explicit close operations

Accept when:
- All HTTP response body readers in integration testing contexts are closed via defer statements immediately after error checking
- Response body content required for assertions is fully read into buffers before processing, with explicit error handling for read failures
- HTTP client instances configure timeouts derived from monitor or request configuration rather than using default or infinite timeouts

## Enforcement

- Verified by: Static analysis tooling configured to detect unclosed HTTP response body readers
- Verified by: Code review checklist requiring explicit body close patterns in all HTTP client usage
- Verified by: Integration test coverage reports validating that body handling paths are exercised
- Violation handling: Static analysis failures block pull request merges until body handling is corrected
- Violation handling: Code review identifies missing defer close patterns and requests changes before approval
- Violation handling: Runtime monitoring detects connection pool exhaustion or file descriptor leaks and triggers alerts for investigation
- Exception process: Streaming response scenarios may request exemption from full body buffering with explicit justification and alternative resource cleanup strategy
- Exception process: Performance-critical paths may propose alternative body handling approaches with benchmarking data demonstrating necessity
- Exception process: All exceptions require documentation in code comments explaining the deviation and mitigation of associated risks