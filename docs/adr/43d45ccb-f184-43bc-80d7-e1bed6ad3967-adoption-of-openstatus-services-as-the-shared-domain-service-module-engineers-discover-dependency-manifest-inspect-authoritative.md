---
version: 2
name: 43d45ccb-f184-43bc-80d7-e1bed6ad3967-adoption-of-openstatus-services-as-the-shared-domain-service-module-engineers-discover-dependency-manifest-inspect-authoritative
description: "Adoption of @openstatus/services as the Shared Domain Service Module: Engineers Discover Dependency Manifest Inspect Authoritative"
metadata:
  actual:
    adr_id: 43d45ccb-f184-43bc-80d7-e1bed6ad3967
    status: accepted
    candidate_group_id: 59a6b1ea-bddf-485b-a89f-dea1337ef130
    evidence:
      source: deep
  scope:
    root: apps/server
    paths:
      - src/routes/slack/**
---

# Adoption of @openstatus/services as the Shared Domain Service Module: Engineers Discover Dependency Manifest Inspect Authoritative

Status: proposed
Date: 2025-05-18
Deciders: Detection Pipeline (automated)

## Scope Paths

Root: `apps/server`

Applies to:
- `src/routes/slack/**`

## Context

- Server endpoints handle external integration events and interactive webhook actions that trigger domain state mutations.
- Embedding domain operations and database access directly into route adapters creates tight coupling between transport contracts and core business logic.
- The codebase establishes @openstatus/services as a centralized shared workspace module to encapsulate domain logic, reusable queries, and workflow orchestrations across application routes.

## Problem Statement

Handling interactive external integrations and webhook requests requires coordinating domain business logic, data validation, and asynchronous confirmation workflows. Without a centralized domain service module, route handlers risk embedding redundant data queries and domain logic directly within transport endpoints, causing maintenance overhead and boundary leakage.

## Decision

1. MUST: Engineers MUST discover the dependency manifest and inspect the authoritative repository lock artifact to verify the exact resolved version of @openstatus/services before consuming or altering its exported interfaces.

## Policy Block

- MUST Engineers MUST discover the dependency manifest and inspect the authoritative repository lock artifact to verify the exact resolved version of @openstatus/services before consuming or altering its exported interfaces.

In scope:
- Server route handlers, webhook receivers, and third-party integration adapters executing domain actions.

Out of scope:
- Ephemeral transport formatting helpers, UI message block definitions, and transport-specific signature verification middleware.

## Rationale

- Centralizing domain operations in @openstatus/services establishes a single source of truth for business workflows accessed by diverse route adapters.
- Decoupling route-level transport logic from business services allows independent testing and evolution of domain workflows without modifying endpoint contracts.
- Observed imports across route adapters demonstrate consistent delegation to @openstatus/services, reinforcing architectural consistency across integration boundaries.

## Consequences

Positive:
- Enforces architectural separation between external transport handlers and core domain operations.
- Promotes reusability of core services across multiple communication channels and API endpoints.
- Simplifies unit testing of domain workflows isolated from HTTP and webhook request contexts.

Negative:
- Requires defining explicit parameter mapping and adapter transformations between transport payloads and service contracts.
- Introduces an additional indirection layer between incoming webhook triggers and underlying data stores.

## Alternatives

- Inline Database Queries and Domain Logic within Route Handlers (rejected)
  Rejected because: Coupling transport handlers directly with database queries and business calculations duplicates domain logic across endpoints and impedes testing isolation.
  When valid: Prototyping isolated demonstration endpoints with no requirement for reuse or shared state.
- Independent Domain Service Modules per Integration Channel (rejected)
  Rejected because: Splitting domain services into per-integration silos fragments core business workflows and prevents unified domain entity management.
  When valid: Third-party integrations with proprietary data models that share zero domain overlap with core platform services.

## Risks

- Service layer bloat and tight coupling between transport-specific constructs and shared services.
  Mitigation: Enforce strict separation between transport request types and internal domain input parameters at service boundaries.
  Owner: Backend Engineering Team
- Circular dependencies between domain service modules and database schema definitions.
  Mitigation: Structure workspace dependency trees such that shared services depend on schema packages unidirectionally while transport routes depend only on services.
  Owner: Architecture Team

## Implementation Notes

- DISCOVERY POLICY (MANDATORY): This ADR omits all tool names, file names, commands, package managers, and version numbers. The consumer MUST derive them from the project repository.

LOCK-VERSION GROUNDING (MANDATORY) — before writing code that uses a versioned library, execute in order:
1. Find the dependency manifest in the repo. It declares ranges, not installed versions.
2. Identify the build tool from the manifest.
3. Inspect the repository lock or resolution artifact to determine the exact resolved version. This artifact is authoritative; build-tool output only verifies the active environment matches it.
4. Look up the official documentation, changelog, or public API reference for that exact version. Do not use training-data recall — fetch or search the public internet for version-specific docs.
5. Confirm every API, class, or function you will call exists in that exact version's documentation before using it.
6. For version-sensitive behavior, re-run steps 3-5 per dependency at point of use.
- Locate the workspace package declaration for @openstatus/services within the monorepo structure to inspect its public interface exports and type definitions.
- When adding endpoints that require interaction confirmation or asynchronous domain action execution, ensure service calls decouple request parsing from workflow execution.

## Continuation Context


Verify commands:
- Discover the root project manifest and execute the designated workspace test suite covering route integration and service adapter modules.
- Discover the repository linter and type-checking scripts from the workspace configuration to validate import boundary compliance across route handlers.

Accept when:
- All server route handlers and protocol adapters route domain operations through @openstatus/services rather than executing domain logic inline.
- Project verification scripts and static analysis suites complete with zero boundary violations.

## Enforcement

- Verified by: Automated static analysis verifying import boundaries between route handlers and internal database schema packages.
- Verified by: Peer code review for new route handlers and integration adapters.
- Violation handling: Build failures triggered by boundary linting rules when route handlers bypass @openstatus/services.
- Violation handling: Rejection of pull requests that implement direct database mutations in transport layer files.
- Exception process: Submit an architectural review request documenting why a specialized route cannot use the shared service layer, subject to lead engineer sign-off.