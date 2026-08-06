# Standardize HTTP Header Manipulation and Query Parameter Access for Service Boundary Definitions: Http Requests Crossing

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- HTTP header manipulation and query parameter access patterns are observed across multiple service boundary components including checker HTTP clients, handler layers, and external API clients
- The codebase demonstrates consistent use of request and response header operations for content negotiation, authentication, and metadata propagation across service boundaries
- Service definitions rely on standardized header access patterns for User-Agent identification, Content-Type negotiation, and Authorization token propagation
- Internal API boundaries coordinate through explicit header and query parameter contracts rather than implicit coupling or shared state

## Problem Statement

Service boundary definitions require consistent mechanisms for HTTP metadata exchange, header manipulation, and query parameter access to maintain clear contracts between internal API components while supporting authentication, content negotiation, and request context propagation.

## Decision

1. MUST: All HTTP requests crossing service boundaries MUST explicitly set User-Agent headers to identify the calling service

## Policy Block

- MUST All HTTP requests crossing service boundaries MUST explicitly set User-Agent headers to identify the calling service

In scope:
- HTTP client implementations making requests to internal or external services
- HTTP handler implementations receiving requests at service boundaries
- Middleware components that inspect or modify request/response headers
- API client libraries that encapsulate external service communication

Out of scope:
- WebSocket or gRPC communication protocols
- Message queue or event bus payloads
- Database query parameters or connection strings
- File system or object storage access patterns

## Rationale

- The evidence shows consistent header manipulation patterns across checker HTTP clients, handler layers, and external API clients, indicating an established architectural approach to service boundary contracts
- Explicit header operations for User-Agent, Content-Type, and Authorization appear in 4 files with 90.77% confidence, demonstrating this is a deliberate pattern rather than isolated implementation
- Query parameter access through framework methods and header-based metadata propagation enables loose coupling between services while maintaining clear interface contracts
- The pattern supports observability, authentication, and content negotiation requirements without introducing shared state or implicit dependencies

## Consequences

Positive:
- Clear service boundary contracts through explicit HTTP metadata exchange reduce coupling and improve testability
- Standardized header manipulation enables consistent authentication, tracing, and content negotiation across all internal APIs
- Framework-provided accessor methods reduce parsing errors and improve code maintainability
- Explicit User-Agent identification improves observability and enables service-specific rate limiting or routing

Negative:
- Header-based contracts require careful coordination when evolving API interfaces to avoid breaking changes
- Proliferation of custom headers can lead to undocumented implicit contracts if not properly governed
- Framework-specific accessor methods create coupling to the HTTP framework choice
- Header inspection and manipulation adds minor overhead to request processing paths

## Alternatives

- Use shared data structures or in-process function calls for service communication instead of HTTP boundaries (rejected)
  Rejected because: The evidence shows deliberate HTTP-based service boundaries with explicit header contracts, indicating a distributed or modular architecture where HTTP provides clear interface separation and independent deployability
  When valid: Valid only for tightly coupled components within a single deployment unit where HTTP overhead is prohibitive
- Encode all metadata in request/response bodies rather than headers (rejected)
  Rejected because: HTTP headers provide standardized locations for authentication, content negotiation, and metadata that are understood by intermediaries, proxies, and observability tools, while body encoding would require custom parsing at every layer
  When valid: Valid for application-specific metadata that has no standard HTTP header representation and is only meaningful to application logic
- Use gRPC with protocol buffer metadata instead of HTTP headers (deferred)
  Rejected because: Not applicable as current evidence shows HTTP-based service boundaries; migration would require significant architectural change
  When valid: Valid for new services requiring strongly-typed contracts, bidirectional streaming, or performance-critical RPC patterns

## Risks

- Inconsistent header naming conventions across services could lead to integration failures or duplicate metadata propagation
  Mitigation: Establish and document standard header naming conventions; implement shared constants or configuration for header names; add integration tests validating header contracts
  Owner: engineering team
- Sensitive data in headers could be logged or exposed through observability tooling
  Mitigation: Implement header sanitization in logging middleware; use standard Authorization header format that observability tools recognize as sensitive; audit header propagation paths
  Owner: engineering team
- Framework-specific accessor methods create migration friction if HTTP framework changes
  Mitigation: Encapsulate header access behind internal adapter interfaces; document framework dependencies; maintain test coverage for header manipulation logic
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
- Establish shared constants or configuration for standard header names to ensure consistency across service boundaries and reduce typo-related integration failures
- Implement middleware or interceptor patterns to centralize header manipulation logic for cross-cutting concerns like authentication, tracing, and request identification
- Create integration test suites that validate header contracts between services, including presence, format, and propagation of required headers across multi-hop request chains

## Continuation Context


Verify commands:
- Discover and execute the project's test suite focusing on HTTP client and handler integration tests that validate header manipulation and query parameter access
- Locate and run static analysis or linting rules that enforce header naming conventions and required header presence at service boundaries
- Identify and execute end-to-end tests that verify header propagation across multi-service request flows

Accept when:
- All HTTP client implementations set User-Agent headers and validate Content-Type on requests with bodies
- Authorization headers are present in all external service client requests and use bearer token format
- Integration tests pass demonstrating correct header propagation and query parameter access across service boundaries

## Enforcement

- Verified by: Integration test suites validating header contracts between services
- Verified by: Code review checklist items for HTTP client and handler implementations
- Verified by: Static analysis rules detecting missing or malformed header operations
- Violation handling: Pull requests failing integration tests for header contracts are blocked from merge
- Violation handling: Code review identifies missing header operations and requests changes before approval
- Violation handling: Runtime monitoring alerts on missing User-Agent or Authorization headers at service boundaries
- Exception process: Document exception rationale in code comments explaining why standard header operations are not applicable
- Exception process: Obtain architecture review approval for service boundaries that deviate from standard header contracts
- Exception process: Add compensating controls such as alternative authentication mechanisms or custom integration tests