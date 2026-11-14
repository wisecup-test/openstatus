---
version: 2
name: d66de4d0-d4fb-4f90-b6e4-20590d0c7892-trpc-server-procedure-middleware-composition-middleware-functions-extend-procedure-context-immutably
description: "@trpc/server Procedure Middleware Composition: Middleware Functions Extend Procedure Context Immutably"
metadata:
  actual:
    adr_id: d66de4d0-d4fb-4f90-b6e4-20590d0c7892
    status: accepted
    candidate_group_id: 1d5776e9-747d-46ba-b94d-7cef937afdf0
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/**
---

# @trpc/server Procedure Middleware Composition: Middleware Functions Extend Procedure Context Immutably

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/**`

## Context

- API service boundaries require consistent execution of cross-cutting concerns including execution timing, performance telemetry, and client authentication.
- Scattering security enforcement and diagnostic measurements across individual procedure handlers causes implementation divergence and potential authorization omissions.
- The API layer adopts @trpc/server to provide end-to-end type safety and composable request processing pipelines.

## Problem Statement

Exposing API endpoints without centralized middleware pipelines leads to inconsistent diagnostic instrumentation, fragmented authentication enforcement, and duplicated boilerplate within individual procedure handlers.

## Decision

1. SHOULD: Middleware functions SHOULD extend procedure context immutably, forwarding enriched identity and telemetry metadata downstream to subsequent middleware and handlers.

## Policy Block

- SHOULD Middleware functions SHOULD extend procedure context immutably, forwarding enriched identity and telemetry metadata downstream to subsequent middleware and handlers.

In scope:
- All remote procedure definitions and router compositions exposed through @trpc/server.
- Cross-cutting boundary concerns including request execution timing, operational telemetry, and authentication guards.

Out of scope:
- Domain business services and data repository functions executed beneath the procedure boundary.
- Standalone utility functions that operate independently of API procedure request contexts.

Exceptions:
- EXC-24-001: Health check and synthetic ping endpoints require zero authentication and uninstrumented latency.

## Rationale

- Centralizing timing and authentication logic in reusable procedure middleware guarantees uniform metric capture and access control across all routes.
- Composing procedure builders through chaining enables fine-grained specialization between public procedures and authenticated procedures without duplicating shared telemetry logic.
- Static analysis of API boundary definitions confirms that all procedure declarations inherit timing middleware and route-appropriate authentication guards.

## Consequences

Positive:
- Centralizes cross-cutting timing metrics, structured logging, and authentication across all exposed endpoints.
- Guarantees type-safe context augmentation where downstream handlers inherit validated user and workspace metadata.
- Eliminates duplicated guard logic and defensive authorization checks within individual procedure resolvers.

Negative:
- Chained middleware layers add sequential call overhead to procedure execution pathways.
- Procedure builder composition introduces tight coupling to framework-specific context extension semantics.

## Alternatives

- Inline Verification and Telemetry in Procedure Resolvers (rejected)
  Rejected because: Duplicating timing calculations and authentication checks across individual endpoints creates inconsistent telemetry, increases boilerplate, and introduces security drift.
  When valid: Isolated single-endpoint prototypes operating outside shared service boundaries.
- Transport-Level HTTP Gateway Interceptors (rejected)
  Rejected because: External transport-level interceptors lack typed procedure metadata, procedure input access, and direct context injection into procedure resolvers.
  When valid: Coarse network filtering and edge rate-limiting applied indiscriminately before request routing.

## Risks

- Cumulative execution latency across chained middleware layers degrades endpoint throughput.
  Mitigation: Profile middleware execution overhead and avoid blocking I/O operations prior to procedure handler dispatch.
  Owner: API Engineering Team
- Unhandled errors in early middleware stages prevent completion tracking and telemetry emission.
  Mitigation: Wrap downstream invocations in try-finally blocks within telemetry middleware to guarantee metric emission on failure.
  Owner: API Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Middleware functions must handle downstream execution asynchronously and compute total duration upon promise resolution.
- Context enrichment must return a new context object containing validated identity records rather than mutating existing request state.

## Continuation Context


Verify commands:
- Discover and execute the repository type-checking task configured in the project workspace to validate procedure builder middleware chaining and context inference.
- Discover and run the test suite covering API procedure definitions and middleware execution pipelines to verify timing logging and authorization behavior.
- Discover and execute the repository static analysis suite to confirm compliance with procedure declaration conventions.

Accept when:
- Type checking completes without diagnostic errors across all procedure builder chains and downstream context references.
- Middleware tests verify that duration logging and authorization guards execute in prescribed sequence for procedure invocations.
- Static analysis passes with zero violations regarding omitted middleware on protected procedure declarations.

## Enforcement

- Verified by: Automated static analysis verifying procedure builder inheritance and middleware chaining.
- Verified by: Peer code review confirming appropriate base procedure usage on newly defined router endpoints.
- Verified by: Integration test suites verifying sequential execution of timing telemetry and authorization checks.
- Violation handling: Pull requests exposing procedures without required middleware chains are blocked from merging.
- Violation handling: Static analysis failures report missing authentication guards or uninstrumented procedure declarations.
- Exception process: Submit an architectural review request detailing technical constraints preventing standard procedure builder reuse.
- Exception process: Document the approved bypass and compensating controls in the procedure definition file.