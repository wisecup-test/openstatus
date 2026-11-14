---
version: 2
name: 4360074c-b5ca-4108-a0f5-8df3d28672d9-adoption-of-nuqs-for-type-safe-server-side-search-parameter-parsing-before-implementing-updating-query-parameter-parsing
description: "Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Before Implementing Updating Query Parameter Parsing"
metadata:
  actual:
    adr_id: 4360074c-b5ca-4108-a0f5-8df3d28672d9
    status: accepted
    candidate_group_id: d25c74b8-2b62-4f95-8d16-03b44b81995c
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# Adoption of nuqs for Type-Safe Server-Side Search Parameter Parsing: Before Implementing Updating Query Parameter Parsing

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`

## Context

- Route handlers and page entry points in server-rendered web applications receive query parameters as unvalidated string dictionaries from client requests.
- Application pages require type-safe access to query parameters across both server-side data fetching and client-side user interface state synchronization.
- Adopting nuqs with server-side search parameter caching provides deterministic schema validation and prevents runtime type errors caused by malformed query inputs.

## Problem Statement

Server-rendered application routes frequently receive query parameters that dictate pagination, filtering, and view configurations. Reading raw search parameter dictionaries without strict schema validation exposes server components and downstream queries to malformed, unexpected, or malicious input types, while duplicating state parsing logic across server and client boundaries.

## Decision

1. MUST: Before implementing or updating query parameter parsing logic, the consumer MUST inspect the project dependency resolution artifact to determine the exact locked version of the query state library and adhere to its published API specification.

## Policy Block

- MUST Before implementing or updating query parameter parsing logic, the consumer MUST inspect the project dependency resolution artifact to determine the exact locked version of the query state library and adhere to its published API specification.

In scope:
- Server-rendered application page routes that consume URL query parameters for filtering, sorting, pagination, or state initialization.

Out of scope:
- API endpoints that rely exclusively on request body validation.
- Static pages that do not process URL query parameters.

## Rationale

- Enforces deterministic runtime schema validation at the page boundary using searchParamsCache.parse, preventing unvalidated parameters from reaching data services.
- Ensures unified parameter definition schemas across server rendering and client view components, eliminating synchronization drift between server state and URL state.
- Observed adoption across all inspected server page routes demonstrates a standardized architectural approach to URL state parsing within the application.

## Consequences

Positive:
- Guarantees strong type safety for query parameters during server-side page execution.
- Provides seamless interoperability between server-side parameter parsing and client-side URL state updates.
- Eliminates repetitive manual type casting and parameter sanitization code in page entry points.

Negative:
- Couples server page entry points to the schema structure defined in parameter configuration modules.
- Adds an external library dependency for query parameter management across application routes.

## Alternatives

- Manual search parameter parsing using native web URLSearchParams interfaces (rejected)
  Rejected because: Native URLSearchParams requires verbose manual type casting, lacks built-in schema validation, and causes drift between client and server query parsing logic.
  When valid: Valid only in lightweight isolated utility scripts with minimal query parsing requirements.
- Generic schema validation libraries without specialized client query state synchronization (rejected)
  Rejected because: Generic schema validators do not coordinate URL query state synchronization between server rendering and interactive client navigation hooks.
  When valid: Valid for request payload validation or external API response verification.

## Risks

- Breaking changes in query parameter schema definitions causing mismatches with active URLs or bookmarks
  Mitigation: Define default fallback values within parameter schemas to gracefully handle missing or outdated query keys.
  Owner: Application Engineering Team
- Inconsistent import paths between server execution contexts and client components
  Mitigation: Enforce automated static analysis checks to verify that server subpath imports are used in server page modules.
  Owner: Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Declare search parameter parsers using the library schema definitions in dedicated configuration modules co-located with the respective route.
- Execute searchParamsCache.parse at the root of the server page component before passing state to downstream data fetchers or client rendering components.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and linting scripts to verify adherence to search parameter import and parsing rules.
- Discover and execute the test runner to validate route page query parameter parsing and fallback behavior.
- Discover and trigger the project type checking suite to ensure all search parameter types conform to declared schemas.

Accept when:
- All server-side page routes parsing URL query parameters execute searchParamsCache.parse without type errors.
- Automated static analysis and type checking scripts complete with zero violations across all route modules.
- Unit and integration test suites validating search parameter parsing pass successfully.

## Enforcement

- Verified by: Automated continuous integration pipeline running static analysis and type checking on every pull request.
- Verified by: Peer code review verifying that page routes validate query parameters via searchParamsCache.parse.
- Violation handling: Pull requests containing unvalidated direct access to raw search parameter dictionaries will be blocked by continuous integration.
- Violation handling: Code review requires refactoring to use searchParamsCache.parse before approval.
- Exception process: Exceptions require documented justification submitted to the architecture review board explaining why specialized parameter parsing is impractical.
- Exception process: Approved exceptions must document compensatory input sanitization and validation logic.