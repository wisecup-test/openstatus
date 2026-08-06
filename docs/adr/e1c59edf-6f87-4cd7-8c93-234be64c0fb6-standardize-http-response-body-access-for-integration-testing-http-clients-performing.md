# Standardize HTTP Response Body Access for Integration Testing: Http Clients Performing

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase performs HTTP health checks, monitoring, and ping operations across multiple handlers and services that require validation of response bodies
- Integration testing patterns access response.Body, req.Body, res.Body, and data.Body fields across checker, handler, and job components
- HTTP client implementations use standard library constructs with custom header manipulation including User-Agent and Content-Type headers
- External service boundaries require structured body access for assertion evaluation and status updates
- Retry logic with exponential backoff wraps HTTP operations that depend on body content for success determination

## Problem Statement

HTTP response and request body access patterns are inconsistent across integration test boundaries, checker implementations, and handler logic, creating maintenance overhead when validating external service responses and evaluating assertions against body content.

## Decision

1. SHOULD: HTTP clients performing external service checks should implement timeout constraints using duration-based configuration

## Policy Block

- SHOULD HTTP clients performing external service checks should implement timeout constraints using duration-based configuration

In scope:
- HTTP checker implementations that validate external service responses
- Handler functions processing ping and monitoring requests
- Integration test code accessing request or response body fields
- Client wrappers performing assertion evaluation on body content

Out of scope:
- Unit tests that mock HTTP responses without actual network calls
- Internal service-to-service communication using non-HTTP protocols
- Static file serving where body content is not validated
- Streaming responses where body is processed incrementally

## Rationale

- Seven files across checker, handler, and job packages demonstrate consistent Body field access patterns with 90.93% confidence
- HTTP header manipulation patterns for User-Agent and Content-Type appear in multiple boundary implementations indicating established conventions
- Integration testing facet detection correlates with response body access in external client boundaries and service definitions
- Standardizing body access reduces cognitive load when maintaining assertion evaluation and status update logic

## Consequences

Positive:
- Consistent body access patterns reduce maintenance overhead across checker and handler implementations
- Clear header validation conventions improve debugging of Content-Type mismatches
- Standardized User-Agent identification enables better service request tracking and monitoring
- Integration test reliability improves through uniform response body handling

Negative:
- Enforcing Body field access may require refactoring existing code that uses alternative access patterns
- Header validation overhead adds minor latency to request processing paths
- Timeout configuration requirements increase complexity for simple HTTP client use cases

## Alternatives

- Use streaming body readers with incremental processing instead of direct Body field access (rejected)
  Rejected because: Evidence shows direct Body field access in 7 files with no streaming patterns detected; refactoring would contradict established codebase conventions
  When valid: When processing large response bodies that exceed memory constraints or require progressive parsing
- Abstract body access behind a custom interface that wraps HTTP response structures (rejected)
  Rejected because: Adds abstraction layer without clear benefit given standard library Body field already provides consistent interface across detected patterns
  When valid: When migrating to alternative HTTP client libraries or supporting multiple HTTP implementations simultaneously
- Defer all header validation to middleware layer instead of inline Header.Get calls (deferred)
  When valid: When centralizing cross-cutting concerns or implementing service-wide header policies; requires architectural review of middleware capabilities

## Risks

- Body field access without proper closure may cause resource leaks in long-running services
  Mitigation: Establish defer patterns for Body.Close() calls in all HTTP client usage; verify through static analysis
  Owner: engineering team
- Content-Type validation may fail for services returning non-standard or missing headers
  Mitigation: Implement fallback logic for missing Content-Type headers; document expected header behavior in service contracts
  Owner: engineering team
- Timeout configuration inconsistencies across different HTTP client instantiations may cause unpredictable behavior
  Mitigation: Centralize timeout configuration in shared client factory; audit existing timeout values for consistency
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
- Establish Body.Close() defer patterns immediately after successful HTTP response acquisition to prevent resource leaks
- Centralize User-Agent string construction to enable consistent service identification across all external HTTP calls
- Pair all error logging with err.Error() output to maintain consistency with observed logging patterns in checker and handler implementations

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis verification to confirm all HTTP response Body field accesses are paired with Close() calls
- Discover and run the project's integration test suite to validate User-Agent and Content-Type header handling
- Discover and execute the project's linting configuration to detect HTTP client timeout omissions

Accept when:
- All HTTP response body accesses in checker, handler, and job packages use the Body field consistently
- Static analysis confirms no resource leaks from unclosed Body readers
- Integration tests pass with User-Agent headers present in all external service requests
- Content-Type validation occurs before body processing in all handler implementations

## Enforcement

- Verified by: Static analysis tools scanning for HTTP response Body field access patterns
- Verified by: Code review checklist items for User-Agent and Content-Type header validation
- Verified by: Integration test suite execution validating body access and header handling
- Violation handling: Pull requests introducing inconsistent body access patterns are blocked until corrected
- Violation handling: Missing User-Agent or Content-Type validation triggers code review feedback
- Violation handling: Resource leak detection from unclosed Body readers fails continuous integration checks
- Exception process: Document technical justification for alternative body access patterns in code comments
- Exception process: Obtain architecture review approval for streaming body processing implementations
- Exception process: Record exception in ADR amendments with rationale and scope limitations