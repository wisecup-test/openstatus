# Standardize Service Boundary Identification Through Request Context Retrieval: Http Client Interactions

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service operates across multiple handler types (HTTP, TCP, DNS) that process external monitoring requests and must correlate events across service boundaries for observability.
- Request context is stored and retrieved using framework-specific context mechanisms (c.Get, c.Set) to maintain event correlation and request identity across handler invocations.
- Service boundaries are defined by HTTP header inspection patterns (req.Header.Get, response.Header.Get) that determine content types, user agents, and custom headers for external client communication.
- Error logging consistently uses structured context-aware loggers (log.Ctx(ctx).Error(), logger.Error()) that require request-scoped data to produce meaningful diagnostic output.
- The system integrates with external services (Tinybird analytics) and implements retry logic with exponential backoff, requiring stable request identity across multiple attempts.

## Problem Statement

Service boundaries in a distributed monitoring system must be clearly identified and logged to enable request tracing, error correlation, and external client integration, but without a consistent pattern for retrieving and propagating request context, observability becomes fragmented across handler types and external service calls.

## Decision

1. MUST: HTTP client interactions at service boundaries MUST inspect response headers to extract metadata required for logging and downstream processing.

## Policy Block

- MUST HTTP client interactions at service boundaries MUST inspect response headers to extract metadata required for logging and downstream processing.

In scope:
- HTTP, TCP, and DNS monitoring handlers that process external check requests
- External client integrations that send events to analytics or third-party services
- Retry logic implementations that must maintain request identity across multiple attempts
- Health check endpoints that report service identity and regional information

Out of scope:
- Internal service-to-service communication within the same process boundary
- Background jobs or scheduled tasks that do not originate from external requests
- Static configuration loading or initialization routines
- Unit tests that mock request context

## Rationale

- The evidence shows consistent use of c.Get('event') across all handler types (HTTP, TCP, DNS), indicating a deliberate pattern for retrieving request-scoped correlation data at service boundaries.
- Header inspection patterns (req.Header.Get, response.Header.Get) appear in both outbound client requests and inbound request processing, establishing headers as the primary mechanism for service boundary identification.
- Context-aware logging (log.Ctx(ctx).Error()) is used uniformly across all handlers and external client code, demonstrating that request context is the foundation for observability at service boundaries.
- The pattern enables retry logic with exponential backoff to maintain stable event identity across multiple attempts, as evidenced by c.Set('event', t) followed by backoff.Retry operations.

## Consequences

Positive:
- Request tracing across service boundaries becomes consistent and reliable, enabling end-to-end observability for monitoring operations.
- Error logs automatically include request context, reducing mean time to diagnosis for boundary-crossing failures.
- External service integrations can correlate events with originating requests through stable event identifiers.
- Retry logic maintains request identity, preventing duplicate event creation or lost correlation data.

Negative:
- Framework-specific context mechanisms create coupling between handler implementations and the web framework's context API.
- Request context retrieval adds runtime overhead to every service boundary crossing, though typically negligible compared to network I/O.
- Developers must remember to retrieve and propagate context at every boundary, creating opportunities for human error if not enforced by tooling.
- Testing service boundaries requires mocking or stubbing the framework's context mechanism, increasing test complexity.

## Alternatives

- Use global request ID generation without framework context storage (rejected)
  Rejected because: Global state prevents concurrent request processing and eliminates the ability to correlate events with specific request contexts in a multi-threaded or concurrent environment.
  When valid: Only valid in single-threaded, synchronous request processing where concurrency is not a concern.
- Pass event identifiers as explicit function parameters through all layers (rejected)
  Rejected because: Explicit parameter passing creates brittle function signatures that must change whenever new context fields are needed, and pollutes business logic with observability concerns.
  When valid: Valid for small codebases with shallow call stacks where context requirements are stable and unlikely to change.
- Rely on distributed tracing libraries to automatically propagate context (deferred)
  Rejected because: Not rejected; deferred pending evaluation of distributed tracing library integration costs and compatibility with existing logging infrastructure.
  When valid: Valid when distributed tracing infrastructure is already deployed and the team has expertise in trace context propagation standards.

## Risks

- Framework context API changes could break all service boundary implementations if the framework is upgraded without compatibility testing.
  Mitigation: Wrap framework context operations in an internal abstraction layer that isolates handlers from direct framework API dependencies. Maintain comprehensive integration tests that verify context retrieval across all handler types.
  Owner: Platform Engineering Team
- Missing or incorrect context retrieval at new service boundaries could create observability gaps that are difficult to detect until production incidents occur.
  Mitigation: Implement static analysis or linting rules that verify all handler functions retrieve request context before processing. Add integration test assertions that verify context propagation to logging output.
  Owner: Engineering Team
- Context storage overhead could accumulate in long-running requests or retry loops, potentially causing memory pressure.
  Mitigation: Monitor context object sizes in production. Implement context cleanup or reset logic for retry loops that span multiple minutes. Set maximum retry limits to bound context lifetime.
  Owner: SRE Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Create a service boundary abstraction that encapsulates context retrieval, header inspection, and logger initialization to reduce boilerplate and ensure consistency across handler types.
- Establish naming conventions for context keys to prevent collisions and make context usage self-documenting in code reviews.
- Document the expected context fields for each handler type in interface definitions or handler registration code to serve as a contract for new implementations.

## Continuation Context


Verify commands:
- Locate the project's test execution script and run the integration test suite that verifies handler context retrieval across all service boundary types.
- Discover the static analysis or linting configuration and execute checks that verify context-aware logging is used in all error paths.
- Identify the project's code search or grep utility and search for context retrieval patterns to confirm all handlers follow the established pattern.

Accept when:
- All handler types (HTTP, TCP, DNS) successfully retrieve request context and propagate it to structured loggers without runtime errors.
- Static analysis confirms no error logging statements exist at service boundaries that lack context-aware logger initialization.
- Integration tests demonstrate that event correlation identifiers remain stable across retry attempts and appear in all log output.

## Enforcement

- Verified by: Continuous integration pipeline executes static analysis to detect missing context retrieval in handler functions.
- Verified by: Code review checklist includes verification that new service boundaries follow the context retrieval pattern.
- Verified by: Integration test suite validates context propagation to logging output for all handler types.
- Violation handling: Static analysis failures block pull request merging until context retrieval is added to flagged handlers.
- Violation handling: Code review identifies missing context usage and requests changes before approval.
- Violation handling: Integration test failures trigger build failures and prevent deployment to production environments.
- Exception process: Exceptions require written justification explaining why context retrieval is not applicable to the specific boundary.
- Exception process: Platform engineering team reviews exception requests to ensure they do not compromise observability requirements.
- Exception process: Approved exceptions are documented in code comments with references to the exception approval and expiration date for future review.