---
version: 2
name: 3e0087a4-8edc-427e-980d-20a069de4bbd-adoption-of-openstatus-services-internal-service-library-for-api-operations-router-procedure-invocations-domain-service-operations
description: "Adoption of @openstatus/services Internal Service Library for API Operations: Router Procedure Invocations Domain Service Operations"
metadata:
  actual:
    adr_id: 3e0087a4-8edc-427e-980d-20a069de4bbd
    status: accepted
    candidate_group_id: 325c1754-ad4f-40ef-aa1d-165da2308b07
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services Internal Service Library for API Operations: Router Procedure Invocations Domain Service Operations

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/**`

## Context

- API router procedures require access to core domain capabilities including incidents, maintenance windows, status reports, members, and workspace resources.
- Embedding persistence logic directly inside transport route handlers leads to logic duplication across multiple transport protocols and tight coupling with router mechanics.
- The project employs an internal domain service library to encapsulate business logic and data access outside of transport routing layers.
- A service adapter layer provides context transformation and exception mapping between transport routers and domain services.

## Problem Statement

Transport router procedures can easily accumulate direct database queries, ad-hoc entity validation, and divergent error translations, causing business logic fragmentation and preventing code reuse across varying transport protocols.

## Decision

1. MUST: Router procedure invocations of domain service operations MUST catch exceptions and transform domain failures into standardized transport errors using toTRPCError or throwFromException.

## Policy Block

- MUST Router procedure invocations of domain service operations MUST catch exceptions and transform domain failures into standardized transport errors using toTRPCError or throwFromException.

In scope:
- All API router procedure handlers defining queries, mutations, or subscriptions.
- Domain service integrations across workspace packages interacting with core entities.

Out of scope:
- Static client-side assets and utility functions with no server-side business logic.
- Transport middleware that does not interact with domain data or persistent state.

## Rationale

- Static analysis indicates consistent adoption of @openstatus/services subpath modules across multiple API router definitions.
- Routing procedures systematically invoke toServiceCtx for context normalization and toTRPCError or throwFromException for error handling.
- Centralizing domain logic inside a dedicated internal library ensures consistency across operational features and simplifies compliance testing.

## Consequences

Positive:
- Decouples transport routers from database operations and domain logic, enabling shared business rule reuse.
- Standardizes error handling and context propagation across all API procedure handlers.
- Enhances testability by allowing business logic to be tested independently of the transport router layer.

Negative:
- Introduces boundary boilerplate where each route handler must adapt context and catch errors through adapter functions.
- Requires synchronous interface maintenance across both the domain service modules and the transport adapter layer.

## Alternatives

- Direct datastore querying and business logic execution inside API router procedures (rejected)
  Rejected because: Tightly couples transport handling with persistence and domain logic, preventing code reuse across alternative transport interfaces and violating clean boundary separation.
  When valid: Trivial read-only prototypes where domain logic reuse and transport neutrality are not required.
- Monolithic service entry points without adapter functions (rejected)
  Rejected because: Couples service signatures directly to transport context structures and leaks transport-specific error semantics into the core domain library.
  When valid: Single-transport microservices with homogeneous context structures across all layers.

## Risks

- Context drift between transport-specific request structures and the standardized service context.
  Mitigation: Maintain strictly typed context interfaces shared between the transport layer and service definitions.
  Owner: engineering team
- Latency overhead introduced by intermediate context mapping and error wrapping layers.
  Mitigation: Keep adapter transformation functions thin, pure, and thoroughly unit tested.
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
- Ensure that newly authored service functions remain transport-agnostic by consuming standardized service context rather than transport request objects.
- Keep domain exception types distinguishable so that adapter error handling can map domain failures to correct transport status codes.

## Continuation Context


Verify commands:
- Discover the repository test runner script from the configuration manifest and execute the full test suite covering API routes and service adapters.
- Discover the workspace static analysis and type checking scripts from the configuration manifest and execute validation across all affected packages.

Accept when:
- All route procedure calls successfully delegate domain logic to @openstatus/services through service context and error adaptation.
- Type checking and static verification across all API modules pass without unresolved imports or structural boundary violations.
- Integration and unit test suites covering API router endpoints pass with zero unhandled exceptions.

## Enforcement

- Verified by: Automated static analysis and type verification executed during continuous integration.
- Verified by: Architectural review of pull requests modifying router definitions and service interactions.
- Violation handling: Pull requests introducing direct persistence operations or bypassing service context adaptation must be rejected in review.
- Violation handling: Static analysis failures caused by non-compliant service boundaries block build artifact generation.
- Exception process: Exceptions must be formally documented in the repository review process with architectural justification approved by team leads.
- Exception process: Any approved bypass must define a remediation plan and tracking ticket to transition logic into the service layer.