---
version: 2
name: ea65941e-f0e8-426b-bf08-ab388d62c9af-trpc-client-library-adoption-applications-use-trpc-client-dedicated-remote
description: "@trpc/client Library Adoption: Applications Use Trpc Client Dedicated Remote"
metadata:
  actual:
    adr_id: ea65941e-f0e8-426b-bf08-ab388d62c9af
    status: accepted
    candidate_group_id: 3057ad28-51be-466c-8491-81d8aa5079b9
    evidence:
      source: deep
  scope:
    root: apps/web
    paths:
      - src/trpc/**
---

# @trpc/client Library Adoption: Applications Use Trpc Client Dedicated Remote

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/web`

Applies to:
- `src/trpc/**`

## Context

- Applications requiring direct client-to-server and server-to-server internal API communication require end-to-end type safety and consistent protocol serialization without manual schema synchronization.
- Client and server execution environments within the web application share common procedure contracts and serialization configurations while maintaining execution-boundary separation.
- Remote procedure calls require consistent error handling, telemetry interception, and request source attribution across both client-side and server-side callers.

## Problem Statement

Managing client-server API interactions with uncoordinated schemas, disparate request serialization, and fragmented telemetry leads to runtime type mismatch, redundant boilerplate, and inconsistent error observability across runtime boundaries.

## Decision

1. MUST: Applications MUST use @trpc/client as the dedicated remote procedure call client library for communicating with backend procedures across client and server environments.

## Policy Block

- MUST Applications MUST use @trpc/client as the dedicated remote procedure call client library for communicating with backend procedures across client and server environments.

In scope:
- All web application client-side and server-side modules that initiate internal remote procedure calls to backend API procedures.
- Shared client communication configurations and procedure client boundary instantiations.

Out of scope:
- Third-party public external HTTP integrations that do not implement the internal procedure contract.
- Direct database access layers isolated within core service execution environments.

Exceptions:
- EXC-20-001: External third-party webhooks or legacy external integrations communicate via arbitrary un-typed payloads.

## Rationale

- @trpc/client provides end-to-end static type inference directly from backend procedure definitions to callers without manual code generation or duplicate schema definitions.
- Centralizing shared procedure client configurations enables uniform error reporting, serialization transformations, and source metadata attribution across server and browser runtimes.
- Header sanitization in server-side invocation paths eliminates invalid hop-by-hop transport headers when delegating upstream procedure requests.

## Consequences

Positive:
- Eliminates manual client API contract generation while guaranteeing full static type safety across client and server invocation boundaries.
- Standardizes serialization transformations, error tracking, and request source provenance across all internal procedure calls.
- Simplifies refactoring because changes in backend procedure signatures immediately surface as compile-time type errors in calling code.

Negative:
- Couples the client transport layer directly to the procedure client contract and shared serialization specifications.
- Requires strict synchronization of shared dependency versions between calling runtimes and backend service boundaries.
- Server-side invocation contexts require explicit header forwarding and sanitization logic to preserve request tracing context.

## Alternatives

- Manual HTTP client calls with handwritten type definitions (rejected)
  Rejected because: Lacks automated compile-time contract enforcement and increases surface area for runtime schema drift between caller and server.
  When valid: Connecting to unmanaged third-party REST services lacking procedure definitions.
- Code-generated API client from intermediate schema definitions (rejected)
  Rejected because: Introduces an intermediate build step and schema synchronization overhead that slows iterative development cycles.
  When valid: Exposing public external APIs to heterogeneous multi-language third-party consumers.

## Risks

- Version discrepancies between client procedure bindings and server route definitions could lead to runtime invocation errors.
  Mitigation: Enforce lock-version grounding and monorepo contract synchronization across all dependent modules prior to deployment.
  Owner: Engineering team
- Improper header propagation in server-side procedure calls may leak sensitive headers or fail upstream validation.
  Mitigation: Maintain centralized server-side client construction utilities that automatically strip hop-by-hop headers and inject source identifiers.
  Owner: Engineering team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Construct client and server procedure callers using shared client link factories that encapsulate transformer specifications and telemetry listeners.
- Ensure server-side procedure invokers strip hop-by-hop transport headers before forwarding inbound request headers to upstream endpoints.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type verification script to ensure all procedure calls satisfy backend contract signatures.
- Discover and run the project test suite to validate procedure client request serialization and header sanitization behavior.

Accept when:
- The static type verification passes with zero type errors across all client-side and server-side procedure invocation modules.
- Procedure client configurations correctly apply shared serialization transformers and propagate required source headers without hop-by-hop transport headers.

## Enforcement

- Verified by: Automated continuous integration type checking and static analysis workflows.
- Verified by: Pull request code reviews focusing on procedure client boundary configurations.
- Violation handling: Pull requests introducing un-typed remote calls or bypassing shared procedure client utilities will be blocked by automated checks and review gates.
- Exception process: Submit an architectural exception request detailing the requirement for un-typed or alternative transport, reviewed by the engineering architecture team.