# Adopt Structured Error Logging with Context Propagation: Non Critical Errors

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase spans multiple service handlers, job processors, and external client integrations that require consistent error visibility across distributed operations
- Error conditions occur at multiple layers including HTTP request handling, DNS resolution, TCP connectivity checks, and third-party API interactions
- Context propagation through request lifecycles enables correlation of errors with specific operations, regions, and monitoring events
- Structured logging with error extraction provides machine-readable output for aggregation, alerting, and operational analysis

## Problem Statement

Without standardized error logging patterns, failures in distributed health checks, external API calls, and retry operations become difficult to diagnose, correlate across service boundaries, and aggregate for operational visibility. Inconsistent error handling obscures root causes and delays incident response.

## Decision

1. MAY: Non-critical errors in retry loops may be logged at lower severity levels to distinguish transient from terminal failures

## Policy Block

- MAY Non-critical errors in retry loops may be logged at lower severity levels to distinguish transient from terminal failures

In scope:
- HTTP request handlers and middleware
- Background job processors for DNS, TCP, and HTTP checks
- External client integrations and API calls
- Retry and backoff operations
- Service initialization and shutdown sequences

Out of scope:
- Debug-level trace logging for successful operations
- Performance metrics collection
- Audit logging for security events

## Rationale

- Evidence shows consistent use of error extraction methods across 12 files spanning handlers, checkers, and job processors, indicating an established pattern
- Context propagation through logging calls enables correlation with request identifiers and event metadata stored in request contexts
- Structured error logging supports operational requirements for distributed health checking across multiple regions and providers
- The pattern aligns with observability needs for systems performing external connectivity validation with retry semantics

## Consequences

Positive:
- Errors become machine-readable and aggregatable across distributed service instances
- Context propagation enables correlation of failures with specific monitoring events, regions, and retry attempts
- Consistent error extraction patterns reduce cognitive load during incident response
- Structured logs integrate with centralized logging infrastructure for alerting and analysis

Negative:
- Structured logging libraries introduce dependencies that must be maintained and versioned
- Context propagation requires discipline to thread context through all call chains
- Excessive error logging in retry loops can generate high log volumes without additional filtering
- Structured logging may have higher runtime overhead compared to simple string formatting

## Alternatives

- Use standard library logging with unstructured string formatting (rejected)
  Rejected because: Unstructured logs are difficult to parse, aggregate, and query programmatically, reducing operational visibility in distributed systems
  When valid: Appropriate for simple single-instance applications without centralized log aggregation requirements
- Implement custom error tracking with dedicated error reporting service integration (deferred)
  Rejected because: Adds external service dependency and complexity; structured logging provides sufficient visibility for current operational needs
  When valid: Consider when error rates exceed log aggregation capacity or when detailed error analytics and grouping are required
- Log errors only at service boundaries without internal operation logging (rejected)
  Rejected because: Loses visibility into retry behavior, backoff operations, and intermediate failures that are critical for diagnosing distributed health check issues
  When valid: Appropriate for simple request-response services without retry logic or complex failure modes

## Risks

- High-frequency retry operations may generate excessive log volume, increasing storage costs and reducing signal-to-noise ratio
  Mitigation: Implement log sampling or rate limiting for transient errors in retry loops; use lower severity levels for expected transient failures
  Owner: engineering team
- Context propagation failures may result in logs without correlation identifiers, breaking distributed tracing
  Mitigation: Enforce context parameter presence through linting rules; validate context propagation in integration tests
  Owner: engineering team
- Structured logging library API changes may require widespread code updates across all error handling sites
  Mitigation: Abstract logging behind internal interfaces; pin library versions and test upgrades in isolated environments before rollout
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
- Establish a consistent pattern for extracting error messages through the error interface method rather than string formatting, ensuring structured log fields capture error details separately from log messages
- Thread context through all operation chains including HTTP handlers, job processors, and external client calls to enable context-aware logging with correlation identifiers
- Configure log levels to distinguish between transient errors in retry operations and terminal failures, preventing log volume issues while maintaining visibility into failure patterns

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the structured logging library; locate its lock file and resolve the exact version in use
- Search the codebase for error logging patterns and verify that error extraction uses the error interface method rather than string formatting
- Identify test suites that validate context propagation through logging calls and execute them to confirm correlation identifiers are preserved

Accept when:
- All error logging sites extract error details through structured logging library methods with error interface calls
- Context propagation is verified through tests that confirm correlation identifiers appear in logs for distributed operations
- Log output is machine-readable and parseable by the project's log aggregation infrastructure

## Enforcement

- Verified by: Code review checklist requiring context propagation and structured error extraction in all error handling paths
- Verified by: Static analysis rules detecting error logging patterns that use string formatting instead of structured field extraction
- Verified by: Integration tests validating log output format and correlation identifier presence
- Violation handling: Code review feedback requiring revision of error logging to use structured extraction
- Violation handling: Static analysis failures blocking merge until logging patterns conform to standards
- Violation handling: Post-deployment log analysis identifying missing correlation identifiers for remediation
- Exception process: Document justification for alternative logging approach in code comments and architecture decision log
- Exception process: Obtain approval from engineering lead for exceptions in performance-critical paths
- Exception process: Review exceptions quarterly to assess whether standard patterns can be applied