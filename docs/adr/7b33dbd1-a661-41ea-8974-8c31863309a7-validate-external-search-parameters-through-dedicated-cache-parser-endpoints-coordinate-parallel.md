# Validate External Search Parameters Through Dedicated Cache Parser: Endpoints Coordinate Parallel

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- Public API endpoints accept search parameters from external clients as Promise-wrapped objects that require asynchronous resolution before use
- The framework provides search parameters through a Promise interface, requiring explicit parsing before parameter values can be accessed or validated
- Multiple public page endpoints coordinate prefetching operations that depend on validated search parameter values, creating a dependency chain between validation and data loading
- The codebase uses a dedicated search parameter cache parser to centralize validation logic across multiple public API entry points

## Problem Statement

Public API endpoints that accept external search parameters must validate and parse untrusted input before using it to drive data fetching, routing, or business logic. Without centralized validation through a dedicated parser, each endpoint would implement ad-hoc validation, creating inconsistent security boundaries and increasing the risk of injection attacks or malformed data propagating into downstream operations.

## Decision

1. SHOULD: Endpoints SHOULD coordinate parallel prefetching operations only after search parameter validation completes to prevent invalid parameters from triggering unnecessary data operations

## Policy Block

- SHOULD Endpoints SHOULD coordinate parallel prefetching operations only after search parameter validation completes to prevent invalid parameters from triggering unnecessary data operations

In scope:
- All public API page endpoints that accept search parameters from external clients
- All asynchronous page functions that receive searchParams as Promise-wrapped props
- All data prefetching operations that depend on search parameter values
- All routing decisions conditional on search parameter presence or validity

Out of scope:
- Internal API endpoints that receive pre-validated parameters from trusted sources
- Server-side functions that generate search parameters programmatically
- Client-side parameter validation for user experience enhancement
- Static page routes that do not accept dynamic search parameters

## Rationale

- The evidence shows two public page endpoints both using searchParamsCache.parse() to validate external search parameters before prefetching data, demonstrating a consistent validation pattern across the public API surface
- Both endpoints coordinate multiple parallel prefetching operations that depend on validated parameter values, indicating that validation must complete before data operations begin to prevent invalid input from propagating
- The pattern centralizes validation logic in a dedicated cache parser rather than implementing ad-hoc validation in each endpoint, reducing code duplication and ensuring consistent security boundaries
- The framework's Promise-wrapped search parameter interface requires explicit asynchronous parsing, making validation a mandatory step in the request lifecycle rather than an optional enhancement

## Consequences

Positive:
- Centralized validation logic ensures consistent security boundaries across all public API endpoints that accept external search parameters
- Invalid or malicious search parameters are rejected before triggering expensive data prefetching operations, reducing resource waste and attack surface
- The dedicated cache parser provides a single point of maintenance for validation rules, simplifying updates and security patches
- Explicit validation before data operations creates clear separation between untrusted input handling and trusted business logic

Negative:
- Every public endpoint must implement the validation step, adding boilerplate code to each page function
- Asynchronous validation adds latency to the request lifecycle, delaying data prefetching until parameter parsing completes
- Developers must remember to validate search parameters in new endpoints, creating a potential security gap if validation is forgotten
- The validation step creates a dependency on the search parameter cache parser library, coupling endpoint implementation to its API

## Alternatives

- Implement ad-hoc validation in each endpoint without a centralized parser (rejected)
  Rejected because: Ad-hoc validation creates inconsistent security boundaries across endpoints, increases code duplication, and makes it difficult to audit or update validation rules systematically
  When valid: Only appropriate for prototypes or single-endpoint applications where centralized validation overhead exceeds its benefit
- Use framework middleware to validate search parameters before they reach endpoint handlers (rejected)
  Rejected because: The evidence shows validation occurring within endpoint functions after Promise resolution, not in middleware, suggesting the framework's Promise-wrapped parameter interface makes middleware validation impractical
  When valid: Valid if the framework provides middleware hooks that can intercept and validate Promise-wrapped search parameters before endpoint execution
- Defer validation until parameters are used in business logic rather than validating at endpoint entry (rejected)
  Rejected because: Deferred validation allows untrusted input to propagate into data prefetching operations, increasing attack surface and making it difficult to trace which operations depend on validated versus unvalidated data
  When valid: Only appropriate for internal APIs where all callers are trusted and validation serves user experience rather than security

## Risks

- Developers may forget to validate search parameters in new public endpoints, creating security vulnerabilities where untrusted input flows directly into data operations
  Mitigation: Implement automated verification that scans public endpoint functions for search parameter validation calls and fails builds when validation is missing
  Owner: engineering team
- The search parameter cache parser may have bugs or incomplete validation rules that allow malicious input to pass through, compromising all endpoints that depend on it
  Mitigation: Maintain comprehensive test coverage for the cache parser including fuzzing and injection attack scenarios, and conduct regular security audits of validation logic
  Owner: security team
- Asynchronous validation adds latency that may degrade user experience for legitimate requests, especially when validation logic becomes complex
  Mitigation: Profile validation performance and optimize hot paths, consider caching validation results for repeated parameter patterns, and monitor endpoint latency metrics
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
- Locate the search parameter cache parser module in the project and examine its validation schema to understand which parameter types, formats, and constraints are enforced
- When adding new public endpoints that accept search parameters, structure the endpoint function to await search parameter Promise resolution and immediately pass the result to the cache parser before any other operations
- For endpoints that conditionally redirect based on parameter presence, perform validation first and then check the parsed result for required fields rather than checking the raw parameter object

## Continuation Context


Verify commands:
- Discover the project's static analysis or linting configuration and execute the verification script that checks public endpoint functions for search parameter validation calls
- Locate the test suite for the search parameter cache parser and execute tests to confirm validation rules are enforced
- Identify the project's endpoint scanning tool and run it to generate a report of all public page functions, then verify each one includes validation before data operations

Accept when:
- All public API page endpoints that accept search parameters include a call to the cache parser before any data prefetching or routing logic
- Static analysis or automated scanning confirms no public endpoint uses search parameter values without prior validation
- Test coverage for the search parameter cache parser includes validation of all parameter types accepted by public endpoints

## Enforcement

- Verified by: Automated static analysis scans public endpoint functions for search parameter validation patterns during continuous integration
- Verified by: Code review checklist requires reviewers to verify search parameter validation is present in any new or modified public endpoint
- Verified by: Security audit process includes manual inspection of search parameter validation logic and testing with malicious input patterns
- Violation handling: Build fails if static analysis detects public endpoints that accept search parameters without validation
- Violation handling: Pull requests that add or modify public endpoints without validation are blocked until validation is added
- Violation handling: Security team is notified of any validation bypass discovered in production and incident response process is initiated
- Exception process: Exceptions require written justification explaining why validation is not needed for a specific endpoint
- Exception process: Security team must review and approve all exception requests before they are granted
- Exception process: Approved exceptions are documented in code comments with ticket references and are reviewed quarterly for continued validity