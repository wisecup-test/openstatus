# Standardize Structured Logging with zerolog for Observability: Error Messages Not

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service performs distributed health checks across multiple protocols (HTTP, DNS, TCP) from various geographic regions, requiring consistent error tracking and operational visibility
- Error handling patterns appear in 12 files with consistent use of logger.Error() and err.Error() methods, indicating a standardized approach to capturing failure conditions
- The service integrates with external systems (Tinybird analytics, backoff retry mechanisms, HTTP clients) where structured logging provides essential debugging context
- Core libraries detected include context propagation, encoding/json, net/http, and time packages, suggesting a need for request-scoped logging with structured fields

## Problem Statement

Distributed health checking services require consistent, structured logging to diagnose failures across multiple protocols, regions, and retry attempts. Unstructured logging makes it difficult to correlate errors with specific requests, trace retry sequences, or aggregate failure patterns across geographic deployments.

## Decision

1. MUST_NOT: Error messages MUST_NOT be constructed using string concatenation or formatting when the logging library provides structured field attachment

## Policy Block

- MUST_NOT Error messages MUST_NOT be constructed using string concatenation or formatting when the logging library provides structured field attachment

In scope:
- All HTTP, DNS, and TCP checker handler functions
- Job execution functions that perform protocol-specific health checks
- Client wrappers for external services such as analytics platforms
- Retry and backoff logic where errors may occur multiple times
- Server initialization and lifecycle management code

Out of scope:
- Third-party library internal logging
- Standard library debug output
- Development-only debug print statements

## Rationale

- Evidence shows consistent use of logger.Error() and log.Ctx(ctx).Error() patterns across 12 files, indicating an established structured logging convention
- The service architecture requires correlation of errors across distributed regions, retry attempts, and multiple protocol handlers, which structured logging enables through consistent field attachment
- Integration with external analytics systems and the presence of request identifiers in context suggests a need for machine-parseable log output
- The pattern's 90.60% confidence across checker handlers, job executors, and client wrappers demonstrates architectural consistency in error observability

## Consequences

Positive:
- Enables automated log aggregation and analysis across distributed checker regions
- Facilitates correlation of errors with specific health check requests through context-propagated identifiers
- Provides structured data for debugging retry sequences and backoff behavior
- Supports integration with observability platforms that consume JSON-formatted logs

Negative:
- Requires developers to learn structured logging API patterns rather than simple print statements
- Introduces dependency on a specific logging library that must be maintained across service versions
- May increase log volume if verbose structured fields are attached to every log statement
- Context propagation requirements add complexity to function signatures and call chains

## Alternatives

- Use standard library log package with unstructured string formatting (rejected)
  Rejected because: Unstructured logs cannot be efficiently parsed or aggregated across distributed regions, and lack support for request-scoped context propagation
  When valid: Only appropriate for single-instance services with minimal operational requirements
- Implement custom logging abstraction layer over multiple backend libraries (rejected)
  Rejected because: Adds maintenance burden and complexity without evidence of multi-backend requirements in the current architecture
  When valid: When the service must support multiple deployment environments with different logging infrastructure requirements
- Use OpenTelemetry logging with trace correlation (deferred)
  Rejected because: Not rejected, but no evidence of OpenTelemetry adoption in current codebase; may be considered for future observability integration
  When valid: When full distributed tracing is implemented and trace-log correlation becomes a requirement

## Risks

- Logging library version incompatibilities or breaking API changes could require widespread code modifications
  Mitigation: Pin library versions in dependency management and test logging functionality in CI pipeline
  Owner: engineering team
- Excessive structured field attachment could degrade performance in high-throughput scenarios
  Mitigation: Establish guidelines for essential vs. optional fields and benchmark logging overhead in load tests
  Owner: engineering team
- Inconsistent context propagation could result in logs missing request identifiers or trace information
  Mitigation: Implement linting rules to verify context is passed to all logging calls and include context validation in code review
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
- Establish a context initialization pattern in HTTP handlers that attaches request identifiers and event metadata before invoking business logic
- Define standard structured fields for each protocol type (HTTP status codes, DNS record types, TCP connection states) to ensure consistency across handlers
- Configure log output format and destination through environment variables or configuration files to support different deployment environments

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis tooling to verify all error logging calls use structured logging methods
- Locate the project's test suite and run integration tests that validate context propagation through handler chains
- Identify the project's linting configuration and verify rules enforce context-aware logging patterns

Accept when:
- All error logging statements in checker handlers and job executors use structured logging with error object attachment
- Context propagation is verified through the request lifecycle from handler entry to error logging
- Static analysis confirms no string concatenation or formatting is used for error message construction where structured fields are available

## Enforcement

- Verified by: Automated static analysis in continuous integration pipeline
- Verified by: Code review checklist requiring verification of structured logging patterns
- Verified by: Integration tests that validate log output format and field presence
- Violation handling: CI pipeline fails if static analysis detects unstructured error logging
- Violation handling: Code review blocks merge if logging patterns do not follow established conventions
- Violation handling: Runtime monitoring alerts if log parsing failures indicate malformed output
- Exception process: Document technical justification for exception in code comments
- Exception process: Obtain approval from team lead or architect for deviations from standard patterns
- Exception process: Track exceptions in architecture decision log with expiration date for review