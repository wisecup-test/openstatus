# Validate Search Parameters Through Centralized Cache Before Page Rendering: Page Components Await

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Server-side page components receive search parameters as promises that must be awaited and validated before use in data fetching or rendering logic
- The application uses a centralized search parameter cache mechanism to parse and validate incoming URL query parameters in async page components
- Page components coordinate data prefetching through a query client, requiring validated parameters before initiating parallel data fetches
- The framework separates server-side page rendering from client-side hydration, necessitating parameter validation at the server boundary before state initialization

## Problem Statement

Async page components that receive search parameters as promises face a validation gap where unvalidated external input could propagate into data fetching logic, query construction, or conditional routing decisions. Without centralized parsing and validation at the page entry point, each consumer would implement ad-hoc validation, creating inconsistent security boundaries and increasing the risk of injection vulnerabilities or unexpected runtime behavior from malformed parameters.

## Decision

1. MUST: Page components MUST await the search parameter promise and pass it to the validation cache rather than accessing parameter values directly

## Policy Block

- MUST Page components MUST await the search parameter promise and pass it to the validation cache rather than accessing parameter values directly

In scope:
- All async page components in server-side rendering contexts that receive search parameters
- Data prefetching logic that depends on URL query parameters
- Conditional routing or redirect logic based on parameter presence or values
- Type definitions and schemas for expected search parameter shapes

Out of scope:
- Client-side components that receive parameters through props after server-side validation
- API route handlers with separate request validation mechanisms
- Static page components that do not accept dynamic search parameters
- Internal function parameters that are not derived from external URL input

## Rationale

- The evidence shows consistent application of searchParamsCache.parse() at the entry point of async page components before any data fetching or routing decisions, establishing a security boundary at the server-side page layer
- Both detected instances demonstrate the pattern of awaiting the search parameter promise, validating through the cache, and using validated results to drive query prefetching or conditional redirects, indicating a deliberate architectural choice to centralize validation
- The pattern coordinates with the query client prefetch mechanism, ensuring that only validated parameters flow into data fetching operations, reducing the attack surface for injection vulnerabilities
- The 91.20% confidence across 2 files in the dashboard application suggests this is an established convention for server-side page components handling external input

## Consequences

Positive:
- Centralized validation creates a single enforcement point for search parameter security, reducing the likelihood of validation bypass or inconsistent handling across page components
- Type-safe parameter parsing at the page boundary enables early detection of malformed input before it propagates into business logic or database queries
- Explicit validation before prefetching prevents unnecessary or malicious data fetches triggered by crafted URL parameters
- The pattern establishes clear separation between untrusted external input and validated internal state, improving auditability of security boundaries

Negative:
- Every page component must implement validation boilerplate even when parameter validation requirements are simple, increasing initial development overhead
- Centralized cache mechanisms introduce a dependency that must be maintained and versioned consistently across all page components
- Validation failures at the page level may result in redirects or error states that complicate debugging when parameter expectations are not clearly documented
- The pattern requires developers to understand both the async page component model and the validation cache API, increasing the learning curve for new contributors

## Alternatives

- Validate search parameters directly within each data fetching function or query hook rather than at the page component entry point (rejected)
  Rejected because: Decentralized validation creates multiple security boundaries that must be individually audited and maintained, increasing the risk of validation bypass when new data fetching paths are added. The evidence shows validation occurring before prefetch operations, indicating a deliberate choice to validate once at the boundary rather than repeatedly in each consumer.
  When valid: May be appropriate for isolated utility functions that accept parameters from already-validated sources or for internal APIs with separate authentication and authorization layers
- Use framework-level middleware to validate and transform search parameters before they reach page components (rejected)
  Rejected because: Framework middleware operates at a different layer than page-specific validation requirements. The evidence shows page-specific validation caches colocated with page components, suggesting validation rules are tightly coupled to individual page requirements rather than being globally applicable.
  When valid: Appropriate for cross-cutting validation concerns such as authentication tokens, rate limiting parameters, or global security headers that apply uniformly across all routes
- Accept search parameters as plain objects and rely on TypeScript type guards within page component logic (rejected)
  Rejected because: Type guards provide compile-time safety but do not perform runtime validation of external input. The evidence shows explicit parsing through a validation cache, indicating the need for runtime validation to handle malformed or malicious input that TypeScript cannot prevent.
  When valid: Sufficient for internal type narrowing of already-validated data or for discriminated unions where the type guard operates on trusted internal state

## Risks

- Developers may bypass the validation cache by directly accessing search parameter values, especially when under time pressure or when validation requirements seem trivial
  Mitigation: Enforce validation through code review checklists, static analysis rules that flag direct parameter access in page components, and provide clear documentation with examples showing the correct validation pattern
  Owner: Engineering team
- Validation cache implementations may evolve independently across different page components, leading to inconsistent validation behavior or security gaps when schemas diverge from actual validation logic
  Mitigation: Colocate validation cache definitions with search parameter type definitions, implement shared validation utilities for common parameter patterns, and include validation logic in unit tests that verify both positive and negative cases
  Owner: Engineering team
- Version mismatches between the validation library and the framework's async page component model may introduce breaking changes in parameter handling or validation behavior
  Mitigation: Pin validation library versions in the dependency lock file, test validation behavior across framework upgrades, and document any version-specific validation patterns or workarounds in the implementation notes
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
- Define search parameter schemas colocated with page components, typically in a separate module that exports both the type definition and the validation cache instance. This ensures validation logic remains synchronized with type expectations.
- When validation fails for required parameters, prefer explicit redirects to safe default routes rather than rendering error states, as demonstrated in the invite page pattern where missing tokens trigger a redirect to the overview page.
- Coordinate validation with query prefetching by awaiting validation before constructing query options. This ensures that query keys and parameters are derived from validated input, preventing cache pollution from malformed parameters.

## Continuation Context


Verify commands:
- Discover the project's test runner configuration and execute the test suite covering page components with search parameter validation
- Discover the project's static analysis or linting configuration and run checks that identify direct search parameter access without validation
- Discover the project's build process and verify that page components with search parameters compile without type errors when validation cache parsing is applied

Accept when:
- All async page components that receive search parameters demonstrate validation through the centralized cache before using parameter values in data fetching or routing logic
- Test coverage includes both valid and invalid search parameter cases, verifying that validation failures are handled through redirects or error states
- Static analysis or code review confirms no direct access to search parameter values without prior validation in server-side page components

## Enforcement

- Verified by: Code review checklist items requiring validation cache usage in all page components accepting search parameters
- Verified by: Static analysis rules that flag direct search parameter access patterns in async page components
- Verified by: Unit and integration tests covering validation behavior for both valid and malformed parameter inputs
- Violation handling: Pull requests introducing page components without search parameter validation are blocked until validation is added
- Violation handling: Static analysis violations trigger build failures in continuous integration pipelines
- Violation handling: Security review is required for any page component that bypasses the standard validation pattern with documented justification
- Exception process: Document the specific reason why standard validation cannot be applied in the page component's implementation comments
- Exception process: Obtain approval from a security-focused reviewer or architect before merging
- Exception process: Implement alternative validation or sanitization mechanisms and include test coverage demonstrating equivalent security properties