---
version: 2
name: 863b095d-36ed-4ca3-ab07-084f4eff6c50-adoption-of-trpc-server-for-type-safe-api-procedure-architecture-before-implementing-modifying-code-utilizing-trpc
description: "Adoption of @trpc/server for Type-Safe API Procedure Architecture: Before Implementing Modifying Code Utilizing Trpc"
metadata:
  actual:
    adr_id: 863b095d-36ed-4ca3-ab07-084f4eff6c50
    status: accepted
    candidate_group_id: 339fa5ae-e0d7-4a41-b1e8-305ba484af35
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - index.ts
      - src/**
---

# Adoption of @trpc/server for Type-Safe API Procedure Architecture: Before Implementing Modifying Code Utilizing Trpc

Status: proposed
Date: 2025-02-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `index.ts`
- `src/**`

## Context

- The API service layer requires a consistent mechanism to expose typed procedures to client applications without manual endpoint routing and type duplication.
- Cross-cutting requirements including authentication verification, workspace resolution, execution timing, and analytics tracking must execute uniformly across API procedures.
- A consistent error handling and reporting structure is necessary to prevent unhandled internal exceptions from leaking across the network boundary.

## Problem Statement

Managing disparate HTTP endpoint controllers without unified type contracts leads to client-server contract drift, fragmented middleware enforcement, and inconsistent error responses across API surfaces.

## Decision

1. MUST: Before implementing or modifying code utilizing @trpc/server, the consumer MUST inspect the repository lock artifact to determine the exact locked version and verify API compatibility against official version documentation.

## Policy Block

- MUST Before implementing or modifying code utilizing @trpc/server, the consumer MUST inspect the repository lock artifact to determine the exact locked version and verify API compatibility against official version documentation.

In scope:
- All API procedure declarations, router compositions, and endpoint service adapters within the API service package.
- Cross-cutting procedure middlewares responsible for timing, authentication, context enrichment, and error serialization.

Out of scope:
- Third-party incoming webhook callback receivers requiring raw unparsed HTTP request payloads.
- Static asset serving or direct file transfer streams managed outside standard procedure protocols.

## Rationale

- Evidence across eleven API package modules confirms systematic adoption of @trpc/server for router assembly, procedure declarations, and error handling.
- Procedure builders with middleware composition provide a reliable mechanism for enforcing authentication, execution metrics, and workspace context across endpoints.
- Encapsulating failure modes in TRPCError instances maintains consistent client error parsing and prevents exposure of internal execution details.

## Consequences

Positive:
- Guarantees compile-time type safety across client-server procedure invocations without manual code generation steps.
- Centralizes cross-cutting concerns such as authentication, telemetry timing, and context propagation into composable procedure middlewares.
- Standardizes error classification and propagation across all API boundaries via structured error types.

Negative:
- API procedures become tightly coupled to the specific procedural lifecycle and request context model of the adopted library.
- Client consumers outside the supported ecosystem require dedicated adapter layers or OpenAPI generation extensions to consume procedures.
- Breaking API changes across major library revisions necessitate synchronized updates across procedure declarations and client callers.

## Alternatives

- Raw HTTP route handlers with manual request parsing and bespoke JSON serialization (rejected)
  Rejected because: Introduces widespread endpoint boilerplate, requires manual synchronization of request and response types across client and server boundaries, and lacks unified middleware execution pipelines.
  When valid: When building low-level webhook receivers or streaming endpoints that require raw protocol stream management outside standard RPC semantics.
- Schema-first REST or GraphQL API framework with generated code artifacts (rejected)
  Rejected because: Requires additional compilation and schema maintenance lifecycles, separating server implementation from endpoint type contracts.
  When valid: When multi-language third-party public API clients require language-agnostic schema definitions such as OpenAPI or GraphQL schemas.

## Risks

- Monolithic root router assembly can create build-time type instantiation bottlenecks as procedure counts scale.
  Mitigation: Structure procedures into modular domain routers aggregated into a root router hierarchy to preserve module boundaries.
  Owner: engineering team
- Unhandled domain exceptions leaking through procedure handlers may result in generic or unintended error codes.
  Mitigation: Implement boundary adapter functions that isolate domain exceptions and map them explicitly into controlled error representations.
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
- Configure base procedure definitions that establish common request context, identity extraction, and timing telemetry middlewares for reuse across child routers.
- Enforce strict schema validation on all incoming query and mutation payloads before executing business logic.
- Implement dedicated service adapters and error conversion helpers to translate domain service outcomes into appropriate procedure error responses.

## Continuation Context


Verify commands:
- Discover and execute the project dependency and manifest audit scripts to verify lock-file synchronization for all server libraries.
- Discover and execute the project static type analysis and linting scripts to verify compliance of all procedure and router definitions.
- Discover and execute the project automated test suite covering API procedure execution, middleware behavior, and error handling paths.

Accept when:
- All API router modules export procedures constructed using the adopted server procedure builders and middleware chains.
- All API errors originating within procedure handlers conform to standard TRPC error representations.
- Static analysis and build validation confirm zero untyped or disparate procedure declarations across the API package.

## Enforcement

- Verified by: Automated continuous integration checks validating static types and procedure contracts across the repository.
- Verified by: Mandatory peer code review verifying procedure structure, middleware composition, and error encapsulation.
- Violation handling: Pull requests introducing raw unmanaged endpoints or non-standard procedure definitions will fail automated lint and type validation checks.
- Violation handling: Violating pull requests will be blocked from merging until aligned with adopted procedure standards.
- Exception process: Proposals to bypass adopted procedure pipelines or use alternative routing mechanisms must submit an architectural exception request detailing technical necessity and boundary isolation measures to the engineering review team.
- Exception process: Exceptions must document an isolation layer preventing leak of untyped interfaces into the shared application surface.