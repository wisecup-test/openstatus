# Standardize Service Boundary Definitions Through Header and Context Accessors: Implementations Cache Frequently

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service implements HTTP, TCP, and DNS monitoring handlers that must interact with external services and internal event systems
- Request and response metadata must be accessed, modified, and propagated across service boundaries to support monitoring operations
- The codebase uses framework-provided accessor methods to read and write headers, query parameters, and context values at service boundaries
- Six files across the checker application demonstrate consistent patterns for accessing service boundary data through standardized getter and setter methods
- The pattern supports both inbound request processing and outbound client communication with external systems

## Problem Statement

Service boundary interactions require consistent mechanisms for accessing and manipulating request metadata, response headers, and context values. Without standardized accessor patterns, boundary definitions become fragmented, making it difficult to maintain uniform behavior across HTTP, TCP, and DNS monitoring handlers, and complicating integration with external clients and internal event systems.

## Decision

1. MAY: Implementations MAY cache frequently accessed context values in local variables after initial retrieval

## Policy Block

- MAY Implementations MAY cache frequently accessed context values in local variables after initial retrieval

In scope:
- HTTP request and response handlers in monitoring services
- External client initialization and request preparation code
- Context value management for event tracking and request correlation
- Query parameter extraction for handler configuration

Out of scope:
- Internal data structure field access within service logic
- Direct manipulation of underlying transport layer protocols
- Database query parameter binding
- Configuration file parsing

## Rationale

- The evidence shows consistent use of accessor methods across six files with 90.65% confidence, indicating an established architectural pattern rather than isolated implementation choices
- Using framework-provided accessors ensures compatibility with middleware, logging, and monitoring infrastructure that may intercept or modify boundary data
- The pattern appears in multiple handler types (HTTP, TCP, DNS) and both server-side and client-side code, demonstrating broad applicability across service boundary scenarios
- Standardized accessors provide abstraction over underlying data structures, enabling framework upgrades and protocol changes without modifying business logic

## Consequences

Positive:
- Uniform boundary access patterns improve code readability and reduce cognitive load when navigating between different handler implementations
- Framework-managed accessors provide built-in validation, normalization, and error handling for boundary data
- Middleware and instrumentation can reliably intercept boundary interactions through framework hooks
- Testing becomes more straightforward through framework-provided mocking and stubbing utilities for boundary accessors

Negative:
- Dependency on framework-specific accessor APIs creates coupling to the web framework and complicates migration to alternative frameworks
- Accessor method overhead may introduce minor performance costs compared to direct field access in high-throughput scenarios
- Debugging boundary issues may require understanding framework internals when accessor behavior differs from expectations
- Type safety may be reduced when using string-based keys for context value storage and retrieval

## Alternatives

- Direct field access to request and response structures (rejected)
  Rejected because: Direct field access bypasses framework middleware, logging, and validation infrastructure, and creates tight coupling to specific data structure implementations that may change across framework versions
  When valid: In performance-critical inner loops where framework overhead is measured and unacceptable, and middleware interception is explicitly not required
- Custom abstraction layer wrapping all boundary access (rejected)
  Rejected because: Introduces additional maintenance burden and complexity without clear benefit, as the framework already provides tested and documented accessor patterns that integrate with the broader ecosystem
  When valid: When planning migration between multiple frameworks or when boundary access patterns require domain-specific validation logic not provided by the framework
- Mixed approach using accessors for some boundaries and direct access for others (rejected)
  Rejected because: Creates inconsistent patterns across the codebase, making it unclear which approach to use in new code and complicating code review and maintenance
  When valid: Never recommended; consistency is critical for maintainability

## Risks

- Framework version upgrades may introduce breaking changes to accessor method signatures or behavior
  Mitigation: Maintain comprehensive integration tests covering all boundary accessor usage patterns; review framework changelogs during upgrade planning; use semantic versioning constraints in dependency management
  Owner: Engineering team
- String-based context keys lack compile-time type safety and may lead to runtime errors from typos or key mismatches
  Mitigation: Define context key constants in a central location; implement integration tests that verify context value propagation; consider code generation or linting rules to validate key usage
  Owner: Engineering team
- Performance overhead from accessor methods may become significant under extreme load
  Mitigation: Establish performance baselines and monitoring for boundary operations; profile accessor overhead in production-like environments; document any measured performance characteristics
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Identify all locations where service boundaries are crossed by examining handler registration, client initialization, and middleware configuration in the codebase
- Establish naming conventions for context keys used at service boundaries to prevent collisions and improve discoverability
- Document the specific accessor methods used for headers, query parameters, and context values in the project's architecture documentation

## Continuation Context


Verify commands:
- Discover and execute the project's static analysis or linting configuration to verify that boundary accessor patterns are used consistently
- Locate and run the project's integration test suite covering HTTP, TCP, and DNS handlers to confirm accessor method behavior
- Identify the project's code review checklist or guidelines and verify they include boundary accessor pattern requirements

Accept when:
- All service boundary code uses framework accessor methods for headers, query parameters, and context values with no direct field access
- Integration tests pass for all handler types demonstrating correct boundary data propagation
- Static analysis or code review confirms consistent accessor patterns across all boundary interaction points

## Enforcement

- Verified by: Code review checklist verification during pull request approval
- Verified by: Static analysis or linting rules detecting direct field access at service boundaries
- Verified by: Integration test coverage measuring boundary accessor usage patterns
- Violation handling: Pull requests containing direct field access at service boundaries are rejected with guidance to use framework accessors
- Violation handling: Existing violations are tracked as technical debt items and prioritized for refactoring
- Violation handling: New handler implementations must demonstrate accessor pattern compliance before merge
- Exception process: Exception requests must document specific performance requirements or framework limitations that prevent accessor usage
- Exception process: Architecture review board evaluates exception requests with evidence of measured performance impact or technical constraints
- Exception process: Approved exceptions are documented with rationale and reviewed during framework upgrade cycles