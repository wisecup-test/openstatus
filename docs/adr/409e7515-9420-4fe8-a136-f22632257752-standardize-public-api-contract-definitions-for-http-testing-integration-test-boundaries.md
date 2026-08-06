# Standardize Public API Contract Definitions for HTTP Testing: Integration Test Boundaries

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The system performs HTTP health checks and monitoring across multiple regions, requiring consistent contract definitions for request/response validation
- Integration testing spans checker handlers, HTTP jobs, and ping endpoints that all interact with external HTTP services and must validate responses uniformly
- Protocol buffer definitions and handler implementations expose public contracts including Http, Timing, Response, HTTPCheckerHandler, PingData, and EvaluateHTTPAssertions
- The codebase uses structured types for HTTP body validation, header manipulation, and assertion evaluation across seven files in the checker application
- External client boundaries require standardized contract interfaces to ensure consistent behavior when evaluating HTTP responses against defined assertions

## Problem Statement

Without standardized public API contract definitions for HTTP testing, the system risks inconsistent validation logic across handlers, duplicated assertion evaluation code, and fragile integration points between checker components and external HTTP services. This creates maintenance burden and increases the likelihood of divergent behavior when evaluating HTTP responses.

## Decision

1. MUST: All integration test boundaries that interact with external HTTP services MUST use the standardized contract types rather than raw HTTP client types

## Policy Block

- MUST All integration test boundaries that interact with external HTTP services MUST use the standardized contract types rather than raw HTTP client types

In scope:
- HTTP checker handlers and job implementations
- Integration test code that validates external HTTP service responses
- Protocol buffer service definitions for private location communication
- Ping and health check endpoints that evaluate HTTP assertions
- Client wrapper code that interacts with external HTTP APIs

Out of scope:
- Internal logging and observability structures
- Cache layer implementations
- Database query interfaces
- Authentication and secrets handling logic
- Build and deployment configuration

## Rationale

- Seven files across the checker application consistently expose contract types including Http, Response, Timing, HTTPCheckerHandler, PingData, and EvaluateHTTPAssertions, demonstrating an established pattern
- The evidence shows repeated Body field access patterns in integration testing contexts, indicating a standardized approach to request/response validation
- Separation of ProtoNumberAssertionToComparator and ProtoStringAssertionToComparator functions reveals deliberate contract design for type-safe assertion evaluation
- Header manipulation patterns using Set and Get methods appear consistently across HTTP client boundaries, supporting the need for standardized header contract interfaces

## Consequences

Positive:
- Consistent validation logic across all HTTP checker handlers reduces duplication and maintenance burden
- Type-safe contract definitions prevent runtime errors when evaluating assertions against HTTP responses
- Clear separation between request contracts and response contracts improves testability and mocking
- Standardized interfaces enable easier addition of new assertion types and validation rules

Negative:
- Contract definitions create coupling between handler implementations and shared type packages
- Changes to contract interfaces require coordinated updates across multiple handler files
- Additional abstraction layer may obscure direct HTTP client behavior during debugging
- Protocol buffer integration adds complexity when contracts must bridge gRPC and HTTP boundaries

## Alternatives

- Use raw HTTP client types directly in handlers without contract abstraction (rejected)
  Rejected because: Direct use of HTTP client types leads to duplicated validation logic across handlers and makes assertion evaluation inconsistent
  When valid: Only appropriate for simple one-off HTTP requests that do not require assertion evaluation
- Define contract interfaces per handler without shared types (rejected)
  Rejected because: Per-handler contracts prevent code reuse and create divergent validation behavior across the system
  When valid: When handlers have fundamentally different validation requirements that cannot be unified
- Use a single unified contract type for all HTTP interactions (deferred)
  Rejected because: May be too rigid for specialized use cases like ping handlers versus full assertion evaluation
  When valid: If future refactoring reveals that all handlers can converge on identical contract requirements

## Risks

- Contract interface changes break multiple handlers simultaneously
  Mitigation: Implement versioned contract interfaces and provide adapter functions for backward compatibility during transitions
  Owner: engineering team
- Protocol buffer contract definitions diverge from HTTP handler contracts
  Mitigation: Establish automated validation that protocol buffer message types satisfy HTTP contract interfaces
  Owner: engineering team
- Contract abstraction hides important HTTP client behavior needed for debugging
  Mitigation: Ensure contract types preserve access to underlying HTTP request and response objects for inspection
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
- Define contract types in a shared package that all checker handlers import, ensuring Body, Header, Timeout, and assertion fields are consistently structured
- Implement assertion evaluator functions that accept contract types rather than raw HTTP responses, separating number and string comparison logic
- Ensure protocol buffer message types implement or can be adapted to the standardized HTTP contract interfaces for cross-boundary communication

## Continuation Context


Verify commands:
- Discover the project's test execution script and run integration tests for HTTP checker handlers to verify contract compliance
- Locate the project's static analysis configuration and execute type checking to confirm all handlers use standardized contract types
- Identify the project's assertion validation test suite and execute it to verify separation of number and string comparator functions

Accept when:
- All HTTP checker handlers use standardized contract types for request and response structures
- Integration tests pass with consistent assertion evaluation across all handler implementations
- Static analysis confirms no direct use of raw HTTP client types in handler validation logic

## Enforcement

- Verified by: Continuous integration pipeline executes type checking and integration tests on every commit
- Verified by: Code review process verifies new handlers use standardized contract types
- Verified by: Automated static analysis flags direct use of raw HTTP client types in validation code
- Violation handling: Pull requests that introduce raw HTTP client usage in handlers are blocked until refactored to use contracts
- Violation handling: Existing violations are tracked in technical debt backlog with priority based on handler criticality
- Violation handling: Integration test failures due to contract misuse trigger immediate investigation and remediation
- Exception process: Request exception through architecture review board with justification for why standard contracts are insufficient
- Exception process: Document approved exceptions in handler code comments with reference to exception approval
- Exception process: Review all exceptions quarterly to determine if contract interfaces should be extended to eliminate the exception