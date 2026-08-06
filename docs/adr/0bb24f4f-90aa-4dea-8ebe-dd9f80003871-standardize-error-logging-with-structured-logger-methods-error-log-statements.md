# Standardize Error Logging with Structured Logger Methods: Error Log Statements

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker application performs distributed health checks across multiple protocols (HTTP, DNS, TCP, ping) with regional deployment requirements
- Error conditions arise from network operations, external service calls, assertion failures, and retry exhaustion across 12 handler and job implementation files
- Structured logging enables correlation of errors with request context, event identifiers, and regional metadata for distributed tracing
- The codebase uses zerolog library methods including log.Ctx(ctx).Error() and logger.Error() patterns with err.Error() string extraction
- Consistent error logging patterns facilitate automated log aggregation, alerting, and debugging across multiple checker service instances

## Problem Statement

Distributed health checking services require consistent error logging to correlate failures across network boundaries, retry attempts, and regional deployments, but ad-hoc error handling creates gaps in observability and complicates root cause analysis when checks fail.

## Decision

1. MUST: Error log statements MUST extract the error message using the error's string representation method and attach it to the structured log entry

## Policy Block

- MUST Error log statements MUST extract the error message using the error's string representation method and attach it to the structured log entry

In scope:
- HTTP checker handlers and job implementations
- DNS lookup handlers and job implementations
- TCP connection handlers and job implementations
- Ping handlers and regional ping operations
- Backoff retry operations with error conditions
- External client request failures
- Assertion evaluation failures

Out of scope:
- Success path logging at info or debug levels
- Metrics collection and time-series data emission
- Health check endpoint responses
- Configuration loading and validation errors at startup

## Rationale

- The evidence shows consistent use of structured error logging across 12 files with 90.60% confidence, indicating an established pattern rather than isolated usage
- Context-aware logging with log.Ctx(ctx).Error() enables correlation of errors with request identifiers, events, and regional metadata stored in the context chain
- Extracting error strings with err.Error() provides human-readable error messages while maintaining structured log format for automated parsing
- The pattern supports distributed tracing requirements where checker instances across multiple regions must correlate failures with specific monitor configurations and retry attempts

## Consequences

Positive:
- Consistent error logging format enables automated log aggregation and alerting across distributed checker instances
- Context propagation preserves request correlation identifiers through the entire error handling chain
- Structured error logs support efficient debugging by correlating failures with specific monitors, regions, and retry attempts
- Integration with zerolog library provides zero-allocation logging performance for high-throughput health checking operations

Negative:
- Dependency on specific structured logging library creates coupling that complicates library migration
- Context-aware logging requires consistent context propagation through all function call chains
- Error string extraction with err.Error() may lose structured error metadata from wrapped or typed errors
- Additional logging calls increase code verbosity in error handling paths

## Alternatives

- Use standard library log package with unstructured error messages (rejected)
  Rejected because: Unstructured logs lack correlation identifiers and structured fields required for distributed tracing across regional checker deployments
  When valid: Valid only for single-instance services without distributed tracing requirements
- Emit errors only as metrics without detailed log messages (rejected)
  Rejected because: Metrics provide aggregate counts but lack the contextual detail needed to debug specific check failures or assertion violations
  When valid: Valid as a complement to error logging for high-level monitoring dashboards
- Centralize error logging in middleware layer only (rejected)
  Rejected because: Middleware cannot capture operation-specific context such as DNS record types, TCP port numbers, or assertion details from deep in the call stack
  When valid: Valid for HTTP-level errors but insufficient for protocol-specific checker failures

## Risks

- Logging library version changes may introduce breaking API changes in error logging methods
  Mitigation: Pin logging library version in dependency manifest and test error logging paths in integration tests before upgrading
  Owner: engineering team
- High error rates during outages may overwhelm log ingestion systems with duplicate error messages
  Mitigation: Implement log sampling or rate limiting for repeated errors with identical error messages and target resources
  Owner: engineering team
- Context propagation failures may result in error logs without correlation identifiers
  Mitigation: Validate context propagation in unit tests and emit fallback logs with service-level identifiers when context is unavailable
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
- Establish a context propagation pattern where all handler functions receive context as the first parameter and pass it to job implementations and external client calls
- Create helper functions that wrap common error logging patterns with protocol-specific structured fields to reduce code duplication across checker implementations
- Configure log output format and destination through environment variables or configuration files to support different deployment environments without code changes

## Continuation Context


Verify commands:
- Locate the project's test suite directory and execute the integration test runner to verify error logging behavior in failure scenarios
- Search the codebase for error handling patterns and confirm all error returns are accompanied by structured log statements
- Inspect the logging configuration initialization code to verify context propagation is enabled and error level is configured

Accept when:
- All error conditions in checker handlers and job implementations emit structured log entries at error level
- Error logs include context correlation identifiers when context is available
- Integration tests verify error logs contain expected structured fields for each protocol type

## Enforcement

- Verified by: Code review checklist requiring error logging for all error return paths
- Verified by: Static analysis rules detecting error returns without corresponding log statements
- Verified by: Integration test assertions validating error log output format and content
- Violation handling: Code review blocks merge requests missing error logging in new error handling paths
- Violation handling: Static analysis failures trigger build warnings that escalate to errors after grace period
- Violation handling: Production monitoring alerts on unstructured error messages or missing correlation identifiers
- Exception process: Document justification for exception in code comments with reference to specific architectural constraint
- Exception process: Obtain approval from team lead for error handling paths that cannot use structured logging
- Exception process: Add suppression annotation for static analysis rules with ticket reference for future remediation