---
version: 2
name: 45ba6bfa-544b-4350-aced-be48b490de5d-lib-trpc-server-module-adoption-for-server-side-data-access-layout-page-components-receiving-url-search
description: "@/lib/trpc/server Module Adoption for Server-Side Data Access: Layout Page Components Receiving Url Search"
metadata:
  actual:
    adr_id: 45ba6bfa-544b-4350-aced-be48b490de5d
    status: accepted
    candidate_group_id: c87f948b-cbb0-46f6-8ca7-261dcab1560b
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
---

# @/lib/trpc/server Module Adoption for Server-Side Data Access: Layout Page Components Receiving Url Search

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`

## Context

- The dashboard application relies on server-rendered route layouts and pages that require secure, type-safe data prefetching and procedure execution during initial rendering.
- Direct database access in disparate route files creates fragmented connection pooling and bypasses centralized business logic and authorization middleware.
- The internal module @/lib/trpc/server establishes a centralized server-side caller pattern, enabling layout and page components to invoke procedures within the server runtime without network loopback overhead.

## Problem Statement

Server-rendered application routes require access to backend services and domain logic without incurring network latency from HTTP self-calls or creating unmanaged direct database connections across route layouts and pages. The absence of a standardized server-side invocation mechanism leads to duplicate authorization routines, inconsistent error propagation, and fragmented data fetching lifecycles.

## Decision

1. SHOULD: Layout and page components receiving URL search parameters SHOULD coordinate input validation prior to downstream procedure dispatch via @/lib/trpc/server.

## Policy Block

- SHOULD Layout and page components receiving URL search parameters SHOULD coordinate input validation prior to downstream procedure dispatch via @/lib/trpc/server.

In scope:
- Server-rendered layout and page components requiring internal server-side data fetching or procedure execution.
- Application route hierarchy components handling server context and initial data resolution.

Out of scope:
- Client-rendered interactive components executing queries and mutations directly in the user browser.
- Standalone utility modules that perform purely deterministic, non-data transformations.

Exceptions:
- EXC-20-001: A route component requires integration with an external third-party webhook or external legacy endpoint that cannot be routed through internal procedure callers.

## Rationale

- Importing @/lib/trpc/server across route layouts and pages unifies data access patterns, ensuring consistent authorization checks and context propagation across all server-rendered routes.
- Direct internal procedure invocation avoids the serialization and HTTP latency overhead associated with internal network requests while maintaining end-to-end type safety.
- Static analysis across 25 route layout and page files confirms a uniform dependency on @/lib/trpc/server for orchestrating server data dependencies.

## Consequences

Positive:
- Enforces a consistent, type-safe boundary for server-side procedure execution across all application layouts and pages.
- Eliminates network latency overhead by invoking procedures in-process within the server execution context.
- Centralizes request context and authentication propagation into a unified server caller interface.

Negative:
- Couples server-rendered route components directly to the internal @/lib/trpc/server procedure contract and runtime dependencies.
- Requires strict build and import boundaries to prevent leaking server-side caller code into client browser bundles.

## Alternatives

- Direct database or ORM queries within individual page and layout components (rejected)
  Rejected because: Direct database queries bypass procedure authorization middleware, duplicate business logic, and fragment connection management across route components.
  When valid: Valid only in isolated low-level migration scripts or standalone administrative tools outside the application route hierarchy.
- Self-calling internal HTTP API endpoints via network fetch from server components (rejected)
  Rejected because: Internal HTTP calls incur unnecessary serialization overhead, connection latency, and complex base URL routing configurations within deployment environments.
  When valid: Valid when interacting with independently deployed external services across distinct network boundaries.

## Risks

- Accidental import of @/lib/trpc/server into client-side component trees causing runtime bundle failures or leaking server secrets.
  Mitigation: Configure bundler server-only boundary enforcement and automated linter rules to disallow importing server callers in client components.
  Owner: engineering team
- Underlying framework version divergence between server caller utilities and client-side procedure bindings.
  Mitigation: Strict lock-file synchronization and workspace dependency validation during continuous integration builds.
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
- Ensure all invocations of @/lib/trpc/server occur within asynchronous server components or server-side data handlers.
- Propagate user authentication and header contexts into @/lib/trpc/server caller factories during request processing.

## Continuation Context


Verify commands:
- Discover the repository package manifest and execute the workspace type verification script to ensure proper typing across all server callers.
- Discover the workspace linting configuration and run static analysis to verify that server caller imports are isolated from client component modules.
- Discover the repository test runner and execute the automated test suite covering server route rendering and procedure invocation.

Accept when:
- All server-side page and layout data fetches resolve successfully through @/lib/trpc/server without static type errors.
- No client-side components import or execute callers from @/lib/trpc/server.
- Continuous integration checks pass with zero static analysis or compilation violations.

## Enforcement

- Verified by: Continuous integration static type checking and linting pipelines verifying module boundaries and server caller imports.
- Verified by: Peer code review for all new route layouts, pages, and server data fetching logic.
- Violation handling: Pull requests importing @/lib/trpc/server in client contexts or executing unmanaged direct database queries in route files will be blocked by automated checks and review gates.
- Exception process: Architectural exceptions must be submitted via an issue detailing the technical constraints preventing @/lib/trpc/server adoption, requiring formal sign-off from the technical lead.