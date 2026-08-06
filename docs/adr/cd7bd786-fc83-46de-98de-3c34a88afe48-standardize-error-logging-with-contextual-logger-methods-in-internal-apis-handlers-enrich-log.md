# Standardize Error Logging with Contextual Logger Methods in Internal APIs: Handlers Enrich Log

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Internal API handlers and HTTP clients across the checker application consistently use structured logging methods for error reporting
- HTTP request and response processing requires observable error paths for debugging distributed health checks and monitoring operations
- The codebase integrates context-aware logging with error extraction patterns to capture failure details from HTTP operations
- Multiple handler and client modules share a common approach to logging errors using logger.Error() and err.Error() methods
- The pattern emerged across HTTP job execution, ping handlers, checker handlers, and external client interactions

## Problem Statement

Internal APIs handling HTTP operations need a consistent mechanism to log errors with contextual information, enabling operators to diagnose failures in distributed monitoring and health check systems without fragmenting observability across different logging approaches.

## Decision

1. MAY: Handlers MAY enrich log entries with request-specific metadata from headers or query parameters

## Policy Block

- MAY Handlers MAY enrich log entries with request-specific metadata from headers or query parameters

In scope:
- HTTP request handlers in internal APIs
- HTTP client wrappers and external service integrations
- Job execution modules performing HTTP operations
- Ping and health check handlers
- Error handling paths in request processing pipelines

Out of scope:
- Application startup and initialization logging
- Metrics collection and aggregation
- Trace span creation and propagation
- Business logic validation errors that do not involve HTTP operations
- Third-party library internal logging

## Rationale

- The evidence shows consistent use of logger.Error() and log.Ctx(ctx).Error() across 6 files with 90.78% confidence, indicating an established pattern
- Contextual logging enables correlation of errors with request identifiers and execution context in distributed monitoring systems
- Extracting error messages through err.Error() provides string representations suitable for structured log fields
- The pattern supports observability requirements for debugging HTTP client failures, timeout handling, and external service integration issues

## Consequences

Positive:
- Unified error logging approach across internal APIs simplifies log aggregation and analysis
- Context-bound loggers automatically include request metadata and trace identifiers
- Consistent error message extraction enables reliable parsing and alerting rules
- Operators gain visibility into HTTP operation failures across distributed checker components

Negative:
- Requires discipline to ensure all error paths invoke the logger consistently
- Context propagation must be maintained through the call stack to access the logger
- Structured logging libraries may introduce performance overhead in high-throughput scenarios
- Teams must coordinate on log level semantics and field naming conventions

## Alternatives

- Use standard library logging without context binding (rejected)
  Rejected because: Standard library logging lacks structured fields and context propagation, making it difficult to correlate errors with specific requests in distributed systems
  When valid: Acceptable for simple command-line tools or single-threaded applications without distributed tracing requirements
- Return errors to callers without logging at the boundary (rejected)
  Rejected because: Defers logging responsibility to higher layers, risking silent failures when errors are not properly handled by all callers
  When valid: Appropriate for library code where the application layer should control logging policy
- Emit errors as metrics or events only (deferred)
  Rejected because: Metrics provide aggregates but lack the detailed context needed for debugging individual failures
  When valid: Complementary approach for monitoring error rates and triggering alerts, but does not replace detailed logging

## Risks

- Inconsistent logger initialization across modules may result in missing context or nil pointer dereferences
  Mitigation: Establish initialization patterns that guarantee logger availability and validate context propagation in integration tests
  Owner: engineering team
- Excessive logging in high-frequency error scenarios may overwhelm log storage or impact performance
  Mitigation: Implement rate limiting or sampling for known high-volume error conditions and monitor log volume metrics
  Owner: engineering team
- Sensitive data in error messages may be inadvertently logged
  Mitigation: Review error message construction to exclude credentials, tokens, or personally identifiable information and apply redaction filters
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
- Initialize the context-bound logger at the entry point of each HTTP handler and propagate the context through all downstream operations
- Wrap error logging calls with conditional checks when errors may be nil to avoid unnecessary log entries
- Define structured field names for common error attributes to ensure consistency across modules

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting rules that verify logger usage patterns
- Locate the integration test suite for HTTP handlers and confirm tests validate error logging behavior
- Identify the log aggregation query tool and run queries that detect error log entries from internal API modules

Accept when:
- Static analysis reports no violations of logger initialization or error logging patterns
- Integration tests demonstrate error logs contain expected context fields and error messages
- Log queries successfully retrieve error entries with structured fields from all internal API handlers

## Enforcement

- Verified by: Static analysis tools scanning for logger method invocations in error handling paths
- Verified by: Code review checklist requiring verification of error logging in new HTTP handlers
- Verified by: Integration tests asserting log output for error scenarios
- Violation handling: Static analysis failures block merge requests until logging patterns are corrected
- Violation handling: Code review identifies missing error logging and requests changes before approval
- Violation handling: Runtime monitoring alerts on modules producing errors without corresponding log entries
- Exception process: Document justification for alternative logging approaches in architectural decision comments
- Exception process: Obtain approval from the observability team lead for deviations from standard patterns
- Exception process: Record exceptions in the project's architectural decision log with expiration dates for review