# Enforce Zod Schema Validation at Integration Boundaries: Schema Validators Enforce

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Integration endpoints receive untrusted external input from multiple sources including HTTP requests, message queues, and third-party webhooks
- Type safety at compile time does not guarantee runtime data integrity when crossing service boundaries or deserializing persisted state
- Multiple integration points share common validation requirements for structured payloads containing identifiers, timestamps, and nested objects
- Schema validation libraries provide declarative constraint enforcement with structured error reporting for debugging and observability

## Problem Statement

Integration boundaries require runtime validation to reject malformed or malicious input before it propagates into business logic, but ad-hoc validation code is error-prone, inconsistent across endpoints, and difficult to maintain as schemas evolve.

## Decision

1. MUST: Schema validators MUST enforce type constraints, required fields, and format validation for all structured data crossing integration boundaries

## Policy Block

- MUST Schema validators MUST enforce type constraints, required fields, and format validation for all structured data crossing integration boundaries

In scope:
- HTTP API endpoints accepting external requests
- Message queue consumers processing asynchronous payloads
- Webhook handlers receiving third-party notifications
- Cache deserialization of persisted structured data
- Service-to-service integration boundaries

Out of scope:
- Internal function parameters within a single service boundary
- Database query results from trusted internal schemas
- Configuration files loaded at application startup
- Type-safe internal data structures already validated at entry

Exceptions:
- EX-001: Performance-critical hot paths where input has been pre-validated by upstream middleware

## Rationale

- Evidence shows consistent use of schema validation at integration boundaries in both Slack confirmation store and screenshot service endpoints, with explicit error handling for validation failures
- The pattern uses declarative schema definitions with type inference, reducing duplication between validation logic and type definitions while maintaining runtime safety
- Structured validation errors enable observability through logging and provide actionable feedback for debugging integration failures
- Schema composition patterns observed in nested payload validation demonstrate reusability and consistency across related data structures

## Consequences

Positive:
- Runtime type safety prevents malformed data from propagating into business logic and corrupting application state
- Declarative schema definitions serve as executable documentation of integration contracts
- Structured validation errors improve debugging and observability at integration boundaries
- Schema composition enables consistent validation logic across related endpoints and reduces code duplication

Negative:
- Schema validation adds runtime overhead to request processing, particularly for complex nested structures
- Schema definitions require maintenance as integration contracts evolve, creating additional coordination points
- Validation error messages may expose internal structure details that require sanitization before external exposure
- Overly strict schemas can cause integration brittleness when external systems make backward-compatible changes

## Alternatives

- Rely on static type checking without runtime validation (rejected)
  Rejected because: Static types are erased at runtime and cannot protect against malformed external input or deserialization attacks
  When valid: Only valid for internal function boundaries where all callers are type-checked at compile time
- Implement manual validation with conditional checks and type guards (rejected)
  Rejected because: Manual validation is error-prone, inconsistent across endpoints, and difficult to maintain as schemas evolve
  When valid: Acceptable for simple single-field validations where schema libraries would add unnecessary complexity
- Use middleware-based validation separate from endpoint handlers (deferred)
  Rejected because: Not rejected but not currently implemented in observed pattern
  When valid: Valid for shared validation logic across multiple endpoints with identical schema requirements

## Risks

- Schema validation performance overhead may impact latency-sensitive integration endpoints under high load
  Mitigation: Profile validation performance in production-like environments and implement caching for compiled schemas where supported
  Owner: Engineering team
- Overly restrictive schemas may reject valid requests from external systems after they make backward-compatible changes
  Mitigation: Design schemas to accept optional fields and use permissive validation for non-critical attributes while enforcing strict validation only for security-critical fields
  Owner: Engineering team
- Validation error messages may leak internal implementation details or security-sensitive information to external callers
  Mitigation: Implement error sanitization layer that logs detailed validation errors internally while returning generic error messages to external clients
  Owner: Security team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define schema validators as constants colocated with endpoint handlers to maintain clear visibility of validation contracts and enable type inference for downstream code
- Use safe parsing methods that return result types for validation errors to enable graceful error handling without exception-based control flow
- Log validation failures with structured error details including the validation library error object to support debugging while sanitizing error messages returned to external clients

## Continuation Context


Verify commands:
- Discover the project's test runner from the dependency manifest and execute the integration test suite covering validation behavior at external boundaries
- Discover the project's static analysis configuration and execute type checking to verify schema definitions provide type inference for validated payloads
- Discover the project's linting configuration and execute validation to ensure schema definitions are colocated with integration endpoint handlers

Accept when:
- All integration endpoints that accept external input have explicit schema validators defined and applied before business logic execution
- Validation failures are logged with structured error details and result in request rejection with appropriate error responses
- Schema definitions provide type inference that eliminates redundant type annotations in endpoint handler code

## Enforcement

- Verified by: Automated integration tests covering validation behavior for valid and invalid payloads at each external boundary
- Verified by: Code review checklist requiring schema validator presence for all new integration endpoints
- Verified by: Static analysis rules detecting integration endpoints without schema validation
- Violation handling: Pull requests adding integration endpoints without schema validation are blocked by code review
- Violation handling: Static analysis violations trigger build failures in continuous integration pipeline
- Violation handling: Production monitoring alerts on unvalidated input reaching business logic through exception tracking
- Exception process: Request architecture review with documented justification for exception
- Exception process: Provide performance benchmarks or technical constraints preventing schema validation
- Exception process: Document alternative validation mechanism and maintain equivalent test coverage
- Exception process: Obtain approval from security team for endpoints handling sensitive data