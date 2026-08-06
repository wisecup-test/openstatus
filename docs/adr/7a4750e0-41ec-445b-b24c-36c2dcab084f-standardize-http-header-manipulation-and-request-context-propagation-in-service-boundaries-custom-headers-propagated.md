# Standardize HTTP Header Manipulation and Request Context Propagation in Service Boundaries: Custom Headers Propagated

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system implements multiple protocol handlers (HTTP, TCP, DNS) that require consistent request metadata propagation across service boundaries
- Request context must be preserved through handler chains, including request identifiers, event tracking, and query parameters accessed via cache-layer abstractions
- External client interactions require standardized header manipulation for User-Agent identification, content negotiation, and custom header injection
- Handler implementations share common patterns for extracting and setting context data through a cache-layer interface that abstracts request/response metadata
- The architecture separates protocol-specific concerns while maintaining uniform context propagation semantics across HTTP, TCP, and DNS handlers

## Problem Statement

Service boundary implementations must propagate request context, manipulate HTTP headers consistently, and maintain metadata across handler chains without coupling protocol-specific logic to business logic, while ensuring external clients present correct identification and content type negotiation.

## Decision

1. MUST: Custom headers MUST be propagated through the request chain by reading from and writing to the header collection using key-value access patterns

## Policy Block

- MUST Custom headers MUST be propagated through the request chain by reading from and writing to the header collection using key-value access patterns

In scope:
- HTTP request handlers that interact with external services
- Protocol handlers (HTTP, TCP, DNS) that require context propagation
- Service boundary implementations that manipulate request/response headers
- Cache-layer abstractions used for request metadata access

Out of scope:
- Internal function calls that do not cross service boundaries
- Direct protocol-specific implementations that do not use the cache-layer abstraction
- Static configuration or environment variable access
- Database or persistent storage operations

## Rationale

- The evidence shows consistent patterns of header manipulation (req.Header.Set, req.Header.Get, response.Header.Get) across multiple handler implementations, indicating a standardized approach to metadata propagation
- Cache-layer operations (c.Get, c.Set, c.Query) appear in all handler files with identical patterns for event tracking and request identifiers, demonstrating architectural intent to abstract request context
- External client instantiation patterns and User-Agent header setting indicate explicit service identification requirements at boundary crossings
- The separation of protocol handlers (HTTP, TCP, DNS) with shared context propagation patterns suggests a deliberate architectural choice to maintain uniform metadata handling across diverse protocols

## Consequences

Positive:
- Consistent request context propagation enables distributed tracing and debugging across service boundaries
- Standardized header manipulation reduces protocol-specific coupling and improves handler testability
- Cache-layer abstraction allows protocol-agnostic business logic that can operate across HTTP, TCP, and DNS handlers
- Explicit User-Agent and Content-Type handling improves service observability and content negotiation reliability

Negative:
- Cache-layer abstraction introduces indirection that may obscure direct protocol-specific operations during debugging
- Mandatory header manipulation adds overhead to every external request even when headers are not semantically required
- Shared context propagation patterns create implicit coupling between handler implementations that must maintain interface compatibility
- Query parameter access through cache layer may limit access to protocol-specific features or advanced query parsing capabilities

## Alternatives

- Use protocol-specific context objects directly in handlers without cache-layer abstraction (rejected)
  Rejected because: Direct protocol coupling would prevent code reuse across HTTP, TCP, and DNS handlers and increase duplication of context propagation logic
  When valid: When implementing protocol-specific features that have no cross-protocol equivalent or when performance overhead of abstraction is prohibitive
- Implement middleware chain for automatic header injection without explicit handler code (rejected)
  Rejected because: Evidence shows explicit header manipulation in handler code rather than middleware patterns, suggesting requirements for handler-specific header logic
  When valid: When header requirements are uniform across all handlers and do not require conditional logic based on handler state
- Use structured context objects with typed fields instead of key-value cache-layer interface (deferred)
  Rejected because: Would provide stronger type safety but requires refactoring existing cache-layer patterns observed in evidence
  When valid: When type safety and compile-time validation outweigh the cost of migrating existing handler implementations

## Risks

- Cache-layer key collisions could cause context data corruption if multiple handlers use identical keys for different purposes
  Mitigation: Establish naming conventions for cache-layer keys with handler-specific prefixes and document reserved key names
  Owner: engineering team
- Missing or incorrect Content-Type headers on external requests could cause downstream service failures or data corruption
  Mitigation: Implement validation in external client wrappers to verify Content-Type is set when request body is present
  Owner: engineering team
- User-Agent header format changes could break external service integrations that parse or filter based on agent strings
  Mitigation: Version User-Agent strings according to semantic versioning and maintain backward compatibility for major version increments
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
- Examine existing handler implementations to identify the cache-layer interface methods and their signatures, then replicate the pattern for event tracking and request identifier propagation in new handlers
- When instantiating external clients, inspect existing patterns for timeout configuration and header manipulation to maintain consistency with established service identification practices
- For Content-Type validation, implement checks that compare the header value against expected MIME types before processing request or response bodies to prevent deserialization errors

## Continuation Context


Verify commands:
- Discover the project's test execution mechanism and run protocol handler test suites to verify cache-layer context propagation
- Locate static analysis or linting configuration and execute checks to identify missing User-Agent or Content-Type header operations in external client code
- Identify integration test suites that exercise service boundaries and verify they validate header presence and correctness

Accept when:
- All handler implementations successfully retrieve and store request context through the cache-layer interface without direct protocol coupling
- External HTTP requests include User-Agent headers and Content-Type headers are validated on inbound requests and set on outbound structured data
- Protocol handler test suites pass with coverage of context propagation and header manipulation scenarios

## Enforcement

- Verified by: Code review verification that new handlers use cache-layer interface for context access
- Verified by: Static analysis checks for missing User-Agent or Content-Type headers in external client instantiation
- Verified by: Integration test execution validating header propagation across service boundaries
- Violation handling: Code review rejection for handlers that bypass cache-layer abstraction without documented justification
- Violation handling: Build failure on static analysis detection of external requests missing required headers
- Violation handling: Test failure on integration tests detecting missing or incorrect context propagation
- Exception process: Document protocol-specific requirements that cannot be satisfied through cache-layer abstraction
- Exception process: Obtain architectural review approval for direct protocol coupling with justification
- Exception process: Add exception annotation in code with reference to approval and alternative approach evaluation