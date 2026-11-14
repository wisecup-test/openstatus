---
version: 2
name: de4ff98d-0fb6-412a-ba4f-32b3aac50a81-server-header-and-cookie-management-with-next-headers-components-modules-operating-client-side-not
description: "Server Header and Cookie Management with next/headers: Components Modules Operating Client Side Not"
metadata:
  actual:
    adr_id: de4ff98d-0fb6-412a-ba4f-32b3aac50a81
    status: accepted
    candidate_group_id: eeb8ffd4-578b-4c7f-bfab-122400d7811e
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/lib/**
---

# Server Header and Cookie Management with next/headers: Components Modules Operating Client Side Not

Status: proposed
Date: 2025-05-14
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`
- `src/lib/**`

## Context

- Server-rendered user interfaces, server actions, and HTTP API route handlers require programmatic access to incoming request headers and cookie state for authentication, redirection, and persistent UI preferences.
- Direct manipulation of raw server request objects creates runtime coupling and breaks compatibility across modern serverless and containerized application execution environments.
- The next/headers module provides an asynchronous, server-native abstraction for reading headers and reading or writing cookies within server execution boundaries.
- Centralizing request metadata and cookie mutations through next/headers standardizes session lifecycle handling, security flag enforcement, and UI preference retrieval across the codebase.

## Problem Statement

Server-side execution contexts need consistent, secure access to HTTP headers and cookies without leaking transport abstractions, breaching component render boundaries, or failing to apply mandatory security attributes to authentication tokens.

## Decision

1. MUST_NOT: Components and modules operating on the client side MUST NOT import or invoke functions from the next/headers module.

## Policy Block

- MUST_NOT Components and modules operating on the client side MUST NOT import or invoke functions from the next/headers module.

In scope:
- Server components, server layouts, server actions, API route handlers, and server utility modules requiring HTTP cookie access or request header inspection.

Out of scope:
- Client components and browser-executed modules.
- Non-HTTP background workers and standalone build-time scripts operating outside the web server request lifecycle.

## Rationale

- Evidence across page routes, layouts, server actions, API routes, and utility modules demonstrates uniform adoption of next/headers for both cookie evaluation and secure cookie assignment.
- Enforcing next/headers isolates request header parsing and cookie mutation behind standard framework primitives that are compatible across containerized and serverless environments.
- Standardizing cookie writes through cookieStore guarantees consistent application of security attributes such as httpOnly, secure, and sameSite across authentication and single sign-on flows.

## Consequences

Positive:
- Provides a unified server-side API for inspecting headers and managing cookie state across diverse server contexts.
- Enhances cookie security by establishing standard practices for httpOnly, secure, and sameSite properties on authentication state.
- Decouples application server logic from platform-specific incoming request abstractions.

Negative:
- Couples server-side cookie and header retrieval to the framework lifecycle and runtime execution constraints.
- Restricts cookie write operations to specific server execution phases, preventing cookie mutation within read-only server component renders.

## Alternatives

- Manual parsing and mutation of incoming request and response headers (rejected)
  Rejected because: Bypasses framework server-side request context lifecycle, introduces platform-specific runtime dependencies, and increases risk of header mutation errors.
  When valid: Standalone HTTP servers operating outside the framework server runtime environment.
- Client-side browser cookie management for authentication and session state (rejected)
  Rejected because: Exposes sensitive authentication tokens to client-side scripts, preventing the use of httpOnly protection and increasing susceptibility to cross-site scripting vulnerabilities.
  When valid: Tracking non-sensitive presentation-level preferences on static client pages.

## Risks

- Attempting to invoke cookie mutation methods inside read-only server component renders will trigger runtime errors.
  Mitigation: Restrict cookieStore set invocations to server actions and route handlers, and validate via linting and integration tests.
  Owner: engineering team
- Omitting required security attributes when persisting sensitive authentication cookies can lead to credential exposure.
  Mitigation: Encapsulate cookie persistence in dedicated server utility helpers that enforce mandatory security flags.
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
- When retrieving cookie values inside server layouts or pages, use helper functions to provide fallback defaults when the target cookie is unset.
- Ensure that all cookie write operations specify appropriate path, maxAge, sameSite, httpOnly, and environment-dependent secure attributes.

## Continuation Context


Verify commands:
- Discover the project manifest and execute the designated type-checking and compilation commands to ensure all references to next/headers satisfy compile-time constraints.
- Discover and run the project code quality and linting verification scripts to ensure cookie manipulation occurs only within permissible server execution boundaries.
- Discover and run the automated test suite to verify that server actions and route handlers successfully write and read cookies as specified.

Accept when:
- Type-checking and compilation tasks complete with zero errors related to next/headers module resolution or invocation signatures.
- Code quality checks complete with zero warnings or errors regarding forbidden client-side imports or invalid server-side cookie mutation.
- Automated functional and integration tests covering server cookie management and header extraction pass successfully.

## Enforcement

- Verified by: Static analysis checks configured in the continuous integration pipeline that flag unauthorized imports of next/headers in client components or invalid cookie writes in render phases.
- Verified by: Peer code review for all modifications involving authentication, session cookies, and route handlers.
- Violation handling: Pull requests violating cookie security attribute requirements or attempting cookie writes in read-only render contexts are blocked from merging until corrected.
- Exception process: Exceptions for custom protocol headers or third-party proxy requirements must be reviewed and approved by the engineering architecture team and documented in code review records.