# Standardize Structured Error Logging with Context Propagation: Before Any Structured

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service operates across multiple regions and cloud providers, requiring consistent error visibility across distributed HTTP, DNS, and TCP monitoring operations
- Error conditions arise from external client interactions, network operations, assertion failures, and integration with third-party APIs where structured context is essential for debugging
- The codebase uses github.com/rs/zerolog/log for structured logging with context propagation through log.Ctx(ctx).Error() patterns observed across 12 files
- Public API contracts expose error states through response bodies and require correlation between request context, retry attempts, and failure modes
- Handler functions coordinate backoff retry logic with external clients where error messages must capture both the failure reason and operational context

## Problem Statement

Distributed monitoring operations across HTTP, DNS, and TCP protocols generate errors from multiple sources including network failures, assertion violations, and external API interactions. Without standardized structured logging that propagates request context, debugging production incidents requires manual correlation across logs, and error messages lack the operational metadata needed to distinguish transient failures from systemic issues.

## Decision

1. MUST: Before using any structured logging library API, the consumer MUST discover the project's dependency lock file, resolve the exact installed version, and verify all method signatures against that version's official documentation

## Policy Block

- MUST Before using any structured logging library API, the consumer MUST discover the project's dependency lock file, resolve the exact installed version, and verify all method signatures against that version's official documentation

In scope:
- All handler functions that process HTTP, DNS, TCP, or ping monitoring requests
- Client interaction code that performs external HTTP requests or network operations
- Retry logic implementations using backoff strategies
- Public API contract implementations that expose error states
- Integration points with third-party APIs where errors must be captured

Out of scope:
- Debug-level logging for non-error conditions
- Metrics collection or telemetry emission
- Error handling logic that does not produce log output
- Test fixtures or mock implementations

## Rationale

- Evidence shows consistent use of log.Ctx(ctx).Error() and logger.Error() patterns across 12 files with significance 0.89-0.92, indicating an established architectural pattern for context-aware error logging
- The pattern coordinates with boundaries.external_clients and boundaries.resilience facets, where error context is critical for distinguishing network failures from application logic errors during retry operations
- Structured error logging with err.Error() field extraction appears in handlers for all protocol types (HTTP, DNS, TCP), demonstrating cross-cutting concern that requires standardization
- The detection of api.public.contracts alongside obs.logging in the same files indicates that error logging directly supports the observability requirements of public API implementations

## Consequences

Positive:
- Request correlation identifiers propagate automatically through context, enabling rapid incident triage across distributed monitoring operations
- Structured error fields enable automated log aggregation and alerting based on error types, regions, or monitor categories
- Consistent error logging patterns reduce cognitive load when debugging across different protocol handlers and client implementations
- Integration with structured logging libraries provides automatic serialization of complex error contexts without manual string formatting

Negative:
- Context propagation requires explicit threading of context objects through all function call chains, increasing function signature complexity
- Structured logging libraries introduce dependency on third-party packages with version-specific APIs that must be maintained
- Over-logging in high-frequency retry loops may generate excessive log volume without corresponding diagnostic value
- Teams must maintain consistency in which structured fields are logged for each error type to preserve queryability

## Alternatives

- Use standard library log package with string formatting for all error logging (rejected)
  Rejected because: String formatting loses structured context needed for automated log aggregation and cannot propagate request correlation identifiers without manual extraction and concatenation
  When valid: Valid only for simple command-line tools or scripts where log aggregation is not required
- Implement custom error wrapping with context fields but use unstructured log output (rejected)
  Rejected because: Custom error wrapping provides context propagation but unstructured output prevents automated parsing and querying in log aggregation systems
  When valid: Valid for applications that write logs to files parsed by custom tooling rather than centralized log aggregation platforms
- Adopt OpenTelemetry tracing for error context instead of structured logging (deferred)
  Rejected because: OpenTelemetry provides superior distributed tracing but requires infrastructure investment and does not replace the need for structured error logs for non-trace-based debugging
  When valid: Valid as a complementary approach when distributed tracing infrastructure is available and trace context can be correlated with log entries

## Risks

- Structured logging library version upgrades may introduce breaking API changes that affect error logging call sites across 12+ files
  Mitigation: Pin dependency versions in lock files and establish integration tests that verify error logging behavior across protocol handlers before upgrading
  Owner: engineering team
- High-frequency error logging in retry loops may overwhelm log aggregation systems or exceed log volume quotas
  Mitigation: Implement sampling or rate-limiting for error logs within retry loops and use log level configuration to control verbosity in production
  Owner: engineering team
- Inconsistent structured field naming across different handlers reduces log queryability and complicates alerting rules
  Mitigation: Establish naming conventions for common structured fields and enforce through code review and linting rules that validate log call patterns
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
- Establish a context propagation pattern where handler entry points extract or create request correlation identifiers and attach them to the context object before passing to downstream functions
- Define standard structured field names for common error contexts such as operation type, retry attempt, external endpoint, and monitor identifier to ensure consistent queryability
- For retry loops with backoff strategies, log errors at the final failure point rather than every attempt, or implement sampling to reduce log volume while preserving diagnostic value

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and lock file, then locate the verification script that validates structured logging patterns across handler implementations
- Identify the project's static analysis or linting configuration and execute the rule that checks for context-aware error logging method usage
- Locate the integration test suite for protocol handlers and run tests that verify error log output includes structured fields and context correlation

Accept when:
- All error logging call sites in handler functions use context-aware structured logging methods with error field extraction
- Static analysis confirms no error logging uses string concatenation or unstructured output methods
- Integration tests verify that error logs from retry operations include correlation identifiers and can be queried by structured fields

## Enforcement

- Verified by: Static analysis rules in continuous integration that detect unstructured error logging patterns
- Verified by: Code review checklist items requiring verification of context propagation in new handler implementations
- Verified by: Integration test coverage requirements for error logging paths in protocol handlers
- Violation handling: CI pipeline fails if static analysis detects error logging without context propagation or structured fields
- Violation handling: Code review blocks merge if new handler functions do not follow established error logging patterns
- Violation handling: Post-incident reviews examine whether error logs contained sufficient context and update standards if gaps are identified
- Exception process: Exception requests must document why structured logging cannot be used and propose alternative observability mechanisms
- Exception process: Architecture review board evaluates whether the exception introduces gaps in operational visibility
- Exception process: Approved exceptions are documented in code comments with expiration dates for re-evaluation