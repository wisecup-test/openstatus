# Validate Search Parameters Through Centralized Cache Parser: Parsed Search Parameters

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Server-side page components receive search parameters as Promise-wrapped objects that require parsing before use
- The application uses a query parameter management library (nuqs) to handle URL state synchronization and validation
- Search parameters flow through async page components that prefetch data and hydrate client-side state
- Multiple page components follow the same pattern of parsing search parameters before conditional logic or data fetching

## Problem Statement

Unvalidated search parameters from URL query strings pose security and reliability risks when used directly in server-side logic, data fetching, or routing decisions. Without centralized validation, each page component must implement its own parsing logic, leading to inconsistent validation rules and potential injection vulnerabilities.

## Decision

1. SHOULD: Parsed search parameters SHOULD be destructured immediately after parsing to make dependencies explicit

## Policy Block

- SHOULD Parsed search parameters SHOULD be destructured immediately after parsing to make dependencies explicit

In scope:
- Server-side page components that accept searchParams props
- Async page functions that perform data prefetching based on URL parameters
- Components that implement conditional routing or redirects based on query parameters

Out of scope:
- Client-side components that read URL state through hooks
- API route handlers that parse request query strings
- Middleware that processes URL parameters before page rendering

## Rationale

- The pattern appears in 2 files with 91.20% confidence, demonstrating consistent adoption of centralized search parameter validation
- Using a cache parser provides type-safe validation and prevents direct access to unvalidated URL input
- The pattern integrates with the query prefetching workflow, ensuring validated parameters are available before data fetching begins
- Centralized parsing reduces code duplication and ensures consistent validation rules across all page components

## Consequences

Positive:
- Type-safe search parameter access prevents runtime errors from malformed or missing query parameters
- Centralized validation logic reduces security vulnerabilities from unvalidated URL input
- Consistent parsing pattern across page components improves code maintainability and readability
- Integration with prefetching workflow ensures validated data is available before client hydration

Negative:
- Additional parsing step adds minimal latency to server-side page rendering
- Developers must maintain separate schema definitions for each page's search parameters
- Changes to search parameter structure require updates to both schema and parsing call sites
- Error handling for invalid search parameters must be implemented at the page level

## Alternatives

- Parse search parameters directly in page components without centralized validation (rejected)
  Rejected because: Direct parsing leads to inconsistent validation logic, duplicated code, and increased security risk from unvalidated URL input
  When valid: Only appropriate for prototype or development environments where security is not a concern
- Validate search parameters in middleware before page rendering (rejected)
  Rejected because: Middleware validation cannot provide page-specific type information and would require complex routing logic to map parameters to page schemas
  When valid: Useful for global parameter validation rules that apply across all pages, such as authentication tokens
- Use framework-native search parameter parsing without external libraries (rejected)
  Rejected because: Framework-native parsing lacks type safety and validation capabilities, requiring manual implementation of validation logic
  When valid: Acceptable for simple applications with minimal search parameter requirements and no type safety needs

## Risks

- Schema definitions may drift out of sync with actual search parameter usage, causing runtime validation failures
  Mitigation: Implement automated tests that verify search parameter schemas match actual page component usage patterns
  Owner: engineering team
- Invalid search parameters may cause page rendering failures if error handling is not implemented
  Mitigation: Establish standard error handling patterns for validation failures, such as redirecting to default page state or showing error boundaries
  Owner: engineering team
- Performance impact from parsing search parameters on every page load may affect server response times
  Mitigation: Monitor server-side rendering performance metrics and implement caching strategies for frequently accessed parameter combinations
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
- Define search parameter schemas in dedicated files co-located with page components to maintain clear ownership and facilitate schema discovery
- Implement the cache parser invocation immediately after the async searchParams prop is received, before any conditional logic or data fetching operations
- For pages with conditional redirects, ensure the parsing operation completes before evaluating redirect conditions to prevent routing decisions based on unvalidated input

## Continuation Context


Verify commands:
- Discover the project's static analysis configuration and execute the type-checking workflow to verify search parameter schemas match usage
- Locate the project's test suite and execute tests covering search parameter validation and parsing behavior
- Identify the project's linting configuration and run the linter to detect direct searchParams access without cache parser invocation

Accept when:
- All server-side page components that accept searchParams props invoke the cache parser before using parameter values
- Type checking passes without errors related to search parameter access or schema mismatches
- No direct access to searchParams properties occurs without prior cache parser invocation

## Enforcement

- Verified by: Static type checking in continuous integration pipeline
- Verified by: Code review process verifying cache parser usage in page components
- Verified by: Automated linting rules detecting unvalidated searchParams access
- Violation handling: Build failures from type checking errors block deployment
- Violation handling: Code review feedback requires revision before merge approval
- Violation handling: Linting violations trigger automated review comments and block merge
- Exception process: Document the specific use case requiring direct searchParams access
- Exception process: Obtain approval from security review for exception
- Exception process: Add inline suppression comment with justification and approval reference