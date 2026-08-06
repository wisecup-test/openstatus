# Adopt Async Server Component Pattern with Parallel Data Prefetching: Server Components Redirect

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Server-side page components in the dashboard application handle search parameters as asynchronous promises rather than synchronous objects, requiring explicit await resolution before use
- Multiple data dependencies are prefetched in parallel using Promise.all coordination to minimize sequential waterfall delays and improve time-to-interactive metrics
- Search parameter validation occurs server-side through a dedicated cache parser before any client-side hydration, establishing an input validation boundary at the server entry point
- The pattern separates server-side data prefetching from client-side rendering through explicit hydration boundaries, isolating async coordination concerns from interactive UI logic

## Problem Statement

Server components that accept user-controlled search parameters must validate and parse those inputs before use, while also coordinating multiple async data fetches efficiently. Without explicit async handling and parallel prefetching, components risk unvalidated input propagation and sequential data loading waterfalls that degrade performance and security posture.

## Decision

1. SHOULD: Server components SHOULD redirect or return error states when required search parameters fail validation rather than proceeding with invalid state

## Policy Block

- SHOULD Server components SHOULD redirect or return error states when required search parameters fail validation rather than proceeding with invalid state

In scope:
- Server-side page components that accept searchParams props
- Route handlers that process user-supplied query parameters
- Server components that coordinate multiple data dependencies before rendering

Out of scope:
- Client-side components that receive validated parameters as props
- API route handlers with different input validation requirements
- Static page components with no dynamic parameters

## Rationale

- The evidence shows consistent use of async function signatures with Promise<SearchParams> types and searchParamsCache.parse() calls, indicating a deliberate pattern for server-side input validation at the entry point
- Both detected files use Promise.all to coordinate multiple prefetchQuery operations, demonstrating a conscious optimization to avoid sequential data loading waterfalls
- The pattern separates concerns by validating inputs server-side before any client hydration occurs, establishing a security boundary that prevents unvalidated user input from reaching client code or backend queries
- With 91.20% confidence across 2 files in the dashboard application, this pattern represents an established architectural approach to async server component design rather than isolated implementation

## Consequences

Positive:
- Input validation occurs at the earliest possible point in the request lifecycle, before any business logic or data fetching executes
- Parallel data prefetching reduces time-to-interactive by eliminating sequential waterfall delays when multiple queries are required
- Explicit async/await syntax makes the asynchronous coordination model visible and auditable in component signatures
- Server-side validation prevents malformed or malicious search parameters from propagating to client-side code or backend systems

Negative:
- Async component signatures increase complexity compared to synchronous parameter access patterns
- Parallel prefetching may load data that is ultimately unused if conditional rendering logic filters results client-side
- Error handling for validation failures requires explicit redirect or error state logic rather than falling back to default values
- The pattern requires coordination between search parameter schemas, validation logic, and query prefetching code across multiple modules

## Alternatives

- Synchronous search parameter access with client-side validation (rejected)
  Rejected because: Client-side validation allows unvalidated parameters to reach the server during initial render, creating a window for injection attacks and requiring duplicate validation logic
  When valid: Only appropriate for purely client-side routing scenarios with no server-side data dependencies
- Sequential data fetching with await chaining (rejected)
  Rejected because: Sequential fetching creates waterfall delays where each query waits for the previous to complete, significantly degrading time-to-interactive metrics
  When valid: Only when data dependencies form a true chain where each query requires results from the previous
- Lazy client-side data loading after hydration (deferred)
  When valid: May be appropriate for below-the-fold content or user-triggered actions where server-side prefetching would waste resources

## Risks

- Developers may forget to await searchParams resolution, causing runtime errors or accessing undefined values
  Mitigation: Enforce type checking that requires Promise resolution and add linting rules to detect unawaited Promise access
  Owner: engineering team
- Parallel prefetching may mask individual query failures if Promise.all error handling is not comprehensive
  Mitigation: Implement per-query error boundaries and logging to surface individual fetch failures while allowing partial success scenarios
  Owner: engineering team
- Search parameter validation schemas may drift from actual usage patterns, causing false rejections or missed validations
  Mitigation: Co-locate validation schemas with component definitions and include validation coverage in integration tests
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
- Define search parameter validation schemas adjacent to the components that consume them to maintain locality of behavior and reduce schema drift
- When adding new prefetch operations to Promise.all arrays, verify that the queries are truly independent and do not have hidden dependencies that would benefit from sequential execution
- Consider implementing a typed wrapper around the search parameter parser that generates TypeScript types from validation schemas to ensure compile-time safety

## Continuation Context


Verify commands:
- Discover the project's type checking configuration and execute the type checker to verify all searchParams props are declared with Promise types
- Locate the project's test suite and run integration tests that verify search parameter validation rejects invalid inputs before data fetching occurs
- Identify the project's linting configuration and execute linters to detect unawaited Promise access in server component files

Accept when:
- Type checking passes with no errors related to searchParams Promise resolution or unawaited async operations
- Integration tests demonstrate that invalid search parameters trigger validation failures before any query prefetching executes
- Code review confirms all server page components with searchParams props use Promise.all for independent data fetches

## Enforcement

- Verified by: Automated type checking in continuous integration pipeline
- Verified by: Integration test suite covering search parameter validation scenarios
- Verified by: Code review checklist requiring verification of async patterns in server components
- Violation handling: Type checking failures block merge to protected branches
- Violation handling: Failed integration tests prevent deployment progression
- Violation handling: Code review findings require resolution before approval
- Exception process: Document technical justification for synchronous parameter access or sequential fetching in component comments
- Exception process: Obtain approval from security team for any search parameter usage that bypasses validation
- Exception process: Record exception in architecture decision log with expiration date for review