# Adopt Standard Library Core Dependencies for HTTP Testing Infrastructure: Http Client Implementations

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The checker service implements HTTP monitoring and validation across multiple handlers, requiring consistent request/response processing, header manipulation, and body serialization for integration testing
- Seven files across the checker application demonstrate repeated use of standard library packages for encoding, networking, and context management in test-adjacent code paths
- External client interactions require timeout configuration, retry logic with exponential backoff, and TLS support, necessitating a stable foundation of core dependencies
- Integration test scenarios process request and response bodies, validate headers, and evaluate assertions against HTTP endpoints, demanding reliable serialization and deserialization primitives

## Problem Statement

The checker service must maintain consistent HTTP testing infrastructure across handlers, jobs, and client implementations without introducing fragmentation in core dependency selection or encoding strategies, while supporting integration test scenarios that validate external service interactions.

## Decision

1. MUST: HTTP client implementations used in integration testing must configure timeouts using standard library time primitives

## Policy Block

- MUST HTTP client implementations used in integration testing must configure timeouts using standard library time primitives

In scope:
- HTTP checker handlers and job implementations
- Integration test request/response processing
- External HTTP client configuration and execution
- Request body serialization and response body deserialization
- Header manipulation and validation logic

Out of scope:
- Unit test mocking frameworks
- End-to-end test orchestration tools
- Performance testing load generators
- Protocol buffer compilation and code generation

## Rationale

- Seven files demonstrate consistent use of standard library packages including bytes, context, encoding/json, net/http, time, crypto/tls, errors, and fmt for HTTP testing infrastructure
- Standard library dependencies provide stable APIs with backward compatibility guarantees, reducing maintenance burden and version conflict risk in testing code
- Integration test scenarios across checker handlers, HTTP jobs, and ping endpoints show uniform patterns of body processing, header manipulation, and client configuration using core packages
- The pattern enables consistent error handling and logging integration through standard error primitives observed across all checker components

## Consequences

Positive:
- Reduced dependency surface area and elimination of third-party package version conflicts in HTTP testing infrastructure
- Consistent serialization and deserialization behavior across all integration test scenarios
- Simplified onboarding for engineers familiar with standard library HTTP and encoding patterns
- Stable API surface with long-term backward compatibility guarantees from language maintainers

Negative:
- Standard library HTTP client lacks advanced features like connection pooling configuration and middleware composition available in third-party libraries
- Manual implementation required for retry logic and resilience patterns not provided by core packages
- Limited built-in support for structured logging and observability integration compared to specialized testing frameworks
- Verbose error handling patterns compared to libraries offering wrapped error types with context

## Alternatives

- Adopt a third-party HTTP testing framework with built-in assertion libraries and mock server capabilities (rejected)
  Rejected because: Introduces additional dependency maintenance burden and version management complexity without addressing the core requirement for consistent request/response processing in production-adjacent integration test paths
  When valid: When test scenarios require extensive mocking of external services or complex assertion DSLs not achievable with standard library primitives
- Use specialized serialization libraries with schema validation for request/response body handling (rejected)
  Rejected because: Evidence shows JSON encoding/decoding using standard library is sufficient for current integration test scenarios, and additional schema validation is handled at the protocol buffer layer
  When valid: When integration tests require runtime schema validation beyond protocol buffer contracts or need to test schema evolution scenarios
- Implement custom HTTP client wrapper abstracting standard library and third-party implementations (deferred)
  Rejected because: Not currently justified by evidence, but may become necessary if client configuration patterns diverge across handlers or if advanced features like circuit breaking are required
  When valid: When multiple HTTP client implementations need to coexist or when cross-cutting concerns like distributed tracing require uniform instrumentation

## Risks

- Standard library HTTP client default behaviors may not align with production requirements for connection reuse, keep-alive, or timeout inheritance
  Mitigation: Document explicit client configuration patterns observed in evidence and establish integration test coverage for timeout and connection behavior
  Owner: engineering team
- Manual retry logic implementation using exponential backoff may introduce inconsistencies across handlers if not centralized
  Mitigation: Extract common retry patterns into shared utilities and enforce usage through code review and static analysis
  Owner: engineering team
- Future requirements for advanced HTTP features may require migration away from standard library clients
  Mitigation: Isolate HTTP client construction behind factory functions to enable future implementation swapping without widespread code changes
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
- Centralize HTTP client construction with explicit timeout configuration to ensure consistent behavior across all integration test scenarios and production handlers
- Establish shared utilities for common patterns including header manipulation, body serialization, and error wrapping to reduce code duplication across checker components
- Document standard library package import conventions and maintain consistency in error handling patterns across all HTTP testing infrastructure

## Continuation Context


Verify commands:
- Discover the project's dependency manifest and identify all standard library package imports used in HTTP testing infrastructure
- Locate and execute the project's integration test suite to verify HTTP request/response processing behavior
- Identify static analysis or linting configuration that enforces standard library usage patterns in test paths

Accept when:
- All HTTP integration test files demonstrate consistent use of standard library packages for encoding, networking, and context management
- HTTP client construction patterns show explicit timeout configuration using standard library time primitives
- Request and response body processing uses standard library JSON encoding/decoding without third-party serialization dependencies

## Enforcement

- Verified by: Static analysis of import statements in integration test files and HTTP handler implementations
- Verified by: Code review verification of HTTP client construction and configuration patterns
- Verified by: Dependency manifest inspection to confirm absence of third-party HTTP testing frameworks in core testing paths
- Violation handling: Code review rejection for introduction of third-party HTTP libraries in integration test infrastructure without architectural justification
- Violation handling: Automated linting failures for non-standard serialization or networking package usage in test paths
- Violation handling: Documentation of exceptions with rationale when advanced features require deviation from standard library
- Exception process: Submit architectural proposal documenting specific standard library limitation and third-party library justification
- Exception process: Obtain approval from engineering team lead with evidence that standard library primitives are insufficient
- Exception process: Document exception in architecture decision log with scope boundaries and migration path if standard library capabilities evolve