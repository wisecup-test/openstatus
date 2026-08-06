# Standardize Service Boundary Definition Through Header and Query Parameter Inspection: Query Parameters Accessed

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service processes HTTP, TCP, and DNS monitoring requests across multiple handlers that require consistent access to request metadata and event context
- Service boundaries are established through inspection of HTTP headers and query parameters to extract routing, authentication, and content-type information
- Multiple handler implementations share a common pattern of retrieving cached event data and query parameters from the request context
- External client interactions require header manipulation for user-agent identification, authorization tokens, and content-type negotiation
- The architecture separates concerns between request handling, external API communication, and internal event processing through consistent boundary inspection patterns

## Problem Statement

Services must establish clear boundaries for request routing, authentication, and data exchange without tightly coupling handler logic to transport-layer details. The system needs a consistent mechanism to inspect and manipulate request metadata across HTTP, TCP, and DNS checker handlers while maintaining separation between internal event processing and external client communication.

## Decision

1. MUST: Query parameters MUST be accessed through the framework's query accessor methods rather than direct URL parsing

## Policy Block

- MUST Query parameters MUST be accessed through the framework's query accessor methods rather than direct URL parsing

In scope:
- HTTP checker handlers processing monitoring requests
- TCP checker handlers establishing connection boundaries
- DNS checker handlers performing resolution checks
- External client wrappers communicating with third-party datastores
- Request context caching mechanisms for event data

Out of scope:
- Internal business logic that does not interact with request boundaries
- Data serialization and deserialization logic
- Retry and resilience mechanisms
- Logging and observability instrumentation

## Rationale

- The evidence shows consistent patterns of header inspection and query parameter access across 6 files with 90.65% confidence, indicating an established architectural convention
- Service boundary definition through header and parameter inspection enables loose coupling between transport mechanisms and business logic while maintaining clear contracts
- The pattern appears in both internal request handling and external client communication, demonstrating its role as a cross-cutting architectural concern
- Standardizing boundary inspection patterns reduces cognitive load and prevents inconsistent metadata extraction across different handler implementations

## Consequences

Positive:
- Clear separation between transport-layer concerns and business logic improves testability and maintainability
- Consistent header and query parameter access patterns reduce implementation errors across handler types
- Explicit boundary definition through inspection enables easier debugging and request tracing
- Standardized authorization and content-type handling simplifies integration with external datastores

Negative:
- Additional abstraction layer for request metadata access may introduce slight performance overhead
- Developers must learn and follow framework-specific accessor patterns rather than direct HTTP primitives
- Changes to boundary inspection patterns require coordinated updates across multiple handler implementations
- Tight coupling to framework-provided context and header accessor methods reduces portability

## Alternatives

- Direct HTTP request object manipulation without framework abstractions (rejected)
  Rejected because: Direct manipulation couples handlers tightly to HTTP transport details and prevents consistent caching patterns across handler types
  When valid: Valid for simple single-handler services without cross-cutting metadata requirements
- Middleware-based header injection and extraction with no handler-level inspection (rejected)
  Rejected because: Pure middleware approach prevents handlers from accessing request-specific metadata needed for conditional logic and external client configuration
  When valid: Valid when all header processing is uniform and requires no handler-specific decisions
- Structured request context objects passed explicitly to all handler functions (deferred)
  Rejected because: Not rejected; deferred pending evaluation of explicit dependency injection benefits versus current implicit context pattern
  When valid: Valid for services prioritizing explicit dependencies and compile-time type safety over framework conventions

## Risks

- Inconsistent cache key naming across handlers leads to event data retrieval failures
  Mitigation: Establish shared constants for cache keys and enforce through code review and static analysis
  Owner: engineering team
- Framework-specific accessor methods may change behavior across versions, breaking boundary inspection logic
  Mitigation: Pin framework versions in dependency manifests and test header/query parameter access patterns in integration tests
  Owner: engineering team
- Missing or malformed headers in external client requests cause silent failures or authentication errors
  Mitigation: Implement explicit header validation before external client invocation and log header construction for debugging
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
- Define shared constants for cache key names used across all handler types to prevent retrieval failures and enable refactoring
- Implement integration tests that verify header and query parameter inspection behavior across all handler types using representative request payloads
- Document the expected header structure for external client requests including User-Agent format and authorization token construction patterns

## Continuation Context


Verify commands:
- Discover and execute the project's test suite focusing on handler integration tests that verify request context caching and header inspection
- Locate and run static analysis tooling configured in the repository to detect inconsistent cache key usage across handler implementations
- Identify and invoke the project's linting configuration to verify header accessor method usage follows framework conventions

Accept when:
- All handler types successfully retrieve event data from request cache using consistent key names
- External client requests include User-Agent and Authorization headers in the expected format
- Integration tests pass for all handler types demonstrating correct header and query parameter inspection

## Enforcement

- Verified by: Integration test suite covering all handler types and external client interactions
- Verified by: Code review checklist items for header and query parameter access patterns
- Verified by: Static analysis rules detecting direct HTTP object manipulation bypassing framework accessors
- Violation handling: CI pipeline fails if integration tests detect missing or malformed headers in external client requests
- Violation handling: Code review blocks merge if handlers bypass framework accessor methods for request metadata
- Violation handling: Runtime logging captures and alerts on cache key retrieval failures indicating inconsistent naming
- Exception process: Document the technical justification for bypassing standard accessor patterns in code comments
- Exception process: Obtain approval from the architecture review team for exceptions requiring direct HTTP object manipulation
- Exception process: Add compensating integration tests covering the exceptional access pattern to prevent regression