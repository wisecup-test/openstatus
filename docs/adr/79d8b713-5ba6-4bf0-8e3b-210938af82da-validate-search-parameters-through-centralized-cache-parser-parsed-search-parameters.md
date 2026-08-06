# Validate Search Parameters Through Centralized Cache Parser: Parsed Search Parameters

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Server-side page components receive search parameters as Promise-wrapped objects from the framework router, requiring asynchronous resolution before use
- Search parameters originate from untrusted client input via URL query strings and require validation before being consumed by application logic or passed to data access layers
- The codebase uses a centralized searchParamsCache parser that provides type-safe validation and parsing of incoming search parameters
- Page components coordinate multiple asynchronous operations including search parameter parsing, query prefetching, and client hydration before rendering

## Problem Statement

Without mandatory validation of search parameters at page component boundaries, untrusted URL query string data can flow directly into application logic, database queries, and API calls, creating injection vulnerabilities and type safety violations that compromise security and runtime stability.

## Decision

1. SHOULD: Parsed search parameters SHOULD be destructured to extract only the specific values required by the page component

## Policy Block

- SHOULD Parsed search parameters SHOULD be destructured to extract only the specific values required by the page component

In scope:
- All server-side page components that accept search parameters
- Route handlers that process URL query strings
- Data fetching logic that depends on search parameter values

Out of scope:
- Client-side components that receive validated parameters as props
- Internal function calls with type-safe parameters
- Static page components without dynamic parameters

## Rationale

- The evidence shows consistent application of searchParamsCache.parse() across page components before any parameter consumption, indicating an established validation boundary pattern
- Both detected instances demonstrate the pattern of awaiting search parameter parsing before prefetching queries that depend on those parameters, preventing untrusted data from reaching data access layers
- The pattern coordinates asynchronous validation with Promise.all to ensure parsing completes before downstream operations, maintaining temporal ordering of security controls
- Centralized validation through a cache parser provides a single enforcement point for input validation rules, reducing the attack surface compared to ad-hoc validation scattered across components

## Consequences

Positive:
- Untrusted URL query string input is validated at page component boundaries before reaching application logic or database queries
- Type safety is enforced for search parameters through centralized parsing, reducing runtime type errors
- Validation logic is consolidated in a single cache parser implementation, improving maintainability and consistency
- The pattern integrates naturally with asynchronous page component architecture and query prefetching workflows

Negative:
- Every page component that accepts search parameters must implement the parsing pattern, increasing boilerplate code
- The centralized cache parser becomes a critical dependency that must be correctly configured for all parameter schemas
- Asynchronous parsing adds latency to page rendering, though this is typically negligible compared to data fetching operations
- Developers must maintain parameter schema definitions in the cache parser configuration alongside component logic

## Alternatives

- Validate search parameters inline within each page component using ad-hoc validation logic (rejected)
  Rejected because: Ad-hoc validation scatters security controls across multiple components, increasing the likelihood of inconsistent validation rules and missed validation points
  When valid: Acceptable only for prototype or single-page applications where centralized validation infrastructure is not justified
- Implement validation as middleware that intercepts requests before they reach page components (rejected)
  Rejected because: Framework routing architecture delivers search parameters directly to page components as props, making middleware interception complex and potentially incompatible with static optimization
  When valid: Valid for API routes or server actions where middleware can intercept before handler execution
- Use framework-native parameter validation if available in future versions (deferred)
  Rejected because: Not currently available in the framework version in use
  When valid: Should be reconsidered when framework provides built-in parameter validation with comparable type safety guarantees

## Risks

- Developers may bypass the cache parser validation when adding new page components, creating unvalidated input paths
  Mitigation: Implement automated detection in CI that scans for page components accepting search parameters without corresponding cache parser invocations
  Owner: engineering team
- Cache parser schema definitions may become stale or incomplete as parameter requirements evolve, allowing unexpected values through validation
  Mitigation: Establish code review checklist requiring schema updates when search parameter usage changes, and implement runtime monitoring for validation failures
  Owner: engineering team
- Validation errors in the cache parser may not be handled gracefully, leading to unhandled promise rejections or error page rendering
  Mitigation: Wrap cache parser invocations in error boundaries and implement fallback behavior for validation failures
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
- Locate the centralized cache parser module in the codebase and examine its schema definition structure to understand how parameter types and validation rules are declared
- When adding new page components that accept search parameters, structure the component to await cache parser resolution before any conditional logic or data fetching operations
- For parameters that are optional or have default values, ensure the cache parser schema explicitly declares these defaults rather than handling them in component logic after parsing

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the linting or type-checking commands to verify search parameter usage patterns
- Locate test files corresponding to page components and execute the test suite to verify cache parser integration
- Search the codebase for page component definitions that accept search parameters and verify each invokes the cache parser before parameter consumption

Accept when:
- All page components that receive search parameters invoke the cache parser before accessing parameter values
- Static analysis or linting passes without violations of search parameter validation rules
- Test coverage includes validation of cache parser integration for all page components with search parameters

## Enforcement

- Verified by: Automated static analysis in continuous integration scanning for page components with unvalidated search parameter access
- Verified by: Code review checklist requiring verification of cache parser invocation for any page component changes involving search parameters
- Verified by: Type checking enforcement ensuring search parameters are accessed only after cache parser resolution
- Violation handling: CI pipeline fails if static analysis detects page components accessing search parameters without cache parser validation
- Violation handling: Code review blocks merge requests that introduce unvalidated search parameter access
- Violation handling: Runtime monitoring alerts on validation failures or unhandled promise rejections from cache parser invocations
- Exception process: Exception requests must document why standard cache parser validation is insufficient for the specific use case
- Exception process: Security review is required for any exception involving search parameters that influence data access or authentication decisions
- Exception process: Approved exceptions must implement equivalent validation guarantees and document the alternative validation mechanism