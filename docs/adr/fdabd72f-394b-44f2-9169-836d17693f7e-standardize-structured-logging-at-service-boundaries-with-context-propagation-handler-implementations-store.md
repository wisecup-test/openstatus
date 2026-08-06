# Standardize Structured Logging at Service Boundaries with Context Propagation: Handler Implementations Store

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service implements HTTP, TCP, and DNS monitoring handlers that interact with external systems and require comprehensive error tracking across service boundaries
- Multiple handler implementations share a common pattern of context-aware logging using log.Ctx(ctx).Error() with structured error messages via err.Error()
- Service boundaries include external HTTP clients with custom headers, retry logic with exponential backoff, and integration with third-party APIs requiring request/response correlation
- The codebase demonstrates consistent use of request context for event tracking through c.Get/c.Set operations and header manipulation for client identification
- Health check endpoints and multi-protocol handlers require uniform observability to diagnose failures across distributed checker regions

## Problem Statement

Service boundaries in distributed monitoring systems require consistent error visibility and request correlation across HTTP, TCP, and DNS protocols, but ad-hoc logging approaches create gaps in traceability when requests traverse multiple handlers, external clients, and retry mechanisms.

## Decision

1. SHOULD: Handler implementations SHOULD store and retrieve event correlation data using the request context storage mechanism

## Policy Block

- SHOULD Handler implementations SHOULD store and retrieve event correlation data using the request context storage mechanism

In scope:
- HTTP handler implementations that invoke external services
- TCP and DNS protocol handlers with retry logic
- Client initialization code that configures timeouts and transport settings
- Event tracking middleware that manages request correlation
- Error paths in backoff retry operations

Out of scope:
- Internal business logic that does not cross service boundaries
- Database query operations within a single service context
- Pure computation functions without I/O
- Static configuration loading at startup

## Rationale

- The evidence shows consistent use of log.Ctx(ctx).Error() across 6 files with 90.65% confidence, indicating an established pattern for context-aware error logging at service boundaries
- Header manipulation patterns (User-Agent, Authorization, Content-Type) combined with logging demonstrate the need for request correlation when interacting with external systems
- The presence of retry logic with exponential backoff in multiple handlers requires structured logging to diagnose transient failures and timeout conditions
- Event tracking through context storage (c.Get/c.Set) provides a foundation for request correlation that must be surfaced in logs for end-to-end traceability

## Consequences

Positive:
- Consistent error visibility across all service boundary interactions enables rapid diagnosis of integration failures
- Request correlation through context propagation allows tracing of requests across multiple handlers and external service calls
- Structured logging with error details preserves diagnostic information needed for debugging distributed system failures
- Standardized header injection enables external systems to identify and correlate requests originating from the checker service

Negative:
- Additional logging overhead at every service boundary may impact performance in high-throughput scenarios
- Context propagation requirements increase complexity in handler implementations and testing
- Structured logging dependencies create coupling to specific logging library APIs
- Header manipulation for every external request adds latency and increases request size

## Alternatives

- Use unstructured string concatenation for error logging without context propagation (rejected)
  Rejected because: Unstructured logs lack request correlation and make it impossible to trace errors across service boundaries in distributed systems
  When valid: Only appropriate for single-instance applications with no external service dependencies
- Implement distributed tracing with span propagation instead of context-based logging (deferred)
  Rejected because: Distributed tracing provides superior observability but requires infrastructure investment and library integration not yet present in the evidence
  When valid: When the system scales to require detailed latency analysis and cross-service dependency mapping
- Centralize all external service calls through a single gateway with unified logging (rejected)
  Rejected because: Gateway pattern adds latency and a single point of failure for multi-protocol monitoring that requires direct TCP and DNS access
  When valid: When all external interactions are HTTP-based and latency tolerance is high

## Risks

- Context propagation failures may result in orphaned log entries that cannot be correlated to originating requests
  Mitigation: Implement middleware that validates context presence and injects fallback correlation identifiers when context is missing
  Owner: engineering team
- Excessive logging at service boundaries may generate high-volume log data that overwhelms log aggregation infrastructure
  Mitigation: Implement sampling strategies for successful requests while maintaining full logging for error conditions and configurable log levels per handler
  Owner: engineering team
- Sensitive data in headers or error messages may be inadvertently logged and exposed in centralized logging systems
  Mitigation: Implement log sanitization middleware that redacts authorization tokens and sensitive query parameters before emission
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
- Implement middleware that injects correlation identifiers into request context at the earliest entry point and ensures context is passed to all downstream logging calls
- Establish a standard set of HTTP headers for external client requests that includes service identification and request correlation tokens
- Create structured error types that capture both the error message and contextual metadata required for service boundary diagnostics

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the logging library, then locate verification scripts that validate context propagation through handler chains
- Identify test suites that exercise service boundary error conditions and confirm structured log output includes correlation identifiers
- Locate static analysis or linting configuration that enforces context-aware logging patterns at service boundaries

Accept when:
- All error log entries at service boundaries include request correlation identifiers extracted from context
- HTTP client requests to external services include identifying headers as verified by integration tests
- Error conditions in retry logic produce structured log entries with error details and attempt counts

## Enforcement

- Verified by: Code review checklist requiring context-aware logging for all new service boundary implementations
- Verified by: Static analysis rules that detect error logging without context propagation
- Verified by: Integration test coverage requirements for service boundary error paths with log assertion
- Violation handling: Pull requests introducing service boundary code without context-aware logging are blocked until corrected
- Violation handling: Existing violations are tracked as technical debt items and prioritized based on service criticality
- Violation handling: Runtime monitoring alerts on log entries missing correlation identifiers to identify enforcement gaps
- Exception process: Exceptions require architectural review approval with documented justification for why context propagation is not feasible
- Exception process: Temporary exceptions must include a remediation plan with timeline for bringing code into compliance
- Exception process: All exceptions are reviewed quarterly to assess whether blocking conditions have been resolved