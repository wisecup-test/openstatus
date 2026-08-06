# Standardize HTTP Header Inspection for Service Boundary Testing: Custom Headers Required

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- HTTP checkers and handlers in the system perform integration testing by inspecting request and response headers to validate service boundary contracts
- Multiple components use header inspection patterns including User-Agent setting, Content-Type validation, and custom header propagation across service boundaries
- The codebase demonstrates a consistent pattern of reading and writing HTTP headers at service boundaries using standard library methods
- Service definitions are tested through header-based contract validation, particularly for Content-Type negotiation and authorization token propagation

## Problem Statement

Integration tests must validate service boundary contracts through HTTP header inspection, but without standardized patterns for header manipulation and validation, tests may miss critical contract violations, fail to propagate required headers, or inconsistently validate Content-Type and authorization requirements across service boundaries.

## Decision

1. SHOULD: Custom headers required for service authentication or tracing SHOULD be propagated through the request chain

## Policy Block

- SHOULD Custom headers required for service authentication or tracing SHOULD be propagated through the request chain

In scope:
- HTTP client implementations that cross service boundaries
- Integration tests that validate external service contracts
- HTTP handlers that process requests from external clients
- Header-based authentication and authorization flows

Out of scope:
- Unit tests that mock HTTP interactions
- Internal function calls that do not cross service boundaries
- WebSocket or gRPC communication protocols
- Static file serving without dynamic headers

## Rationale

- The evidence shows consistent header inspection patterns across three files in the checker application, with 91.23% confidence indicating strong architectural consistency
- Service boundary testing requires header validation to ensure contract compliance, particularly for Content-Type negotiation and authorization token propagation
- The pattern of using standard library header methods provides a stable foundation for integration testing without additional dependencies
- Header inspection at service boundaries enables early detection of contract violations before they propagate through the system

## Consequences

Positive:
- Integration tests can reliably detect service contract violations through header validation
- Consistent User-Agent identification enables service-level monitoring and debugging
- Content-Type validation prevents payload parsing errors at service boundaries
- Header propagation patterns support distributed tracing and authentication flows

Negative:
- Additional test code required to validate headers increases test complexity
- Header inspection logic must be maintained alongside service contract evolution
- Over-reliance on header validation may miss payload-level contract violations
- Custom header requirements create coupling between services

## Alternatives

- Use service mesh sidecar proxies to handle header propagation and validation automatically (rejected)
  Rejected because: Evidence shows direct header manipulation in application code rather than infrastructure-level proxy patterns, and introducing service mesh would require significant architectural changes
  When valid: When deploying to container orchestration platforms with native service mesh support and when header validation logic can be externalized from application code
- Implement middleware layer to centralize header validation logic (deferred)
  Rejected because: Current evidence shows distributed header handling across multiple components; centralization would require refactoring existing patterns
  When valid: When header validation rules become complex enough to justify centralized management or when multiple services need identical validation logic
- Skip header validation in integration tests and rely on contract testing frameworks (rejected)
  Rejected because: Evidence demonstrates active header inspection in integration tests, indicating that runtime header validation is a deliberate testing strategy
  When valid: When service contracts are fully specified in machine-readable formats and contract testing tools provide equivalent coverage

## Risks

- Header validation logic may diverge across different service boundary implementations, leading to inconsistent contract enforcement
  Mitigation: Establish shared header validation utilities and document required headers in service contract specifications
  Owner: engineering team
- Changes to header requirements may break existing integration tests without clear visibility into affected services
  Mitigation: Maintain header contract documentation and use integration test suites as regression detection for header changes
  Owner: engineering team
- Header inspection in tests may create false confidence if payload validation is insufficient
  Mitigation: Ensure integration tests validate both headers and response body content for complete contract verification
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
- Identify all service boundary crossing points in the codebase and ensure each HTTP client sets appropriate User-Agent headers for service identification
- Create shared utilities for common header validation patterns to reduce duplication across integration tests
- Document required headers for each service contract and include header validation in integration test suites
- When adding new service endpoints, include integration tests that validate both request header propagation and response header presence

## Continuation Context


Verify commands:
- Locate the project's integration test suite and execute tests that validate HTTP service boundaries
- Search the codebase for HTTP client instantiations and verify User-Agent header configuration
- Inspect integration test implementations to confirm header validation assertions are present

Accept when:
- All integration tests that cross service boundaries include assertions validating required HTTP headers
- HTTP clients consistently set User-Agent headers identifying the calling service
- Content-Type headers are validated for requests with bodies and set explicitly for JSON payloads

## Enforcement

- Verified by: Integration test suite execution in continuous integration pipeline
- Verified by: Code review verification that new HTTP clients include User-Agent configuration
- Verified by: Static analysis to detect HTTP client instantiations without header configuration
- Violation handling: Integration test failures block deployment when header validation assertions fail
- Violation handling: Code review feedback requires header configuration before merge approval
- Violation handling: Static analysis warnings flag missing header configuration for manual review
- Exception process: Document justification for skipping header validation in specific test cases
- Exception process: Obtain approval from service owner when header requirements cannot be met
- Exception process: Record exceptions in service contract documentation with expiration dates