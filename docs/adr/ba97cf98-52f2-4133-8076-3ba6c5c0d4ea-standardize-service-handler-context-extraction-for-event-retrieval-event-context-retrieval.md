# Standardize Service Handler Context Extraction for Event Retrieval: Event Context Retrieval

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker application implements multiple protocol handlers (HTTP, TCP, DNS) that process incoming requests and perform health checks against external services
- Each handler requires access to event context data that is stored in the request context, necessitating a consistent mechanism for retrieving this data across handler implementations
- The handlers integrate with resilience patterns using exponential backoff retry logic and external client management, requiring coordination between context extraction and execution flow
- Observability requirements mandate structured logging with context propagation, creating a dependency between service boundary definitions and logging infrastructure

## Problem Statement

Service handlers across multiple protocols need a uniform mechanism to extract event context from request objects while maintaining separation between HTTP framework concerns and business logic, ensuring consistent behavior across checker, TCP, and DNS handler implementations.

## Decision

1. MUST: Event context retrieval MUST occur before executing protocol-specific health check operations

## Policy Block

- MUST Event context retrieval MUST occur before executing protocol-specific health check operations

In scope:
- HTTP checker handlers processing health check requests
- TCP checker handlers performing connection validation
- DNS checker handlers executing resolution queries
- All protocol-specific handler implementations within the checker application

Out of scope:
- External client initialization and configuration
- Retry and backoff strategy implementation
- Response assertion evaluation logic
- Cache layer implementation details

## Rationale

- The pattern appears consistently across three handler implementations (checker.go, tcp.go, dns.go) with identical context extraction syntax, indicating an established architectural convention
- Uniform context extraction enables consistent event correlation across protocol boundaries while maintaining handler independence
- The pattern supports integration with observability infrastructure by providing a stable mechanism for context propagation to logging operations
- Standardization reduces cognitive overhead for developers implementing new protocol handlers and ensures predictable behavior across the service boundary layer

## Consequences

Positive:
- Consistent event context retrieval across all protocol handler implementations reduces implementation variance
- Clear separation between HTTP framework concerns and business logic improves testability and maintainability
- Standardized context extraction pattern simplifies onboarding for new protocol handler development
- Integration with observability infrastructure becomes predictable and uniform across service boundaries

Negative:
- Tight coupling to the HTTP framework's context API creates migration friction if framework replacement is required
- String-based context key lookup introduces runtime failure modes that cannot be caught at compile time
- Pattern requires developers to understand framework-specific context management semantics
- Additional indirection layer may obscure direct data flow for developers unfamiliar with the context extraction pattern

## Alternatives

- Pass event data explicitly as function parameters to each handler (rejected)
  Rejected because: Explicit parameter passing would require modifying all handler signatures and break compatibility with the HTTP framework's handler interface contract
  When valid: Valid for new greenfield services where handler interface contracts can be designed without framework constraints
- Use dependency injection to provide event context through handler constructors (rejected)
  Rejected because: Constructor injection does not support per-request event context variation, as handlers are typically instantiated once and reused across multiple requests
  When valid: Valid for application-scoped configuration or dependencies that do not vary per request
- Implement a custom middleware layer that extracts and validates event context before handler execution (deferred)
  Rejected because: Would provide stronger type safety and centralized validation but requires additional infrastructure investment
  When valid: Valid when type safety requirements increase or when context extraction logic becomes complex enough to warrant centralization

## Risks

- Context key string mismatch between setter and getter operations causes silent runtime failures with nil event data
  Mitigation: Implement compile-time constant for context key and add validation checks that return explicit errors when event context is missing
  Owner: engineering team
- Framework version upgrades may introduce breaking changes to context API semantics or method signatures
  Mitigation: Establish integration tests that verify context extraction behavior across handler implementations and include framework upgrade validation in CI pipeline
  Owner: engineering team
- Implicit context extraction pattern may not be discoverable by new team members, leading to inconsistent implementations
  Mitigation: Document the pattern in developer onboarding materials and create handler implementation templates that include context extraction boilerplate
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
- Define a package-level constant for the event context key to prevent string literal duplication and enable compile-time reference checking across handler implementations
- Implement explicit nil checks immediately after context extraction and return structured error responses when event data is missing, rather than allowing nil pointer dereferences in downstream logic
- Consider wrapping the context extraction pattern in a helper function that returns typed event data and error values, providing a single point of maintenance for context retrieval logic

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify the build tool, then locate and execute the project's test suite to verify handler implementations
- Discover the project's static analysis configuration and execute linting or type-checking tools to verify context key consistency across handler files
- Discover the project's integration test infrastructure and execute protocol-specific handler tests to verify event context extraction behavior

Accept when:
- All handler implementations successfully extract event context using the standardized accessor method without runtime panics or nil pointer errors
- Static analysis confirms consistent use of the same context key identifier across all protocol handler implementations
- Integration tests demonstrate successful event context propagation through the complete request lifecycle for HTTP, TCP, and DNS handlers

## Enforcement

- Verified by: Code review process verifies new handler implementations follow the standardized context extraction pattern
- Verified by: Automated integration tests validate event context retrieval behavior across all protocol handlers
- Verified by: Static analysis tools check for consistent context key usage across handler implementations
- Violation handling: Pull requests introducing inconsistent context extraction patterns are blocked until conformance is achieved
- Violation handling: Runtime errors from missing event context trigger alerting and require immediate investigation
- Violation handling: Periodic architecture reviews audit handler implementations for pattern compliance
- Exception process: Exceptions require written justification documenting why the standard pattern cannot be applied
- Exception process: Architecture review board evaluates exception requests and approves only when technical constraints prevent standard implementation
- Exception process: Approved exceptions must be documented in code comments with references to the exception approval decision