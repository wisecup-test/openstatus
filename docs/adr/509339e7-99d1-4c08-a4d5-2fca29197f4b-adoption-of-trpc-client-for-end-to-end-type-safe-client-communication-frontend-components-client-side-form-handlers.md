---
version: 2
name: 509339e7-99d1-4c08-a4d5-2fca29197f4b-adoption-of-trpc-client-for-end-to-end-type-safe-client-communication-frontend-components-client-side-form-handlers
description: "Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Frontend Components Client Side Form Handlers"
metadata:
  actual:
    adr_id: 509339e7-99d1-4c08-a4d5-2fca29197f4b
    status: accepted
    candidate_group_id: ca020432-fbe7-4ec5-b2ca-43bb7b58d3ac
    evidence:
      source: deep
  scope:
    root: apps/dashboard
    paths:
      - src/app/**
      - src/components/**
      - src/lib/trpc/**
---

# Adoption of @trpc/client for End-to-End Type-Safe Client Communication: Frontend Components Client Side Form Handlers

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/dashboard`

Applies to:
- `src/app/**`
- `src/components/**`
- `src/lib/trpc/**`

## Context

- Client UI components, interactive forms, and data tables require consistent, type-safe communication channels to query and mutate application state against backend services.
- Ad hoc HTTP request implementations lack automated contract synchronization, creating divergence risks between backend API schemas and frontend consumer expectations.
- Application client and server execution contexts require structured client instantiation supporting header propagation, cookie forwarding, and serialization handlers.

## Problem Statement

Manual network request orchestration and untyped client calls introduce runtime contract drift, redundant serialization code, and elevated maintenance overhead across frontend forms, interactive tables, and server-rendered data wrappers.

## Decision

1. MUST_NOT: Frontend components and client-side form handlers MUST NOT issue raw untyped network requests for operations that have corresponding remote procedure definitions in the API layer.

## Policy Block

- MUST_NOT Frontend components and client-side form handlers MUST NOT issue raw untyped network requests for operations that have corresponding remote procedure definitions in the API layer.

In scope:
- Client-side components, forms, and interactive views invoking remote procedures.
- Server-side client proxy utilities constructing client instances for remote procedure calls.

Out of scope:
- Independent external third-party webhook receivers or public endpoints operating outside the internal remote procedure contract.

## Rationale

- Adopting @trpc/client establishes an end-to-end typed contract linking frontend invocations directly with backend route declarations, preventing runtime payload mismatches.
- Standardizing client instantiation through @trpc/client centralizes serialization, authentication header propagation, and telemetry hooks across client and server runtimes.
- Integrating @trpc/client with reactive state hooks streamlines caching, background refetching, and mutation lifecycle management in complex form and table interfaces.

## Consequences

Positive:
- Compile-time type verification catches API signature changes and invalid payload structures before deployment.
- Centralized request pipeline ensures consistent telemetry logging and error reporting across all remote procedure invocations.
- Reduced boilerplate by eliminating manual serialization transformations and handwritten endpoint contracts.

Negative:
- Tight coupling between client integration layers and the procedure routing architecture limits protocol flexibility.
- Requires lockstep type synchronization between client consumers and backend definition packages within the monorepo workspace.

## Alternatives

- Direct fetch calls with manually declared TypeScript interface definitions (rejected)
  Rejected because: Manually declared interfaces introduce maintenance overhead and drift out of synchronization with server contracts when schemas evolve.
  When valid: When integrating with external third-party REST services that do not publish TypeScript router contracts.
- Code-generated REST or GraphQL client SDKs (rejected)
  Rejected because: Code generation introduces secondary build artifacts and intermediate synchronization steps that are redundant when full-stack TypeScript schemas are available.
  When valid: When consuming APIs shared across heterogeneous non-TypeScript client applications.

## Risks

- Client-side bundling of server-specific dependencies when instantiating client proxies
  Mitigation: Strictly isolate server client utilities using server-only environment boundary markers and lint rules.
  Owner: engineering team
- Unhandled RPC exceptions leaking internal server details or breaking client UI state
  Mitigation: Enforce centralized error handling and logging hooks in shared client configuration links.
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
- Configure shared client links to attach standard telemetry logging, request headers, and custom serialization transformers across all client instances.
- Ensure server-executed client instances forward incoming session cookies and authentication headers while enforcing server execution boundaries.

## Continuation Context


Verify commands:
- Discover the workspace verification script in the repository manifest and execute type checking across all client and API packages.
- Locate the repository testing script from the configuration files and execute the test suite to validate client query and mutation execution.
- Inspect the workspace linting scripts and execute static analysis to verify that no direct untyped network client calls bypass the remote procedure layer.

Accept when:
- Static type analysis completes with zero errors across all client remote procedure invocation points.
- All automated integration and component tests exercising client queries and mutations execute successfully.
- Code review confirms all new client data fetching and mutation operations route through the standardized remote procedure client.

## Enforcement

- Verified by: Continuous integration pipeline running automated type checking and test suites on pull requests.
- Verified by: Peer code review verifying compliance with client architecture and absence of untyped network calls.
- Violation handling: Pull requests containing untyped remote calls or unverified client patterns fail continuous integration checks and cannot be merged.
- Violation handling: Flagged architectural violations must be refactored to use the standardized client before review approval.
- Exception process: Exceptions for direct HTTP requests must submit an architecture review request detailing why the endpoint cannot be modeled as a remote procedure.
- Exception process: Approved exceptions must be documented with rationale and an explicit exemption comment referencing the review decision.