---
version: 2
name: b89dbfd7-960a-4381-bb5a-0978f41f8f9d-adoption-of-openstatus-services-page-subscriber-for-subscriber-domain-operations-router-procedures-invoking-openstatus-services-page
description: "Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Router Procedures Invoking Openstatus Services Page"
metadata:
  actual:
    adr_id: b89dbfd7-960a-4381-bb5a-0978f41f8f9d
    status: accepted
    candidate_group_id: 01c762af-4a84-4af7-9403-58e61f130235
    evidence:
      source: deep
  scope:
    root: packages/api
    paths:
      - src/router/**
---

# Adoption of @openstatus/services/page-subscriber for Subscriber Domain Operations: Router Procedures Invoking Openstatus Services Page

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `packages/api`

Applies to:
- `src/router/**`

## Context

- API router endpoints require access to status page subscriber records for public token verification and authenticated administrative listing.
- Directly embedding persistence operations and business logic within transport router definitions leads to schema coupling and logic duplication.
- The codebase establishes domain-specific service packages to encapsulate subscriber operations behind dedicated functional boundaries.

## Problem Statement

Scattering subscriber persistence and validation logic across API router endpoints creates maintenance friction, complicates schema migrations, and violates domain boundary separation.

## Decision

1. MUST: Router procedures invoking @openstatus/services/page-subscriber functions MUST convert inbound request context using a service context adapter and pass structured inputs conforming to the service contract.

## Policy Block

- MUST Router procedures invoking @openstatus/services/page-subscriber functions MUST convert inbound request context using a service context adapter and pass structured inputs conforming to the service contract.

In scope:
- API procedures and routers handling status page subscriber subscriptions, token verification, and lifecycle workflows.

Out of scope:
- Direct database operations on unrelated domain entities that have no subscriber interaction.
- Client-side applications that communicate with the subscriber domain exclusively via external protocol endpoints.

## Rationale

- Delegating subscriber domain operations to @openstatus/services/page-subscriber establishes a clean boundary between network routing concerns and subscriber domain logic.
- Centralizing subscriber retrieval and listing logic within a dedicated service module prevents schema and domain rule drift across disparate router entry points.
- Structured adaptation between API procedure context and service context ensures consistent authorization, session handling, and error propagation.

## Consequences

Positive:
- Encapsulates subscriber data operations and token validation logic behind a stable, dedicated service interface.
- Simplifies router procedures by reducing boilerplate data orchestration to direct service invocations.
- Promotes testability by allowing subscriber domain services to be tested independently from transport and routing layers.

Negative:
- Introduces an additional layer of context adaptation and error mapping between routers and service functions.
- Requires coordination across package boundaries when updating subscriber domain contracts or input schemas.

## Alternatives

- Direct inline database queries within each API router procedure (rejected)
  Rejected because: Duplicating query logic and validation rules across multiple router procedures increases maintenance overhead and risks schema inconsistencies.
  When valid: Valid only in single-file prototypes or disposable scripts where domain boundaries do not exist.
- Embedding subscriber logic into an all-inclusive monolithic API layer (rejected)
  Rejected because: Couples routing and transport mechanics directly to domain state, preventing reuse across background workers and external processes.
  When valid: Valid when application architecture is strictly single-tiered with no shared worker or background domains.

## Risks

- Service context mismatch if the router fails to supply necessary authorization or tenancy fields to the service adapter.
  Mitigation: Enforce schema validation at service input boundaries and utilize typed service context mapping functions.
  Owner: engineering team
- Unhandled domain exceptions escaping through the router without proper client-facing error mapping.
  Mitigation: Wrap all service procedure invocations in structured try-catch blocks delegating to standardized error transformation helpers.
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
- Locate the shared service context adapter to transform router execution contexts into domain-compatible service contexts prior to invocation.
- Ensure all input payloads passed to subscriber service functions are pre-validated against input schemas in procedure definitions.

## Continuation Context


Verify commands:
- Discover and run the project static analysis and type checking script to verify interface compatibility with the subscriber service module.
- Discover and run the project test suite covering router endpoints and domain service integration.

Accept when:
- Static type analysis completes with zero errors across all router procedures importing the subscriber service.
- Automated test suites verify that subscriber token resolution and list retrieval operate correctly via the service boundary.

## Enforcement

- Verified by: Automated static analysis and type checks within the continuous integration pipeline.
- Verified by: Peer code review verifying that new subscriber logic delegates to the service package.
- Violation handling: Pull requests containing direct database queries for subscriber entities outside the dedicated service module will be blocked.
- Exception process: Exceptions must be documented with an architectural justification and approved by the engineering leads.