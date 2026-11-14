---
version: 2
name: 7d21ed9c-0994-406e-94e4-e74443cff981-adoption-of-connectrpc-connect-for-rpc-routing-and-service-handlers-serialization-payload-normalization-errors-encountered-during
description: "Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Serialization Payload Normalization Errors Encountered During"
metadata:
  actual:
    adr_id: 7d21ed9c-0994-406e-94e4-e74443cff981
    status: accepted
    candidate_group_id: a63a9164-0f39-4ae0-992b-b56f3c02ef8f
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/rpc/**
---

# Adoption of @connectrpc/connect for RPC Routing and Service Handlers: Serialization Payload Normalization Errors Encountered During

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/rpc/**`

## Context

- The backend application exposes network endpoints that require strict contract definitions, consistent transport semantics, and type-safe integration across services.
- Multiple handler modules and the central router declare dependencies on @connectrpc/connect and protocol schema packages to process incoming procedure calls.
- A uniform RPC framework standardizes message encoding, routing abstractions, and client-server contract enforcement across boundary services.

## Problem Statement

Unstructured HTTP endpoint architectures often suffer from client-server schema drift, repetitive manual deserialization logic, and lack of compile-time contract verification, leading to runtime failures and inconsistent API surfaces across distributed service boundaries.

## Decision

1. SHOULD: Serialization and payload normalization errors encountered during protocol translation SHOULD be caught and logged with structured context prior to returning protocol-compliant error codes.

## Policy Block

- SHOULD Serialization and payload normalization errors encountered during protocol translation SHOULD be caught and logged with structured context prior to returning protocol-compliant error codes.

In scope:
- All server-side remote procedure call service routes and endpoint handlers.
- Request and response message conversions between internal models and RPC schemas.

Out of scope:
- Static web asset serving and non-RPC HTTP endpoints.
- Internal standalone utility functions that do not participate in network message dispatch.

## Rationale

- Static analysis of the codebase demonstrates widespread adoption of @connectrpc/connect across routing and handler components, standardizing RPC endpoint declarations.
- Adopting schema-driven RPC via @connectrpc/connect enforces strong compile-time type verification, reducing manual payload validation boilerplate and transport inconsistencies.
- Centralizing endpoint dispatch through the framework simplifies route configuration and unifies error handling across diverse service domains.

## Consequences

Positive:
- Guarantees static type safety and schema validation across RPC service interfaces.
- Eliminates custom serialization and routing boilerplate across individual endpoint handlers.
- Ensures consistent error handling and response status propagation aligned with standard protocol conventions.

Negative:
- Increases dependency coupling to the RPC framework and associated schema compilation toolchains.
- Requires explicit conversion layers when mapping between protocol buffer data structures and internal persistence models.

## Alternatives

- Ad-hoc REST API endpoints using standard HTTP handlers (rejected)
  Rejected because: Lacks automated contract synchronization and static typing across service boundaries, requiring manual request parsing and validation.
  When valid: When serving simple public webhooks or endpoints consumed by third parties requiring minimal schema overhead.
- GraphQL schema definitions and execution resolvers (rejected)
  Rejected because: Introduces significant query planning complexity and computational runtime overhead unnecessary for fixed-contract procedure calls.
  When valid: When client applications require flexible, ad-hoc entity querying and client-driven field selection.

## Risks

- Schema serialization mismatches between storage representations and RPC transport models.
  Mitigation: Implement dedicated converter functions that validate and normalize data structures at the handler boundary.
  Owner: Backend Engineering Team
- Breaking API changes introduced when updating service interface definitions.
  Mitigation: Maintain backward-compatible protocol schema fields and verify all service consumers against updated contracts before deployment.
  Owner: Backend Engineering Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Define RPC routes in the centralized routing manifest and delegate execution logic to dedicated service handlers.
- Utilize converter functions to isolate schema parsing exceptions and maintain explicit separation between transport contracts and persistence entities.

## Continuation Context


Verify commands:
- Discover and run the project type-checking script using the designated repository runner to verify type safety across all RPC routes and handlers.
- Discover and execute the test runner target for handler unit and integration suites to confirm schema compliance and error handling.
- Execute the repository linting suite to ensure import structures and handler conventions adhere to project standards.

Accept when:
- The repository type checker completes with zero errors across all RPC routing and handler modules.
- All test suites for RPC route registration and message conversion pass successfully.
- No untyped or ad-hoc request body parsers bypass the RPC framework boundary.

## Enforcement

- Verified by: Continuous integration type checking and automated test execution on all pull requests.
- Verified by: Peer code review verifying that new RPC endpoints adhere to the adopted framework and schema patterns.
- Violation handling: Pull requests containing untyped HTTP endpoints or manual bypasses of the RPC framework are blocked from merging.
- Violation handling: Code review comments require refactoring to use the standard RPC router and handler architecture.
- Exception process: Exceptions for non-RPC public endpoints must be documented and approved by backend engineering leads.