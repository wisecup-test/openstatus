---
version: 2
name: f8fe9a1d-3b25-4be5-b2bb-18cfd1bb3728-adoption-of-next-headers-for-server-side-request-context-management-modules-executing-client-side-universal-execution
description: "Adoption of next/headers for Server-Side Request Context Management: Modules Executing Client Side Universal Execution"
metadata:
  actual:
    adr_id: f8fe9a1d-3b25-4be5-b2bb-18cfd1bb3728
    status: accepted
    candidate_group_id: 1c61ef2b-7759-42f4-82a3-4ce67ddac0d2
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/lib/**
      - src/trpc/**
---

# Adoption of next/headers for Server-Side Request Context Management: Modules Executing Client Side Universal Execution

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/lib/**`
- `src/trpc/**`

## Context

- Server-side utilities and internal API callers require access to incoming HTTP request metadata, including client preference cookies and proxy headers.
- Threading HTTP request objects manually through business logic and utility functions creates unnecessary coupling and verbose function signatures.
- The application requires a consistent, framework-supported mechanism to read and forward request-bound state exclusively within server contexts.

## Problem Statement

Server-side functions requiring client preferences, regional timezone headers, or downstream authentication context must obtain request-bound metadata without parameter drilling or leaking transport details across module boundaries.

## Decision

1. MUST_NOT: Modules executing in client-side or universal execution environments MUST NOT import or invoke functions from next/headers.

## Policy Block

- MUST_NOT Modules executing in client-side or universal execution environments MUST NOT import or invoke functions from next/headers.

In scope:
- Server-side utility modules accessing incoming HTTP request headers or cookies
- Server-side API client callers propagating request context to internal services

Out of scope:
- Client-side rendering components and hooks
- Isomorphic validation schemas and utility modules executing across shared environments

## Rationale

- The next/headers module provides asynchronous and synchronous accessors to incoming request headers and cookies directly within server execution lifecycles.
- Centralizing request context access through next/headers ensures clean module contracts and prevents accidental exposure of request-bound state to client-side code.
- Evidence across server utilities demonstrates consistent use of next/headers for reading preference cookies, extracting regional network headers, and preparing downstream API caller headers.

## Consequences

Positive:
- Eliminates parameter drilling of HTTP request objects through application service layers and utility functions.
- Standardizes access to incoming cookies, geolocation headers, and client preferences across server-side integrations.
- Enables centralized sanitization and forwarding of request headers to downstream API callers.

Negative:
- Restricts utilities consuming request headers and cookies strictly to server-side execution contexts, preventing isomorphic usage.
- Accessing dynamic request metadata at runtime opts calling routes into dynamic rendering behavior unless explicitly handled.

## Alternatives

- Explicit Request Object Threading (rejected)
  Rejected because: Passing request instances through every functional layer creates tight coupling between domain utilities and transport protocols while complicating function signatures.
  When valid: When executing within custom non-framework runtimes lacking asynchronous request-scoped storage capabilities.
- Custom Asynchronous Local Storage Context Wrappers (rejected)
  Rejected because: Maintaining proprietary request context wrappers duplicates framework-native context isolation mechanisms and introduces redundant state management overhead.
  When valid: When running on standalone backend servers independent of framework-managed request lifecycles.

## Risks

- Accessing next/headers outside of an active server request lifecycle results in runtime execution errors.
  Mitigation: Guard header and cookie reads within fallback handling or ensure caller execution is strictly bounded to the active request lifecycle.
  Owner: Server Architecture Team
- Unintended invocation of dynamic header accessors can force static server routes into dynamic on-demand rendering.
  Mitigation: Review server route configuration and isolate dynamic header reads to data-fetching functions where dynamic rendering is intended.
  Owner: Server Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Encapsulate repeated header and cookie retrieval patterns in dedicated server utility functions to ensure consistent error handling and schema validation.
- When constructing downstream service client requests, clone the incoming header list and explicitly delete hop-by-hop headers to prevent protocol conflicts.

## Continuation Context


Verify commands:
- Discover and execute the repository static analysis and linting scripts to verify that next/headers imports are restricted to server-side modules.
- Discover and run the project test suite to confirm that server request context utilities correctly retrieve and sanitize headers and cookies under mock request environments.

Accept when:
- Static analysis confirms all imports of next/headers reside within designated server execution boundaries with zero client-side imports.
- Automated test executions verify that header forwarding routines strip hop-by-hop transport headers and cookie parsing validates input against schema definitions.

## Enforcement

- Verified by: Automated static analysis checks enforcing server-only import boundaries on next/headers.
- Verified by: Pull request peer reviews verifying header sanitization and validation on all request-bound data extraction.
- Violation handling: Static analysis failures for prohibited client-side imports of next/headers block continuous integration builds.
- Violation handling: Pull requests introducing unvalidated cookie parsing or raw request object parameter drilling must be refactored prior to merge.
- Exception process: Submit an architectural exception request detailing runtime constraints that prevent the use of standard server request context APIs.
- Exception process: Obtain approval from the server architecture team and document the compensating request-threading pattern in module documentation.