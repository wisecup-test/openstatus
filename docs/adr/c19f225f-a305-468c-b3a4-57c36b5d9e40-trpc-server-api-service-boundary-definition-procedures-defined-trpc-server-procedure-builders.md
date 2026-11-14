---
version: 2
name: c19f225f-a305-468c-b3a4-57c36b5d9e40-trpc-server-api-service-boundary-definition-procedures-defined-trpc-server-procedure-builders
description: "@trpc/server API Service Boundary Definition: Procedures Defined Trpc Server Procedure Builders"
metadata:
  actual:
    adr_id: c19f225f-a305-468c-b3a4-57c36b5d9e40
    status: accepted
    candidate_group_id: 1c9c1530-10ff-44aa-8e8f-5c16d37ddbc1
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server API Service Boundary Definition: Procedures Defined Trpc Server Procedure Builders

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/**`

## Context

- The API package coordinates request handling, context enrichment, and domain service dispatch across multiple feature routers.
- External inputs require strict schema validation, consistent error masking, and graceful degradation during downstream datastore interruptions.
- Cross-cutting concerns including timing metrics, tenant identification, and authentication state must be applied uniformly without polluting router implementations.

## Problem Statement

Exposing API endpoints across micro-packages without unified contract definitions leads to contract drift, inconsistent input validation, and fragmented enforcement of authentication and request telemetry.

## Decision

1. MUST: API procedures MUST be defined using @trpc/server procedure builders to establish formal service and boundary contracts.

## Policy Block

- MUST API procedures MUST be defined using @trpc/server procedure builders to establish formal service and boundary contracts.

In scope:
- All API router modules, procedure definitions, and middleware chains defined within the API boundary package.

Out of scope:
- Internal domain services, direct database schema declarations, and standalone worker background tasks.

## Rationale

- Adopting @trpc/server enables declarative procedure definitions that pair input validation schemas with type-safe resolver functions.
- Middleware composition allows operational concerns like request timing and session verification to be chained consistently across procedure definitions.
- Standardizing on typed RPC procedures establishes clear isolation between incoming transport-level parameters and underlying domain service adapters.

## Consequences

Positive:
- Eliminates contract drift between client consumption and server-side procedure execution via shared TypeScript type inference.
- Centralizes cross-cutting operational concerns such as request timing, authentication gating, and context derivation in reusable procedure middleware.
- Enforces explicit input and output data validation boundaries prior to invoking backend business services.

Negative:
- Tight coupling between client and server TypeScript contracts can complicate independent API versioning across distributed repositories.
- Complex procedure pipelines with extensive context transformations can increase TypeScript compilation duration.

## Alternatives

- Direct untyped HTTP REST routing with manual schema parsing (rejected)
  Rejected because: Lacks automated end-to-end compile-time contract synchronization and requires repetitive boilerplate for input validation and context extraction.
  When valid: When servicing external third-party webhooks that require strict arbitrary HTTP method and path bindings without RPC client capabilities.
- GraphQL schema-first API boundary definition (rejected)
  Rejected because: Introduces additional schema compilation and runtime query execution overhead compared to direct TypeScript-native procedure definitions.
  When valid: When clients require arbitrary field selection and heterogeneous client platforms without TypeScript sharing.

## Risks

- Unvalidated procedure outputs may inadvertently expose internal datastore fields across public boundaries.
  Mitigation: Enforce explicit output schemas on public procedures to prevent accidental internal data leakage.
  Owner: engineering team
- Large router definitions can degrade TypeScript language server responsiveness and increase compilation time.
  Mitigation: Isolate procedural routers into cohesive domain sub-routers and reuse common input validation fragments.
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
- Configure procedure context factories to extract incoming request metadata, cookies, and trace headers into strongly typed context objects before procedure invocation.
- Construct modular routers by grouping domain-specific procedures into distinct router definitions that compose into a unified root router.

## Continuation Context


Verify commands:
- # Discover and execute project test suite for API procedures
npm test
- # Discover and run static type checking across the API package
npm run typecheck

Accept when:
- All API procedures verify compile-time type safety across input, output, and context boundaries.
- Static analysis confirms schema validation is attached to all externally reachable procedure inputs.
- Middleware execution pipelines enforce timing and authentication constraints across all procedure invocations.

## Enforcement

- Verified by: Automated continuous integration type checking and lint rules verifying procedure definitions and schema bindings.
- Verified by: Peer code review validating adherence to procedure construction guidelines and context isolation.
- Violation handling: Procedures missing input schema definitions or bypassing standard procedure builders are rejected at pull request review and build time.
- Exception process: Exceptions requiring raw HTTP endpoints or custom protocol handling must submit an architectural variance detailing security and validation mitigations.