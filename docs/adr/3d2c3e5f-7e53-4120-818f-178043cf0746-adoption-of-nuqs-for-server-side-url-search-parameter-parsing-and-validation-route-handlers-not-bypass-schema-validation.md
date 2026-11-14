---
version: 2
name: 3d2c3e5f-7e53-4120-818f-178043cf0746-adoption-of-nuqs-for-server-side-url-search-parameter-parsing-and-validation-route-handlers-not-bypass-schema-validation
description: "Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Handlers Not Bypass Schema Validation"
metadata:
  actual:
    adr_id: 3d2c3e5f-7e53-4120-818f-178043cf0746
    status: accepted
    candidate_group_id: b6caa4e1-a734-4f95-91e7-b9071cb6b4c1
    evidence:
      source: deep
  scope:
    root: apps/status-page
    paths:
      - src/app/**
---

# Adoption of nuqs for Server-Side URL Search Parameter Parsing and Validation: Route Handlers Not Bypass Schema Validation

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/status-page`

Applies to:
- `src/app/**`

## Context

- Application route components accept dynamic search parameters from incoming HTTP requests representing user navigation and query state.
- Unvalidated search parameter parsing introduces security risks and runtime type coercion errors across server components.
- Server route components require structured and cached parameter validation before hydrating client components or querying backend services.

## Problem Statement

Route components that read URL search parameters directly from incoming requests risk runtime type mismatches and security vulnerabilities if query inputs are not validated against a strict schema, while duplicating query parsing logic between server and client components leads to inconsistent state synchronization.

## Decision

1. MUST_NOT: Route handlers MUST NOT bypass schema validation by casting raw search query records to expected domain types without executing parser validation.

## Policy Block

- MUST_NOT Route handlers MUST NOT bypass schema validation by casting raw search query records to expected domain types without executing parser validation.

In scope:
- Server-rendered route page components that receive and process URL search parameters.
- Shared search parameter definitions consumed by both server page handlers and client components.

Out of scope:
- Internal API endpoints that do not bind state to browser URL search queries.
- Static routes that operate without dynamic search query parameters.

## Rationale

- Adopting a dedicated search parameter cache parser establishes a single source of truth for query schemas and prevents unvalidated input from reaching downstream components.
- The nuqs/server parsing pattern isolates input validation at the route boundary while enabling seamless type-safe sharing with client-side state hooks.
- Automated parsing mitigates query string injection and malformed input edge cases across localized and domain-routed views.

## Consequences

Positive:
- Guarantees type-safe input validation and sanitization for all URL search parameters prior to page rendering.
- Eliminates duplicate parsing logic between server components and client components via shared parameter cache definitions.
- Reduces runtime errors stemming from unexpected null, undefined, or malformed query string values.

Negative:
- Introduces runtime dependency coupling on a specific URL state management library across route handlers.
- Requires developers to maintain synchronized schema declarations between server-side parsers and client state hooks.

## Alternatives

- Manual raw query string parsing via standard URL search parameters APIs (rejected)
  Rejected because: Lacks centralized schema validation and requires repetitive type coercion and defensive checks in every route handler.
  When valid: Simple single-page utilities with strictly primitive string flags and no shared state requirements.
- Custom generic schema validation libraries without framework-integrated URL state bindings (rejected)
  Rejected because: Fails to provide bidirectional synchronization between server-side caching and client-side URL state transitions.
  When valid: Non-web API endpoints that do not synchronize state with browser address bar navigation.

## Risks

- Schema evolution breaking existing bookmarks or shared links containing legacy query formats.
  Mitigation: Define fallback values and graceful degradation within the parameter schema configurations to absorb unexpected query string values safely.
  Owner: engineering team
- Upstream library API shifts affecting server-side cache parsing conventions.
  Mitigation: Encapsulate schema definitions in dedicated parameter modules so underlying library APIs remain decoupled from core domain rendering logic.
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
- Colocate search parameter schema parser cache definitions alongside route handlers or in dedicated parameter modules to facilitate reuse between server and client boundaries.
- Pass parsed parameter cache instances to client components rather than forwarding unvalidated raw query parameter objects.

## Continuation Context


Verify commands:
- Discover and execute the repository static type checker to confirm that all search parameter parsing calls adhere to defined schema types.
- Discover and run the project test suite to verify route parameter parsing and validation error handling.

Accept when:
- Static type checking and automated test suites confirm all route search parameters pass through validated cache parser instances.
- No server route handlers access raw search query parameters directly without parsing.

## Enforcement

- Verified by: Automated static analysis and linting rules prohibiting direct unparsed query parameter consumption in route components.
- Verified by: Peer code review verification against centralized search parameter schema definitions.
- Violation handling: Build failure during continuous integration when unparsed query parameter accesses are detected.
- Violation handling: Pull request rejection for route implementations that bypass schema cache parsers.
- Exception process: Exceptions require an architectural review by the engineering team documenting the specific technical constraint precluding standard schema parsing.
- Exception process: Temporary waivers must provide an alternate validated boundary and specify an expiration date for refactoring.