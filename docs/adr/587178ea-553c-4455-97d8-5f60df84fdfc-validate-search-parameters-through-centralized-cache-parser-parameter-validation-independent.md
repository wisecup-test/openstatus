# Validate Search Parameters Through Centralized Cache Parser: Parameter Validation Independent

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Server-side page components receive search parameters as promises that must be awaited and validated before use
- Search parameters arrive from external HTTP requests and represent untrusted user input requiring validation
- The codebase uses a centralized search parameter cache parser to validate and type-check incoming query parameters
- Async page components coordinate prefetching of query data and parameter validation in parallel using Promise.all
- The pattern appears in dashboard routes handling monitor listings and invitation flows where query parameters control data fetching

## Problem Statement

Server-side page components must validate untrusted search parameters from HTTP requests before using them to fetch data or make routing decisions, but without a consistent validation mechanism, each component might implement ad-hoc parsing that misses edge cases, allows invalid data through, or creates type safety gaps between the parameter schema and consuming code.

## Decision

1. SHOULD: Parameter validation and independent query prefetching SHOULD be coordinated in parallel when no data dependencies exist between them

## Policy Block

- SHOULD Parameter validation and independent query prefetching SHOULD be coordinated in parallel when no data dependencies exist between them

In scope:
- Server-side page components that receive search parameters from routing
- Async page functions that prefetch data based on query parameters
- Route handlers where search parameters control application behavior or data access

Out of scope:
- Client-side components that receive parameters through props
- API route handlers with request body validation
- Static page components without dynamic parameters

## Rationale

- The evidence shows consistent use of searchParamsCache.parse() across multiple page components before parameter values are consumed, indicating a deliberate validation strategy
- Both detected instances validate parameters before conditional logic (redirect) or data fetching (queryOptions), demonstrating that validation gates downstream operations
- The pattern coordinates validation with parallel prefetching using Promise.all, showing architectural intent to validate early while maintaining performance
- Centralized validation through a cache parser provides type safety and consistent error handling across all routes that accept search parameters

## Consequences

Positive:
- Type-safe search parameter access with compile-time guarantees about parameter shape and values
- Consistent validation logic across all page components eliminates duplicate parsing code
- Early validation prevents invalid parameters from reaching data fetching or business logic layers
- Parallel coordination of validation and prefetching maintains page load performance while ensuring safety

Negative:
- Additional async coordination complexity in page components that must await both parameter parsing and query prefetching
- Centralized parser configuration must be maintained as new routes with different parameter schemas are added
- Validation failures in the parser may require custom error handling or fallback behavior in each page component

## Alternatives

- Validate search parameters inline within each page component using manual type guards and parsing logic (rejected)
  Rejected because: Creates duplicate validation logic across components, increases risk of inconsistent handling, and loses type safety guarantees that centralized schema validation provides
  When valid: For one-off pages with unique parameter requirements that don't fit the centralized schema
- Pass raw unvalidated search parameters to data fetching functions and validate at the data layer (rejected)
  Rejected because: Delays validation until after routing decisions, allows untrusted input deeper into the application, and couples data layer to HTTP parameter formats
  When valid: Never - validation should occur at the boundary where external input enters the system
- Use framework-level middleware to validate all search parameters before page components execute (deferred)
  When valid: If the framework provides middleware hooks that can access route-specific parameter schemas and run before page component execution

## Risks

- Parser configuration drift where new routes add parameters without updating the centralized schema, causing validation to reject valid requests
  Mitigation: Implement integration tests that exercise all routes with their expected parameter combinations and fail on validation errors
  Owner: engineering team
- Validation errors may not provide clear user feedback if the parser throws exceptions that aren't caught and translated to user-facing messages
  Mitigation: Wrap parser calls in try-catch blocks that map validation errors to appropriate HTTP responses or redirect to error pages with context
  Owner: engineering team
- Performance degradation if the parser performs expensive validation operations that block page rendering
  Mitigation: Profile parser execution time and optimize schema validation; consider caching parsed results keyed by raw parameter strings
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
- Define search parameter schemas in a centralized location that exports typed cache parsers for each route or route group, ensuring schema definitions are co-located with their validation logic
- Structure page components to await parameter parsing and coordinate with prefetch operations using Promise.all when operations are independent, maintaining both safety and performance
- Handle validation failures explicitly by checking for required parameters after parsing and implementing appropriate fallback behavior such as redirects or error pages

## Continuation Context


Verify commands:
- Discover the project's test execution script in the dependency manifest and run the test suite covering page components with search parameter validation
- Discover the project's static analysis or type-checking script and execute it to verify that search parameter access is type-safe throughout page components
- Discover the project's linting configuration and run the linter to detect any direct access to raw search parameters that bypasses validation

Accept when:
- All page components that receive search parameters parse them through the centralized cache parser before use
- Type checking passes without errors related to search parameter access or undefined property access on parameter objects
- Test suite confirms that invalid search parameters are rejected and valid parameters are correctly parsed and typed

## Enforcement

- Verified by: Static type checking in continuous integration that fails on unsafe parameter access
- Verified by: Code review checklist requiring validation of search parameter handling in new page components
- Verified by: Integration tests that exercise parameter validation paths and assert on validation behavior
- Violation handling: Pull requests with unvalidated search parameter access are blocked by code review
- Violation handling: Type checking failures in CI prevent merge until parameter access is properly validated
- Violation handling: Runtime validation errors are logged and monitored to detect schema drift or missing validations
- Exception process: Document the specific parameter validation requirement that cannot be met through the centralized parser
- Exception process: Propose alternative validation approach with equivalent type safety and error handling guarantees
- Exception process: Obtain approval from tech lead and document the exception in the page component with rationale